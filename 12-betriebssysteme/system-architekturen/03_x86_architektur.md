# 🧠 x86-Architektur: Grundlagen für die Cybersicherheit

## Inhaltsverzeichnis
- [Einleitung: Relevanz für die Cybersicherheit](#einleitung-relevanz-für-die-cybersicherheit)
- [Architektur-Grundlagen](#architektur-grundlagen)
- [Der Stack und die Registersätze](#der-stack-und-die-registersätze)
- [Der evolutionäre Speicheraufbau (Memory Layout)](#der-evolutionäre-speicheraufbau-memory-layout)
- [Sicherheitsrelevante Konsequenzen](#sicherheitsrelevante-konsequenzen)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung: Relevanz für die Cybersicherheit
Die **x86-Architektur** (und ihre 64-Bit-Erweiterung **x64** oder **AMD64**) ist die dominante Befehlssatzarchitektur (ISA) in Desktop-PCs und Servern. Für die Cybersicherheit ist sie die Grundlage für:
- **Exploit-Entwicklung:** Verständnis, wie man den **Instruction Pointer (EIP/RIP)** kontrolliert.
- **Malware-Analyse:** Reverse Engineering von Binärdateien.
- **Kernel-Sicherheit:** Analyse von Ring 0-Operationen.

Das Verständnis der Register und des Speicheraufbaus ist notwendig, um die Funktionsweise von Angriffen wie **Buffer Overflows** und Abwehrmechanismen wie **DEP (Data Execution Prevention)** oder **ASLR (Address Space Layout Randomization)** zu verstehen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Architektur-Grundlagen
Die x86-Architektur ist ein **CISC (Complex Instruction Set Computer)**-Design. Sie definiert, wie die CPU mit dem Speicher und den Registern interagiert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Die zentrale Komponente: Die CPU
Die CPU führt Befehle aus, die aus dem Speicher gelesen werden. Diese Befehle manipulieren Daten, die sich entweder im Speicher (RAM) oder in den **Registern** befinden.

| Komponente | Beschreibung | Sicherheitsrelevanz |
| :--- | :--- | :--- |
| **Befehlssatz (Instruction Set)** | Die Menge aller Befehle (Opcodes), die die CPU ausführen kann (z. B. `MOV`, `JMP`, `CALL`). | Code-Injektion und Shellcode basieren auf diesen Befehlen. |
| **Register** | Kleine, extrem schnelle Speichereinheiten direkt in der CPU zur temporären Speicherung von Daten und Adressen. | Kontrolliert den Programmfluss (EIP/RIP) und die Funktion (Argumente in RAX/EAX). |
| **MMU (Memory Management Unit)** | Verwaltet die Übersetzung von virtuellen Adressen in physische Adressen. | Setzt den **Speicherschutz (DEP/NX)** durch. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der Stack und die Registersätze
### Was sind Register? (Die "Variablen" der CPU)

Register sind die schnellsten Speichereinheiten der CPU, direkt im Prozessorkern. Man kann sie sich als die Arbeitsvariablen oder temporären Speicherplätze der CPU vorstellen. Im Gegensatz zum Hauptspeicher (RAM) sind Register nur wenige Takte entfernt.

- **Verwendung als Variablen:** Register dienen als blitzschnelle Zwischenspeicher, um die Ergebnisse von Berechnungen (Arithmetik) zu halten, bevor sie zurück in den RAM oder Stack geschrieben werden.

- **Verwendung als Pointer:** Sie speichern Speicheradressen (Pointer), die auf Daten im RAM zeigen. Die wichtigsten Pointer sind der Instruction Pointer und der Stack Pointer, welche den Programmfluss steuern.

### Das Prinzip des Stacks

Der Stack ist ein kritischer Speicherbereich, der nach dem **LIFO** (**Last-In**, **First-Out**)-Prinzip arbeitet. Er wird verwendet für:

- Speicherung lokaler Variablen.

- Übergabe von Funktionsargumenten.

- Speicherung der **Rücksprungadresse** (Return Address), wohin das Programm nach Beendigung einer Funktion zurückkehren soll.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Register: Steuerung und Datenfluss

Register sind essenziell, da sie den Zustand und den Fluss des Programms steuern. Ein Verständnis ihrer Rolle ist der Schlüssel zur Analyse von Assembler-Code.

| Register (32-Bit) | Register (64-Bit) | Funktion und Sicherheitselevanz |
| :--- | :--- | :--- |
| **EIP** | **RIP** | **Instruction Pointer:** Enthält die Adresse des nächsten auszuführenden Befehls. Das Hauptziel von Buffer Overflows, da das Überschreiben dieses Pointers den Programmfluss steuert. |
| **ESP** | **RSP** | **Stack Pointer:** Zeigt immer auf die **aktuelle Spitze** des Stacks (die zuletzt gespeicherte Adresse). Dient zur Verwaltung des Speichers beim Funktionsaufruf und -ende |
| **EBP** | **RBP** | **Base Pointer:** Zeigt auf den Anfang des aktuellen Stack-Frames. Hilft bei der Adressierung lokaler Variablen und Parameter innerhalb einer Funktion. |
| **EAX** | **RAX** | **Accumulator/Datenregister:** Wird primär für arithmetische Operationen verwendet. Enthält meist den **Rückgabewert** einer Funktion. |
| **ECX** | **RCX** | **Counter/Datenregister:** Oft als Zähler für Schleifen (`LOOP`) verwendet. Im 64-Bit-Modus: Speichert das erste Funktionsargument. |
| **EDX** | **RDX** | **Datenregister:** Dient der Datenspeicherung. Im 64-Bit-Modus: Speichert das zweite Funktionsargument. |
| N/A | **R8**, **R9** | **Datenregister (nur 64-Bit):** Werden primär verwendet, um das dritte und vierte Funktionsargument zu speichern. |



**ASCII-Grafik: x86 Register-Hierarchie (64-Bit zu 8-Bit):**
```text
|-------------------------------------- qword (64-Bit) ------------------------|
|                       |------- dword (32-Bit) -------------------------------|
|                                                          |--- word (16byte)--|
|                                                          |  byte(8) | byte(8)|
+---------+----------------------------+-------------------+----------+--------+----------------------------------------+
| 64-Bit  | 32-Bit (E-Prefix)          | 16-Bit (X-Suffix) | Hohe     | Nied.  | Rolle                                  |
| Register| (Erweitert)                | (Original)        | 8-Bit(H) | 8-Bit  |                                        |
+=========+============================+===================+==========+=======+=========================================+
| RAX     | EAX                        | AX                | AH        | AL    | Akkumulator (Rückgabewert)             |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RBX     | EBX                        | BX                | BH        | BL    | Basis-Register (Speicher-Basis)        |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RCX     | ECX                        | CX                | CH        | CL    | Zähl-Register (Schleifen / Argument 1) |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RDX     | EDX                        | DX                | DH        | DL    | Daten-Register (Argument 2)            |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RSI     | ESI                        | SI                | (N/A)     | (N/A) | Source-Index (Quell-Pointer)           |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RDI     | EDI                        | DI                | (N/A)     | (N/A) | Destination-Index (Ziel-Pointer)       |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RSP     | ESP                        | SP                | (N/A)     | (N/A) | Stack-Pointer                          |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| RBP     | EBP                        | BP                | (N/A)     | (N/A) | Base-Pointer (Frame-Basis)             |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
| R8-R15  | R8D-R15D                   | R8W-R15W          | (N/A)     | (N/A) | Allgemeine Register (Argument 3-6)     |
+---------+----------------------------+-------------------+-----------+-------+----------------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der evolutionäre Speicheraufbau (Memory Layout)
Die Größe des adressierbaren Speichers hat sich mit der Entwicklung der Architektur dramatisch verändert. Der Sprung von 16-Bit zu 32-Bit zu 64-Bit hat direkte Auswirkungen auf die Komplexität und die Verteidigungsstrategien.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 16-Bit (Real Mode, DOS)
* **Adressraum:** $2^{16} \text{ Bit} = 64 \text{ KiB}$ pro Segment. (Gesamt: $1 \text{ MiB}$ mit Segmentierung).
* **Konsequenz:** Der Adressraum ist extrem begrenzt. Kein Speicherschutz vorhanden.
* **Sicherheitsrelevanz:** Kaum relevant für moderne Systeme, aber historisch wichtig für die Grundlagen der Segmentierung.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 32-Bit (x86, IA-32)
* **Adressraum:** $2^{32} \text{ Bit} = 4 \text{ GiB}$.
* **Aufteilung (Standard):** Meist $2 \text{ GiB}$ für den Benutzer-Space und $2 \text{ GiB}$ für den Kernel-Space (kann variieren, z. B. $3 \text{ GiB}$ zu $1 \text{ GiB}$).
* **Konsequenz:** Der Stack Pointer (`ESP`) und der Instruction Pointer (`EIP`) sind 32 Bit breit. Ein Exploit muss eine 32-Bit-Adresse bereitstellen, um den Programmfluss umzulenken.

### 64-Bit (x64, AMD64)
* **Adressraum (Theoretisch):** $2^{64} \text{ Bit}$.
* **Adressraum (Praktisch):** Derzeit nutzen CPUs nur 48 Bit oder 52 Bit (entspricht 256 TiB bis 4 PiB), was als **Canonical Form** bezeichnet wird.
* **Aufteilung:** Die Adressräume für Benutzer-Space und Kernel-Space sind durch die obere Hälfte der 64 Bit getrennt.
* **Konsequenz:**
    * **Größere Adressen:** Exploit-Entwickler müssen 64-Bit-Adressen finden, was durch **ASLR** (Address Space Layout Randomization) stark erschwert wird.
    * **Register:** Die Anzahl der Register (R8 bis R15) wurde erweitert, und die Art der Funktionsübergabe wurde standardisiert (Argumente werden nun in Registern übergeben, nicht mehr primär auf dem Stack).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### ASCII-Grafik: Die Evolution des Adressraums

```text
+-----------------------+ +--------------------------+  +-------------------------------------------------+
|  16-Bit (Historisch)  | |  32-Bit (x86)            |  |  64-Bit (x64)                                   |
|                       | |                          |  |                                                 |
|  ~1 MB Gesamt (Segm.) | |  4 GB Gesamt             |  |  128 TB bis 4 PB (Canonical Address Space)      |
|                       | | +----------------------+ |  | +---------------------------------------------+ |
|   Code/Data/Stack     | | | Kernel-Space (2 GB)  | |  | | High Half (Kernel Space)                    | |
|   sehr limitiert      | | +---------+------------+ |  | +---------------------------------------------+ |
|                       | | | Benutzer-Space (2 GB)| |  | | Unused/Reserved                             | |
+-----------------------+ | +----------------------+ |  | +---------------------------------------------+ |
                          +--------------------------+  | | Low Half (User Space)                       | |
                                                        | +---------------------------------------------+ |
                                                        +-------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsrelevante Konsequenzen

### Buffer Overflows (BOP)

- **Angriff:** Ein Angreifer schreibt mehr Daten in einen Stack-Puffer, als dieser aufnehmen kann, um die benachbarte **Rücksprungadresse** (**EIP**/**RIP**) zu überschreiben.

- **Ziel:** Das Programm dazu zwingen, den Code des Angreifers (Shellcode) auszuführen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Verteidigung (DEP / NX-Bit)

- **Prinzip:** Die MMU kann Speicherseiten als nicht ausführbar (**NX**, **No-eXecute**) markieren.

- **Effekt:** Wenn ein Angreifer Code in den Stack (der als Datensegment markiert ist) injiziert und versucht, diesen auszuführen, löst die CPU eine Schutzverletzung aus.

- **Konsequenz:** Dadurch sind klassische Code-Injection-Angriffe nicht mehr direkt möglich. Dies führte zur Entwicklung von **Return-Oriented Programming** (**ROP**).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Return-Oriented Programming (ROP)

- **Technik:** Wenn DEP aktiv ist, kann der Angreifer keinen neuen Code ausführen. Stattdessen wird die überschriebene Rücksprungadresse auf eine Kette kleiner, bereits existierender Code-Snippets in den geladenen Bibliotheken (sog. Gadgets) umgelenkt.

- **Ziel:** Durch geschicktes Aneinanderreihen dieser Gadgets kann der Angreifer trotzdem seine schädliche Funktion ausführen, z. B. den Speicherschutz (DEP) selbst deaktivieren.


## Nützliche Links

- [WikiBooks - x86 Assembly/X86 Architecture (english)](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture)
- [Wikipedia: x86-Architektur](https://de.wikipedia.org/wiki/X86-Architektur)
- [Microsoft: x64 Calling Convention (english)](https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention?view=msvc-170)

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
