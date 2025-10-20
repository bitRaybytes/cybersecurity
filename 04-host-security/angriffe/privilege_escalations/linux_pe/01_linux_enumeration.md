# 🐧 Linux Enumeration – Phase 1: Privilege Escalation & Recon

## Inhaltsverzeichnis
- [Einleitung - Was ist Privilege Escalation?](#einleitung---was-ist-privilege-escalation)
- [Ziel der Linux-Enumeration](#ziel-der-linux-enumeration)
- [1. Allgemeine System- und Kernel-Informationen](#1-allgemeine-system--und-kernel-informationen)
- [2. Benutzer & Rechte](#2-benutzer--rechte)
- [3. Benutzerrechte & sudo](#3-benutzerrechte--sudo)
- [4. Prozesse & laufende Dienste](#4-prozesse--laufende-dienste)
- [5. Cronjobs & zeitgesteuerte Tasks](#5-cronjobs--zeitgesteuerte-tasks)
- [6. Dateien mit gefährlichen Rechten](#6-dateien-mit-gefährlichen-rechten)
- [7. Interessante Dateien & Pfade](#7-interessante-dateien--pfade)
- [8. Netzwerk- und Routing-Informationen](#8-netzwerk--und-routing-informationen)
- [9. Mounts & Dateisysteme](#9-mounts--dateisysteme)
- [10. Wichtige Utility-Befehle zur Datenanalyse](#10-wichtige-utility-befehle-zur-datenanalyse)
- [Automatisierte Enumeration](#automatisierte-enumeration)
- [Haftungsausschluss](#haftungsausschluss)

 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung - Was ist Privilege Escalation?
**Privilege Escalation** ist ein fundamentaler Schritt im Post-Exploitation-Prozess. Es beschreibt die Ausnutzung einer Sicherheitslücke oder Fehlkonfiguration, um von einem eingeschränkten Benutzerkonto (niedrige Privilegien) auf ein Konto mit höheren Berechtigungen zu wechseln, idealerweise das `root`-Konto (höchste Privilegien) eines Linux-Systems.

Technisch gesehen zielt der Angriff darauf ab, die von den Systemdesignern festgelegten Zugriffs- und Berechtigungsgrenzen zu umgehen. Dies kann durch die Manipulation grundlegender Systemdateien, Konfigurationen, laufender Prozesse oder durch die Ausnutzung von Fehlern im Kernel selbst erreicht werden.

### Die Rolle der Enumeration

Die **Enumeration** ist die erste Phase und die entscheidende Voraussetzung für jede erfolgreiche Privilege Escalation. Sie ist die systematische Sammlung aller möglichen Informationen über das Zielsystem, um verwundbare Vektoren zu finden. Ohne eine umfassende Enumeration bleibt der Angreifer blind.

> 💡 **Denke daran:** Jeder Befehl, jede Konfigurationsdatei und jeder laufende Prozess kann einen Hinweis auf eine mögliche Rechteausweitung liefern – sei es ein SUID-Binary, ein falsch konfigurierter Cronjob oder ein veralteter Kernel.


## Ziel der Linux-Enumeration

Die Linux-Enumeration dient dazu, Informationen über ein kompromittiertes System zu sammeln, um Schwachstellen zu identifizieren und eventuell höhere Rechte (z. B. root) zu erlangen.

> 🎯 Ziel: Zugang, Persistenz, Privilege Escalation, lateral Movement



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Allgemeine System- und Kernel-Informationen
Diese Befehle dienen dazu, das Betriebssystem und den Kernel zu identifizieren. Veraltete oder nicht gepatchte Kernel sind ein Hauptvektor für Kernel-Exploits.


| Befehl | Beschreibung | Relevanz für PrivEsc |
|--------|--------------|----------------------|
| `uname -a` | Zeigt Kernel-Version, Architektur und Hostnamen. | Kritisch für die Suche nach spezifischen Kernel-Exploits (z.B. Dirty COW). | 
| `cat /proc/version` | Zeigt die spezifische Kernel-Version und den Compiler-Pfad. | Zusätzliche Detailinformationen zur Kernel-Umgebung. | 
| `hostname` | Zeigt den Hostnamen. | Identifikation des Ziels und manchmal relevant in Konfigurationsdateien. |
| `cat /etc/{issue,*release}` | Zeigt die Distribution (Ubuntu, Debian, CentOS) und Version. |Dient zur Identifikation von Distro-spezifischen Schwachstellen. |
| `uptime` | Zeigt die Systemlaufzeit. | Lange Laufzeiten deuten auf fehlendes, regelmäßiges Patching hin. |
 

> Achte auf alte Kernel oder nicht gepatchte Versionen!



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 2. Benutzer & Rechte
Hier suchst du nach unberechtigten Benutzern, potenziellen Anmeldeinformationen und der aktuellen Sicherheitskontext des kompromittierten Accounts.

| Befehl | Beschreibung | Relevanz für PrivEsc |
|--------|--------------|----------------------|
| `id` | Zeigt die aktuelle Benutzer-ID (`UID`), effektive Gruppen (`EUID`) und Gruppenzugehörigkeiten. | Gibt sofort Aufschluss über die aktuellen Rechte und Gruppen-Privilegien. |
| `whoami` | Zeigt den aktuellen Benutzernamen. | Basis-Identifikation. |
| `groups` | Listet alle Gruppen auf, in denen der Benutzer Mitglied ist. | Gruppenrechte (z.B. `docker`, `adm`, `lxd`) können sofort zur PrivEsc führen. |
| `cat /etc/passwd` | Listet alle Benutzerkonten und deren Shells auf. | Identifikation existierender Konten, die gehackt werden könnten. |
| `cat /etc/shadow` | Enthält gehashte Passwörter (normalerweise nur für Root lesbar). | Kritisch: Wenn lesbar, können Hashes offline geknackt werden. |
| `last` | Zeigt die Login-Historie der Benutzer an. | Identifiziert aktive Benutzer und potenzielle Lateral-Movement-Ziele. |



```bash
# Aktuelle Benutzer- und Gruppeninformationen
id
groups

# Liste aller Benutzer und deren Home-Verzeichnisse/Shells
cat /etc/passwd

# System-Login-Historie
last
```

- Sind sensible Benutzer vorhanden? (z. B. `test`, `backup`, `admin`)
- Kann man `.bash_history` anderer Benutzer lesen?



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Benutzerrechte & sudo
Dieser Abschnitt konzentriert sich auf die mächtigsten und oft übersehenen direkten Angriffsvektoren: Sudo-Rechte, die Nutzung von Umgebungsvariablen und Shared Library Hijacking.

| Befehl | Beschreibung | Relevanz für PrivEsc |
|--------|--------------|----------------------|
| `sudo -l` | Zeigt an, welche Befehle der aktuelle Benutzer ohne Passwort oder mit einem anderen Benutzer (z.B. root) ausführen darf. | Der wichtigste und direkteste PrivEsc-Vektor. Suchen Sie nach Binaries auf GTFOBins. |
| `env` | Zeigt die Umgebungsvariablen. | Suche nach hartkodierten Passwörtern, API-Keys oder Hinweisen auf einen manipulierbaren PATH. |
| `echo $PATH` | Zeigt die Reihenfolge der Verzeichnisse, in denen Befehle gesucht werden. | Entscheidend für PATH-Hijacking. Wenn ein beschreibbarer Pfad am Anfang steht, kann man eigene Skripte vor dem System-Binary ausführen. |
| `ldconfig -p` | Zeigt die vom System bekannten Shared Libraries. | relevant für LD_PRELOAD Hijacking, wenn Programme Shared Libraries dynamisch laden und man die Umgebung beeinflussen kann. |

```bash
# Prüft auf NOPASSWD-Sudo-Einträge – sofortige Root-Möglichkeit?
sudo -l

# Umgebungsvariablen prüfen (Passwörter, PATH-Variablen)
env

# Aktueller Bash-Verlauf (oft Klartext-Passwörter)
cat ~/.bash_history

```

- Hat der Benutzer Root-Befehle ohne Passwort?
- Gibt es eine Umgehung wie `sudo` `less`, `nano`, `awk`, `python`, `tar`?



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 4. Prozesse & laufende Dienste
Hier untersuchst du, was gerade auf dem System abläuft und welche Dienste lauschen. Prozesse, die als root laufen und Daten lesen/schreiben, können anfällig für Signalketten oder Buffer Overflows sein.

| Befehl | Beschreibung | Relevanz für PrivEsc |
|--------|--------------|----------------------|
| `ps aux / ps -ef` | Zeigt alle laufenden Prozesse mit deren Benutzer (`USER`), Prozess-ID (`PID`) und dem vollständigen Befehl (`COMMAND`). | Identifikation von Root-Prozessen, die möglicherweise verwundbare Argumente verwenden oder auf Dateien zugreifen. | 
| `ss -tuln` | Listet alle lauschenden `TCP`- und `UDP-Ports`, numerisch. | Identifikation interner Dienste (`127.0.0.1`), die ungesichert sind. (Alternative zu `netstat`). | 
| `systemctl list-units --type=service` | Listet alle Dienste im Systemd-Format auf (bei modernen Distros). | Prüft, welche Dienste aktiv sind und deren Konfigurationsdateien man untersuchen sollte. | 
| `lsof -i` | Listet alle Dateien auf, die von Prozessen geöffnet wurden. | Zeigt, welche Prozesse auf welche Netzwerkressourcen zugreifen. | 



```bash
# Zeigt alle laufenden Prozesse (Suche nach Root-Prozessen)
ps aux

# Zeigt alle lauschenden Ports und deren zugehörige Prozesse
netstat -tulpen
ss -tuln
```

- Gibt es verdächtige Prozesse?
- Welche Dienste laufen lokal (`127.0.0.1`)?



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 5. Cronjobs & zeitgesteuerte Tasks
Cronjobs sind zeitgesteuerte Befehle, die oft mit Root-Rechten laufen, um administrative Aufgaben zu erfüllen. Wenn das Skript, das der Cronjob ausführt, beschreibbar ist, kann man eigenen Code einschleusen.


```bash
# Listet die Cronjobs des aktuellen Benutzers auf
crontab -l

# Listet die System-Cronjobs auf (oft als root)
cat /etc/crontab

# Untersucht die Verzeichnisse der Cron-Aufgaben
ls -la /etc/cron.*
```
- **Angriffsvektoren:** (unsichere Skripte oder manipulierte Pfade)
    1. **Writable Scripts:** Kannst du das Skript im Cronjob überschreiben?
    2. **PATH Hijacking:** Wird ein Befehl im Cron-Skript ohne absoluten Pfad aufgerufen?



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Dateien mit gefährlichen Rechten
SUID (`Set User ID`) und SGID (`Set Group ID`) sind die kritischsten Dateiberechtigungen. Das SUID-Bit erlaubt es, ein Programm mit den Rechten des Dateieigentümers auszuführen (wenn der Eigentümer `root` ist, läuft das Programm als Root).
```bash
# **Kritischer Check:** Sucht nach allen SUID-Files (/4000)
# 2>/dev/null verhindert "Permission denied"-Fehler
find / -perm -4000 -type f 2>/dev/null

# Sucht nach allen SGID-Files (/2000)
find / -perm -2000 -type f 2>/dev/null

# Sucht nach allen Dateien, die vom aktuellen Benutzer beschrieben werden können
find / -writable -type f 2>/dev/null

# Sucht nach allen SUID-Files (/4000)
find / -perm -u=s -type f 2>/dev/null
```

Wichtige SUID-Binaries (Anfällig für Missbrauch):   
`/bin/bash`, `/usr/bin/nmap`, `/usr/bin/find`, `/usr/bin/perl`, `/usr/bin/python`

> Erste Anlaufstelle für Exploits: [GTFOBins](https://gtfobins.github.io/)

 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 7. Interessante Dateien & Pfade
Suche gezielt nach Anmeldeinformationen (Passwörter, Schlüssel, Tokens) in Konfigurations- oder Verlaufsdateien.

### 7.1 SSH-Keys und History
```bash
# Zeigt den SSH-Privatschlüssel (falls vorhanden)
cat ~/.ssh/id_rsa

# Zeigt erlaubte Public Keys für den Login
cat ~/.ssh/authorized_keys

# Sucht nach SSH-Keys anderer Benutzer
find /home/ -name 'id_rsa' -type f -readable 2>/dev/null
``` 

### 7.2 Passwörter und Konfigurationen
```bash
# Durchsucht die Bash-Historie nach Passwörtern, etc.
cat ~/.bash_history

# Suche nach Schlüsselwörtern wie 'pass' oder 'key' in gängigen Pfaden
grep -rni "password\|key\|cred" /var/www 2>/dev/null
grep -rni "password\|key\|cred" /etc/apache2 2>/dev/null

# Suche nach Webserver- und Datenbank-Config-Dateien
find /var/www/ -name "*.php" -exec grep -i "db_pass\|connect" {} \; 2>/dev/null
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 8. Netzwerk- und Routing-Informationen
Diese Befehle helfen beim lateralen Movement – der Ausweitung des Angriffs auf andere Systeme im Netzwerk.

```bash
# Zeigt alle Netzwerkschnittstellen und IP-Adressen an
ip a 
# Oder ältere Systeme:
ifconfig

# Zeigt die Routing-Tabelle
ip r

# Zeigt DNS-Einstellungen an (wichtig für interne Hostnamen-Auflösung)
cat /etc/hosts
cat /etc/resolv.conf

# Zeigt ARP-Cache an (welche IPs sind kürzlich kommuniziert?)
arp -a
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 9. Mounts & Dateisysteme
Überprüfe, ob Netzlaufwerke (`NFS`, `SMB/CIFS`) gemountet sind. Insbesondere NFS-Freigaben mit der Option `no_root_squash` sind ein direkter Root-Vektor, da man dort eigene SUID-Binaries ablegen kann.

```bash
# Zeigt alle gemounteten Dateisysteme
mount

# Zeigt die Größe und den verfügbaren Speicherplatz des Dateisystems
df -h

# Zeigt die Konfiguration der Mountpoints
cat /etc/fstab

```

> Abgleich mit CVE-Datenbanken → Alte Pakete = Exploit-Möglichkeit



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 10. Wichtige Utility-Befehle zur Datenanalyse
In der Enumeration fallen schnell riesige Datenmengen an (z. B. 10.000 Zeilen von `find` oder `ps aux`). Die folgenden Befehle sind unverzichtbar, um die Ausgabe zu filtern, zu bearbeiten und das Wesentliche zu extrahieren.

| Befehl | Beschreibung | Relevanz für PrivEsc |
|--------|--------------|----------------------|
| `grep` | Filtern: Zeigt Zeilen, die ein bestimmtes Muster enthalten/ausschließen. | `find / -perm /4000 2>/dev/null` |
| `cut` | Schneiden: Extrahiert Spalten (Felder) aus dem Input basierend auf einem Trennzeichen. | `cat /etc/passwd` |
| `sort` | Sortieren: Sortiert Textzeilen alphabetisch oder numerisch. | `ps aux` |
| `locate` | Suchen: Sucht Dateien über eine indexierte Datenbank (schnell, aber unzuverlässig bei neuen Dateien). | `locate /var/www/*.conf` |






```bash
# Beispiel: Finde alle SUIDs, die das Wort 'perl' im Pfad haben
find / -perm -4000 -type f 2>/dev/null | grep perl

# Beispiel: Finde alle SUIDs und sortiere sie nach Pfad
find / -perm -4000 -type f 2>/dev/null | sort

grep -i "password" /etc/*.conf
find / -name "*.php" | xargs grep -i "db_pass"
```

- Klartext-Passwörter in Skripten, Configs oder Umgebungen?
- `.env` oder `.git/config`?





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Automatisierte Enumeration
Obwohl manuelle Enumeration das Verständnis fördert, sparen automatisierte Tools in realen Szenarien Zeit und minimieren die Wahrscheinlichkeit, einen Vektor zu übersehen.


| Tool               | Beschreibung                       |
| ------------------ | ---------------------------------- |
| [`LinPEAS`](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS) | Vollautomatische Enumeration |
| [`Linux Smart Enum`](https://github.com/diego-treitos/linux-smart-enumeration) | Minimalistisches Bash-Tool |
| [`LinEnum`](https://github.com/rebootuser/LinEnum) | Klassisches Linux-Enum-Tool |
| [`pspy`](https://github.com/DominicBreuker/pspy) | Prozess- und Cronwatcher ohne Root |
| [`GTFOBins`](https://gtfobins.github.io/) | Exploitable Binaries mit SUID/Sudo |

```bash
# Beispiel für LinPEAS.sh Download und Ausführung
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Oktober 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---