# 🔧 Chipsatz und Datenverarbeitung in ITK-Systemen


## Inhaltsverzeichnis
- [Einführung](#einführung)  
- [Aufgaben des Chipsatzes](#aufgaben-des-chipsatzes)  
- [Northbridge und Southbridge](#northbridge-und-southbridge)  
- [CPU-Verbindungsarten](#cpu-verbindungsarten)  
  - [Front Side Bus (FSB)](#front-side-bus-fsb)  
  - [HyperTransport (HT)](#hypertransport-ht)  
  - [QuickPath Interconnect (QPI)](#quickpath-interconnect-qpi)  
  - [Direct Media Interface (DMI)](#direct-media-interface-dmi)  
- [Sicherheitsrelevanz des Chipsatzes](#sicherheitsrelevanz-des-chipsatzes)  
- [ASCII-Übersicht: Chipsatz-Architektur](#ascii-übersicht-chipsatz-architektur)  
- [Fazit](#fazit)  
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einführung
Der **Chipsatz** ist das unbesungene Herz des Mainboards und das zentrale Steuerungssystem, das den gesamten Datenfluss im Computer reguliert. Früher oft als ein Satz von zwei separaten Chips (Northbridge und Southbridge) implementiert, sind die meisten modernen Systeme mittlerweile auf eine effizientere Architektur mit einem einzigen **Platform Controller Hub** (**PCH**) umgestiegen. Unabhängig von der Architektur bleibt die Aufgabe des Chipsatzes dieselbe: Er stellt sicher, dass alle Komponenten reibungslos miteinander kommunizieren. Für uns als IT-Sicherheitsexperten ist der Chipsatz von besonderer Bedeutung, da er eine tief liegende und somit kritische Angriffsoberfläche darstellt. 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Aufgaben des Chipsatzes
Die Rolle des Chipsatzes geht weit über die bloße Datenübertragung hinaus. Er agiert als intelligenter Vermittler zwischen der Hochgeschwindigkeitswelt der CPU und den zahlreichen langsameren Komponenten. Zu seinen Kernaufgaben gehören die Verwaltung der Kommunikation zwischen der CPU und anderen Hardware-Teilen, die Unterstützung bei der Speicherverwaltung sowie die Steuerung aller angeschlossenen Peripheriegeräte wie USB-Geräte, Festplatten und Netzwerkschnittstellen. Darüber hinaus ist der Chipsatz für die Energieverwaltung (**ACPI**) verantwortlich, die Betriebs- und Energiesparmodi steuert, und er ermöglicht die Kompatibilität sowie Erweiterungsoptionen für das gesamte System.

- Verwaltung der Kommunikation zwischen CPU und anderen Komponenten.  
- Unterstützung der Speicherverwaltung.  
- Energieverwaltung (inkl. **ACPI** für Betriebsmodi und Energiesparzustände).  
- Steuerung der Peripheriegeräte (USB, LAN, SATA etc.).  
- Takt- und Frequenzabstimmung für Datenbusse.  
- Bereitstellung von **Kompatibilitäts-** und **Erweiterungsoptionen**.  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Northbridge und Southbridge
In der traditionellen Chipsatz-Architektur wurden die Aufgaben klar zwischen zwei Komponenten aufgeteilt:


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Northbridge
Die Northbridge war für die Verwaltung der **leistungsintensiven Kommunikation** zuständig. Sie stellte die direkte Verbindung zwischen der CPU, dem **Arbeitsspeicher** (**RAM**) und der **Grafikkarte** (**GPU**) über einen **PCIe-x16-Anschluss** her. Aufgrund ihrer Nähe zu diesen Hochgeschwindigkeitskomponenten war sie für die schnelle Versorgung mit Daten unerlässlich und gewährleistete eine flexible Kompatibilität der Hardware.

- Verwaltung der **leistungsintensiven Kommunikation**: CPU, RAM, GPU (z. B. PCIe x16, AGP)  
- Speichercontroller-Anbindung  
- Vorteil: Nähe zu Leistungskomponenten, schnelle Datenversorgung, flexible Kompatibilität  

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Southbridge
Die Southbridge, auch als **I/O-Controller Hub** bekannt, hatte die Aufgabe, die langsameren Ein- und Ausgabe-Geräte zu verwalten. Sie war der zentrale Knotenpunkt für Schnittstellen wie SATA, IDE, USB und LAN. Zudem war sie für das Power Management der Peripherie zuständig. Ihre Stärke lag in der Kompatibilität mit einer Vielzahl unterschiedlicher Geräte und einer klaren Aufgabenteilung. Ihr Nachteil war jedoch ihre begrenzte Leistungskapazität bei hoher Auslastung.

- Verwaltung der **I/O-Geräte**: SATA, IDE, USB, LAN  
- Energieverwaltung für Peripherie (Power Management)  
- Vorteile: Kompatibilität mit vielen Geräten, klare Aufgabenteilung  
- Nachteile: Begrenzte Leistung bei hoher Last, da nicht direkt für Hochgeschwindigkeits-Kommunikation ausgelegt  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## CPU-Verbindungsarten
Um die Datenübertragung zwischen der CPU und dem Chipsatz zu optimieren, wurden verschiedene Bus-Architekturen entwickelt, die sich grundlegend in ihrer Funktionsweise unterscheiden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Front Side Bus (FSB)  
Der FSB war eine ältere, parallele Verbindungstechnologie, die als Hauptverbindung zwischen der CPU und dem Chipsatz (Northbridge) diente. Trotz seiner weiten Verbreitung war er im Vergleich zu modernen seriellen Verbindungen relativ langsam und ineffizient.

- Hauptverbindung CPU <-> Chipsatz  
- Zugriff auf RAM, GPU, Laufwerke  
- Beispiel: ~200 MHz, ~800 MT/s  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HyperTransport (HT)  
Als Nachfolger des FSB etablierte sich HyperTransport, insbesondere in AMD-Systemen. Er wechselte von einer parallelen zu einer **seriellen Datenübertragung** und nutzte eine **Point-to-Point-Topologie**. Dadurch konnte eine skalierbare Bandbreite und eine deutlich niedrigere Latenz erreicht werden.

- Wechsel von **parallel** zu **seriell**  
- Skalierbare Breite (8, 16, 32 Leitungen) und Frequenz  
- **Point-to-Point-Topologie** -> niedrigere Latenz  

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### QuickPath Interconnect (QPI)  
Intel führte QPI als Konkurrenz zu HyperTransport ein. Auch QPI setzte auf eine Point-to-Point-Topologie und zeichnete sich durch die Nutzung mehrerer Kanäle aus. Die wichtigste Neuerung war die Vollduplex-Kommunikation, die es ermöglichte, Daten gleichzeitig zu senden und zu empfangen.

- **Point-to-Point-Topologie**  
- Nutzung mehrerer Kanäle  
- **Vollduplex-Kommunikation** (gleichzeitiges Senden & Empfangen)  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Direct Media Interface (DMI)  
DMI wird von Intel als eine Art „Datenautobahn“ zur Verbindung zwischen CPU und Chipsatz (Northbridge/PCH) genutzt. Es ist eine Hochgeschwindigkeits-Schnittstelle, die einen schnellen Datentransfer und eine direkte Kommunikation zwischen der CPU und den angeschlossenen Komponenten ermöglicht.

- Verbindung zwischen CPU ↔ Chipsatz (North-/Southbridge bzw. PCH)  
- Hoher Datendurchsatz  
- Direkte Kommunikation von CPU und Komponenten  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsrelevanz des Chipsatzes
Aus der Perspektive der Cybersicherheit ist der Chipsatz ein höchst sensibles Bauteil. Er stellt eine Root-of-Trust-Komponente dar, da Angriffe auf dieser Ebene oft unterhalb des Betriebssystems ablaufen und somit von herkömmlichen Sicherheitslösungen schwer zu erkennen sind.

- **Manipulation am Chipsatz** Bösartige Firmware oder Microcode-Updates, die auf den Chipsatz geladen werden, können Angreifern eine dauerhafte Präsenz im System verschaffen. Sie können den gesamten Datenfluss überwachen und manipulieren.

- **DMA-Angriffe** Angriffe über **Direct Memory Access** (**DMA**), insbesondere über Schnittstellen wie PCIe oder Thunderbolt, sind eine der größten Bedrohungen. Sie ermöglichen es einem Angreifer mit physischem Zugang, den RAM direkt zu lesen oder zu schreiben, ohne die CPU oder das Betriebssystem zu passieren.
- **ACPI-Schwachstellen:** Schwachstellen in der **ACPI-Spezifikation** oder ihrer Implementierung können missbraucht werden, um Systemzustände zu manipulieren, Sicherheitsmechanismen zu umgehen oder Daten aus dem Arbeitsspeicher auszulesen, während sich der PC in einem Ruhezustandsmodus befindet. 
- **Physischer Zugriff** Direkter Zugang zu den internen Anschlüssen (SATA, M.2, PCIe) ermöglicht es einem Angreifer, sensible Daten zu stehlen oder manipulierte Hardware einzuschleusen, die das System kompromittiert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## ASCII-Übersicht: Chipsatz-Architektur
```text
         +-------------------+
         |       CPU         |
         | (Rechenkerne)     |
         +-------------------+
                |        |
                |        | (High-Speed, z. B. PCIe x16)
                v        v
         +----------------------+
         |     Northbridge      |
         | (RAM, GPU Controller)|
         +----------------------+
                  |
                  v
         +----------------------+
         |     Southbridge      |
         | (I/O: SATA, USB, LAN)|
         +----------------------+
                  |
       -----------------------------
       |      |       |       |    
     SATA    USB     LAN    Audio
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Fazit

Der Chipsatz ist das **Rückgrat der Systemkommunikation**. Seine Entwicklung von North-/Southbridge zu integrierten PCH-Systemen hat Leistung, Energieeffizienz und Platzbedarf verbessert. Gleichzeitig bleiben Schnittstellen, Energieverwaltung und DMA-Mechanismen sicherheitskritische Angriffspunkte. Wer IT-Systeme härtet oder untersucht, muss daher den Chipsatz und seine Firmware immer als potenzielle Root of Trust-Schwachstelle betrachten.


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