# 🐧 Linux Cheatsheet – Grundlagen & häufige Befehle

> Diese Übersicht enthält nützliche Linux-Kommandos für den täglichen Gebrauch in Pentesting, Systemadministration oder beim Lernen.

---

## 📂 Navigation im Dateisystem

| Befehl                       | Beschreibung                          |
|------------------------------|---------------------------------------|
| `pwd`                        | Zeigt das aktuelle Verzeichnis        |
| `ls`                         | Listet Dateien im aktuellen Ordner    |
| `ls -la`                     | Alle Dateien inkl. versteckter        |
| `cd /pfad/zum/ordner`        | Wechselt in ein Verzeichnis           |
| `cd ~` oder `cd`             | Zum Home-Verzeichnis wechseln         |
| `cd ..`                      | Ein Verzeichnis höher                 |

---

## 📄 Datei- & Ordneroperationen

| Befehl                       | Beschreibung                              |
|------------------------------|-------------------------------------------|
| `touch datei.txt`            | Leere Datei erstellen                     |
| `mkdir neuerOrdner`          | Ordner erstellen                          |
| `rm datei.txt`               | Datei löschen                             |
| `rm -r ordner/`              | Ordner rekursiv löschen                   |
| `cp quelle.txt ziel.txt`     | Datei kopieren                            |
| `mv quelle.txt ziel.txt`     | Datei verschieben / umbenennen            |
| `cat datei.txt`              | Inhalt anzeigen                           |
| `less datei.txt`             | Inhalt seitenweise anzeigen               |
| `head -n 10 datei.txt`       | Erste 10 Zeilen anzeigen                  |
| `tail -n 10 datei.txt`       | Letzte 10 Zeilen anzeigen                 |
| `nano datei.txt`             | Datei im Editor öffnen                    |
| `chmod +x script.sh`         | Datei ausführbar machen                   |

---

## 🔍 Suchen

| Befehl                               | Beschreibung                              |
|--------------------------------------|-------------------------------------------|
| `find / -name "datei.txt"`           | Sucht Datei systemweit                    |
| `find . -type f -name "*.log"`       | Sucht log-Dateien im aktuellen Ordner     |
| `grep "text" datei.txt`              | Durchsucht Datei nach Text                |
| `grep -r "text" ./`                  | Rekursive Suche in Ordnern                |
| `locate datei.txt`                   | Schnelle Dateisuche (mit Index)           |
| `which befehl`                       | Zeigt Pfad eines installierten Befehls    |

---

## 📦 Paketverwaltung

### Debian/Ubuntu (APT)

| Befehl                         | Beschreibung                           |
|--------------------------------|----------------------------------------|
| `sudo apt update`              | Paketlisten aktualisieren              |
| `sudo apt upgrade`             | Alle Pakete aktualisieren              |
| `sudo apt install paketname`   | Paket installieren                     |
| `sudo apt remove paketname`    | Paket entfernen                        |

### Arch (Pacman)

| Befehl                         | Beschreibung                           |
|--------------------------------|----------------------------------------|
| `sudo pacman -Syu`             | Systemupdate                           |
| `sudo pacman -S paketname`     | Paket installieren                     |

---

## 👤 Benutzer & Rechte

| Befehl                             | Beschreibung                             |
|------------------------------------|------------------------------------------|
| `whoami`                           | Aktueller Benutzername                   |
| `id`                               | Benutzer- und Gruppen-IDs anzeigen       |
| `sudo befehl`                      | Befehl als Admin ausführen               |
| `adduser username`                 | Neuen Benutzer erstellen                 |
| `passwd`                           | Passwort ändern                          |
| `chmod 755 datei.sh`               | Rechte ändern                            |
| `chown user:gruppe datei.txt`      | Besitzer ändern                          |

---

## 📊 Systemüberwachung

| Befehl                 | Beschreibung                          |
|------------------------|---------------------------------------|
| `top`                  | Prozessanzeige (live)                 |
| `htop`                 | Erweiterter Prozessmonitor            |
| `ps aux`               | Alle laufenden Prozesse               |
| `df -h`                | Festplattenbelegung                   |
| `du -sh ordner/`       | Größe eines Ordners                   |
| `free -h`              | RAM-Auslastung                        |
| `uptime`               | Systemlaufzeit                        |

---

## 🌐 Netzwerk

| Befehl                       | Beschreibung                            |
|------------------------------|-----------------------------------------|
| `ip a` oder `ifconfig`       | IP-Adressen anzeigen                    |
| `ping -c 4 domain.de`        | Ping-Test (4 Pakete)                    |
| `netstat -tuln`              | Offene Ports anzeigen                   |
| `ss -tuln`                   | Modernes netstat-Äquivalent             |
| `curl http://seite.de`       | HTTP-Request                            |
| `wget http://seite.de/datei` | Datei herunterladen                     |
| `nmap -sS 192.168.0.1`       | Portscan (mit nmap)                     |

---

## 🧰 Nützliche Tools (falls installiert)

| Tool       | Zweck                                    |
|------------|------------------------------------------|
| `tmux`     | Terminal-Multiplexer                     |
| `nc`       | Netcat – Portkommunikation               |
| `jq`       | JSON-Parser für die Kommandozeile        |
| `tree`     | Ordnerstruktur als Baum anzeigen         |

---

## 🔐 Rechte & Sicherheit

| Befehl                       | Beschreibung                          |
|------------------------------|---------------------------------------|
| `sudo su`                    | Als Root einloggen                    |
| `visudo`                     | Sudo-Konfiguration bearbeiten         |
| `ufw status`                 | Firewall-Status prüfen (Ubuntu)       |
| `ufw enable`                 | Firewall aktivieren                   |

---

## 🧹 Cleanup & Logins

| Befehl                  | Beschreibung                         |
|------------------------|---------------------------------------|
| `history`              | Letzte Befehle anzeigen               |
| `clear`                | Terminal leeren                      |
| `exit`                 | Terminal-Session beenden             |
| `last`                 | Letzte Logins anzeigen               |
| `who`                  | Wer ist eingeloggt?                  |

---

## 🧠 Bonus: Nützliche Tastenkürzel im Terminal

| Kürzel           | Funktion                                 |
|------------------|------------------------------------------|
| `Ctrl + C`       | Prozess abbrechen                        |
| `Ctrl + D`       | Logout / Ende Eingabe                    |
| `Ctrl + L`       | Terminal leeren                          |
| `Ctrl + R`       | Rückwärtssuche im Befehlshistorie        |
| `Tab`            | Autovervollständigung                    |
| `!!`             | Letzten Befehl wiederholen               |
| `!xyz`           | Letzten Befehl mit "xyz" starten         |

---

> ✨ **Tipp**: Nutze `man befehl`, um die Manpage eines Befehls zu lesen, z. B. `man ls`.

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!