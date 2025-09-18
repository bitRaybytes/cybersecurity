# 🤖 Cyber-Physische Systeme (CPS) – Grundlagen und Sicherheit

## Inhaltsverzeichnis
- [Was sind Cyber-Physische Systeme (CPS)?](#was-sind-cyber-physische-systeme-cps)
- [Physische Objekte in CPS](#physische-objekte-in-cps)
- [Funktionsweise von IoT-Geräten](#funktionsweise-von-iot-geräten)
- [Kommunikation zwischen CP-Systemen](#kommunikation-zwischen-cp-systemen)
- [Vorteile von CPS](#vorteile-von-cps)
- [Einsatzgebiete von CPS](#einsatzgebiete-von-cps)
- [Entwicklung der Technik in Generationen](#entwicklung-der-technik-in-generationen)
- [Grundlagen und Anforderungen an CPS](#grundlagen-und-anforderungen-an-cps)
- [Hauptbestandteile und Interaktion von CPS](#hauptbestandteile-und-interaktion-von-cps)
- [Sicherheitsherausforderungen und Angriffsvektoren](#sicherheitsherausforderungen-und-angriffsvektoren)
- [Fazit und Verteidigungsstrategien](#fazit-und-verteidigungsstrategien)
- [Haftungsausschluss](#haftungsausschluss)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was sind Cyber-Physische Systeme (CPS)?

Ein **Cyber-Physisches System** (**CPS**) ist ein System, das physische Komponenten (z. B. Sensoren, Aktoren) mit digitalen Komponenten (z. B. Software, Computer) verbindet und steuert. Es handelt sich um ein Netzwerk von Komponenten, die über eine Dateninfrastruktur (z. B. Internet) miteinander kommunizieren und Daten austauschen.

**Merksatz:** Fast alle internetfähigen Geräte, die über ein Netzwerk miteinander kommunizieren und physische Vorgänge beeinflussen, können als Cyber-Physische Systeme betrachtet werden.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Physische Objekte in CPS
- **Smart Home:** Eine Waschmaschine, die man von unterwegs einschalten kann, Kaffeemaschinen, Trockner, Rolläden, Heizung und Haushaltsgeräte.

- **Unterhaltungselektronik:** TV, Spielekonsolen und Musikanlagen.

- **Energie- & Wassermanagement:** Systeme zur automatisierten Einsparung von Energie und Wasser.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Funktionsweise von IoT-Geräten
**IoT-Geräte** (Internet of Things) sind die Hardware-Komponenten von CPS. Um miteinander kommunizieren zu können, werden sie in ein Netzwerk mit Internetzugang eingebunden. Dadurch kann der Benutzer über eine Anwendung oder Benutzeroberfläche die Einstellungen vornehmen und die Geräte steuern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Kommunikation zwischen CP-Systemen
Die Kommunikation läuft über die jeweilige OSI-Schicht ab.
Mehr zum Thema OSI-Schichtenmodell erhältst du [hier](/02-network-security/grundlagen/osi_schichtenmodell.md).

| OSI-Schicht | Beschreibeung |
|-------------|---------------|
| **Anwendungsschicht** | **MQTT** ([Message Queue Telemetry Transport](/02-network-security/grundlagen/mqtt_protokoll_grundlagen.md)) |
| **Transportschicht** | **TCP/UDP** (Transmission Control Protocol und User Datagram Protocol) |
| **Vermittlungsschicht** | **IPv4**, **IPv6**, **6loWPAN** (IPv6 over Low Power Wireless Area Personal Network) |
| **Sicherungsschicht** | **LPWAN** (Low Power Wide Area Network) 500 Meter bis 10 Km. |
| **Bitübertragungsschicht** | Mobilfunk, Wi-Fi/802.11, Bluetooth Low Energy (BLE), iBeacon, ZigBee, Z-Wave, NFC, RFID |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vorteile von CPS
- **Effizienzsteigerung:** CPS können Prozesse optimieren und die Effizienz steigern.

- **Automatisierung:** Automatisierte Prozesse reduzieren den Bedarf an menschlichen Eingriffen.

- **Sicherheit:** CPS können die Sicherheit erhöhen, z. B. durch das Erkennen von Gefahren und das Auslösen von Schutzmaßnahmen.

- **Anpassungsfähigkeit:** CPS sind flexibel und können an sich ändernde Bedingungen angepasst werden.

- **Datenerhebung und -analyse:** CPS ermöglichen die Sammlung und Analyse großer Datenmengen, was zu besseren Erkenntnissen führen kann.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einsatzgebiete von CPS

| Einsatzgebiet | Beschreibung | Aufgabe |
|---------------|--------------|---------|
| **Smart Home** | Einsatz von CPS im Privatbereich. | Ziel: Komfort, Sicherheit, Energieeffizienz, Unterhaltung. |
| **IIoT** | Ein Konzept wie IoT, jedoch für die Industrie (Industrial IoT). | Z.B. Steuerung von Fertigungsanlagen, Optimierung von Produktionsketten und die Verarbeitung/Speicherung von Daten. |
| **Smart Factory** | Einsatz von CPS im Unternehmensbereich. | Ziele: Produktionsprozesse optimieren, organisieren, verwalten und steuern. |
| **Smart City** | Einsatz von CPS im öffentlichen Raum. | Ziele: Umweltverschmutzung reduzieren, Parkleitsysteme, Verkehrssteuerung, Erkennung von Gas-/Wasserlecks. |
| **Consumer-Bereich** | Kunden interagieren mit Smartphones, Tablets und Co. mit Dienstleistungen von Unternehmen. | Gesundheitstracker, SmartWatches, Smart-TVs und Co. |
| **VR/AR/Spracherkennung** | Nutzung von CPS-Technologie zur Schaffung immersiver Erfahrungen. | Steuerung von Geräten per Sprache. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Entwicklung der Technik in Generationen

- **Generation 1.0 (ca. 1790):** **Mechanisierung**; Dampfmaschine, mechanischer Webstuhl.

- **Generation 2.0 (ca. 1870):** **Massenproduktion**; Elektrifizierung, Industrialisierung.
- **Generation 3.0 (ca. 1970):** **Automatisierung**; Elektronik, IT-Systeme.
- **Generation 4.0 (ca. 2017):** **Cyber-Physische Systeme**, IoT, Netzwerke, Smart Factory.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundlagen und Anforderungen an CPS

| Begriff | Erläuterung |
|---------|-------------|
| **Konnektivität** | Die Kommunikationsbasis mit Funktechniken (4G/5G, Bluetooth, WLAN, ZigBee, Z-Wave) und Protokollen (OPC UA, Modbus). |
| **Offene Schnittstellen** | APIs ermöglichen universellen Zugriff, sind aber auch ein Einfallstor für Angriffe. |
| **Management** | Werkzeuge zur Steuerung, Überwachung, Fernwartung und Durchführung von Software-Updates. |
| **Datenanalyse** | Untersuchen von gesammelten Daten auf Muster und Abweichungen zur Prozessoptimierung. |
| **Reporting** | Visualisierung der Daten auf Dashboards oder in Berichten. |
| **Sicherheit** | Funktionen wie Authentifizierung, Verschlüsselung und geschützter Datentransfer, um vor möglichen Angriffen zu schützen. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Hauptbestandteile und Interaktion von CPS

Ein CPS ist ein geschlossener Regelkreis, der eine Interaktion zwischen der physischen und der digitalen Welt ermöglicht. Sensoren erfassen Daten aus der Umwelt, die dann verarbeitet werden, um über Aktoren eine physische Reaktion auszulösen.

**Grafik: Beispielhafte Darstellung zu den Hauptbestandteilen eines CPS und die Interaktion mit der Umwelt**
```text
  +----------Cyber-Physisches-System----------+
  |                                           |  
  |  +-------------+       +---------------+  |                       +------------+
  |  |             |<------|  Sensoren     |  |  <==kommunizieren==>  |            |
  |  | Prozessoren |------>+---------------+  |  <=================>  |            |
  |  |             |<------| Kommunikation |  |  <===wahrnehmen====>  |   Umwelt   |
  |  |             |------>+---------------+  |  <=================>  |            |
  |  |             |<------| Aktorik       |  |  <===interagieren==>  |            |
  |  +-------------+       +---------------+  |  <=================>  +------------+
  |                                           |
  +-------------------------------------------+     
```

- **Sensorik:** Mittels Sensoren (z.B. Temperatur-Sensor, Infrarot-Sensor, Feuchtigkeits-Sensor) kann ein CPS Werte aus seiner Umwelt aufnehmen und verarbeiten.

- **Aktorik:** CPS können antriebstechnische Baueineinheiten (Aktoren) wie Motoren, Ventile oder Zylinder besitzen. Sie interagieren mit der Umwelt. 
- **Prozessoren:** Die Verarbeitung läuft über Mikrocontroller (Einplatinen-Computersysteme (**SBC** **S**mall **B**oard **C**ontroller) wie `Raspberry Pi`, `Arduino`), der erfasste Daten nach Instruktionen verarbeitet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsherausforderungen und Angriffsvektoren
Die Verbindung von physischer und digitaler Welt schafft einzigartige Sicherheitsrisiken, da ein digitaler Angriff physische Schäden verursachen kann.


| Angriffsvektor | Beschreibung | Angriffsziel |
|----------------|--------------|--------------|
| **Gerätemanipulation** | Kompromittierung der IoT-Geräte-Firmware. | Sensoren, Aktoren, Endgeräte. |
| **Netzwerkangriffe** | Abfangen, Manipulieren oder Stören der Kommunikation. | Kommunikationsprotokolle, Datenübertragung. |
| **Datenmanipulation** | Verfälschen von Sensordaten, um falsche Aktionen auszulösen. | Datenintegrität. |
| **Ausfall von Geräten** | Gezieltes Abschalten von kritischen Geräten (Denial of Service). | Verfügbarkeit des Systems. |
| **Software-Schwachstellen** | Ausnutzung von Fehlern in der Steuerungssoftware. | Anwendungen, Betriebssysteme. |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Die drei Säulen der Sicherheit in CPS
Die traditionelle **CIA-Triade** (**Confidentiality**, **Integrity**, **Availability**) wird bei CPS durch neue Herausforderungen erweitert.

- **Vertraulichkeit (Confidentiality):** Schutz der sensiblen Daten, die von Sensoren erfasst werden.

- **Integrität (Integrity):** Sicherstellung, dass die Daten von Sensoren und die Befehle an Aktoren nicht manipuliert wurden. Eine manipulierte Temperaturmessung könnte zu einer Explosion führen.

- **Verfügbarkeit (Availability):** Die Systeme müssen jederzeit funktionieren. Ein Ausfall in einem medizinischen CPS kann lebensgefährlich sein.

- **Sicherheit (Safety):** Verhindern, dass Angriffe zu physischen Schäden, Verletzungen oder Tod führen.

- **Datenschutz (Privacy):** Schutz der personenbezogenen Daten, die von IoT-Geräten gesammelt werden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fazit und Verteidigungsstrategien
Die Sicherheit von CPS ist komplex, da sie die Disziplinen der **IT-Sicherheit** (Schutz von Daten und Software) und der **OT-Sicherheit** (Schutz der physischen Systeme) miteinander verbindet. Angreifer zielen nicht nur auf Daten, sondern auch auf die physische Realität ab.

Um diese Systeme effektiv zu schützen, müssen Verteidiger einen mehrschichtigen Ansatz verfolgen:

- **Netzwerk-Segmentierung:** Trennen Sie kritische CPS-Systeme von der restlichen IT-Infrastruktur.

- **Strenge Authentifizierung:** Jedes Gerät und jeder Benutzer sollte sich authentifizieren, bevor es/er auf ein System zugreift.

- **Verschlüsselung:** Schützen Sie die Kommunikation zwischen den Komponenten und die auf den Geräten gespeicherten Daten.

- **Regelmäßige Updates:** Stellen Sie sicher, dass die Firmware und Software der Geräte stets aktuell sind, um bekannte Schwachstellen zu beheben.

CPS sind der nächste Schritt in der Evolution der IT. Ein tiefes Verständnis ihrer Architektur und ihrer einzigartigen Sicherheitsrisiken ist entscheidend, um unsere vernetzte Welt zu schützen.



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