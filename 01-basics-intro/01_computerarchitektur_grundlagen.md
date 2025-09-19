# 💻 Grundlagen der Computerarchitektur
## Inhaltsverzeichnis
- [Einleitung: Der Blick unter die Haube](#einleitung-der-blick-unter-die-haube)
- [Das EVA-Prinzip](#das-eva-prinzip)
- [Die Von-Neumann-Architektur](#die-von-neumann-architektur)
    - [Was ist so revolutionär daran?](#was-ist-so-revolutionär-daran)
- [Der Fetch-Decode-Execute-Zyklus (FDEC)](#der-fetch-decode-execute-zyklus-fdec)
- [Die CPU und ihre Komponenten](#die-cpu-und-ihre-komponenten)
    - [Aufbau einer CPU](#aufbau-einer-cpu)
    - [CPU Komponenten im Detail](#cpu-komponenten-im-detail)
        - [Steuerwerk (Control Unit)](#steuerwerk-control-unit)
        - [Rechenwerk (Processing Unit)](#rechenwerk-processing-unit)
        - [Data und Cache Unit](#data-und-cache-unit)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung: Der Blick unter die Haube
Um Cyber-Sicherheit auf einem professionellen Niveau zu betreiben, ist es unerlässlich, die fundamentalen Bausteine eines Computers zu verstehen. Dieser Guide erklärt die **grundlegenden Prinzipien der Datenverarbeitung** und die **Architektur der Hauptprozessoren (CPUs)**, die das Herzstück jedes Systems bilden. Schwachstellen auf dieser fundamentalen Ebene können die gesamte Sicherheit eines Systems gefährden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das EVA-Prinzip
Das **EVA-Prinzip** (**E**ingabe-**V**erarbeitung-**A**usgabe), auch bekannt als IPO-Modell, ist ein grundlegendes Modell der Datenverarbeitung. Es beschreibt, wie Daten durch ein Computersystem fließen.

- **Eingabe (Input):** Daten werden über Eingabegeräte (z. B. Tastatur, Maus) erfasst.

- **Verarbeitung (Process):** Die Daten werden durch die CPU bearbeitet und umgewandelt.

- **Ausgabe (Output):** Die verarbeiteten Daten werden über Ausgabegeräte (z. B. Monitor, Drucker) dargestellt.

Dieses einfache Prinzip gilt für jeden Computer, von der einfachen Fernbedienung bis zum Supercomputer.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Die Von-Neumann-Architektur
Die **Von-Neumann-Architektur**, benannt nach dem Mathematiker John von Neumann, ist das **Referenzmodell**, auf dem die meisten modernen Computer basieren. Sie war revolutionär, weil sie erstmals ein programmierbares System ermöglichte, dessen Hardware nicht für jedes Problem neu gebaut werden musste.

**Das Grundprinzip:** Befehle und Daten werden im selben Speicher abgelegt und über denselben Bus transportiert.

**Die Sicherheitslücke:** Die fehlende Trennung von Code und Daten ist die grundlegende Schwachstelle der Von-Neumann-Architektur. Sie ermöglicht es, dass bösartige Daten (z. B. aus einem [Pufferüberlauf](/04-host-security/angriffe/buffer_overflow.md)) als ausführbarer Code interpretiert werden können, um die Kontrolle über das System zu übernehmen.

```text
       +-----------------------------------------+
       |           Zentraleinheit                |
       |   +-------------+  +------------------+ |
       |   |  Steuerwerk |  | Rechenwerk (ALU) | |
       |   +------++-----+  +-------++---------+ |
       |          ||                ||           |
       +----------||----------------||-----------+
       +----------||----------------||-----------+
       |               Bus-System                |
       +------------------++---------------------+
                          ||
              +-----------++------------+
              |                         |
              V                         V
    +----+----+--------+          +--------------+
    | Ein-/Ausgabewerk |<-------->| Speicherwerk |
    +------------------+          +--------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Was ist so revolutionär daran?
Bisherige Systeme mussten für ein Problem hergestellt werden. Von-Neumann-Architektur ist **programmierbar**!  
**Bedeutet:** Die Hardwarestruktur kann, durch programmierbaren Speicher, für mehrere Probleme verwendet werden.

- **Eingabewerk:** Dient zur Kommunikation. Befehls- und Informationsübermittlung.  
- **Ausgabewerk:** Gibt das Ergebnis aus!
- **Speicherwerk:** Konstanter Speicher, speichert Daten und Befehle.
- **Steuerwerk:** Steuert und überwacht den Verlauf. Befehlszähler (Inkrement) und Befehlsregister.
- **Rechenwerk:** Operanden und das Addierwerk (sequentielle Bearbeitung der Anweisungen/Befehle).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Der Fetch-Decode-Execute-Zyklus (FDEC)
Der **FDEC** ist der grundlegende Arbeitsprozess einer CPU. Er beschreibt, wie die CPU Befehle aus dem Speicher holt, interpretiert und ausführt.

- **Fetch (Holen):** Der nächste Befehl wird vom Speicher (RAM) in die CPU geholt.

- **Decode (Dekodieren):** Die CPU entschlüsselt den Befehl, um zu verstehen, was zu tun ist.

- **Execute (Ausführen):** Die Operation wird vom Rechenwerk (ALU) ausgeführt.

Dieser sequenzielle Zyklus ist für die korrekte Ausführung von Programmen unerlässlich. Schwachstellen auf dieser Ebene, wie z.B. bei der spekulativen Ausführung von Befehlen, führten zu kritischen Angriffen wie **Spectre** und **Meltdown**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Die CPU und ihre Komponenten
Die **CPU** (**Central Processing Unit**) ist das Herzstück des Computers. Sie besteht aus mehreren Kernkomponenten, die die Datenverarbeitung und -speicherung steuern.

- **Steuerwerk (Control Unit):** Steuert und überwacht den gesamten Ablauf der Befehlsausführung. Es enthält den Befehlszähler und das Befehlsregister.

- **Rechenwerk (Processing Unit):** Führt arithmetische und logische Operationen (ALU) sowie Gleitkommaberechnungen (FPU) aus.

- **Cache Unit:** Ein extrem schneller Zwischenspeicher, der die Zugriffszeit auf häufig genutzte Daten und Befehle verkürzt.

- **Bus-Systeme:** Dienen der Datenübertragung zwischen allen Komponenten des Computers. Es gibt sie in serieller, paralleler und gemultiplexter Form.
    - **serielle Datenübertragung:** Daten werden bitweise nacheinander verschickt.
    - **parallele Datenübertragung:** Mehrere Bits werden gleichzeitig verschickt.
    - **multiplexe Datenübertragung:** Bits werden über mehrere Leitungen ungeordnet verschickt.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Aufbau einer CPU
```text
+---------------------------------------PROZESSOR (CPU)------------------------------------+
| +--------------Control Unit---------------+ +------Processing Unit------+ +-Cache Unit-+ |
| | +----------+                            | | +----------+              | |            | |
| | |Instrucion| +----------+ +----------+  | | |Arithmetic| +----------+ | | +-------+  | |
| | |Decode    | |Control   | |Control   |  | | |Logic     |=| Register | | | |Data   |  | |
| | |Unit      | |Logic     | |Logic     |  | | |Unit      | +----------+ | | |Cache  |  | |
| | +----++----+ +----++----+ +----++----+  | | +----++----+              | | +--++---+  | |
| |      ||           ||           ||       | |      ||                   | |    ||      | |
| |      ||           ||       +---++-----+ | |      ||                   | |    ||      | |
| |      ||           ||       |Interface | | |      ||                   | |    ||      | |
| |      ||===========||=======|Unit      |=|=|======||===================|=|====||      | |
| |      ||           ||       +----++----+ | |      ||                   | |    ||      | |
| |      ||           ||            ||      | |      ||                   | |    ||      | |
| |      ||           ||            ||      | |      ||                   | |    ||      | |
| | +----++----+ +----++----+       ||      | | +----++----+ +----------+ | | +--++---+  | |
| | |Execution | |Internal  |       ||      | | |Floating  |=|Register  | | | |Code   |  | |
| | |Unit      | |ROM       |       ||      | | |Point Unit| +----------+ | | |Cache  |  | |
| | +----------+ +----------+       ||      | | +----------+              | | +-------+  | |
| +---------------------------------||------+ +---------------------------+ +------------+ |
+-----------------------------------||-----------------------------------------------------+
                                    ||
                                    || <-- Verbindung mit dem Chipsatz (Chipset)
                                +---++----+
    <=== interne Verbindung ===>| Chipset |<=== externe Verbindung ===>
                                +---------+     
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### CPU Komponenten im Detail
#### Steuerwerk (Control Unit)
| Komponenten | Definition |
|-------------|------------|
| **Instruction Decode Unit** | **Befehlsdecoder**; Übersetzt eingehende Befehle und leitet sie an die Ausführungseinheit weiter. |
| **Execution Unit** | **Ausführungseinheit**; Führt den in einem Befehl enthaltenen Code aus. |
| **Control Logic** | **Kontrolleinheit**; Steuert den Ablauf der Programme. |
| **Internal ROM** | **Interner ROM-Speicher**; Read Only Memory für feste interne Anweisungen. |
| **Interface Logic** | **Steuereinheit**; Steuert und überwacht die internen Verbindungen der CPU. |
| **Interface Unit** | **Schnittstelle**; Bindeglied zwischen den internen CPU-Verbindungen und dem Chipsatz. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Rechenwerk (Processing Unit)
| Komponenten | Definition |
|-------------|------------|
| **Arithmetic Logic Unit (ALU)** | **Arithmetisch-logische Einheit**; Führt mathematische (arithmetische) und logische Rechenoperationen aus. |
| **Floating Point Unit (FPU)** | **Gleitkomma-Rechner**; Führt Berechnungen mit Gleitkommazahlen aus. |
| **Register** | **Register-Speicher**; Spezieller und extrem schneller Speicher für Zwischenergebnisse und Operanden. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Data und Cache Unit
| Komponenten | Definition |
|-------------|------------|
| **Data und Code Cache** | **Cache-Speicher**; Ein extrem schneller Zwischenspeicher für Daten und Befehle, um die Zugriffszeit auf den Hauptspeicher zu verkürzen. |

**Sicherheitsrelevanz der Caches:** Die **L1-**, **L2-** und **L3-Caches** sind für die Leistung entscheidend. Allerdings können aus der Art, wie sie auf Daten zugreifen, Rückschlüsse auf die verarbeiteten Informationen gezogen werden. Dieses Phänomen wird bei sogenannten **Side-Channel-Angriffen** ausgenutzt, um sensible Daten wie kryptografische Schlüssel zu extrahieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links

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