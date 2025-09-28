# Speicherverwaltung

# Inhaltsverzeichnis
- [Einleitung: Die zentrale Rolle der Speicherverwaltung](#einleitung-die-zentrale-rolle-der-speicherverwaltung)
- [Speicherhierarchie (Speicherpyramide)](#speicherhierarchie-speicherpyramide)
- [Virtuelle Speicherverwaltung](#virtuelle-speicherverwaltung)
  - [Adressräume](#adressräume)
  - [Transformation](#transformation)
- [Auslagerungsspeicher: Paging vs. Swapping](#auslagerungsspeicher-paging-vs-swapping)
  - [Paging](#paging)
  - [Swapping (Komplettverschiebung)](#swapping-komplettverschiebung)
- [Die Speicherstruktur eines Prozesses](#die-speicherstruktur-eines-prozesses)
- [Sicherheitsmechanismen der Speicherverwaltung](#sicherheitsmechanismen-der-speicherverwaltung)
  - [1. Speichersegmentierung und -schutz (Memory Protection)](#1-speichersegmentierung-und--schutz-memory-protection)
  - [2. Data Execution Prevention (DEP / NX-Bit)](#2-data-execution-prevention-dep--nx-bit)
  - [3. Address Space Layout Randomization (ASLR)](#3-address-space-layout-randomization-aslr)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung: Die zentrale Rolle der Speicherverwaltung

Die Speicherverwaltung ist eine der zentralen Aufgaben eines jeden Betriebssystems (OS) und ein unentbehrliches Betriebsmittel für die Programmausführung.

**Kernaufgaben der Speicherverwaltung:**

1. **Zuweisung:** Zuweisung von Speicher an Tasks (Programm- und Datenspeicher).

2. **Buchführung:** Verwaltung freier und belegter Speicherbereiche.

3. **Schutz:** Bereitstellung von Mechanismen zum Schutz vor unbefugten Speicherzugriffen aus anderen Programmen (Prozessisolation).

4. **Effizienz:** Geschickte Nutzung der Speicherhierarchie (Caching, Auslagerung).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Speicherhierarchie (Speicherpyramide)

Der ideale Speicher (extrem kostengünstig, unendlich groß, unendlich schnell und nonvolatil) ist irreal. Die reale Speicherverwaltung versucht, durch eine Hierarchie einen Kompromiss aus Geschwindigkeit, Kosten und Persistenz zu finden.

**Prinzip:** Je näher an der CPU, desto schneller, kleiner und teurer der Speicher.

| Speichertyp | Größenordnung | Zugriffszeit | Volatilität | Speicherkapazität |
|-------------|---------------|--------------|-------------|-----------|
| **CPU-Register** | Byte | Prozessortakt (sehr schnell) | volatil | verschwindend klein |
| **Prozessor-Caches** (L1, L2, L3) | KiB bis MiB | Einige Dutzend Taktzyklen | volatil | minimal |
| **Arbeitsspeicher** (DDR-SDRAM) | MiB bis GiB | Hunderte Taktzyklen | volatil | klein |
| **Sekundärspeicher** (SSD/HDD) | GiB bis TiB | Tausende Taktzyklen (Block-Zugriff) | non-volatil | groß bis sehr groß |
| **Tertiärspeicher** | TiB bis PiB | Sehr langsam (Bsp.: DVD, CD, Streamer, Magnetband, Archivsysteme) | non-volatil | variiert je nach Medium stark |


```text   
+--------------------------------------------------------------+
| Merke: Je höher in der Hierarchie, desto schneller,          |
| aber auch teurer ist der zur Verfügung stehende Speichertyp. |
+--------------------------------------------------------------+

schnell
   ^             /\
   |            /__\---------- CPU-Register
   |           /____\--------- Prozessor-Caches
   |          /______\-------- Arbeitsspeicher (RAM)
   |         /________\------- Sekundärspeicher Halbleiter
   |        /__________\------ Sekundärspeicher magnetisch
   v       /____________\----- Tertiärspeicher
langsamer
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Virtuelle Speicherverwaltung

Die meisten Programme sind zu groß für den verfügbaren Hauptspeicher. Die virtuelle Speicherverwaltung (VSV, konzipiert von Fotheringham, 1961) löst dieses Problem.

**Grundidee:** Erlaubt, dass der Programmcode, die Daten und der Stack größer sind als der verfügbare physische Hauptspeicher.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Adressräume

- **Logischer Adressraum (Virtueller Adressraum):** Der gedachte Speicheradressraum, den die Programmbefehle referenzieren. Er bleibt nach dem Laden in den Speicher unverändert und ist in der Regel größer als der physische Adressraum.

- **Physischer Adressraum:** Der reale Adressraum des Arbeitsspeichers (RAM).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Transformation
Zur Abarbeitung der Befehle ist eine **dynamische Adresstransformation** (**DAT**) erforderlich, die die logische Adresse in eine physische Adresse umwandelt. Diese Aufgabe übernimmt die **Memory Management Unit** (**MMU**), die heute meist auf dem CPU-Chip untergebracht ist.

Die VSV ist für den Anwender vollkommen transparent.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Auslagerungsspeicher: Paging vs. Swapping

**Auslagerungsspeicher** (Swap Space) erweitert die Arbeitsspeicherkapazität unter Zuhilfenahme externer Massenspeicher. Das OS entscheidet, welche Programmabschnitte geladen werden und welche ausgelagert werden.

### Paging

- **Grundidee:** Der virtuelle Adressraum wird in gleich große Stücke (**Seiten**, engl. pages) unterteilt. Auch der physische Adressraum wird in gleich große Stücke (**Seitenrahmen**, engl. page frames) unterteilt.

- **Funktion:** Durch Paging kann der Hauptspeicher Seiten verwenden, die sich auf einem sekundären Speichergerät befinden.

- **Mechanismus:** Die Seitentabelle prüft, ob sich eine Seite im Hauptspeicher befindet. Bei einem Seitenfehler (Page Fault) wird die benötigte Seite vom sekundären Speicher in einen leeren Seitenrahmen im Hauptspeicher eingelagert.

- **Vorteil:** Flexibel, da nur einzelne Seiten verschoben werden. Ermöglicht es, mehr Prozesse gleichzeitig im Hauptspeicher zu halten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Swapping (Komplettverschiebung)

- **Grundidee:** Es werden alle zu einem Prozess gehörenden Segmente (der gesamte Prozessdatenspeicher) zwischen Hauptspeicher und einem Swap-Bereich auf dem sekundären Speichergerät verschoben.

- **Vorteil:** Eher geeignet bei sehr hohen Workloads, da die Verwaltung einfacher ist.

- **Nachteil:** Erzeugt höhere Arbeitslasten und ist weniger flexibel, da immer der komplette Speicherbereich verschoben wird.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die Speicherstruktur eines Prozesses

Für die IT-Sicherheit ist die Aufteilung des virtuellen Adressraums eines Prozesses von größter Bedeutung, da dies die Angriffsziele wie [Buffer-Overflows](/04-host-security/angriffe/buffer_overflow.md) definiert.

```text
      Hohe Adressen (oben)
+-----------------------------+ 
| Stack (Wächst nach unten)   | <- Lokale Variablen, Rücksprungadressen (Ziel von Stack Overflows)
|-----------------------------| 
| Veralteter Stack            | 
|-----------------------------| 
| Heap (Wächst nach oben)     | <- Dynamisch allokierter Speicher (malloc, new) (Ziel von Heap Overflows)
|-----------------------------|
| BSS (Uninitialisierte Daten)| <- Statische Variablen
|-----------------------------|
| Data (Initialisierte Daten) | <- Globale und statische Variablen
|-----------------------------|
| Text/Code                   | <- Programmanweisungen (Maschinencode)
+-----------------------------+ 
   Niedrige Adressen (unten)
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsmechanismen der Speicherverwaltung

Das Betriebssystem und die Hardware nutzen spezielle Mechanismen, um die Integrität und Isolation der Prozessspeicher zu gewährleisten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. Speichersegmentierung und -schutz (Memory Protection)

Die MMU setzt Zugriffsrechte (Lesen, Schreiben, Ausführen) für jede Speicherseite durch.

- Jeder Prozess erhält einen isolierten Adressraum. Ein Zugriff auf Speicher außerhalb des eigenen Adressraums führt zu einem Segmentierungsfehler (Segmentation Fault) oder einem Speicherschutzfehler (Protection Fault), was das Programm beendet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Data Execution Prevention (DEP / NX-Bit)

- **Ziel:** Verhindert, dass bösartiger Code ausgeführt wird, der in einen Datensegment (wie Stack oder Heap) eingeschleust wurde.

- **Funktion:** Das **Non-Execute Bit** (**NX-Bit**) der CPU markiert Speicherseiten als nicht ausführbar. Wenn ein Programm versucht, Code aus einem solchen Bereich auszuführen, wird die Ausführung blockiert.

- **Verteidigung:** Primäre Abwehr gegen den klassischen **Stack-Overflow-Angriff** (**Code Injection**).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. Address Space Layout Randomization (ASLR)

- **Ziel:** Erschwert Angreifern das Auffinden kritischer Speicheradressen.

- **Funktion:** Bei jedem Start eines Programms werden die Startadressen wichtiger Speicherbereiche (Stack, Heap, Bibliotheken wie libc) zufällig im virtuellen Adressraum verschoben.

- **Verteidigung:** Macht es extrem schwierig, die Rücksprungadresse für Return-Oriented Programming (ROP) oder Return-to-libc Angriffe präzise zu treffen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links

- [Wikipedia: Speicherverwaltung](https://de.wikipedia.org/wiki/Speicherverwaltung)
- [OWASP: Buffer Overflow Prevention (Bezieht sich stark auf die Speicherstruktur)](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
- [BSI: IT-Grundschutz (Abschnitt SYS.2.2 Server-Betriebssystem)](https://www.google.com/search?q=BSI%3A+IT-Grundschutz+%28Abschnitt+SYS.2.2+Server-Betriebssystem%29&sca_esv=1ab80d3e8e62071f&hl=de&sxsrf=AE3TifMAONqx4kaKbRPgYX9b29aoYZDM6A%3A1759059052250&source=hp&ei=bBzZaOCSDY-L7NYPptSOkA4&iflsig=AOw8s4IAAAAAaNkqfDheVKtaiQEhSCYAfC2-4goRMKdA&ved=0ahUKEwjg-MjPrfuPAxWPBdsEHSaqA-IQ4dUDCBk&uact=5&oq=BSI%3A+IT-Grundschutz+%28Abschnitt+SYS.2.2+Server-Betriebssystem%29&gs_lp=Egdnd3Mtd2l6Ij1CU0k6IElULUdydW5kc2NodXR6IChBYnNjaG5pdHQgU1lTLjIuMiBTZXJ2ZXItQmV0cmllYnNzeXN0ZW0pSMoRUABYAHAAeACQAQCYAWSgAWSqAQMwLjG4AQPIAQD4AQL4AQGYAgCgAgCYAwCSBwCgB3GyBwC4BwDCBwDIBwA&sclient=gws-wiz)



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