# Betriebssysteme

## Inhaltsverzeichnis
- [Definition & Rolle des Betriebssystems](#definition--rolle-des-betriebssystems)
   - [Wer benötigt ein Betriebssystem?](#wer-benötigt-ein-betriebssystem)
- [Geschichtlicher Abriss](#geschichtlicher-abriss)
- [Grundfunktion eines Betriebssytems](#grundfunktion-eines-betriebssytems)
- [Klassifizierung von Betriebssystemen](#klassifizierung-von-betriebssystemen)
- [Aufbau eines Betriebssystems](#aufbau-eines-betriebssystems)
- [Kernel-Arten im Sicherheitskontext](#kernel-arten-im-sicherheitskontext)
- [Sicherheitsmechanismen und Zugriffsmodelle](#sicherheitsmechanismen-und-zugriffsmodelle)
   - [Trusted Computing Base (TCB)](#trusted-computing-base-tcb)
   - [Zugriffskontrollmodelle](#zugriffskontrollmodelle)
- [Komponenten eines Betriebssystems](#komponenten-eines-betriebssystems)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Definition & Rolle des Betriebssystems

Unter **Betriebssystem** (engl. **Operating System**, **OS**) versteht man die grundlegende Software, die zusammen mit den Hardwareeigenschaften des Computers die Basis zum Betrieb bildet. Es steuert und überwacht die Abarbeitung von Programmen und macht die Nutzung des Computers erst möglich.

- **Definition nach DIN 44300:**  
“Die Programme eines digitalen Rechensystems, die zusammen mit den Eigenschaften dieser Rechenanlage die Basis der möglichen Betriebsarten des digitalen Rechensystems bilden und insbesondere die Abwicklung von Programmen steuern und überwachen.”



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Wer benötigt ein Betriebssystem?

Kurz: Alle Geräte, die einen Prozessor beinhalten, benötigen ein Betriebssystem oder zumindest eine Firmware-Basis:

- PCs, Server und Mainframes
- IoT-Geräte, SmartHome-Geräte
- Embedded Computer, Single-Board-Computer
- Telefone, Smartphones, Router, Switche, Drucker
- Geräte der Unterhaltungselektronik



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Geschichtlicher Abriss

Die Entwicklung der Betriebssystem in eng mit der Hardware-Entwicklung verknüpft:

| Generation | Zeitrahmen | Technologie & Merkmale | Betriebssytem-Entwicklung |
|------------|------------|------------------------|---------------------------|
| Erste | 1945-55 | Binäre Rechner (Relais/Röhren). Steuerung über Klinkenfelder (verdrahtete Programme). | Kein OS nötig. |
| Zweite | 1955-65 | Rechnersytem auf Transistorbasis. Stapelverarbeitung (**Batch Processing**) | Vorläufer-OS zur sequenziellen Steuerung der Programmausführung |
| Dritte | 1965-80 | Zeitalter der zentralen Großrechner. Integrierte Schaltungen. **Time-Sharing** und **Multitasking** | Erste Großrechnerbetriebssysteme wie **UNIX** |
| Vierte | ab 1980 | Sinkende Hardwarepreise, Prozessorschaltkreise (CPUs). Personal Computer (PCs) | Einfache OS wie **CP/M**, **DOS**, **Windows 3.x.** |
| Vierte (Modern) | ab 1990 | 32- und 64-bit-Architekturen. Netzwerkfähige, komplexe, multithread-fähige Systeme. | Windows NT/95 ff., Linux, Apple macOS, mobile OS (iOS, Android). |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundfunktion eines Betriebssytems

Ein Betriebssytem fungiert als zentraler Vermittler und Ressourcenmanager.

| Rolle | Aufgaben |
|-------|----------|
| **Schnittstelle zur Hardware** | Verwaltung von Prozessor, Arbeitsspeicher, Datenspeicher und Peripherie. |
| **Schnittstelle zur Anwendungen** | Bereitstellung von Diensten (Sytem-APIs) und Treibern. |
| **Schnittstelle zum Benutzer** | Bereitstellung von Kommandozeile (CLI) oder grafischer Oberfläche (GUI). |
| **Prozessormanager** | Planung, Erstellung, Ausführung und Beendigung von Prozessen. |
| **Datenmanager** | Verwaltung von Massenspeicher, Dateisystem (NTFS, ext4, APFS etc.) und -strukturen. |
| **Benutzermanager** | Verwaltung von Benutzerkonten, Rechten und Zugriffskontrolle. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Klassifizierung von Betriebssystemen
Betriebsysteme lassen sich nach verschiedenen Kriterien einteilen:

| Kriterium | Einteilung | Beispiele |
|-----------|------------|-----------|
| **Anzahl der Benutzer** | Benutzerloste Systeme, Einbenutzersystem, Mehrbenutzersystem | Drucker, DOS, Windows 95 ff., Unixiode |
| **Gleichzeitig bearbeitbare Prozesse** | Singletasking OS, Multitasking OS | DOS, Windows, Linux |
| **Anzahl unterstützer Prozessoren** | Einprozessor-BS, Mehrprozessor-OS | DOS, Windows NT ff., Unixiode |
| **Einsatzzweck** | Einzelplatz/Client, Server/Supercomputer, Großrechner, Mobile OS | Windows, Linux Server, Android/iOS |
| **Plattform** | IBM-kompatibler PC, ARM, Mainframes | Windows, Linux, macOS |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Verbreitung (Stand Juli 2025 - weltweit):
- Desktop: Windows (~71,88%), macOS (~8,69%), Linux (~3,8%)
- Server: Linux (~80,0%), Windows Server (~20%)
- Mobile: Android (~72,03%), Apple iOS/iPadOS (~27,59%)





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Aufbau eines Betriebssystems
Architektur des Betriebssystems (Schichtenmodell)

Die logische Strukturierung eines OS erfolgt in mehreren Schichten oder Schalen, um Abstraktion und Wartbarkeit zu gewährleisten.

1. **Unterste Schicht (Hardware-Abstraktionsschicht):** Alle hardwareabhängigen Teile (Interrupts, Geräte). Setzt unmittelbar auf der Firmware auf.

2. **Nächste Schicht (Geräteverwaltung):** Grundlegende Ein-/Ausgabe-Dienste für Massenspeicher und Peripherie.

3.  **Darauffolgende Schicht (Dateisystem/Netzwerk):** Behandlung von Kommunikations-, Netzwerkdiensten, Dateien und Dateisystemen.

4. **Höhere Schichten:** Anwendungsschnittstellen (System Calls), Benutzeroberfläche.

Jede Schicht bildet eine abstrakte (virtuelle) Maschine und kommuniziert mit benachbarten Schichten über wohldefinierte Schnittstellen (Protokolle).

```text
                
                            +---------------------------------------+
                            |               Anwendung               |
                            |                       +-----------+   |
                            +-----------------------| Protokoll |---+
                                     /\             +----||-----+
                                +----||-----+            \/
                   +------  +---| Protokoll |-----------------------+   ---------+
    Abstrakte      |        |   +-----------+                       |            |
    (virtuelle) ---+        |  Gegebenenfalls weitere Schicht(en)   |            |
    Maschine       |        |                       +-----------+   |            |
                   +------  +-----------------------| Protokoll |---+            |
                                     /\             +----||-----+                |
                                +----||-----+            \/                      |
                   +------  +---| Protokoll |-----------------------+            |
    Abstrakte      |        |   +-----------+                       |            |
    (virtuelle) ---+        |   Kommunikations- und Netzwerkdienste,|            |
    Maschine       |        |   Dateien und Dateisysteme            |            |
                   |        |                       +-----------+   |            |
                   +------  +-----------------------| Protokoll |---+            |
                                     /\             +----||-----+                |
                                +----||-----+            \/                +----------------+
                   +------  +---| Protokoll |-----------------------+      | Betriebssystem |
    Abstrakte      |        |   +-----------+                       |      +----------------+  
    (virtuelle) ---+        |  Grundlegenden Ein-/ Ausgabe-Dienste  |            |
    Maschine       |        |  für Massenspeicher & Peripheriegeräte|            |
                   |        |                       +-----------+   |            |
                   +------  +-----------------------| Protokoll |---+            |
                                     /\             +----||-----+                |
                                +----||----------+       \/                      |
                                | Virtuelle      |                               |
                   +------  +---| Betriebsmittel |------------------+            |
    Abstrakte      |        |   +----------------+                  |            |
    (virtuelle) ---+        |   Hardwareabhängige Teile des OS,     |            |
    Maschine       |        |   Verarbeitung von Interrupts         |            |
                   |        |                       +-----------+   |            |
                   +------  +-----------------------| Protokoll |---+   ---------+
                                     /\             +----||-----+                          
                                +----||-----------+      \/                                
                            +---| reale Betriebs- |---------------------+    
                            |   | mittel          |                     |
                            |   +-----------------+                     |
                            |              Firmware (UEFI)              |
                            |-------------------------------------------|
                            | Hardware (Mainboard, Geräte & Peripherie) |
                            +-------------------------------------------+
                
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Schichten- und Schalenmodell
```text
     Schichtenmodell             ||               Schalenmodell
+-----------------------+        ||       +-------------------------------+       
|       Benutzer        |        ||       |      Benutzerschnitstelle     |        
+---/\------------||----+        ||       | +---------------------------+ |       
+---||------------\/----+        ||       | |    Anwendungsprogramme    | |       
|  Anwendungsprogramme  |        ||       | | +-----------------------+ | |       
+---/\------------||----+        ||       | | |  Betriebssystemkernel | | |       
+---||------------\/----+        ||       | | |   +--------------+    | | |       
|  Betriebssystemkernel |        ||       | | |   |   Firmware   |    | | |       
+---/\------------||----+        ||       | | |   | +----------+ |    | | |       
+---||------------\/----+        ||       | | |   | | Hardware | |    | | |       
|       Firmware        |        ||       | | |   | +----------+ |    | | |       
+-----------------------+        ||       | | |   +--------------+    | | |       
|  Anwendungsprogramme  |        ||       | | +-----------------------+ | |       
+-----------------------+        ||       | +---------------------------+ |       
                                 ||       +-------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Kernel-Arten im Sicherheitskontext

Der **Kernel** ist der Kern des Betriebssystems, der im privilegierten Kernelmodus (Ring 0) läuft und direkten Zugriff auf die Hardware hat. Die Architektur des Kernels beeinflusst die Stabilität und die Größe der **Trusted Computing Base** (**TCB**) – der Gesamtheit der Komponenten, denen Vertrauen geschenkt werden muss.


| Kernel-Art | Beschreibung | Sicherheit & Stabilität | Beispiele |
|------------|--------------|-------------------------|-----------|
| **Monolithischer Kernel** | Alle Hauptkomponenten (Dateisystem, Treiber, Memory Manager) laufen im Kernelmodus (Ring 0). | **Nachteil:** Absturz einer Komponente führt zum Komplettabsturz. Große Angriffsfläche (große TCB). | UNIX, BSD, einige Linux-Varianten |
| **Microkernel** | Nur minimale Komponenten (Memory Manager, Scheduler, IPC) laufen im Kernelmodus. Andere Dienste (Treiber, Dateisystem) laufen im **Benutzermodus** (Ring 3). | **Vorteil:** Sehr stabil, da Fehler in Modulen im Benutzermodus nicht zum Komplettabsturz führen. Kleine TCB. **Nachteil:** Langsamer aufgrund erhöhter Kommunikation. | Minix, Google Fuchsia |
| **Hybridkernel** | Verbindet Eigenschaften: Wichtige Treiber und IPC laufen im Kernelmodus für Geschwindigkeit; andere Dienste im Benutzermodus. | Ein Kompromiss aus Geschwindigkeit und Stabilität. | Windows NT/XP/10, macOS |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsmechanismen und Zugriffsmodelle
### Ring-Modell (Privilegien-Level)

Das Ring-Modell organisiert die Komponenten nach ihrem Privilegien-Level. Ring 0 (Kernel-Modus) hat die höchsten Rechte, Ring 3 (User-Modus) die niedrigsten. Diese Isolation ist das Fundament der OS-Sicherheit.

- **Ring 0 (Kernelmodus):** Hier läuft der Betriesbssystem-Kernel und hochprivilegierte Treiber. Diese Ebene hat vollen Zugriff auf all Hardware- und Speicherbereiche.

- **Ring 1 und 2:** Diese Stufen sind in modernen Allzwecksystemen oft nicht im Einsatz, können aber für bestimmte Treiber oder in Virtualisierungsumgebungen genutzt werden, die einen Teil des Kernels isolieren.

- **Ring 3 (Benutzermodus):** Diese Ebene ist für normale Anwendungsprogramme vorgesehen. Programme in Ring 3 dürfen nur auf ihren eigenen Speicher zugreifen und können keine gefährlichen oder hardwarenahen Befehle wie Interrupts ein- und ausschalten.

```text
         +------------------------------------+
         | Ring 3 (Benutzermodus)             |
         |------------------------------------|
         |  Anwendungsprogramme, User-Dienste |
         |  (Eingeschränkte Rechte)           |
         +-------------^----------------------+
                       | 
                       | (System Calls/APIs)
                       |
         +-------------v----------------------+
         | Ring 0 (Kernelmodus)               |
         |------------------------------------|
         |  Kernel, Treiber, Hardware-Zugriff |
         |  (Volle Kontrolle über die CPU)    |
         +------------------------------------+
```

- **Ringübergänge:** Ein Prozess kann von einem höheren RAng in einen niedrigeren wechseln, um privilegierte Operationen durchzuführen, muss aber dann wieder in den ursprünglichen, weniger privilegierten Ring zurückkehren.

- **Speicherschutz:** Physischer Speicher wird in virtuelle Seiten aufgeteilt, deren Zugriffsrechte in einer Speichertabelle festgelegt sind, die die MMU (Memory Management Unit) auswertet. 

- **Zweck:** Das Ring-Model dient dem Prinzip der geringsten Privilegien (Principle of Least Privilege), indem es den Zugriff auf Systemressourcen stark einschränkt und so die Stabilität und Sicherheit des Systems gewährleistet. Programme können so nicht unkontrolliert auf Hardware zugreifen oder andere Prozesse manipulieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Trusted Computing Base (TCB)

- **Definition:** Die Menge aller Hardware-, Firmware- und Software-Komponenten in einem Computersystem, die für die Durchsetzung der Sicherheitsrichtlinien zuständig sind.

- **Sicherheitsrelevanz:** Je kleiner die TCB (wie beim Microkernel), desto einfacher ist es, ihre Korrektheit zu prüfen und desto weniger Angriffsfläche bietet das System.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Zugriffskontrollmodelle

Das OS verwaltet, welche Benutzer und Prozesse auf welche Ressourcen (Dateien, Geräte, Speicher) zugreifen dürfen.


| Modell | Beschreibung | Beispiel |
|--------|--------------|----------|
| **Directionary Access Control** (**DAC**) | Der **Eigentümer** einer Ressource kann die Zugriffsrechte (Lesen, Schreiben, Ausführen) nach eigenem Ermessen an andere Benutzer delegieren. | UNIX-Dateirechte (`chmod`), Windows NTFS-Berechtigungen. |
| **Mandatory Access Control** (**MAC**) | Die Zugriffsrechte werden von einer zentralen Autorität (dem OS-Kernel) auf Basis von Sicherheits-Labels (z. B. Vertraulichkeitsstufen) festgelegt und können vom Eigentümer nicht geändert werden. | SELinux, militärische und Hochsicherheitsumgebungen. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Komponenten eines Betriebssystems

| Modell | Beschreibung | Beispiel |
|--------|--------------|----------|
| **Ressourcenmanager** | Kennt alle Ressourcen und Leistungsanforderungen. Weist allen Prozessen die angefragten Ressourcen zu | **Prozessisolation:** Verhindert, dass ein Prozess den Speicher oder die Ressourcen eines anderen Prozesses manipuliert. |
| **Prozessmanager** | Verwaltet alle laufenden Prozesse. Stellt die IPC (Inter Process Communication) zur Verfügung. | Sicherstellung, dass IPC-Kanäle nur zwischen autorisierten Prozessen genutzt werden (z. B. durch Berechtigungsprüfungen). |
| **Benutzermanager** | Verwaltet Benutzerkonten und -rechte (UAC unter Windows, `sudo` unter Linux). | Fundament der **Authentifizierung** und **Autorisierung**. | 
| **Datenmanager** | Verwaltet Massenspeicher und Dateisysteme (z. B. NTFS, ext4). | Durchsetzung von Zugriffsrechten auf Dateiebene (DAC/MAC). |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links
- [Wikipedia: Betriebssystem](https://de.wikipedia.org/wiki/Betriebssystem)
- [Wikipedia: Privilegienstufe](https://de.wikipedia.org/wiki/Privilegienstufe)


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