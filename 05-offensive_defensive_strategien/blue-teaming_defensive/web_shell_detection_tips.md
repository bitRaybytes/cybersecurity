# 🛡️ Web Shell Detection Tips

> **Hinweis:** Diese Datei ist Teil des Blue Team-Bereichs des Repositories und dient ausschließlich zur Verteidigung und Früherkennung von Angriffen – nicht zur Durchführung selbiger.

**Ziel dieser Datei:**  
Diese Datei dient Blue Teamern und IT-Sicherheitsverantwortlichen zur schnellen Orientierung 
bei der Erkennung und Abwehr von Webshells auf kompromittierten Systemen. Webshells ermöglichen 
Angreifern oft den Fernzugriff auf Server – und sind daher eine kritische Bedrohung.



## Inhaltsverzeichnis
- [Typische Auffälligkeiten](#typische-auffälligkeiten)
- [Tools & Techniken zur Erkennung](#tools--techniken-zur-erkennung)
- [Best Practices zur Härtung](#best-practices-zur-härtung)
- [Weitere Ressourcen](#weitere-ressourcen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Typische Auffälligkeiten

Bei forensischen Analysen oder Monitoring sollten folgende Indikatoren beachtet werden:

### Verdächtige Dateinamen / Uploads

- Dateien mit doppelter Erweiterung:  
  `cmdshell.php.jpg`, `backdoor.php%00.jpg`, `web.php5`
- Unerwartete Erweiterungen wie:  
  `.phtml`, `.php4`, `.php5`, `.pht`, `.inc`, `.phar`
- Dateinamen mit `cmd`, `shell`, `eval`, `upload`, `backdoor`

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Verdächtiger Code innerhalb von Dateien

- Verwendung gefährlicher PHP-Funktionen:
  - `eval()`, `system()`, `shell_exec()`, `passthru()`, `exec()`
  - `base64_decode()`, `gzinflate()`, `str_rot13()`, `assert()`
- Obfuskierter Code (z. B. Base64 oder Hex verschlüsselt)
- Dynamisch generierte Funktionennamen:  
  z. B. `($func = 'sys'.'tem')($cmd);`

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Log-/Verhaltensauffälligkeiten

- GET-Parameter mit Shell-Befehlen (z. B. `?cmd=ls`)
- Unerwartete POST- oder GET-Anfragen auf verdächtige Dateien
- Anfragen mit `curl`, `wget`, `whoami`, `id`, `cat /etc/passwd` in den Parametern



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Tools & Techniken zur Erkennung

### Dateibasierte Analyse

- **YARA Rules**
  - Erkennung von typischen Shell-Mustern im Quelltext
  - Beispiel: Regel für `eval(base64_decode(...))`
- **Antivirus-/EDR-Signaturen**
  - Viele AV-Produkte erkennen bekannte Webshell-Signaturen
- **Regex-Suche**
  - Suchen nach `eval\(base64_decode\(`, `assert\(` etc.
- **Diff-Tools**
  - Änderungen in Web-Verzeichnissen erkennen (z. B. mit `inotify`, `ossec`)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Laufzeitanalyse & Logüberwachung

- **Log-Analyse**
  - Webserver-Logs auf ungewöhnliche Requests überwachen
  - z. B. Apache/Nginx Logs mit `cmd=`, `eval`, `.php.jpg`
- **WAF (Web Application Firewall)**
  - Nutze ModSecurity-Regeln zur Blockierung verdächtiger Parameter
- **SIEM-Systeme**
  - Automatisierte Alarmierung bei verdächtigen Zugriffsmustern



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Best Practices zur Härtung

### 1. Uploads & Ausführung trennen
- Ladeverzeichnis (z. B. `/uploads`) darf keine `.php`-Dateien ausführen
- Webserver-Config: `php_admin_flag engine off` für Upload-Pfade

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### 2. Whitelisting statt Blacklisting
- Nur bestimmte Dateitypen zulassen (`.jpg`, `.png`, `.pdf`)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### 3. Input validieren & filtern
- Keine Dateiumbenennungen oder Erweiterungen auf Serverseite übernehmen
- MIME-Type-Prüfung, Magic-Bytes-Check

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### 4. Rechte & Isolation
- Webserver unter Low-Privilege-User laufen lassen (z. B. `www-data`)
- Dateiberechtigungen restriktiv setzen (z. B. 640)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### 5. Monitoring & Alerting
- Dateisystemmonitoring (z. B. `tripwire`, `ossec`)
- Logs rotieren, zentralisieren und auswerten (z. B. mit `logwatch`, `ELK`)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Weitere Ressourcen

- [OWASP Web Shell Detection](https://owasp.org/www-community/attacks/Web_Shell)
- [YARA Rule Database (GitHub)](https://github.com/Yara-Rules/rules)
- [MITRE ATT&CK - T1505.003: Web Shell](https://attack.mitre.org/techniques/T1505/003/)



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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---