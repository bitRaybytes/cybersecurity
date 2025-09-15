# 🛡️ CSRF – Cross-Site Request Forgery



## Inhaltsverzeichnis
- [Was ist CSRF?](#was-ist-csrf)
- [Phasen einer CSRF-Attacke](#phasen-einer-csrf-attacke)
- [CSRF-Angriffsarten](#csrf-angriffsarten)
- [Unterschied zu XSS](#unterschied-zu-xss)
- [Typisches Beispiel (HTML)](#typisches-beispiel-html)
- [Bedingungen für CSRF](#bedingungen-für-csrf)
- [Besonders gefährdet sind](#besonders-gefährdet-sind)
- [CSRF in der Praxis erkennen](#csrf-in-der-praxis-erkennen)
- [Tools für CSRF-Tests](#tools-für-csrf-tests)
- [Serverseitige Schutzmaßnahmen](#serverseitige-schutzmaßnahmen)
- [Best Practices für Entwickler](#best-practices-für-entwickler)
- [Übungsplattformen](#übungsplattformen)
- [Weiterführende Links](#weiterführende-links)
- [Haftungsausschluss](#haftungsausschluss)



## Was ist CSRF?

**CSRF (Cross-Site Request Forgery)**, auch bekannt als „Session Riding“, ist eine Web-Sicherheitslücke, bei der ein Angreifer den Browser eines bereits authentifizierten Nutzers dazu bringt, unbeabsichtigt eine schädliche Anfrage an eine Webanwendung zu senden. Die Gefahr liegt darin, dass der Angreifer keine direkten Informationen des Opfers stehlen muss, sondern lediglich dessen gültige Sitzung ausnutzt. Er erzwingt eine Aktion, die scheinbar vom legitimen Nutzer selbst stammt.

> **Merke:** CSRF zielt darauf ab, den **Zustand** der Webanwendung zu verändern (z. B. Geldüberweisung, Passwortänderung, Datenlöschung) und nutzt die im Browser gespeicherten Sitzungscookies aus.

### Beispiel-Szenario:

Der Angreifer weiß, dass ein bestimmtes HTML-Formular oder eine URL-Struktur auf einer Website, zum Beispiel einer Bank, eine kritische Aktion auslöst. Er erstellt dann eine manipulierte Seite, die eine solche Anfrage im Hintergrund ausführt.

Wenn das Opfer, während es bei der Bank eingeloggt ist, diese manipulierte Seite besucht, sendet sein Browser die Anfrage automatisch mit den gültigen Sitzungscookies. Die Bankseite kann die gefälschte Anfrage nicht von einer echten unterscheiden und führt die Aktion aus.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Phasen einer CSRF-Attacke

CSRF-Attacken haben üblicherweise drei essentielle Phasen:

- Der Angreifer kennt das Format der Anfragen der Webanwendung zur Ausführung einer bestimmten Aufgabe und sendet dem Opfer einen schädlichen Link.
- Cookies überprüfen die Identität des Nutzers auf der Webseite und werden bei jeder Domänenanfrage auf den Link vom Angreifer automatisch übertragen.
- Unzureichende Sicherheitsmaßnahmen verhindern, dass Webseite zwischen authentischen und gefälschten Anfragen unterscheiden kann.

**Vereinfachte Darstellung des Ablaufs:**
```text
 +--------------+
 | Angreifer-   |
 | Website      |
 +--------------+
        ^
        | sendet schädliche Anfrage
        | (mit GET, POST, IMG-Tag etc.)
        v
 +--------------+     +-----------------+
 | Angreifer    | --> | Browser des     |
 | sendet Link  |     | Opfers (Login)  |
 +--------------+     +-----------------+
        |                    |
        |                    | inkl. Session-Cookie
        v                    v
 +--------------+    +------------------+
 | E-Mail /     | -->| Legitime Website |
 | Social Media |    | (z.B. Bank)      |
 +--------------+    +------------------+
                             |
                             v
                       +-------------+
                       | Aktion wird |
                       | ausgeführt  |
                       +-------------+

```
<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## CSRF-Angriffsarten

### Traditionelles CSRF

Diese Art von CSRF-Attacke fokussiert sich auf die Status-Veränderung Maßnahmen, wenn Anfragen übermittelt werden. Dem Opfer wird eine konkrete Handlung als "normal" suggeriert, sodass er diese Ausführen soll. Beim Ausführen durch den User, zum Beispiel mit einem Klick auf einen schadhaften Link des Angreifers, werden die schadhaften Payloads ebenfalls ausgeführt.

**Ablauf eines traditionellen CSRF-Angriffs:**

1. Das Opfer ist bspw. in seinem Online-Banking-Account angemeldet. Währenddessen erstellt der Angreifer einen schadhaften Link, den er dem Opfer zusendet.
2. Die E-Mail wird voraussichtlich vom Opfer im selben Browser geöffnet.
3. Sobald der Link angeklickt wurde, überträgt der schadhafte Code die Daten zum Angreifer oder überweist Beträge an Konten des Angreifers.

### XMLHttpRequest CSRF

Asynchrone CSRF-Attacken passieren, wenn keine kompletten `Page Request-Response-Cycle` eingegangen sind. Diese Art von Angriffsmöglichkeit findet sich oft in modenernen Webanwendungen wider, die asynchrone Serverkommunikation (über `XMLHttpRequest` oder `Fetch API`) und JavaScript verwenden, um dymische Benutzeroberflächen zur Verfügung zu stellen.

- Ausnutzung asynchroner Aufrufe anstelle herkömmlicher Formularaufrufe.

**Beispiel:**



## Unterschied zu XSS

| Merkmal     | CSRF                                | XSS                              |
|-------------|-------------------------------------|----------------------------------|
| **Ziel**        | Authentifizierte Nutzeraktionen     | Skriptausführung im Browser      |
| **Voraussetzung** | Aktive Session & Authentifizierung | Kein Login nötig                 |
| **Gefahr**      | Zustand ändert sich (z. B. Überweisung) | Meist Datendiebstahl/Defacing |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Typisches Beispiel (HTML)

```html
<form action="http://bank.com/transfer" method="POST">
  <input type="hidden" name="amount" value="1000">
  <input type="hidden" name="toAccount" value="attacker123">
  <input type="submit" value="Click here!">
</form>

<!-- Oder automatisiert -->
<img src="http://bank.com/transfer?to=attacker123&amount=1000">
```
> Wird der Benutzer zur Seite gelockt, wird ohne sein Zutun die Anfrage ausgeführt – sofern er eingeloggt ist.



## Bedingungen für CSRF

- Der Nutzer ist bereits authentifiziert (eingeloggt)
- Session Cookie wird automatisch gesendet
- Die Zielseite hat keine Schutzmechanismen

### Schritte zum CSRF
1. **Authentifizierung:** Das Opfer meldet sich auf Zielwebseite an und erhält einen Sitzungscode, welcher seine Sitzung authentifiziert.
2. **Böswillige Anfrage:** Angreifer erstellt eine Anfrage an die Zielwebseite, die eine vom Angreifer gewünschte Aktion durchführt (Banküberweisung, Passwort ändern usw.)
3. **Ausführung:** Wenn Opfer auf böswillige Webseite kommt oder manipulierten Link aus eMail anklickt, während er noch auf der Zielwebseite angemeldet ist, fügt der Browser automatisch den Sitzungscookie in die Anfrage ein und täuscht eine legitime Anfrage des Opfers vor.
4. **Angriff:** Die Zielwebseite verarbeitet die Anfrage und führt sie aus, das sie offenbar von einem legitimen Nutzer stammt.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Besonders gefährdet sind:

- Admin-Panels
- Formular-basierte Aktionen (Passwort ändern, Konto löschen, E-Mail ändern)
- REST-APIs ohne Token



## CSRF in der Praxis erkennen

| Methode                       | Beispiel                               |
| ----------------------------- | -------------------------------------- |
| Manuelle Überprüfung von Formularen | Überprüfe im Quellcode von Webseiten, ob Formulare versteckte Felder mit Namen wie csrf_token oder Ähnlichem enthalten. Fehlen diese, kann dies ein Indikator für eine Anfälligkeit sein. |
| Cookies analysieren           | Prüfe in den Entwickler-Tools, ob Cookies das Attribut SameSite=None besitzen. SameSite=Strict oder Lax sind Indikatoren für einen vorhandenen Schutz.|
| Burp Suite Repeater | Teste eine kritische Anfrage im Burp Suite Repeater. Entferne den CSRF-Token (falls vorhanden) und sende die Anfrage erneut. Wenn die Aktion erfolgreich ausgeführt wird, ist die Seite anfällig. |



## Tools für CSRF-Tests

| Tool               | Beschreibung                               |
| ------------------ | ------------------------------------------ |
| Burp Suite         | Interception & Repeater für manuelle Tests |
| OWASP ZAP          | Fuzzer für Anfragen                        |
| CSRF PoC Generator | HTML-Payloads erzeugen                     |
| Postman            | Testen von API-Endpunkten                  |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Serverseitige Schutzmaßnahmen

### 1. CSRF-Token verwenden
Der CSRF-Token ist die Standardmethode zur Abwehr von Cross-Site Request Forgery. Es handelt sich um einen einzigartigen, zufällig generierten Wert, der bei jeder Anfrage an den Server mitgesendet werden muss.

- **Generierung:** Der Server generiert einen zufälligen Token, speichert ihn in der Benutzersitzung und bindet ihn in ein verstecktes Feld im HTML-Formular ein.
- **Überprüfung:** Bei Eingang der Anfrage vergleicht der Server den im Formular übermittelten Token mit dem in der Sitzung gespeicherten. Stimmen sie nicht überein, wird die Anfrage abgelehnt.

```html
<form action="/transfer" method="POST">
  <input type="hidden" name="csrf_token" value="randomvalue123_abc">
  ...
</form>
```

### 2. SameSite Cookie Attribut
Mit dem `SameSite`-Attribut können Browser gesteuert werden, ob ein Cookie bei Cross-Site-Anfragen mitgesendet wird.

- `SameSite=Strict`: Der Browser sendet Cookies nur, wenn die Anfrage von derselben Top-Level-Domain kommt.
- `SameSite=Lax`: Der Browser sendet Cookies nur bei GET-Anfragen von derselben Top-Level-Domain. **Bietet bereits einen guten Grundschutz**.
- `SameSite=None`: Der Browser sendet Cookies bei allen Anfragen (erfordert **HTTPS**).

```http
Set-Cookie: sessionid=xyz; SameSite=Strict
```

Verhindert automatisches Mitsenden bei Cross-Origin-Requests

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>
### 3. Origin/Referer Header prüfen
Viele Webanwendungen prüfen, ob die Anfrage von einer vertrauenswürdigen Domäne stammt. Dies geschieht durch die Überprüfung der HTTP-Header `Origin` und `Referer`. Dies ist jedoch keine absolute Sicherheit, da diese Header manipulierbar sein können.


```python
if request.headers.get('Referer') != "[https://legitime-website.com](https://legitime-website.com)":
    # Anfrage ablehnen
    return 403

```

### 4. Nur POST für kritische Aktionen

Kein `GET` für sensible Änderungen

### 5. Content Security Policy (CSP)

Mit **CSP** definieren, welche Inhalte vertrauenswürdig sind.

### 6. CAPTCHAS implementieren

CAPTCHAS bauen eine zusätzliche Sicherheitsschicht auf, da User diese zunächst lösen müssen (Angreifer kann sie remote nur schwer lösen).

### 7. Double Submit Cookie
Bei dieser Methode wird ein CSRF-Token in ein Cookie und in ein verstecktes Formularfeld geschrieben. Der Angreifer kann das Cookie zwar nicht lesen, da es durch die **Same-Origin-Policy** geschützt ist, aber er könnte es für seine gefälschte Anfrage nutzen. Da er das zweite Feld nicht kennt, kann er keine Übereinstimmung herstellen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Best Practices für Entwickler

| Empfehlung                            | Warum?                            |
| ------------------------------------- | --------------------------------- |
| **Setze CSRF-Tokens** | Implementiere eine serverseitige Logik zur Generierung und Validierung von CSRF-Tokens für alle zustandsändernden Anfragen (z.B. POST, PUT, DELETE).     |
| Setze `SameSite=Lax/Strict` Cookies   | Setze das Attribut `SameSite=Lax` oder `Strict` für deine Sitzungscookies. |
| **Verwende keine GET-Requests** | Kritische Aktionen, die den Zustand ändern, sollten niemals über einen GET-Request aufgerufen werden. |
| Nutze CAPTCHA bei sensiblen Aktionen  | Stoppt Automatisierung            |






## Übungsplattformen

| Plattform        | Inhalt                                           |
| ---------------- | ------------------------------------------------ |
| PortSwigger Labs | CSRF-Labs mit verschiedenen Schwierigkeitsgraden |
| TryHackMe        | Module wie „OWASP Top 10“ -> CSRF                |
| DVWA             | CSRF-Stufen: Low – High                          |
| bWAPP            | Simulation echter Angriffe                       |



## Weiterführende Links

- [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP CSRF Attack](https://owasp.org/www-community/attacks/csrf)
- [PortSwigger CSRF Labs](https://portswigger.net/web-security/csrf)
- [Mozilla MDN – SameSite Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value)


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

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---