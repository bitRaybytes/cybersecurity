# ⏲ Arbeitsspeicher (RAM) – Grundlagen, Typen und Kennzahlen

Der **Random Access Memory** (**RAM**) – im Deutschen meist als **Arbeitsspeicher** bezeichnet – ist ein zentraler Bestandteil jeder Computer-Hardware. Er dient als schneller, temporärer Speicher, in dem Programme und Daten liegen, auf die die [**CPU**](/01-basics-intro/hardware_grundlagen/cpu.md) (**Central Processing Unit**) direkt zugreifen kann.

Während Daten auf der **Festplatte** (**HDD**/**SSD**) dauerhaft gespeichert bleiben, ist RAM **flüchtig** – das bedeutet: Beim Herunterfahren des Systems gehen die darin befindlichen Informationen verloren. Genau deshalb spielt RAM eine Schlüsselrolle, wenn es um die **Geschwindigkeit**, **Stabilität** und **Effizienz** eines Systems geht.


## Inhaltsverzeichnis
- [Funktionsweise des RAM](#funktionsweise-des-ram)
- [Speicherkapazität und Kennzahlen](#speicherkapazität-und-kennzahlen)
- [Bauformen: DIMM vs. SO-DIMM](#bauformen-dimm-vs-so-dimm)
- [UDIMM vs. RDIMM](#udimm-vs-rdimm)
- [RAM-Generationen](#ram-generationen)
- [RAM-Technologien im Detail](#ram-technologien-im-detail)
- [Dual-Channel-Technologie](#dual-channel-technologie)
- [Sicherheitsrelevanz von RAM](#sicherheitsrelevanz-von-ram)
- [Zusammenfassung](#zusammenfassung)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Funktionsweise des RAM

Die zentrale Aufgabe des Arbeitsspeichers ist es, als ultraschnelle Zwischenschicht zwischen der CPU und dem deutlich langsameren Massenspeicher (HDD/SSD) zu agieren. Wenn ein Programm oder eine Datei geöffnet wird, wird es von der Festplatte in den RAM geladen. Von dort aus kann die CPU extrem schnell auf die benötigten Daten zugreifen, um die Befehle zu verarbeiten.

```yaml
Ein einfacher Datenfluss sieht wie folgt aus:

CPU  ⇄  RAM  ⇄  HDD/SSD
```
Ein einfacher Datenfluss sieht also so aus, dass Programme von der Festplatte in den RAM geladen werden, damit die CPU dann direkt mit diesen Daten arbeiten kann.  RAM ist deutlich schneller als SSDs oder HDDs, jedoch kleiner in der Gesamtkapazität.

- Programme werden von der Festplatte in den RAM geladen.

- Die CPU kann dann direkt mit diesen Daten arbeiten.

- RAM ist deutlich schneller als SSDs oder HDDs, jedoch kleiner in der Gesamtkapazität.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Speicherkapazität und Kennzahlen

- **Kapazität:** Dies ist die Gesamtmenge an Daten, die im RAM gespeichert werden können, gemessen in Megabyte (MB), Gigabyte (GB) oder Terabyte (TB).

- **Taktfrequenz:** Die Taktfrequenz, gemessen in Megahertz (MHz) oder Gigahertz (GHz), beschreibt die Anzahl der Taktzyklen pro Sekunde. Eine höhere Taktfrequenz bedeutet, dass der RAM schneller Daten lesen und schreiben kann.

- **Bandbreite:** Dies ist die maximale Datenmenge, die pro Sekunde zwischen RAM und CPU übertragen werden kann. Die Bandbreite berechnet sich aus der Taktfrequenz und der Busbreite.
```text
              Taktfrequenz (MT/s) x Busbreite (in Bytes)
Bandbreite = --------------------------------------------       
                                  8
                          
Beispiel: DDR5 mit 4.800 MHz und 64 Bit Busbreite erreicht ca. 38,4 GB/s
```
- **CAS Latency (CL):** Die **Column Address Strobe Latency** ist eine entscheidende Kennzahl, die die Verzögerungszeit zwischen der Anfrage eines Datenpakets und dessen tatsächlicher Bereitstellung beschreibt. Je niedriger dieser Wert, desto schneller die Reaktionszeit des Arbeitsspeichers.

- **Datenbusbreite:** Die Busbreite, typischerweise 32-Bit, 64-Bit oder 128-Bit, gibt an, wie viele Datenbits gleichzeitig übertragen werden können.

- **Spannung (Voltzahl):** Die Betriebsspannung, in der Regel 1,2V–1,35V, hat einen direkten Einfluss auf den Energieverbrauch und die Wärmeentwicklung des Speichermoduls. Niedrigere Spannungen führen zu einem geringeren Energieverbrauch und weniger Wärme.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Bauformen: DIMM vs. SO-DIMM
RAM-Module gibt es in verschiedenen physischen Bauformen, die für unterschiedliche Gerätetypen optimiert sind:

| Typ     | DIMM (Desktop, Server)         | SO-DIMM (Laptop, kompakte Geräte) |
| ------- | ------------------------------ | --------------------------------- |
| Größe   | Standard (lang)                | ca. 67 mm (halb so lang)          |
| Pins    | 184, 240, 288                  | 144, 200, 260                     |
| Einsatz | Mainboards im PC/Server        | Laptops, Notebooks                |
| Vorteil | Leistungsstark, hohe Kapazität | Kompakt, energieeffizient         |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## UDIMM vs. RDIMM
Die Speichermodule werden zusätzlich in ungepufferte (**UDIMM**) und gepufferte (**RDIMM**) Module unterteilt. Der Unterschied liegt in der Art, wie sie mit dem Speichercontroller der CPU kommunizieren, und hat große Auswirkungen auf Stabilität und Einsatzgebiet.

| Typ               | UDIMM (Unbuffered)    | RDIMM (Registered)                 |
| ----------------- | --------------------- | ---------------------------------- |
| Speicher-Technologie| ungepuffert         | gepuffert                          |
| Puffer            | kein Zwischenspeicher | Register-Chip als Zwischenspeicher |
| Geschwindigkeit   | schneller, günstiger  | stabiler, aber etwas langsamer     |
| ECC-Unterstützung | nein                  | ja                                 |
| Einsatzgebiet     | Desktop-PCs           | Server                             |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## RAM-Generationen
Die am häufigsten verwendete RAM-Technologie ist **DDR** (**Double Data Rate**). Der Name leitet sich davon ab, dass sie Daten sowohl an der steigenden als auch an der fallenden Flanke des Taktsignals überträgt. Mit jeder neuen Generation (DDR2, DDR3, DDR4, DDR5) werden die Taktfrequenzen erhöht und die Energieeffizienz weiter verbessert.

- **DDR (Double Data Rate):** Überträgt Daten bei steigender und fallender Taktflanke → doppelte Speicherbandbreite.

- **DDR2, DDR3, DDR4, DDR5:** Jede Generation bringt höhere Taktraten, mehr Bandbreite und bessere Energieeffizienz.

- **DRAM, SDRAM, SRAM:** Unterschiedliche Speichertechnologien mit eigenen Vor- und Nachteilen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## RAM-Technologien im Detail
Es gibt grundlegende Unterschiede in der Art, wie RAM-Zellen Daten speichern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### SRAM (Static RAM)
SRAM speichert Daten in sogenannten Flip-Flop-Schaltungen, die sehr stabil sind und keinen ständigen Refresh benötigen. Dies macht SRAM extrem schnell und energieeffizient, aber auch sehr teuer. Aus diesem Grund wird SRAM hauptsächlich für kleine, extrem schnelle Speicher wie den CPU-Cache oder Puffer verwendet.

- Speicherung in Flip-Flop-Schaltungen (Transistorpaare).
- Kein Refresh notwendig -> sehr schnell, stromsparend, aber teuer.
- Anwendung: Cache, Puffer.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### DRAM (Dynamic RAM)
DRAM speichert Daten in Kondensatoren, die ihre Ladung schnell verlieren. Aus diesem Grund müssen die Daten ständig durch einen "Refresh-Zyklus" aufgefrischt werden, was DRAM langsamer und stromintensiver macht. Dank seiner deutlich höheren Speicherdichte und geringeren Kosten ist DRAM die Standardtechnologie für den Hauptarbeitsspeicher.

- Speicherung in Kondensatoren -> regelmäßiger Refresh notwendig.
- Günstig, aber langsamer und stromintensiver.
- Anwendung: PCs, Laptops, Server.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### SDRAM (Synchronous DRAM)
SDRAM ist eine Weiterentwicklung von DRAM, die sich mit dem CPU-Takt synchronisiert. Dadurch können Daten präziser und effizienter verarbeitet werden.

- Synchronisiert mit dem CPU-Takt -> effizienter als klassisches DRAM.
- Unterstützt Burst-Modus für schnelleren Zugriff.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Dual-Channel-Technologie

Durch den Einsatz von zwei identischen RAM-Riegeln im sogenannten **Dual-Channel-Betrieb** kann die Speicherbandbreite nahezu verdoppelt werden. Dies führt zu einer signifikanten Leistungssteigerung bei Anwendungen, die eine hohe Datenrate benötigen.

**Voraussetzung:** Beide Module sollten in **Taktfrequenz**, **Größe** und **Hersteller** übereinstimmen, um volle Kompatibilität zu gewährleisten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## ECC (Error Correcting Code)
Gerade in **Server-Umgebungen** ist die Stabilität und Korrektheit der Daten entscheidend. **ECC-RAM** kann in der Regel einen **Single-Bit-Fehler** automatisch korrigieren und einen **Multi-Bit-Fehler** zumindest erkennen. Während die Fehlerkorrektur mit Kosten und einer minimalen Leistungseinbuße einhergeht, ist ECC für kritische Systeme, die maximale Datenintegrität benötigen, unerlässlich.

- **Vorteil:** Höhere Zuverlässigkeit.
- **Nachteile:** Teuerer und minimal langsamer.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsrelevanz von RAM
Aus der Sicht eines IT-Sicherheitsexperten ist RAM nicht nur eine Rechenressource, sondern auch ein potenzielles Angriffsziel. Physischer Zugriff auf den Arbeitsspeicher kann es Angreifern ermöglichen, sensible Daten wie Verschlüsselungsschlüssel oder Passwörter zu extrahieren, selbst nachdem das System heruntergefahren wurde (Cold Boot Attack). Zudem können Angreifer über Schnittstellen mit **Direct Memory Access** (**DMA**) den Arbeitsspeicher direkt manipulieren, ohne, dass das Betriebssystem dies bemerkt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Zusammenfassung
RAM ist weit mehr als nur „Speicherplatz“ im PC. Er beeinflusst maßgeblich:

- **Geschwindigkeit** (Taktfrequenz, Bandbreite, Latenz)
- **Stabilität** (UDIMM vs. RDIMM, ECC-Unterstützung)
- **Energieeffizienz** (Spannung, Bauform)

Ein gutes Verständnis von RAM hilft nicht nur bei der Hardware-Auswahl, sondern auch bei der **Optimierung von Performance und Sicherheit** – besonders in professionellen IT-Umgebungen.


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