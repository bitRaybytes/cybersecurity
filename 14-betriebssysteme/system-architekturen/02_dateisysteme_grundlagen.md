# 📂 Dateisysteme – Grundlagen und Sicherheit
## Inhaltsverzeichnis

- [Dateisystem: Ein Begriff, zwei Bedeutungen](#dateisystem-ein-begriff-zwei-bedeutungen)
- [Arten von Dateisystemen](#arten-von-dateisystemen)
- [Dateisystemstruktur unter Linux](#dateisystemstruktur-unter-linux)
    - [Verzeichnisbaum nach FHS](#verzeichnisbaum-nach-fhs)
    - [Befehle zur Navigation (Linux)](#befehle-zur-navigation-linux)
- [Dateisystemstruktur unter macOS](#dateisystemstruktur-unter-macos)
    - [Befehle zur Navigation (macOS)](#befehle-zur-navigation-macos)
- [Dateisystemstruktur unter Windows](#dateisystemstruktur-unter-windows)
    - [Befehle zur Navigation (Windows)](#befehle-zur-navigation-windows)
- [Wichtige Dateisysteme im Vergleich](#wichtige-dateisysteme-im-vergleich)
- [Sicherheitsaspekte von Dateisystemen](#sicherheitsaspekte-von-dateisystemen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dateisystem: Ein Begriff, zwei Bedeutungen
Der Begriff **Dateisystem** kann zwei Bedeutungen haben:

1. Er bezeichnet den gesamten übergeordneten **Verzeichnisbaum** und die Verzeichnisstruktur eines Betriebssystems.

2. Er steht für die individuell einbindbaren Dateisysteme, die auf Datenträger-Partitionen oder Volumes existieren.

Während ein Dateisystem je nach Partition oder Volume eingesetzt wird, ist es wichtig zu verstehen, dass das Dateisystem der eigentliche Inhalt ist, während die Partition nur der Rahmen ist, in dem der Speicherplatz dafür zur Verfügung gestellt wird.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Arten von Dateisystemen
### Lineare Dateisysteme
Merkmale sind hierbei, dass die Dateien in nur einer Ebene strukturiert und auf dem Datenträger (typischerweise) nacheinander abgelegt sind.

### Hierarchische Dateisysteme
Merkmal ist eine baumartige Struktur, die im Wurzelverzeichnis neben regulären Dateien auch Verweise auf weitere Verzeichnisse (die wiederum Unterverzeichnisse enthalten können) enthält.

```text
Hierarchische Dateisystem-Struktur
.
├── Dokumente
│   ├── Bericht.docx
│   └── Notizen.txt
├── Bilder
│   ├── Urlaub.jpg
│   └── Profilfoto.png
├── Programme
│   ├── app.exe
│   └── config.ini
└── README.md
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dateisystemstruktur unter Linux
Den **Linux-Verzeichnisbaum** gibt es nicht. Jede Distribution hat eine individuelle Verzeichnishierarchie entwickelt. Allerdings gibt es einige Verzeichnisse, die überall vorhanden sein sollen. Als Leitlinie für den Baum gilt der sogenannte Dateisystemstandard **FSSTND** (**File System Standard**). Der Linux-Verzeichnisbaum ist dabei keineswegs willkürlich gewählt, sondern bietet eine durchdachte Struktur.

Der Aufbau des Verzeichnisbaums erfüllt die folgenden Anforderungen:

- Möglichst kleine Systempartitionen mit allen zur Reparatur des Systems wichtigen Programmen und Einstellungen, was ein Auslagern von Anwendungen nach `/usr` erlaubt.

- Auslagern der Anwendungssoftware auf Netzlaufwerke (`/usr` im Netz und `/usr/local` lokal).

- Alle Anwendungsprogramme liegen direkt im Pfad des Benutzers (einige wenige `bin/` Verzeichnisse) und müssen dort nicht einzeln eingetragen werden.

- Programme zur Systemverwaltung liegen in getrennten Verzeichnissen (`/sbin`) und sind nur für den Administrator im Pfad.

- Fast der gesamte Verzeichnisbaum kann auch nur lesend verwendet werden, d.h. alles außer `/dev`, `/var` und `/home`.

- Die Systemkonfiguration ist möglichst konzentriert (`/etc`) und kann leicht gesichert werden.

- Zentrale Speicherung aller Benutzerdaten in `/home`.

### Verzeichnisbaum nach FSSTND
```text
/              - Wurzelverzeichnis
├── bin        - grundlegende Systembefehle (für alle Benutzer)
├── boot       - statische Dateien des Bootloaders
├── dev        - Gerätedateien
├── etc        - spezifische Konfigurationsdateien
├── home       - Benutzerverzeichnisse 
├── lib        - Kernel-Module und dynamische Bibliotheken
├── media      - Einhängepunkte für Wechseldatenträger
├── mnt        - temporäre Einhängepunkte für Dateisysteme
├── opt        - zusätzliche Softwarepakete
├── root       - Benutzerverzeichnis für Benutzer root (optional)
├── run        - virtuelles Verzeichnis für Prozessinformationen
├── sbin       - wichtige Systembefehle
├── srv        - Daten, die von Diensten angeboten werden
├── tmp        - temporäre Dateien
├── usr        - Verzeichnisstruktur für systemübergreifende Binärdateien
└── var        - Verzeichnisstruktur für variable Dateien
```

### Befehle zur Navigation (Linux)

| Befehl | Beschreibung |
|--------|--------------|
| `pwd` | Anzeige des aktuellen Verzeichnisses. |
| `cd` | Wechseln des Verzeichnisses. |
| `ls` | Auflisten aller Dateien in einem Verzeichnis. |
| `touch` | Aktualisiert die Zeitstempel einer Datei. |
| `mkdir` | Verzeichnis anlegen. |
| `rm` | Löschen (remove). |
| `mv` | Verschieben von Dateien und Verzeichnissen (move), auch zum Umbenennen. |
| `chmod` | Ändert die Rechte der Datei. |
| `chown` | Ändert den Eigentümer der Datei. |
| `chgrp` | Ändert die Gruppe der Datei. |
| `cat` | Concatenate - Zeigt den Inhalt einer Datei an. |
| `head` | Zeigt die ersten Zeilen einer Datei an. |
| `tail` | Zeigt die letzten Zeilen einer Datei an. |
| `vi` + `nano` | Texteditoren zum Ändern von Inhalten. |
| `man` | Benutzerhandbuch. |
| `cp` | Kopieren von Dateien oder Verzeichnissen. |
| `less` | Öffnet eine Datei nur zur Anzeige (nicht änderbar). |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dateisystemstruktur unter macOS
```text
/                - Wurzelverzeichnis
├── applications - Ordner für installierte Programme (Apps)
├── developer    - Entwicklerwerkzeuge, Dokumentation und Dateien, wenn Tools installiert
├── library      - Gemeinsam genutzte Bibliotheken
├── network      - Netzwerkbezogene Geräte, Server, Bibliotheken
├── system       - Betriebssystemkomponenten und -ressourcen
├── home         - normalerweise leer unter macOS
├── users        - Alle Benutzerkonten auf der Maschine und zugehörige Dateien
├── volumes      - Bereitgestellte Geräte und Volumes
├── bin          - Wesentliche gemeinsame Binärdateien, wie grundlegende Terminal-Befehle
├── etc          - Systemkonfiguration des lokalen Computers
├── dev          - Gerätedateien, alle Dateien, Peripheriegeräte (Maus, Tastatur, usw.)
├── usr          - 2. Haupthierarchie enthält Unterverzeichnisse, Infos, Konfig-Dateien
├── sbin         - wichtige Systemverwaltungs-Binaries
├── tmp          - temporäre Dateien, Caches, etc
├── var          - Variable Daten, enthält Dateien, deren Inhalt sich ändert
├── private      - spezieller Speicherort, der /var, /tmp und andere sensible Daten enthält
├── opt          - Optionale Software und Pakete
└── cores        - Speicherabbilder (Core Dumps) von Programmen, die abstürzen
```

### Befehle zur Navigation (macOS)

| Befehl | Beschreibung |
|--------|--------------|
| `pwd` | Anzeige des aktuellen Verzeichnisses. |
| `cd` | Wechseln des Verzeichnisses. |
| `ls` | Auflisten aller Dateien in einem Verzeichnis. |
| `touch` | Aktualisiert alle Zeitstempel einer Datei. |
| `mkdir` | Verzeichnis anlegen. |
| `rm` | Löschen (remove). |
| `mv` | Verschieben von Dateien und Verzeichnissen (move), auch zum Umbenennen. |
| `chmod` | Ändert die Rechte der Datei. |
| `chown` | Ändert den Eigentümer der Datei. |
| `chgrp` | Ändert die Gruppe der Datei. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dateisystemstruktur unter Windows
Die Struktur der Hauptordner in Windows 11 unterscheidet sich grundsätzlich von den Verzeichnisstrukturen linuxoider Systeme. Während Linux-Systeme Dateien bevorzugt funktionsbezogen speichern, speichern Windowssysteme Dateien bevorzugt anwendungsbezogen. Die Dateisystemstruktur ist proprietär, d. h. sie kann nur auf einem herstellereigenen Computermodell eingesetzt werden.

Einige der Hauptordner und ihre Funktionen:

| Pfad | Beschreibung |
|------|--------------|
| `C:\Windows` | Hauptverzeichnis des OS. |
| `C:\Windows\System32` | Speichert wichtige Systemdateien, Treiber und Bibliotheken. |
| `C:\Program Files` | Anwendungen und Programme für 64-Bit-OS. |
| `C:\Program Files (x86)` | Anwendungen und Programme für 32-Bit-OS. |
| `C:\Users` | Benutzerverzeichnis aller Benutzer auf der Maschine. |
| `C:\Users\Public` | Gemeinsam nutzbares Verzeichnis. |
| `C:\User\Default` | Template der Standard-Benutzereinstellungen. |

**Anmerkung:** Windows verfügt über Schutzmechanismen für diese Ordner, einschließlich Zugriffsberechtigungen und Funktionen wie dem Windows-Dateischutz, die nicht autorisierte Änderungen an Systemdateien verhindern.

### Befehle zur Navigation (Windows)

| Befehl | Beschreibung |
|--------|--------------|
| `cd` |  Wechseln des Verzeichnisses. |
| `dir` |  Zeigt den Inhalt des aktuellen Verzeichnisses an. |
| `mkdir` |  Verzeichnis erstellen. |
| `rmdir` |  Verzeichnis löschen. |
| `del` |  Löschen einer Datei. |
| `move` |  Verschieben von Dateien und Verzeichnissen. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wichtige Dateisysteme im Vergleich

### 1. exFAT
**exFAT** (**Extended File Allocation Table**) wurde 2006 eingeführt und ist für Flash-Speicher optimiert. Es ist genauso schlank wie FAT32, hat aber keine derartigen Dateigrößenbeschränkungen.

- **Speicherbegrenzung:** Dateigröße und Partitionsgröße maximal 128 Petabyte.

- **Idealer Verwendungszweck:** Größere Dateien, wenn NTFS nicht kompatibel ist.

- **Kompatibilität:** Kompatibel mit allen Windows-Versionen und modernen macOS-Systemen. Ältere Linux-Versionen benötigen zusätzliche Software.

### 2. FAT32
**FAT32** (**File Allocation Table 32**) wurde 1995 mit Windows 95 eingeführt. Es ist sehr kompatibel, da es der Standard auf vielen USB-Sticks ist.

- **Nachteile:** Nicht geeignet für internen Speicher (da langsam), fehlende Sicherheits-Features und eine Dateigrößenbeschränkung von 4 GB.

- **Idealer Verwendungszweck:** Externe USB-Drives und andere Geräte, die nicht mehr als 4 GB Speicher pro Datei beanspruchen.

### 3. NTFS
**NTFS** (**New Technology File System**) wurde 1993 mit Windows NT 3.1 eingeführt und bietet viele Features, die FAT32 und exFAT nicht haben, wie z. B. Sicherheit mit Zugriffsrechte-Management.

- **Nachteile:** Nur eingeschränkte Kompatibilität mit anderen Betriebssystemen. macOS ist nur lesend, und Linux eventuell auch nur lesend.

- **Idealer Verwendungszweck:** System-Drives und interne Speicher, die ausschließlich für Windows vorgesehen sind.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsaspekte von Dateisystemen
Die Wahl des richtigen Dateisystems und die korrekte Verwaltung der Zugriffsrechte sind essenziell für die IT-Sicherheit.

- **Zugriffsrechte:** Dateisysteme wie NTFS oder die in Linux/macOS verwendeten Dateisysteme (ext4, APFS) ermöglichen die granulare Zuweisung von Rechten (Lesen, Schreiben, Ausführen), um unautorisierte Zugriffe zu verhindern.

- **Journaling:** Systeme mit Journaling wie NTFS oder ext4 erhöhen die Datenintegrität. Wenn ein System abstürzt, kann das Dateisystem durch das Journaling den konsistenten Zustand schnell wiederherstellen, ohne dass es zu Datenverlusten oder Korruption kommt.


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