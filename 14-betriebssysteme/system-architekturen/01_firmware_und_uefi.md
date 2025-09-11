# 💻 Firmware und UEFI: Das Fundament der Cyber-Sicherheit

## Inhaltsverzeichnis
- [Einleitung: Die unsichtbare Software](#einleitung-die-unsichtbare-software)
- [UEFI als Nachfolger des BIOS](#uefi-als-nachfolger-des-bios)
    - [Vorteile von UEFI](#vorteile-von-uefi)
- [Was macht UEFI?](#was-macht-uefi)
- [Der Systemstart: Vom Bootloader zum Kernel](#der-systemstart-vom-bootloader-zum-kernel)
- [Secure Boot: Die digitale Signatur des Bootvorgangs](#secure-boot-die-digitale-signatur-des-bootvorgangs)
- [Weitere Firmware-Interfaces](#weitere-firmware-interfaces)
- [Informationsgewinnung aus dem System](#informationsgewinnung-aus-dem-system)
- [Begriffe und Abkürzungen](#begriffe-und-abkürzungen)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

## Einleitung: Die unsichtbare Software
**Firmware** ist die erste Software, die auf einem Computer nach dem Einschalten ausgeführt wird. Sie ist fest auf einem Speicherchip auf der Hauptplatine (Mainboard) verankert und hat eine entscheidende Aufgabe: **die Initialisierung der Hardware**.

Ohne sie wären CPU, RAM und Festplatte nur nutzlose Bauteile. Die Firmware agiert als Brücke zwischen der physischen Hardware und dem Betriebssystem. Schwachstellen in dieser Schicht sind besonders kritisch, da sie es einem Angreifer ermöglichen, sich extrem tief im System einzunisten, selbst wenn das Betriebssystem neu installiert wird.

Der Begriff leitet sich davon ab, dass Firmware funktional fest mit der Hardware verbunden ist. Auch außerhalb eines PCs findet man Firmware in nahezu jedem modernen Gerät, von Mobiltelefonen über Drucker bis hin zu Smart-Home-Geräten, wo sie auch oft als Systemfirmware bezeichnet wird.

## UEFI als Nachfolger des BIOS
Das **UEFI** (**Unified Extensible Firmware Interface**) ist das moderne Firmware-Interface und der Nachfolger des jahrzehntelang dominanten **BIOS** (**Basic Input Output System**). Während das BIOS eine einfache, textbasierte und sehr begrenzte 16-Bit-Umgebung bot, ist UEFI eine leistungsstarke, flexible und erweiterbare 32- oder 64-Bit-Software.

### Vorteile von UEFI

| Merkmal | **UEFI** | **BIOS** |
|---------|----------|----------|
| **Geschwindigkeit** | Schnellerer Systemstart durch optimierte Initialisierung. | Langsamer, sequenzieller Start. |
| **Festplattenkapazität** | Unterstützt Festplatten > 2 TB durch GUID-Partitionstabelle (GPT). | Maximal 2 TB durch Master Boot Record (MBR). |
| **Sicherheit** | Bietet Funktionen wie **Secure Boot**. | Keine integrierten Sicherheitsfunktionen. |
| **Schnittstelle** | Grafische Benutzeroberfläche (GUI), Mausunterstützung. | Textbasierte, tastaturgesteuerte Oberfläche. |
| **Funktionalität** | Integrierte Netzwerkfähigkeit, Bootmanager, Multitasking. | Sehr begrenzte Funktionalität. |


## Was macht UEFI?

Die Hauptaufgabe von UEFI ist es, den Computer vom ausgeschalteten Zustand bis zur Übergabe der Kontrolle an das Betriebssystem zu bringen.

1. **Systemprüfung (POST):** UEFI initialisiert und überprüft die wesentlichen Hardware-Komponenten wie CPU, RAM und Grafikkarte.

2. **Laden von Treibern:** Es kann native 64-Bit-Treiber laden, was eine schnelle Initialisierung der Hardware (z. B. Netzwerk-Controller oder Speicher) ermöglicht.

3. **Boot-Manager:** UEFI hat einen eigenen Boot-Manager, der es dem Benutzer erlaubt, aus einer Liste von Boot-Optionen (z. B. verschiedenen Betriebssystemen) zu wählen. Dies eliminiert die Notwendigkeit für separate Bootloader wie GRUB.

4. **Übergabe an den Bootloader:** UEFI übergibt die Kontrolle an den Betriebssystem-Bootloader, der dann den Kernel des Betriebssystems startet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Der Systemstart: Vom Bootloader zum Kernel

Der Bootvorgang ist eine Kette von Ereignissen, die sicherstellen, dass dein System korrekt hochfährt.

```text
Prinzipieller Systemstart:
                                                Start
                                                  V
                          +-----------------------------------------------+
                          |                     Start                     |
                          | (Mainboard wird stabil mit Spannung versorgt) |
                          +-----------------------------------------------+  
                                                  V
                          +-----------------------------------------------+
                          | SoC-Firmware-Startladeprogramme               |
                          | initialisieren den minimalen Hardwareersatz   | 
                          +-----------------------------------------------+
                                                  V
                          +-----------------------------------------------+
                          | UEFI wird geladen, lädt dann Gerätetreiber    |
          +----+          +-----------------------------------------------+
          |GUI |<-----+                           V
+-------+ +----+      | +--------------------------------------------------+
| SHELL |<------------+-| Komponenten auf dem Mainboard werden parallel    |
+-------+             | | initialisiert. CPU, RAM, Chipsatz, Schnitstellen |
    +----------+<-----+ +--------------------------------------------------+
    | Netzwerk |                                  V
    +----------+          +-----------------------------------------------+<------------+
                          | UEFI lädt eigenen Bootmanager                 |<------+     |
                          +-----------------------------------------------+       |     |
                                                  V                           BS nutzt  |
                          +-----------------------------------------------+   Treiber   |
                          | Bootmanager lädt Betriebssystem               |-------+     |
                          +-----------------------------------------------+            API
                                                  V                                     |
                          +-----------------------------------------------+             |
                          | Betriebssystem wird ausgeführt                |-------------+
                          +-----------------------------------------------+
```

## Secure Boot: Die digitale Signatur des Bootvorgangs
Secure Boot ist eine zentrale Sicherheitsfunktion von UEFI. Sie stellt sicher, dass nur vertrauenswürdige Bootloader und Betriebssysteme geladen werden.

- **Vertrauenskette:** Secure Boot nutzt eine Kette digitaler Signaturen. Der **Platform Key** (**PK**) des Hardware-Herstellers sorgt dafür, dass nur signierte Firmware zugelassen wird. **Key Exchange Keys** (**KEK**) erlauben es Betriebssystem-Herstellern, eigene Bootloader zu signieren.

- **Funktionsweise:** Die UEFI-Firmware prüft die digitale Signatur des Bootloaders, bevor sie ihn ausführt. Ist die Signatur nicht gültig, wird der Start blockiert. Eine "schwarze Liste" gesperrter Schlüssel verhindert zudem das Booten bekannter Malware.

- **Hinweis für Linux-Nutzer:** Größere Linux-Distributionen lassen ihre Bootloader von Microsoft signieren, um die Kompatibilität mit den meisten Secure Boot-fähigen Systemen zu gewährleisten. Für spezielle Anwendungen oder um bestimmte Sicherheitssysteme zu umgehen, ist es oft notwendig, Secure Boot im UEFI-Setup zu deaktivieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Weitere Firmware-Interfaces
Es ist wichtig zu wissen, dass es neben BIOS und UEFI auch andere Firmware-Schnittstellen gibt:

- **Open Firmware (IEEE-1275):** Ein plattform- und prozessorunabhängiger Industriestandard, der vor allem bei älteren PowerPC- und SPARC-Workstations verwendet wurde.

- **Coreboot (vormals LinuxBIOS):** Ein Open-Source-Firmware-Projekt, das das proprietäre BIOS/UEFI ersetzen soll. Es zeichnet sich durch seine extrem schnelle Bootzeit aus und wird in bestimmten spezialisierten Systemen eingesetzt.

- **m1n1:** Eine spezielle, von Asahi Linux entwickelte Firmware für Apples M1/M2-Hardware, um das Booten von Linux zu ermöglichen. Sie ist ein exzellentes Beispiel für das Reverse Engineering proprietärer Boot-Prozesse.






## Informationsgewinnung aus dem System
Als IT-Experte ist es wichtig zu wissen, wie du die Firmware-Informationen deines Systems abfragen kannst. Einige der folgenden Befehle benötigen Administratorrechte.

- **Windows:**

    - `msinfo32.exe`: Zeigt den BIOS-Modus an (Legacy oder UEFI).
    - `diskmgmt.msc`: Im Fenster der Datenträgerverwaltung kannst du den Partitionsstil (GPT oder MBR) der Festplatte einsehen.
    - `bcdedit.exe`: Dieses Kommandozeilen-Tool zeigt den Boot-Manager-Pfad an. Lädt ein System eine .efi-Datei, nutzt es den UEFI-Modus.

- **Linux:**

    - `efibootmgr`: Zeigt die Boot-Einträge und ihre Reihenfolge an.
    - `dmesg | grep -i efi`: Zeigt die UEFI-spezifischen Kernelmeldungen beim Boot an.

## Begriffe und Abkürzungen
- **GPT** (**GUID-Partition Table**): Das neuere Partitionsschema, das große Festplatten und viele Partitionen unterstützt.

- **MBR** (**Master Boot Record**): Das ältere Partitionsschema, beschränkt auf 2 TiB und 4 primäre Partitionen.

- **NVRAM** (**non-volatile random access memory**): Nichtflüchtiger Speicher, in dem UEFI-Einstellungen gespeichert werden.

- **SoC** (**System-on-a-Chip**): Ein einzelner Chip, der fast alle Mainboard-Komponenten integriert.

- **OTPROM**, **EPROM**, **EEPROM**: Verschiedene Typen von Read-Only Memory, die zur Speicherung der Firmware genutzt werden.


## Nützliche Links
- [UEFI Forum](https://uefi.org/)

- [Microsoft Doku zu Secure Boot](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-secure-boot)

- [Asahi Linux m1n1 Doku](https://leo3418.github.io/asahi-wiki-build/m1n1user-guide/)


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