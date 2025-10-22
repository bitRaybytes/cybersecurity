# HTTP Request Smuggling (HRS)
Dieses Dokument beschreibt die Schwachstelle HTTP Request Smuggling (HRS) – oft als HTTP Desynchronisation bezeichnet. Es konzentriert sich auf die technischen Mechanismen, die es Angreifern ermöglichen, die Grenzen von HTTP-Anfragen zu manipulieren.


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
    - [Ziel dieser Datei](#ziel-dieser-datei)
    - [Wichtige Hinweise](#wichtige-hinweise)
- [Moderne Infrastruktur](#moderne-infrastruktur)
    - [Load Balancer und Reverse Proxy](#load-balancer-und-reverse-proxy)
    - [Die Rolle des Cachings](#die-rolle-des-cachings)
- [Der HTTP-Request Header](#der-http-request-header)
    - [Header und ihre Rolle](#header-und-ihre-rolle)
    - [Content-Length Header](#content-length-header)
    - [Transfer-Encoding Header](#transfer-encoding-header)
- [Wo treten HTTP Request Smugglings auf?](#wo-treten-http-request-smugglings-auf)
- [Das Kernproblem: HTTP-Desynchronisation (HTTP Desync)](#das-kernproblem-http-desynchronisation-http-desync)
    - [Die Regel: RFC-Konflikt](#die-regel-rfc-konflikt)
- [Angriffsklassifikationen nach Parsing-Priorität](#angriffsklassifikationen-nach-parsing-priorität)
    - [CL.TE: Content-Length gewinnt vorne, Transfer-Encoding gewinnt hinten](#clte-content-length-gewinnt-vorne-transfer-encoding-gewinnt-hinten)
    - [TE.CL: Transfer-Encoding gewinnt vorne, Content-Length gewinnt hinten](#tecl-transfer-encoding-gewinnt-vorne-content-length-gewinnt-hinten)
- [Transfer-Encoding Obfuscation](#transfer-encoding-header)
- [Auswirkungen und Angriffsziele](#auswirkungen-und-angriffsziele)
- [Schutzmaßnahmen und Mitigation](#schutzmaßnahmen-und-mitigation)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

**HTTP Request Smuggling** ist eine Schwachstelle im HTTP-Header, bei dem der Angreifer schadhafte HTTP-Request sendet, damit diese vom Backend ausgeführt werden.


In Webanwendungen werden HTTP Request Smuggling im `Content-Length`- und `Transfer-Encoding`-Header ausgenutzt, die den Ende eines Request-Body definieren, um durch schadhafte Anfragen Zugriff auf die Produktionsumgebung zu erhalten.

`Request Splitting` oder `HTTP desync` sind Angriffe, die möglich werden durch die `keep-alive connections` und das `HTTP Pipelining`. Diese Angriffe erlauben mehrere Anfragen über eine TCP-Connection zu senden. 

Bei der Kalkulation der `Content-Length` und des `Transfer-Encoding` ist es wichtig, die Anwesenheit des `\r` (**Carriage Return**) und der `\n` (**NewLine**) zu berücksichtigen. Diese Zeichen haben nicht nur einen formatierenden Aspekt des HTTP-Protokolls, sondern spielen auch für den Wert es kalkulierenden Ergebnisses eine große Rolle.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Ziel dieser Datei

Diese Datei soll dir das Thema HTTP Request Smuggling näher bringen. Du wirst lernen, 

1. was HTTP Request Smuggling ist und welche Auswirkungen es hat.
2. wie du HTTP Request Smuggling Angriffe erkennst.
3. welche Maßnahmen du verwenden kannst und vorbeugend Schutz treffen kannst.


Davor solltest du die Grundlagen von [HTTP/HTTPS](/03-web-security/grundlagen/03_http_https_grundlagen.md) kennen und wissen wie [HTTP-Header Protokolle](/03-web-security/grundlagen/03b_http_header_grundlagen.md) funktionieren.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Hinweise
- Geschmuggelte Anfragen (Smuggeld Requests) können Sicherheitsmaßnahmen von WAFs umgehen, was ein hohes Risiko an unberechtigten Zugriffen darstellt.
- Angreifer können den `Web Cache` durch das Einschleusen von geschmuggelten Anfragen manipulieren. So können sie Nutzern inkorrekte oder schadhaften Daten präsentieren.
- Geschmuggelte Anfragen können verkettet werden, um das Ausmaß zu erweitern und andere Schwachstellen angreifen zu können.
- Schwer zu erkennen aufgrund der komplizierten Art und Weise dieser Schwachstelle, die oft unerkannt bleibt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Moderne Infrastruktur
Das Web bzw. die Web Anwendungen basieren heutzutage nicht mehr auf monolithischen Strukturen, sondern sind eine Konfiguration aus verschiedenen Frameworks, Komponenten und mehr, die miteinander interagieren und kommunizieren.

Im Wesentlichen haben alle Web Anwendungen eine ähnliche Infrastruktur:

- **Frontend-Server:** Der Frontend-Server ist meist ein **reverse Proxy** oder ein **Load Balance**, der die Anfragen des Clients an den Backend-Server leitet.
- **Backend-Server:** Ein Backend-Server verarbeitet die Nutzeranfragen, interagiert mit der Datenbank und liefert die Ergebnisse an das Frontend.
- **Databases:** Datenbanken wie `MySQL`, `PostgreSQL` oder `NoSQL` sind dauerhafte Speicher Systeme, die die Daten der Anwendung lagern.
- **APIs (Application Programming Interfaces):** APIs sind Schnittstellen, die es Anwendungen erlauben miteinander zu kommunizieren und zu integrieren.
- **Micorservices:** Viele moderne Web Anwendungen nutzen verschiedene Microservice-Dienste, die über das Netzwerk vie `HTTP/REST` oder `gRPC` kommunizieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Load Balancer und Reverse Proxy

1. **Load Balancer:** 
    - Load Balancer sind Dienste, die eingehenden Datenverkehr verwalten und damit zuständig sind für die reibungslose Übertragung. 
    - Sie sichern dabei die Server-Struktur so ab, dass kein Server mit Anfragen überladen wird und es nicht zu einem (kurzzeitigen) Serviceabbruch führt. 
    - Load Balancer mit Reverse Proxy: `AWS Elastic Load Balancing`, `HAProxy`, `F5 BIG-IP`.

2. **Reverse Proxy:** 
    - Ein Reverse Proxy nimmt Anfragen eines Nutzer entgegen und leitet sie an das Backend weiter. Es sitzt dabei noch vor dem eigentlichen Server(n).
    - Können als Load Balancer genutzt werden, doch ihre eigentliche Funktion liegt darin, als Single-Access-Point zu fungieren und auch Kontrolle für die Backend-Server zu bieten.
    - Revery Proxy: `NGINX`, `Apache` mit `mod_proxy` oder `Varnish`.


```text
                                                          +----------+
                                                    +---->| Server 1 |-----------+
                                                    |     +----------+           |
+------+      +---------+     +----------------+    |     +----------+       +---v----------------+
| User |----> | Request |---> | Load Balancer/ |----+---->| Server 2 |------>| Backend-Ressourcen |
+------+      +---------+     | Reverse Proxy  |    |     +----------+       +---^----------------+
                              +----------------+    |     +----------+           |
                                                    +---->| Server 3 |-----------+
                                                          +----------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Die Rolle des Cachings

Caching ist eine Technik zum Speichern und Wiederverwenden zuvor abgerufener Daten oder berechneter Ergebnisse, um nachfolgende Anfragen und Berechnungen zu beschleunigen. Im Kontext der Web-Infrastruktur:

1. **Content Caching:** Durch das strategische speichern des Webinhalts, der sich nicht laufend verändern (Bilder, CSS, JS-Dateien), kann durch Content Caching die Ladezeit auf dem Server reduziert und die Auslieferung an den Nutzer schneller durchgeführt werden.

2. **Database Query Caching:** Datenbanken können häufig verwendete Datenbankanfragen im Speicher lagern und so die Geschwindigkeit der Datenbereitstellung erhöhen.

3. **Full-page Caching:** Die gesamte Webseite wird gecached, damit der Inhalt nicht neu vom Server geliefert werden muss. 

4. **Edge Caching/CDNs:** Content Delivery Networks (CDNs) lagern Inhalte näher am eigentlichen Nutzer, also am "Ende" (**Edge**) des Netzwerks. Das reduziert die Latenz und erhöht die Geschwindigkeit des Nutzerzugriffs weltweit.

5. **API Caching:** Wenn Antworten gecached werden, kann das einen signifikanten Beitrag zu Erhöhung der Geschwindigkeit führen, indem es den zu verarbeitenden Backend Prozess entlastet.

```text
                 +---------------+
           +---->| Browser Cache |
           |     +---------------+
+------+   |     +-----------+
| User |---+---->| CDN Cache |
+------+   |     +-----------+
           |     +--------+        +--------------+
           +---->| Server |---+--->| Server Cache |
                 +--------+   |    +--------------+
                              |    +----------+      +----------------+
                              +--->| Database |----->| Database Cache |
                                   +----------+      +----------------+
```

> Wenn das Caching korrekt implementiert wurde, kann es einen signifikaten Boost in der Performance und die Responsivität der Web Anwendung verbessern. Wichtig ist das richtige Management solcher Caching Techniken.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der HTTP-Request Header

Jede HTTP Anfrage umfasst zwei Hauptteile: den **Header** und den **Body**.

**Beispielhafter Aufbau eines Requests:**
```text

1.    POST /admin/login/ HTTP/1.1
------------------------------------------------------
2.    Host: example.com
      Content-Length: 35
      Content-Type: application/x-www-form-urlencoded
      User-Agent: Mozilla/5.0
      Transfer-Encoding: chunked
------------------------------------------------------
3.    username=attacker&password=password
```

1. **Request Line:** `POST /admin/login HTTP/1.1` bildet die erste Zeile der Request Anfrage. Sie enthält mindestens drei "Dinge":
    - **die Methode:** Die Methode beschreibt, wie der Server mit der Anfrage umzugehen hat. Das Beispiel oben zeigt die `POST`-Methode, die die jeweilige Anweisung an den Server übermittelt.
    - **den Pfad:** Der Pfad identifiziert die Ressourcen auf dem Server. Das Beispiel oben zeigt den Pfad `/admin/login/`.
    - **die HTTP-Version:** Die HTTP-Version zeigt die Spezifikation des HTTP-Servers, mit dem der Nutzer kommunizieren will.

2. **Request Headers:** Der Request Header beinhaltet Metadaten des Requests, die an den Server übermittelt werden wie der `Content-Typ`, das gewünschte Antwortverhalten des Servers und etwaige Authentifizierungs-Tokens. Es ist quasi der Briefumschlag, der die Adresse des Empfängers und des Senders trägt.

3. **Message Body:** Daten, die mit dem Request gesendet werden. Diese können leer bei einem `GET` und bei einem `POST` beschrieben sein mit Daten, die bspw. aus Formularfeldern, JSON-Payloads oder Datei-Uploads stammen. 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Header und ihre Rolle
Header spielen eine wichtige Rolle, wenn Daten über Web Anwendungen zum Verarbeiten an den Server übermittelt werden. Sie geben an, wie die Daten vom Server verarbeitet werden sollen. Das passiert durch das Parsen des Requests und Einflussnahme auf das Caching Verhalten.

Das korumpieren von HTTP-Header wie den Content-Length und den Transfer-Encoding können Schwachstellen der Web Anwendung hervorrufen. Wenn beispielsweise ein Proxy Server durch die Manipulation die Daten nicht mehr richtig verarbeiten kann, wird es im schwer fallen herauszufinden, wo die Daten enden und andere Daten anfangen.

```text             
                                         +--------------------+
                                    +--->| Request Processing |
                                    |    +--------------------+
                                    |    +--------------+
                                    |--->| Body Parsing |
+--------------+      +--------+    |    +--------------+
| HTTP Request |----->| Header |----+    +-----------------------+
+--------------+      +--------+    |----| Caching & Compression |
                                    |    +-----------------------+
                                    |    +------------------------------+
                                    +--->| Authentication & Redirection |
                                         +------------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Content-Length Header

Der **Content-Length**-Header gibt Aufschluss über den Request- oder Response-Body in Bytes. Dies nutzt der Server um herauszufinden, wie viel Daten er zu empfangen hat, sodass serverseitig auf Vollständigkeit der Nachricht überpüft werden kann (ist der Content vollständig übermittelt worden?).

```text
POST /admin/login/ HTTP/1.1
Host: example.com
Content-Length: 35
Content-Type: application/x-www-form-urlencoded

username=attacker&password=password
```

Dieser Request zeigt, dass die Content-Length **35 Bytes** beträgt. Dies erfährst du, wenn du jeden Buchstaben der Message `username=attacker&password=password` zählst.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Transfer-Encoding Header
Der **Transfer-Encoding**-Header wird für dafür genutzt, um zu definieren, welche Verschlüsselung (Encoding) für den Message-Body genutzt wird. `chunked` ist der gewöhnlichste und am häufigsten in Anspruch genommen Wert. Er sagt aus, dass die Nachricht in Segmente im Hexadecimal-Format geteilt wird.

Es gibt weitere Werte für den Transfer-Encoding-Header wie `compress`, `deflate` und `gzip`, von denen jeder seinen eigenen Typ von Verschlüsselung bietet.

**Zum Beispiel:**
```text
POST /submit HTTP/1.1
Host: good.com
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked   <-- 14 Bytes Body-Daten (inkl. Leerzeichen)
    
b                            <-- b = 11 in Hexadezimal
q=smuggledData 
0
```

Im obigen Beispiel spefiziert das `b` die Größe des folgenden Chunks (b in Hexadezimal ist `11`). Dabei stellt `q=smuggeldData` die eigentliche Daten dar. Die nächste Zeile `0` signalisiert, dass die Nachricht im Message-Body zu Ende ist.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wo treten HTTP Request Smugglings auf?
HTTP Request Smugglings treten aufgrund von Diskrepanzen in der Interpretation der verarbeiteten Daten auf (Frontend- und Backend-Server).

**Zum Beispiel:**

1. Wenn die Header `Transfer-Encoding` und `Content-Length` zur gleichen Zeit vorhanden sind, dann können Unklarheiten in der Verarbeitung auftreten.

2. Manche Komponenten priorisieren `Content-Length` und andere `Transfer-Encoding`.

3. Diese Unterscheidung kann dazu führen, dass die eine Komponenten die Daten noch verarbeitet, während eine andere denkt, dass der Prozess bereits beendet ist. Das führt zum "Schmuggeln".



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das Kernproblem: HTTP-Desynchronisation (HTTP Desync)

Der HTTP Request Smuggling-Angriff basiert auf einer **Desynchronisation** (**Desync**) zwischen dem Frontend-Server (Proxy/Load Balancer) und dem Backend-Server.

Beide Server müssen sich auf eine eindeutige Methode einigen, um das Ende eines HTTP-Requests zu bestimmen. Bei modernen HTTP-Verbindungen (die `keep-alive` nutzen) dient der Request-Body oft als Trennzeichen zwischen zwei separaten Anfragen.

Wenn beide Server unterschiedliche Regeln zur Bestimmung der Request-Länge anwenden, liest der nachgelagerte Server einen Teil der vom Angreifer gesendeten Daten als den Header des nächsten, unschuldigen Requests. Dieser "geschmuggelte" Teil verunreinigt die Pipeline.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Die Regel: RFC-Konflikt

Laut HTTP-Spezifikation ([RFC 7230, Sektion 3.3.3](https://datatracker.ietf.org/doc/html/rfc7230#section-3.3.3)) sollte der Header `Transfer-Encoding: chunked` immer Vorrang vor `Content-Length` haben, wenn beide Header in einem Request vorhanden sind.

Die Schwachstelle entsteht, wenn ein Server (Front- oder Backend) diese Regel missachtet oder aufgrund der Syntax (Obfuscation) einen Header ignoriert, den der andere Server akzeptiert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Angriffsklassifikationen nach Parsing-Priorität

HRS-Angriffe werden nach der unterschiedlichen Header-Priorität des Frontend- und des Backend-Servers klassifiziert. Die beiden Haupttypen sind **CL.TE** und **TE.CL**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### CL.TE: Content-Length gewinnt vorne, Transfer-Encoding gewinnt hinten
Das **CL.TE Request Smuggling** bezieht sich auf die mutmaßliche Diskrepanz zwischen Frontend und Backend. Wenn die Header Content-Length und Transfer-Encoding gleichzeitig in einem Request genutzt werden, dann interpretiert das Frontend den Content-Length Header und sendet genau die X-Bytes an das Backend, die es soll. Das Backend erhält dann den gesamten Message Body, wobei an dieser Stelle der Rest der Nachricht im Backend-Buffer liegt und als Anfang der nächsten Nachricht interpretiert.

> Es ist wichtig, die korrekte Größe der Nachricht des Messages Bodies im Content-Length Header anzugeben, da das Frontend sonst inkorrekte Daten übermittelt.

- **Frontend:** Priorisiert den Header `Content-Length` (CL).

- **Backend:** Priorisiert den Header `Transfer-Encoding: chunked` (TE).


**Visualisierung des Angriffs in CL.TE Reihenfolge:**
```text
                               (Content-Length)                     (Transfer-Encoding)
+-----------+                    +----------+                          +---------+
| Angreifer |                    | Frontend |                          | Backend |
+-----------+                    +----+-----+                          +----+----+
      | Request mit beiden Headern    | (CL.TE Reihenfolge)                 |
      |------------------------------>|------------------------------------>|
      |                               |                                     |
      |<------------------------------|<------------------------------------|
      |  Sendet regulären Response    | Prozessiert geschmuggelten Request  |        
+-----------+                    +----+-----+                          +----+----+
| Angreifer |                    | Frontend |                          | Backend |
+-----------+                    +----------+                          +---------+
```


**Ablauf des Angriffs:**
1. Der Angreifer sendet einen Request, der beide Header enthält.

2. **Frontend (CL):** Liest den Wert von `Content-Length` (z.B. X Bytes) und leitet genau diesen gesamten Body an das Backend weiter.

3. **Backend (TE):** Ignoriert `Content-Length`. Es sucht nach dem `Transfer-Encoding: chunked`-Muster. Es sieht den ersten Chunk, verarbeitet diesen und beendet den Request, wenn es den `0` Chunk sieht, der das Ende signalisiert.

4. **Desync:** Da der Body, den das Frontend gesendet hat, länger ist als das, was das Backend als Ende interpretiert hat, bleibt der Rest des Payloads im Backend-Buffer zurück. Dieser Rest wird als der Anfang der nächsten Anfrage behandelt.



**CL.TE Payload-Beispiel**
```text
POST / HTTP/1.1
Host: example.com
Content-Length: 13          <-- Frontend liest 13 Bytes Body
Transfer-Encoding: chunked

8                           <-- Backend interpretiert dies als Chunk-Größe (8 Hex = 8 Bytes)
SMUGGLED
0                           <-- Backend interpretiert dies als Ende des Requests
<CRLF>                      <-- Verbleibt im Buffer, beginnt den nächsten Request
GET /admin HTTP/1.1
Host: example.com
Foo: x
```

**Ergebnis:**

- **Request 1 (Ursprünglich):** `POST /` (wird normal verarbeitet).

- **Request 2 (Geschmuggelt):** `GET /admin HTTP/1.1...` bleibt im Buffer und wird mit der nächsten legitimen Anfrage eines unschuldigen Benutzers kombiniert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### TE.CL: Transfer-Encoding gewinnt vorne, Content-Length gewinnt hinten
Der **TE.CL Request Smuggling** basiert darauf, dass der Angreifer beide Header in einem Request sendet. Ähnlich dem CL.TE Angriff erhält das Frontend den ersten Transfer-Encoding Header und das Backend erwartet den Content-Length.


- **Frontend:** Priorisiert den Header `Transfer-Encoding: chunked` (TE).

- **Backend:** Priorisiert den Header `Content-Length` (CL).



**Visualisierung des Angriffs in TE.CL Reihenfolge:**
```text
                              (Transfer-Encoding)                    (Content-Length)
+-----------+                    +----------+                          +---------+
| Angreifer |                    | Frontend |                          | Backend |
+-----------+                    +----+-----+                          +----+----+
      | Request mit beiden Headern    | (TE.CE Reihenfolge)                 |
      |------------------------------>|------------------------------------>|
      |                               |                                     |
      |<------------------------------|<------------------------------------|
      |  Sendet regulären Response    | Prozessiert geschmuggelten Request  |        
+-----------+                    +----+-----+                          +----+----+
| Angreifer |                    | Frontend |                          | Backend |
+-----------+                    +----------+                          +---------+
```


**Ablauf des Angriffs:**

1. Der Angreifer sendet einen Request, der beide Header enthält. Der `Transfer-Encoding`-Header ist dabei so manipuliert, dass das Frontend ihn akzeptiert.

2. **Frontend (TE):** Liest `Transfer-Encoding` und interpretiert den Body als Chunks. Es liest alle Chunks bis zum `0`-Chunk und beendet den Request (z.B. X Bytes).

3. **Backend (CL):** Ignoriert `Transfer-Encoding`. Es liest den Wert von `Content-Length` (z.B. Y Bytes).

4. **Desync:** Der Angreifer setzt `Content-Length` auf einen kleineren Wert (Y) als der gesamte Payload, den das Frontend sendet (X). Das Backend hört nach Y Bytes auf zu lesen. Der Rest des Payloads (der geschmuggelte Request) wird für die nächste Anfrage als Body im Buffer zurückgelassen.


**TE.CL Payload-Beispiel:**
```text
POST / HTTP/1.1
Host: example.com
Content-Length: 15          <-- Backend liest 15 Bytes Body
Transfer-Encoding: chunked  <-- Frontend interpretiert Chunking

78                           <-- Chunk-Größe (78 Hex für 120 Dezimal)
GET /admin HTTP/1.1
0                           <-- Ende des Requests für Frontend
<CRLF>                      <-- Der Rest des Payloads, der durch Content-Length nicht beachtet wurde
```

**Ergebnis:**

- **Frontend:** Liest bis zum `0`-Chunk, sendet alles an das Backend.

- **Backend:** Liest nur 15 Bytes (definiert durch `Content-Length: 15`). Der Rest des Körpers (`<CRLF>GET /admin...`) bleibt im Puffer und wird mit der nächsten Anfrage eines Opfers zusammengeführt.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Transfer-Encoding Obfuscation
Die **TE.TE Request Smuggling** Methode geschieht, wenn beide Parteien (Front- und Backend) den Transfer-Encoding Header nutzen. Anders als bei dem CL.TE oder dem TE.CL Angriff werden an das Front- und an das Backend der Transfer-Encoding Header gesendet.

Die Ambivalenz bei der Request-Längenbestimmung ist die Wurzel der Schwachstelle. Angreifer nutzen Header-Obfuscation (Verschleierung), um sicherzustellen, dass nur einer der beiden Server den Header akzeptiert.

Da Firewalls (WAF) oft nach den Headern `Content-Length` und `Transfer-Encoding` in einem einzigen Request suchen, können folgende Techniken zur Obfuscation verwendet werden:


| Obfuscation-Technik | Header-Beispiel | Ziel |
|---------------------|-----------------|------|
| **Doppel-Header** | `Transfer-Encoding: chunked` `Transfer-Encoding: identity` | Ein Server akzeptiert nur den ersten, der andere nur den letzten. |
| **Falsche Groß-/Kleinschreibung** | `tRaNsfer-EnCoDiNg: chunked` | Manche ältere Backend-Server ignorieren Header, die nicht exakt `Transfer-Encoding` geschrieben sind. |
| **Whitespace-Füllung** | `Content-Length : 10` | Hinzufügen von Leerzeichen oder Tabs (`\t`) zwischen Header-Name und Doppelpunkt. |
| **Ungültige Zeichen** | `Content-Length: 10\r\n\t` | Einbindung von horizontalen Tabs (`\t`) oder anderen Zeichen, die von einem Server ignoriert, vom anderen jedoch akzeptiert werden. |


**TE.CL Payload-Beispiel:**
```text
POST / HTTP/1.1
Host: example.com
Content-length: 4
Transfer-Encoding: chunked   <-- Wird vom Frontend gelesen
Transfer-Encoding: chunked1  

4e
POST /update HTTP/1.1
Host: example.com
Content-length: 15

isadmin=true
0        
```


**Ablauf des Angriffs:**

1. Der Angreifer sendet einen Request, der den `Transfer-Encoding`-Header enthält. Der `Transfer-Encoding`-Header ist dabei so manipuliert, dass das Frontend ihn akzeptiert.

2. **Frontend (TE):** Liest `Transfer-Encoding` und interpretiert den Body als Chunks. Er ignoriert dabei den `Transfer-Encoding: chunked1`, liest alle Chunks bis zum `0`-Chunk und beendet den Request (z.B. X Bytes).

3. **Backend (CL):** Interpretiert den `Transfer-Encoding: chunked1`, indem es entweder diese Form ignoriert und den Part wie das Frontend verarbeitet oder er verarbeitet den Prozess aufgrund des vorhandenen `non-standard` komplett andersartig. Wenn es dabei nur die $4 Bytes$ der ursprünglichen Content-Length interpretiert, dann wird `POST /update HTTP/1.1/` als ein neuer Request behandelt.

Der geschmuggelte Request mit dem `isadmin=true` Parameter wird vom Backend verarbeitet als wäre es eine neue legitime und separate Anfrage. Mit diesem Angriff kann so der Zugriff auf nicht authorisierte Bereiche erlangt werden, die für Endnutzer nicht bestimmt sind.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Auswirkungen und Angriffsziele

Die erfolgreiche Desynchronisation ermöglicht es dem Angreifer, beliebige HTTP-Anfragen in den Request-Strom einzuschleusen, die dann im Namen des nächsten unschuldigen Benutzers ausgeführt werden.

**Mögliche Angriffsziele:**

- **Cache Poisoning:** Einschleusen einer Anfrage, die eine Antwort mit langen Cache-Headern erzwingt (z.B. für eine Admin-Seite). Der Cache speichert dann die Admin-Antwort und liefert sie an alle nachfolgenden Benutzer aus, bis der Cache abläuft.

- **Session Hijacking:** Auslesen von Authentifizierungs-Cookies oder Session-IDs des nächsten Opfers, indem der geschmuggelte Request an eine vom Angreifer kontrollierte Ressource weitergeleitet wird.

- **Umgehung von WAFs und Access Controls:** Der geschmuggelte Request-Body wird vom Frontend/WAF möglicherweise nicht als vollständiger Request erkannt, wodurch Sicherheitsprüfungen umgangen werden.

- **Admin-Zugriff:** Direkter Angriff auf interne Endpunkte (z.B. `/admin`, `/api/reset`), die für externe Benutzer nicht zugänglich sein sollten, da der Angriff aus Sicht des Backend-Servers von der Proxy-IP stammt (vertrauenswürdige Quelle).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


##  Schutzmaßnahmen und Mitigation

Die Behebung von HTTP Request Smuggling erfordert eine konsequente und gleichmäßige Request-Verarbeitung über alle Komponenten hinweg.

- **Uniform Header Handling:** Stelle sicher, dass alle Komponenten (Proxy, Load Balancer, Webserver) denselben HTTP-Parser verwenden oder so konfiguriert sind, dass sie dieselben Regeln zur Bestimmung der Request-Länge anwenden.

- **Transfer-Encoding verbieten:** Deaktiviere die Unterstützung für `Transfer-Encoding: chunked` im Frontend-Proxy, wenn es nicht zwingend erforderlich ist. Oder entferne den Header explizit, bevor er an das Backend weitergeleitet wird.

- **Klarheit bei Konflikten:** Konfiguriere den Frontend-Proxy so, dass er die Verbindung sofort schließt oder einen Fehler zurückgibt, wenn sowohl `Content-Length` als auch `Transfer-Encoding` in einem Request vorhanden sind.

- **Härten des Headers:** Verwerfen alle Requests, die ungewöhnliche oder obfuskierte Header-Syntax enthalten (z.B. ungewöhnliche Whitespaces, doppelte Header).

- **HTTP/2:** Der Wechsel zu `HTTP/2` (oder idealerweise `HTTP/3`) löst das Problem des Request Smuggling, da `HTTP/2` Frames für die Länge verwendet und das Parsen des Headers irrelevant wird.

- **Schulung der Teams:** Lass Development- und Operations-Teams wissen, welche Gefahren das Request Smuggling hat und zeige ihnen, wie sie diese Risiken minimieren können.

- **Netzwerkanalysen:** Beobachte den Datenverkehr im Netzwerk regelmäßig, damit du Anzeichen von Request Smugglings erkennst.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links

- [Wallarm.com: HTTP Request Smuggling](https://lab.wallarm.com/what/http-anforderungsschmuggel-alles-was-sie-wissen-muessen/?lang=de)
- [Outpost24.com Blog Beitrag](https://outpost24.com/de/blog/http-request-smuggling-exploit-walkthrough/)
- [PortSwigger.net: HTTP Request Smuggling](https://portswigger.net/web-security/request-smuggling)
- [Wikipedia: HTTP Request Smuggling](https://en.wikipedia.org/wiki/HTTP_request_smuggling)
- [TryHackMe Room: HTTP Request Smuggling](https://tryhackme.com/room/httprequestsmuggling)




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
