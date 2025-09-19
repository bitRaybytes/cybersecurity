# 💾 Nicht-flüchtiger Speicher – Grundlagen und Technologien

Im Gegensatz zum Arbeitsspeicher (RAM), der seine Daten nur so lange speichert, wie Strom anliegt, bezeichnet man als **nicht-flüchtigen Speicher** alle Speicherarten, die auch **ohne Energiezufuhr** Informationen dauerhaft behalten können.  
Diese Speicher sind ein essenzieller Bestandteil der Computerarchitektur, da sie Programme, Firmware und Daten langfristig sichern. Sie bilden die Grundlage für BIOS/UEFI, Betriebssysteme, USB-Sticks, SSDs und viele andere Geräte, die wir täglich verwenden.

## Inhaltsverzeichnis
- [Arten von nicht-flüchtigem Speicher](#arten-von-nicht-flüchtigem-speicher)
- [ROM (Read Only Memory)](#rom-read-only-memory)
- [PROM (Programmable ROM)](#prom-programmable-rom)
- [EPROM (Erasable Programmable ROM)](#eprom-erasable-programmable-rom)
- [EEPROM (Electrically Erasable Programmable ROM)](#eeprom-electrically-erasable-programmable-rom)
- [Flash-Speicher](#flash-speicher)
- [NAND- und NOR-Technik](#nand--und-nor-technik)
- [Einschränkungen von Flash-Speicher](#einschränkungen-von-flash-speicher)
- [Fazit](#fazit)
- [Haftungsaussschluss](#haftun)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Arten von nicht-flüchtigem Speicher

```text
Nicht-flüchtiger Speicher
│
├── ROM (Read Only Memory)
│   ├── PROM (Programmable ROM)
│   ├── EPROM (Erasable PROM, UV-Licht löschbar)
│   └── EEPROM (Electrically Erasable PROM)
│        └── Flash-Speicher
│             ├── NAND-Flash (seriell, blockweise)
│             └── NOR-Flash (parallel, direkt zugreifbar)

```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## ROM (Read Only Memory)

Der klassische **ROM** ist ein nur lesbarer Speicher, der bereits bei der **Herstellung programmiert** wird.
Er enthält grundlegende Anweisungen, die das System zum Starten benötigt, beispielsweise das **BIOS**.  
Einmal gespeicherte Daten können in dieser Form nicht mehr verändert oder gelöscht werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## PROM (Programmable ROM)

**PROM** steht für **P**rogrammable **R**ead **O**nly **M**emory. Im Gegensatz zum ROM wird er als leerer Chip ausgeliefert und kann dann einmalig mit einem speziellen Programmiergerät beschrieben werden. Nach dem ersten Beschreiben ist eine Änderung der Daten nicht mehr möglich. Prominentestes Beispiel für den Einsatz ist die Speicherung von Firmware in der Hardware oder von spezifischen Daten.

- **PROM** steht für **P**rogrammable **R**ead **O**nly **M**emory.

- Er wird einmalig programmierbar ausgeliefert.
- Nach dem ersten Beschreiben kann er nicht mehr verändert werden.
- Typischer Einsatz: Speicherung von Firmware oder Hardware-spezifischen Daten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## EPROM (Erasable Programmable ROM)
Als Weiterentwicklung des PROM wurde der **EPROM** eingeführt. Dieser ist nicht nur programmierbar, sondern kann auch gelöscht und wiederbeschrieben werden. Die Löschung erfolgt über **ultraviolettes Licht**, das durch ein kleines, meist sichtbares Fenster im Chip-Gehäuse auf den Speicher wirkt. Für die Löschung und das erneute Beschreiben ist ein spezielles Gerät erforderlich, was den Prozess umständlich macht. Aus diesem Grund wird EPROM heute kaum noch verwendet.

- **EPROM** ist lösch- und wiederbeschreibbar.

- Die Löschung erfolgt über **UV-Licht**, das durch ein kleines Fenster im Chip-Gehäuse auf den Speicher wirkt.

- Zum erneuten Beschreiben benötigt man ein spezielles Gerät (Programmiergerät).

- Heute wird EPROM kaum noch verwendet, da modernere Technologien verfügbar sind.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## EEPROM (Electrically Erasable Programmable ROM)
Das **EEPROM** ist eine bedeutende Weiterentwicklung des EPROM. Die Daten können hier **elektrisch gelöscht und geändert** werden, und zwar sogar auf Bit-Ebene. Dieser Vorgang wird oft als **„Flashen“** bezeichnet. EEPROM ist langlebiger und flexibler als EPROM und wird heute noch in modernen Anwendungen wie BIOS-Chips oder Mikrocontrollern eingesetzt.

- Weiterentwicklung des EPROM.

- Daten können **elektrisch gelöscht und geändert** werden, sogar auf **Bit-Ebene**.

- Dieser Vorgang wird oft als **„Flashen“** bezeichnet.

- EEPROM ist langlebiger und flexibler als EPROM.

- Moderne Anwendungen reichen von **BIOS-Chips** bis hin zu Mikrocontrollern.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Flash-Speicher

Flash ist eine Variante des EEPROM und heute die wohl wichtigste Form nicht-flüchtigen Speichers.
Dazu gehören **USB-Sticks**, **SD-Karten** und **SSDs**, die wir im Alltag ständig nutzen.
Flash-Speicher sind einfach wiederbeschreibbar, haben aber eine **begrenzte Lebensdauer** , da jede Speicherzelle nur eine bestimmte Anzahl von Löschzyklen überstehen kann.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## NAND- und NOR-Technik

Innerhalb des Flash-Speichers gibt es zwei dominierende Technologien:  **NAND-Flash** und **NOR-Flash**. Sie unterscheiden sich in ihrer Schaltungsweise, was zu unterschiedlichen Stärken und Schwächen führt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### NAND-Flash
NAND-Flash-Speicherzellen sind in **Reihenschaltung** angeordnet, ähnlich einem NAND-Gatter. Diese Architektur ist sehr kostengünstig und platzsparend, was sie ideal für die Speicherung **großer Datenmengen** macht. Typische Anwendungen sind SSDs, USB-Sticks und Speicherkarten. Der Nachteil ist, dass kein direkter Zugriff auf einzelne Speicherzellen möglich ist. Daten werden stattdessen in Blöcken gelesen und geschrieben.

- Speicherzellen werden in **Reihenschaltung** angeordnet (ähnlich einem NAND-Gatter).

- Günstig und sehr platzsparend -> ideal für **große Datenmengen**.
- Typische Anwendungen: **SSDs**, **USB-Sticks**, **Speicherkarten**.
- Nachteil: kein direkter Zugriff auf einzelne Speicherzellen, Daten werden blockweise gelesen/geschrieben.
- Lebensdauer: bis zu **2 Millionen Löschzyklen**.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### NOR-Flash
NOR-Flash-Speicherzellen sind **parallel verschaltet**, was einen **direkten und schnellen Zugriff** auf jede einzelne Speicherzelle ermöglicht. Dies macht NOR-Flash ideal für die Speicherung von Firmware, die direkt vom Speicher ausgeführt werden muss. Typische Anwendungen sind BIOS-Chips oder Firmware in Mikrocontrollern. NOR-Flash hat kürzere Zugriffszeiten, ist jedoch weniger speicherdicht und teurer als NAND-Flash.

- Speicherzellen sind **parallel verschaltet** (ähnlich einem NOR-Gatter).

- Ermöglicht **direkten und schnellen Zugriff** auf einzelne Speicherzellen.
- Typische Anwendungen: **Firmware-Speicher** in Mikrocontrollern.
- Kürzere Zugriffszeiten als NAND, aber weniger speicherdicht.
- Lebensdauer: ca. **10.000 – 100.000 Löschzyklen**.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einschränkungen von Flash-Speicher
Trotz ihrer weiten Verbreitung haben Flash-Speicher auch Einschränkungen, die besonders im Kontext der Datensicherheit beachtet werden müssen:

- **Begrenzte Lebensdauer:** Jeder Speicherchip hat eine maximale Anzahl an Lösch- und Schreibzyklen. Mit zunehmender Abnutzung steigt die Fehleranfälligkeit.

- **Retention:** Auch ohne Nutzung kann die Ladung der Speicherzellen über die Jahre nachlassen, was zu Datenverlust führen kann.

- **Ungleichgewicht von Lese- und Schreibgeschwindigkeit:** Schreiben ist in der Regel deutlich langsamer.

- **Fehlerbehandlung:** Mit zunehmender Nutzung werden Fehlerkorrekturmechanismen (**ECC**) wichtiger. Spürbare Verlangsamung kann ein Hinweis sein, dass der Speicher bald ersetzt werden sollte.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsrelevanz

Für IT-Sicherheitsexperten ist das Verständnis von nicht-flüchtigem Speicher entscheidend. Firmware-Angriffe auf BIOS-Chips oder Mikrocontroller können Rootkits installieren, die selbst nach einem Neustart bestehen bleiben. Zudem können manipulierte USB-Sticks, die Flash-Speicher nutzen, eine Bedrohung darstellen. Ein weiterer wichtiger Aspekt ist das **sichere Löschen** von Daten. Ein einfaches Löschen der Partition reicht bei SSDs oft nicht aus, da Datenreste in der **Wear-Leveling-Schicht** verbleiben können.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fazit

Nicht-flüchtige Speicher sind unverzichtbar für die IT-Welt.
Von **ROM-Chips**, die grundlegende Startinformationen enthalten, bis zu hochperformanten **NAND-Flash-Speichern** in SSDs: Jede Technologie hat ihre Stärken und Schwächen.
Besonders in der IT-Sicherheit spielt das Wissen um diese Speicherarten eine Rolle – etwa beim Firmware-Hacking, BIOS-Forensik oder beim sicheren Löschen von Daten.

Wer die Grundlagen versteht, kann nicht nur Hardware besser einordnen, sondern auch Angriffs- und Schutzszenarien fundierter analysieren.



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