# HTTP/2 Request Smuggling

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [HTTP/2](#http2)
- [HTTP/2 Desync](#http2-desync)
    - [H2.CL Smuggling](#h2cl-smuggling)
    - [H2.TE Smuggling](#h2te-smuggling)
    - [CRLF Injections](#crlf-injections)
- [HTTP/2 Request Tunneling](#http2-request-tunneling)
    - [HTTP/2 Request Tunneling: Leaking Internal Headers](#http2-request-tunneling-leaking-internal-headers)
    - [HTTP/2 Request Tunneling: Bypassing Frontend Restrictions](#http2-request-tunneling-bypassing-frontend-restrictions)
    - [HTTP/2 Request Tunneling: Web Cache Poisoning](#http2-request-tunneling-web-cache-poisoning)
- [h2c Smuggling](#h2c-smuggling)
    - [HTTP Versions-Verhandlung](#http-versions-verhandlung)
    - [h2c Upgrades Beispiel](#h2c-upgrades-beispiel)
    - [Tunneling Requests via h2c Smuggling](#tunneling-requests-via-h2c-smuggling)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Prüfmethoden für Penetrationstester (konkret, reproduzierbar)](#prüfmethoden-für-penetrationstester-konkret-reproduzierbar)
- [Zusammenfassung (short checklist)](#zusammenfassung-short-checklist)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung
Das HTTP/2 Protokoll ist die Erweiterung des Vorgängers HTTP/1.1. Der Aufbau ist ähnlich, jedoch gibt es im HTTP-Request Besonderheiten, die du in dieser Datei erfahren wirst. Es ist wichtig, dass du die [Grundlagen von HTTP und HTTPS](/03-web-security/grundlagen/03_http_https_grundlagen.md) kennst. Auch wäre es von Vorteil, wenn du dich mit Tools wie Burp oder ZAP auskennst.

Das Ziel von HTTP/2 ist es, die Latenz und das Head-of-line-blocking zu reduzieren, indem es Requests und Responses im Multiplexing-Verfahren behandelt.

> **Multiplexing:** Ein Verfahren, bei dem mehrere Signale oder Datenströme zu einem einzigen zusammengesetzten Signal kominiert werden, um eine gemeinsame Übertragungsleistung effizienter zu nutzen. Am Zielort wird mit `demultiplexer` das Signal wieder in die ursprünglichen Datenpakete umgewandelt. 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## HTTP/2
Im Gegensazu zum ursprünglichen HTTP, bringt die zweite Version des HTTP-Protokolls einige Spezifikationen mit sich. HTTP/2 löst das Problem der HTTP/1.1 Version, in der es Angreifern möglich ist, das Nachrichtenformat zu ändern und darüber hinaus die Kommunikation bestimmen, die zwischen Client und Server existiert.

Der Hauptunterschied von HTTP/2 und HTTP/1.1 liegt in der Verarbeitung der Daten. Währen das HTTP/1.1 Protokoll Daten in menschenlesbarem Format interpretiert, übermittelt das HTTP/2 Protokoll Daten in Binär.

Der Vorteil liegt darin, dass mit dieser Änderung die Ausführung schneller und effizienter ist, da Maschinen mit Binärlogik besser umgehen können.

**Vereinfachte Darstellung eines HTTP/2 Requests:**
```http
+-------------------------------------------------------------------------+
| Original HTTP/2 Header Aufbau (beispielhaft)                            |
| 50 52 49 20 2a 20 48 54 54 50 2f 32 2e 30 0d 0a 0d 0a 53 4d 0d 0a 0d 0a |
+-------------------------------------------------------------------------+

vereinfachte Darstellung          |     Normaler HTTP/1.1 Request Header
HTTP/2.0                          |     HTTP/1.1
:method     POST                  |     POST /hello HTTP/1.1\r\n
:path       /hello                |     User-Agent: MOzilla Firefox\r\n
:scheme     https                 |     Host: 10.10.10.10:8080\r\n
:authority  10.10.10.10:8080      |     \r\n
user-agent  Mozilla Firefox       |     q=blablabla
content-length 14                 |
q=blablabla                       |
```

Der HTTP/2 Request hat folgende Eigenschaften:

- **Pseudo-Header:** Header, die mit einem `:` beginnen, sind die minimalsten Anforderungen, die für einen gültigen HTTP/2 Request benötigt werden. Im Beispiel oben werden `:method`, `:path`, `:scheme` und `:authority` als Pseudo-Header verwendet.
- **Header:** Nach den Pseudo-Headern kommen die regulären Header wie `user-agent` und `content-length` wie du sie aus den "normalen" HTTP-Request Headern gewohnt bist (**beachte das lower-case Muster in HTTP/2**).
- **Request Body:** Genau wie HTTP/1.1, beinhaltet der HTTP/2 Request Body die ursprünglichen Daten (`GET`, `POST` usw.) 


> **Hinweis:** Auch, wenn `Content-Length` und `Transfer-Encoding` in einem HTTP/2-Request Header ignoriert werden, senden moderne Browser diese Header dennoch aus Gründen des HTTP-Downgrades mit.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## HTTP/2 Desync
**HTTP/2 Downgrading** passiert, wenn ein Reverse Proxy Inhalte an den Client per HTTP/2 übermittelt (Frontend), die es von einem Backend-Server mit HTTP/1.1 erhält.   
Diese Art und Weise der Implementation macht es möglich, dass HTTP Request Smuggling Angriffe populierbar sind, jedoch nur in Systemen, in denen HTTP/2 Downgrades passieren.

```text
+--------+              +----------------+                +--------------------+
| Client |<==[HTTP/2]==>| Frontend Proxy |<==[HTTP/1.1]==>| Backend Web Server |
+--------+              +----------------+                +--------------------+
z.B. Chrome Browser
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### H2.CL Smuggling
**H2.CL-Smuggling** bezeichnet eine Klasse von Misskonfigurationen, die auftreten, wenn ein Frontend (HTTP/2-fähiger Proxy/Load-Balancer) einen in HTTP/2 empfangenen Request in ein **HTTP/1.1-Format** für ein Backend übersetzt und dabei die Längenangabe (`Content-Length`) falsch bildet oder unterschiedlich interpretiert wird. Durch absichtliche Manipulation oder Mehrdeutigkeit der Längenangabe kann ein Angreifer erreichen, dass das Backend zusätzliche Bytes als neue, eigenständige Request(s) interpretiert - also **Request Smuggling**.

**Warum das passiert (technisch):**

- HTTP/2 transportiert Body-Daten in **DATA-Frames** (keine CRLF-getrennten Headers/Body-Grenzen wie bei HTTP/1.1).

- Beim Übersetzen in HTTP/1.1 muss das Frontend den Body als zusammenhängende Byte-Sequenz an das Backend übergeben und dabei typischerweise einen `Content-Length`-Header setzen.

- **Fehlerquellen:** Falsche Berechnung der Länge, doppelte/konfliktbehaftete `Content-Length`-Header, oder unterschiedliche Prioritätensetzung beim Handhaben von mehrfachen Length-Angaben.

- **Ergebnis:** Backend liest N Bytes (laut CL) und sieht danach zusätzlich Bytes im Socket - diese werden als neue Request interpretiert.


**Ablauf (vereinfachtes Ablaufdiagramm):**
```scss
Client (HTTP/2) --> Frontend Proxy (terminiert HTTP/2, übersetzt in HTTP/1.1) --> Backend (HTTP/1.1)
                           [Fehlerhafte CL-Bildung]
```

**Beispiel (sequentiell, vereinfachte Darstellung):**

1. Client sendet per HTTP/2 einen Request mit einem Body von 20 Bytes.

2. Proxy bildet daraus ein HTTP/1.1-Request an das Backend und schreibt fälschlich `Content-Length: 10`.

3. Das Backend liest 10 Bytes als Body; die restlichen 10 Bytes verbleiben auf dem Socket → Backend interpretiert danach die weiteren Bytes als Anfang eines neuen Requests.

**Hypothetischer HTTP/1.1-Payload (was das Backend sieht):**
```http
POST /upload HTTP/1.1
Host: backend.local
Content-Length: 10
[CRLF]
<10 bytes von Body (Teil)>
POST /admin HTTP/1.1
Host: backend.local
Content-Length: 0
[CRLF]
```

-> Das zweite `POST /admin` ist vom Angreifer eingeschleust, weil die Frontend-Längenangabe das Socket-Stream-Layout zerstört hat.

**Beweisbarkeit / Erkennung beim Test:**

- Sende Requests über HTTP/2 mit unterschiedlicher Body-Größe und beobachte, ob das Backend zusätzliche Requests erhält (z. B. durch Erzeugen auffälliger Pfade, Logging, oder „canary“-Requests).

- Prüfe Frontend-Logs/Konfiguration: wie wird die `Content-Length` berechnet? Werden DATA-Frames aggregiert? Werden mehrere `Content-Length`-Header normalisiert/erlaubt?




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### H2.TE Smuggling
**H2.TE-Smuggling** beruht auf falscher Übersetzung des `TE`-Headers (oder der Behandlung von `Transfer-Encoding`) beim Übergang HTTP/2 → HTTP/1.1. Im HTTP/2-Kontext ist `Transfer-Encoding` grundsätzlich anders relevant: der `TE` Header darf in HTTP/2 nur die Werte für Trailers spezifizieren (z. B. `te: trailers`), während `Transfer-Encoding: chunked` ein HTTP/1.1-Mechanismus ist. Wenn ein Proxy `te: chunked` oder andere `TE`-Indikationen falsch in `Transfer-Encoding: chunked` für das Backend überführt, kann das Backend den Request als `chunked` codiert parsen - und das eröffnet Smuggling-Wegen.

**Warum das passiert (technisch):**

- HTTP/2 Header erlauben `te` mit bestimmten Werten; manche Proxies konvertieren `te` oder bestimmte Header zu HTTP/1.1 `Transfer-Encoding` unsauber.

- Wird beim Backend `Transfer-Encoding: chunked` angenommen, liest es den Body bis zum `chunked`-Terminator (`0\r\n\r\n`) - wenn der Frontend-Proxy aber die echten DATA-Bytes linear sendet, kann ein Angreifer durch kontrollierte Bytes zusätzliche Request-Bytes anfügen, die nach dem `chunked`-Terminator als neue Request interpretiert werden.

**Ablauf (vereinfachtes Ablaufdiagramm):**
```scss
Client (HTTP/2, te: trailers?) -> Frontend (konvertiert fehlerhaft) -> Backend (interpretiert chunked)
```

**Konkretes Beispiel (Textform):**

- Angreifer sendet HTTP/2 HEADER inklusive:
```makefile
:method: POST
:path: /upload
:authority: backend.local
te: trailers
```

- Ein buggy Proxy wandelt dies beim Forwarding in:
```http
POST /upload HTTP/1.1
Host: backend.local
Transfer-Encoding: chunked
[CRLF]
<chunked body data according to proxy>
```

- Falls der Proxy die body-Bytes so setzt, dass nach dem Chunk-Terminator zusätzliche Bytes folgen, interpretiert das Backend diese zusätzlichen Bytes als neue (geschmuggelte) Request(s).

**Typische Signaturen im Testing:**
- Anwenden von `te: trailers`, `te: chunked`, oder ungewöhnliche Kombinationen mit `content-length`.

- Senden von Payloads, die bei korrekter `chunk`-Parsing Lageänderungen bei der Stream-Trennung verursachen (z. B. bewusste Null-Chunks).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### CRLF Injections
**CRLF-Injektionen** entstehen, wenn ein System zulässt, dass `\r` (CR) oder `\n` (LF) ungefiltert in Header-Werte oder anderen Header-konstruierenden Kontexten ankommen und anschließend so interpretiert werden, dass neue Header oder Request-Zeilen erzeugt werden. In HTTP/2 ist die Oberfläche reduziert (Header sind in HPACK kodiert), aber Implementierungsfehler beim Decoding oder beim Übersetzen in HTTP/1.1 können CRLF-Sequenzen wiederherstellen und so Injektionen ermöglichen.


**Warum das passiert (technisch):**

- In HTTP/2 sind Header als Strings in HPACK kodiert; ein korrekter HPACK-Decoder sollte keine CRLF-Metazeichen in der erzeugten Header-Namen/-Werten zu verwertbaren Raw-CRLFs in der resultierenden HTTP/1.1-Repräsentation führen.

- Fehlerhafte Sanitization oder direkte Zusammensetzung von Header-Lines (z. B. `sprintf("%s: %s\r\n", name, value)`) ohne Prüfung der Inhalte erlaubt es, dass ein value `\r\nHost: attacker` enthält und so zusätzliche Header/Requests entstehen.


**Beispiel (vereinfachte, angreifbare Kette):**
- Angreifer sendet in einem Header-Wert (HTTP/2 HPACK codiert) den String `value = "foo\r\nContent-Length: 0\r\n\r\nGET /admin HTTP/1.1\r\nHost: backend.local\r\n\r\n"`.
- Ein buggy Proxy decodiert HPACK, formt Header-Lines und fügt diese unverändert in das HTTP/1.1-Wire-Format: dadurch entstehen neue Header-Zeilen und ein neuer Request.

**Konkretes demonstratives Example (wie das Backend es sehen könnte):**

```http
POST /upload HTTP/1.1
Host: backend.local
X-User-Name: foo
Content-Length: 0

GET /admin HTTP/1.1
Host: backend.local
```
-> `GET /admin` ist eingeschleust.


**Testmethodik / Erkennung:**
- Packe CRLF (`%0d%0a`) in Header-WErte (HPACK kodiert) und beobachte Backend-Logik.

- Achte auf Proxy-Code, der Header-Werte ohne Validierung zusammenbaut.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## HTTP/2 Request Tunneling
Unter **HTTP/2 Request Tunneling** wird hier die Nutzung von HTTP/2-Mechanismen verstanden, um HTTP/1.1-Requests oder Requests für andere Ressourcen „durchzuschleusen“ bzw. Requests an interne Services weiterzuleiten - häufig über Mehrfachanfragen in einem Stream, Pseudo-Headers oder via speziellen Routen. In Smuggling-Kontext bedeutet das: ein Angreifer benutzt die Art und Weise, wie Frontend und Backend Request-Grenzen übersetzen, um nicht vorgesehene interne Endpunkte zu erreichen.

**Mechaniken:**

- **Multiplexing:** Viele Streams parallel; wenn die Übersetzung falsch arbeitet, kann ein Stream-Grenzwert die Grenzen eines anderen Streams überlappen.

- Header-/Body-Inkonsistenzen (CL/TE) → Einschleusen von weiteren Request-Zeilen oder Pfaden.

- Verwendung bestimmter pseudo-Header Kombinationen, um das Übersetzungsverhalten zu triggern.

**Beispiel-Szenario:**

- Ein Proxy übersetzt HTTP/2 → HTTP/1.1; ein Angreifer erzeugt in einem Stream eine Abfolge, die beim Backend als Anfrage an `/internal/status` interpretiert wird - obwohl der Proxy eigentlich nur `/upload` forwarden sollte.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HTTP/2 Request Tunneling: Leaking Internal Headers
**Leaking internal headers** beschreibt, dass Header, die eigentlich intern bleiben sollten (z. B. `X-Internal-Admin: true`, `X-Backend-Server`), durch falsche Normalisierung oder Logging an den Client zurückgelangen - oder dass ein Angreifer durch Smuggling Antworten provoziert, in denen interne Header sichtbar sind.

**Warum das passiert (technisch):**

- Beim Smuggling werden oft zusätzliche Responses/Requests erzwungen. Server antwortet auf die eingeschmuggelte Request-Zeile und legt in dieser Antwort interne Header offen.

- Fehlerhafte Header-Stripping am Proxy kann interne Header durchlassen.

**Beispiel:**

- Angreifer schmuggelt Request `/admin` in den Stream.

- Backend antwortet mit `200 OK` und gibt in der Antwort Header `X-Backend-Version: 1.2.3` aus.

- Der Frontend-Proxy sendet diese Response zum Client (oder loggt sie unverschlüsselt), wodurch interne Informationen geleakt werden.

**Testmethodik / Erkennung:**

- Prüfe Antworten auf unerwartete Header (z. B. debug/stack/identifizierende Header).

- Simuliere smuggling-Versuche und beobachte, ob zusätzliche Responses mit internen Headern ausgeliefert werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HTTP/2 Request Tunneling: Bypassing Frontend Restrictions
Hierunter fallen Fälle, in denen Frontend-Einschränkungen (z. B. Pfad-Whitelist, WAF-Regeln) durch Smuggling umgangen werden und Backend interne Funktionen erreichen.

**Mechanismen:**

- Wenn das Frontend Filter auf Header/Pfad Ebene anwendet, aber Backend die tatsächliche Parselogik anders interpretiert, kann ein eingeschmuggelter Request einen Pfad erreichen, der am Frontend hätte blockiert werden sollen.

- Z.B. Filter prüft `:path` der HTTP/2 HEADERS, aber Backend liest eine nachträglich eingeschmuggelte Request-Zeile mit anderem Path.

**Beispiel (Ablauf):**

1. Frontend blockiert `/admin` in HTTP/2 Pseudoheader `:path`.

2. Angreifer schmuggelt durch CL/TE-Manipulation eine zweite Request-Zeile `GET /admin HTTP/1.1` in den Socket, die das Backend ohne Frontend-Checks verarbeitet.

3. Backend führt `/admin` aus, obwohl Frontend es blockiert hat.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HTTP/2 Request Tunneling: Web Cache Poisoning
**Cache Poisoning** passiert, wenn eine Antwort, die eigentlich für einen bestimmten Request bestimmt ist, im Cache unter einer anderen Key/URL gespeichert wird und zukünftige Nutzer diese manipulierte Antwort erhalten. HTTP/2-Smuggling kann dazu verwendet werden, dass Backend oder Cache eine Antwort auf eine eingeschmuggelte Request abspeichert.

**Mechanik bei HTTP/2-Smuggling:**

- Wenn ein eingeschmuggelter Request an einen Cache nötige Cache-Control/URL liefert, kann die resultierende Antwort unter einem legitimen Key gecached werden.

- Ein falsch synchronisierter Frontend/Cache-Proxy, das Request-Grenzen nicht sauber behandelt, kann Antworten falsch zuordnen.

**Beispiel:**

1. Angreifer schmuggelt eine Request-Zeile `GET /index.html` mit manipulierter Host/Header/Query.

2. Backend liefert eine Antwort mit `Cache-Control: public, max-age=3600`.

3. Proxy/Cache speichert diese Antwort für `/index.html`.

4. Normale Benutzer erhalten die manipulierte, vom Angreifer herbeigeführte Antwort.

**Testhinweis:**

- Verwende unique tokens in Requests und prüfe Cache-Keys nach Smuggling-Versuchen.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## h2c Smuggling
`h2c` bezeichnet **HTTP/2 cleartext** (keine TLS) - also HTTP/1.1 Upgrade auf HTTP/2 via `Upgrade: h2c` oder h2c Start-Prior Knowledge (selten). h2c-Smuggling umfasst Schwächen bei der Upgrade-Implementierung: insbesondere wenn ein Server oder Proxy HTTP/1.1 und HTTP/2 Parser nebeneinander hat und die Übergabe/Upgrade-Sequenz nicht sauber getrennt ist.

**Relevante RFC-Mechanik:**

- **HTTP/1.1 Upgrade auf h2c ist möglich:** Client sendet `Upgrade: h2c` und HTTP2-Settings Header, Server antwortet mit `101 Switching Protocols` und dann folgt HTTP/2 Frames.

- **Die Upgrade-Sequenz ist komplex:** beide Parser (HTTP/1.1 und HTTP/2) müssen synchron und korrekt implementiert werden.


**Warum Smuggling möglich ist:**

- Wenn Upgrade-Handling buggy ist, kann ein Angreifer den Übergang so manipulieren, dass Reste der HTTP/1.1-Bytes als zusätzliche Anfrage interpretiert werden.

- Manche Proxies erlauben h2c in internen Netzwerken - ein Angreifer kann h2c benutzen, um Proxies in einen undefinierten Zustand zu bringen.

**Beispiel (vereinfachter Ablauf):**

- Client initiiert Upgrade: `Upgrade: h2c` mit HTTP2-Settings.

- Server antwortet (buggy) `101 Switching Protocols` aber liest weiterhin Reste als HTTP/1.1 Requests - Angreifer nutzt Überschussbytes zur Einschleusung.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HTTP Versions-Verhandlung
**Versionsverhandlung** findet beim Verbindungsaufbau statt (TLS-ALPN, HTTP/1.1 Upgrade, Prior Knowledge). Probleme entstehen, wenn Frontend und Backend unterschiedliche Erwartungen an die verwendete HTTP-Version haben bzw. die Konvertierung inkonsistent ist.

**Mechanismen:**

- **TLS ALPN (Application-Layer Protocol Negotiation):** Client und Server verhandeln HTTP/2 (h2) vs HTTP/1.1 über TLS-Handshake. Bei ALPN-Mismatch kann ein Proxy dennoch HTTP/2 terminieren und HTTP/1.1 zum Backend nutzen.

- **HTTP/1.1 Upgrade (h2c):** Wie oben beschrieben.

- **Prior Knowledge:** Client sendet HTTP/2-Frames direkt ohne Preface (nur in speziellen Szenarien).

**Testrelevanz:**

- Untersuche, ob Frontend via ALPN HTTP/2 terminiert, aber Backend HTTP/1.1 verwendet - das ist der klassische Ausgangspunkt für H2→H1 Smuggling-Probleme.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### h2c Upgrades Beispiel
```http
# Client (HTTP/1.1) versucht Upgrade auf h2c
GET / HTTP/1.1
Host: example.com
Connection: Upgrade, HTTP2-Settings
Upgrade: h2c
HTTP2-Settings: <base64url(HTTP2 SETTINGS frame)>
```

- Server antwortet `101 Switching Protocols` wenn Upgrade akzeptiert; danach beginnt HTTP/2 Frame-Austausch.

- Wenn Server oder Proxy nach dem `101` noch Reste der ursprünglichen HTTP/1.1-Payload fehlerhaft verarbeitet, kann das zu Smuggling führen.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Tunneling Requests via h2c Smuggling
Bei h2c kann durch unsaubere Übergabe an einen Backend-Parser ein Angreifer zusätzliche HTTP/1.1 Request-Zeilen in die Verbindung schreiben, welche dann vom Backend ausgeführt werden - dadurch „tunnelt“ er Requests durch die Upgrade-Sequenz.

**Beispielablauf (vereinfacht):**

- Angreifer initiiert h2c Upgrade und fügt in den letzten Bytes vor Protokollwechsel eine weitere HTTP/1.1 Request-Zeile ein.

- Bei falscher Implementierung wird diese zweite Request-Zeile vom Backend verarbeitet.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Schutzmaßnahmen


| Schutzmaßnahme | Beispiel |
|----------------|----------|
| **HTTP/2-aware Komponenten einsetzen (Grundprinzip)** | <ul><li>Verwende Proxies und Load-Balancer, die vollständig HTTP/2-aware sind und die Übersetzung H2 → H1 korrekt und sicher implementieren (z. B. geprüfte Versionen von nginx, Envoy, HAProxy mit aktuellen Patches).</li><li>Stelle sicher, dass Frontend und Backend dieselbe Interpretation von `CL/TE` und Header-Normalisierung teilen.</li></ul> |
| **Strikte Header-Normalisierung & Canonicalization** | <ul><li>Normalisiere Header (kleinbuchstabig) und entferne/ablehne doppelte oder widersprüchliche `Content-Length`-Header (RFC7230: multiple `Content-Length` nur akzeptieren wenn alle Werte identisch).</li><li>Entferne oder normalisiere TE Header - akzeptiere nur `te: trailers` in HTTP/2 und mappe niemals te auf `Transfer-Encoding` ohne Validation.</li></ul> |
| **Keine CRLF in Header-Werten erlauben** | <ul><li>Auf allen Ebenen: serverseitige/Proxy-seitige Validierung, die CR (`\r`) oder LF (`\n`) in Header-Namen/-Werten ablehnt (RFC konform: Header-values dürfen keine raw CRLF enthalten).</li><li>Reject oder sanitize Header-Werte mit `%0d/%0a` Sequenzen.</li></ul> |
| **Konsistente Längenberechnung beim Übersetzen** | <ul><li>Aggregiere DATA-Frames korrekt und berechne `Content-Length` anhand der tatsächlichen Byteanzahl.</li><li>Vermeide es, `Transfer-Encoding` zu setzen, wenn nicht wirklich `chunked` encoding benutzt wird.</li></ul> | 
| **Logging, Monitoring, Canary-Requests** | <ul><li>Setze Canary-Requests/unique tokens, die dir zeigen, ob Backend unerwartete Requests erhält.</li><li>Logge Roh-Socket-Ereignisse und beobachte unerwartete Post-Body Bytes / mehrere Requests in Folge auf einem Socket.</li></ul> |
| **Defense in Depth (WAF / ACLs / Auth)** | <ul><li>WAF-Regeln gegen typische Smuggling-Pattern (z. B. unterschiedliche CL-Angaben, `te: chunked` in HTTP/2 Kontext, CRLF sequences).</li><li>Interne Ressourcen sollten zusätzliche Authentifizierungs-/Autorisierungsstufen besitzen (nicht nur auf Frontend/Path-Filter vertrauen).</li></ul> |
| **H2C / Upgrade einschränken** | <ul><li>Deaktiviere h2c (cleartext HTTP/2) in produktiven Umgebungen, sofern nicht zwingend benötigt. Erlaube HTTP/2 nur über TLS+ALPN.</li><li>Wenn h2c unvermeidbar ist, betreibe strikte Prüfungen der Upgrade-Sequenz.</li></ul> |
| **Regelmäßige Updates und CVE-Monitoring** | <ul><li>Betreibe Upgrades der eingesetzten HTTP/2 Implementationen; implementiere Patches, die bekannte Bugs im Frame-Parser oder HPACK beheben.</li></ul> |
| **Backend hart konfigurieren** | <ul><li>Backend-Server sollten robust gegen unaufgeräumte Sockets sein: bspw. Zeitüberschreitungen für Idle-Bytes, Limits für Anzahl/Größe pipelined requests, strikte Header-Validation.</li></ul> |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Prüfmethoden für Penetrationstester (konkret, reproduzierbar)

**1. `Content-Length` Manipulation (H2.CL Test)**

- Sende per HTTP/2 einen Request mit Body N Bytes, beobachte welcher `Content-Length` der Proxy an das Backend sendet; variiere N und vergleiche die Backend-Logs.

- Instrumentiere eine zweite „canary“ Request-Line im Body (z. B. `GET /canary/<token> HTTP/1.1`) und prüfe das Backend Logging.

**2. TE / Transfer-Encoding Tests (H2.TE Test)**

- Sende `te: trailers`, `te: chunked` und beobachte die Header, die der Proxy an das Backend schreibt.

- Teste Folgen mit `chunked` terminator Bytes und kontrolliere, ob das Backend weitere Requests interpretiert.

**3. CRLF Injection Tests**

- Füge URL-/Header-Werte mit encodierten CRLF ein (`%0d%0a`) und beobachte, ob das Backend daraus neue Header erstellt oder Requests empfängt.

**4. h2c Upgrade Tests**

- Versuche Upgrade Szenarien (nur in Testumgebungen!) und prüfe Verhalten bei `101 Switching Protocols`; sende danach edge-bytes bevor du in HTTP/2-Frames wechselst.

**5. Cache Poisoning Checks**

- Verwende einzigartige tokens in Anfragen (z. B. Query `?t=xyz`) und beobachte, ob Antwort im Cache für andere Keys landet.

> **Hinweis für Tests:** Führe die Tests nur in autorisierten Umgebungen und mit Zustimmung durch. Dokumentiere reproduzierbare Testfälle (Requests, erwartete Backend-Logs, Ergebnisse).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Zusammenfassung (short checklist)

- Definiere die Angriffsvektoren: H2.CL, H2.TE, CRLF, h2c Upgrade, Tunneling

- Liefern von reproduzierbaren Testschritten (siehe Prüfmethoden).

- Gebe konkrete Hardening-Maßnahmen (Header-Normalisierung, TE/CL Regeln, CRLF-Filter, ALPN only, h2c deaktivieren).

- Weise auf Monitoring (Canaries, Rohsocket Logs) und regelmäßige Updates hin.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links

- [PortSwigger.net: HTTP/2: The Sequel is Always Worse](https://portswigger.net/research/http2)
- [BishopFox: h2c Smuggling: Request Smuggling Via HTTP/2 Cleartext (h2c)](https://bishopfox.com/blog/h2c-smuggling-request)
- [TryHackMe Room: HTTP/2 Request Smuggling](https://tryhackme.com/room/http2requestsmuggling)
- [Wikipedia: Head of Line Blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

📅 **Letzte Aktualisierung:** Oktober 2025   
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
