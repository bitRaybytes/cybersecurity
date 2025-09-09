# 🛡️ CSRF – Cross-Site Request Forgery



## Inhaltsverzeichnis
- [Was ist CSRF?](#was-ist-csrf)
- [Unterschied zu XSS](#unterschied-zu-xss)
- [Typisches Beispiel (HTML)](#typisches-beispiel-html)
- [Bedingungen für CSRF](#bedingungen-für-csrf)
- [Besonders gefährdet sind](#besonders-gefährdet-sind)
- [CSRF in der Praxis erkennen](#csrf-in-der-praxis-erkennen)
- [Tools für CSRF-Tests](#tools-für-csrf-tests)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Best Practices für Entwickler](#best-practices-für-entwickler)
- [Angriff vs. Verteidigung](#angriff-vs-verteidigung)
- [Übungsplattformen](#übungsplattformen)
- [Weiterführende Links](#weiterführende-links)
- [Haftungsausschluss](#haftungsausschluss)



## Was ist CSRF?

**CSRF (Cross-Site Request Forgery)** ist eine Web-Sicherheitslücke, bei der ein Angreifer den Browser eines authentifizierten Nutzers dazu bringt, unbeabsichtigt schädliche Anfragen an eine Webanwendung zu senden.

### 🔍 Beispiel-Szenario:

Ein eingeloggter Nutzer besucht eine manipulierte Website → Diese Seite sendet im Hintergrund eine POST-Anfrage an eine Bankseite, um Geld zu überweisen.

➡️ **Ohne Nutzerinteraktion. Ohne Wissen.**



## Unterschied zu XSS

| Merkmal     | CSRF                                | XSS                              |
|-------------|-------------------------------------|----------------------------------|
| Ziel        | Authentifizierte Nutzeraktionen     | Skriptausführung im Browser      |
| Voraussetzung | Aktive Session & Authentifizierung | Kein Login nötig                 |
| Gefahr      | Zustand ändert sich (z. B. Überweisung) | Meist Datendiebstahl/Defacing |



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
1. **Authentifizierung:** Das Opfer meldet sich auf einer legitimen Webseite an und erhält einen Sitzungscode, welcher seine Sitzung authentifiziert.
2. **Böswillige Anfrage:** Angreifer erstellt eine Anfrage an die legitime Webseite, die eine vom Angreifer gewünschte Aktion durchführt (Banküberweisung, Passwort ändern usw.)
3. **Ausführung:** Wenn Opfer auf böswillige Webseite kommt oder manipulierten Link aus eMail anklickt, während er noch auf der legitimen Webseite angemeldet ist, fügt der Browser automatisch den Sitzungscookie in die Anfrage ein und täuscht eine legitime Anfrage des Opfers vor.
4. **Angriff:** Die legitime Webseite verarbeitet die Anfrage und führt sie aus, das sie offenbar von einem legitimen Nutzer stammt.




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
| Form ohne Token prüfen        | Kein verstecktes `<input name="csrf">` |
| `SameSite=None` Cookies       | CORS-Anfälligkeit prüfen               |
| Burp Suite Repeater einsetzen | Aktion ohne gültigen CSRF-Token testen |



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

## Schutzmaßnahmen

### 1. CSRF-Token verwenden

- Zufälliger Token wird in Session gespeichert
- Muss bei jeder Anfrage mitgesendet werden
- Beispiel mit HTML:
```html
<input type="hidden" name="csrf_token" value="randomvalue123">
```

### 2. SameSite Cookie Attribut
```http
Set-Cookie: sessionid=xyz; SameSite=Strict
```

Verhindert automatisches Mitsenden bei Cross-Origin-Requests

### 3. Origin/Referer Header prüfen

```python
if request.headers['Referer'] != "https://example.com":
reject()
```

### 4. Nur POST für kritische Aktionen

Kein `GET` für sensible Änderungen



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Best Practices für Entwickler

| Empfehlung                            | Warum?                            |
| ------------------------------------- | --------------------------------- |
| Verwende CSRF-Tokens                  | Verhindert Session-Missbrauch     |
| Setze `SameSite=Lax/Strict` Cookies   | Browserseitige Prävention         |
| Authentifizierung regelmäßig erneuern | Angreifer verliert Sessionzugriff |
| Nutze CAPTCHA bei sensiblen Aktionen  | Stoppt Automatisierung            |



## Angriff vs. Verteidigung

| Rolle       | Aufgabe                                         |
| ----------- | ----------------------------------------------- |
| Angreifer   | Ausnutzen der Authentifizierung                 |
| Verteidiger | Sitzung absichern, Tokens prüfen, Header prüfen |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Übungsplattformen

| Plattform        | Inhalt                                           |
| ---------------- | ------------------------------------------------ |
| PortSwigger Labs | CSRF-Labs mit verschiedenen Schwierigkeitsgraden |
| TryHackMe        | Module wie „OWASP Top 10“ → CSRF                 |
| DVWA             | CSRF-Stufen: Low – High                          |
| bWAPP            | Simulation echter Angriffe                       |



## Weiterführende Links

- [OWASP CSRF Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [PortSwigger CSRF Labs](https://portswigger.net/web-security/csrf)
- [Mozilla MDN – SameSite Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value)



## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---