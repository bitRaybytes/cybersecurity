🛰️ OSI-Schichtenmodell – Cheat Sheet

🔎 Einleitung

Das OSI-Modell (Open Systems Interconnection) ist ein Referenzmodell für Netzwerkprotokolle, das von der ISO (International Organization for Standardization) entwickelt wurde.
Es teilt die Netzwerkkommunikation in 7 Schichten auf, die jeweils bestimmte Aufgaben haben.

Jede Schicht baut auf den darunterliegenden Schichten auf und stellt der nächsthöheren Schicht Dienste bereit. Das Modell beschreibt den Weg von Daten von einer Anwendung auf einem Host zu einer Anwendung auf einem anderen Host.

---

## Übersicht der 7 Schichten

```text
+-------------------------------------+--------------------------------+--------------------+--------------------------+-------------------------------------+
| OSI-Schicht                         | Protokoll-Beispiele            | Einheit (PDU)      | Geräte/Beispiele         | Einordnung                          |
+-------------------------------------+--------------------------------+--------------------+--------------------------+-------------------------------------+
| 7 Anwendungsschicht (Application)   | HTTP, HTTPS, FTP, SMTP, DNS    | Daten              | Browser, Mail-Client     |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+         Anwendungsschicht           |
| 6 Darstellungsschicht (Presentation)| TLS/SSL, JPEG, MPEG, JSON, XML | Daten              | Betriebssystem           |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+         (anwendungsbezogen)         |
| 5 Sitzungsschicht (Session)         | RPC, NetBIOS, SMB              | Daten              | Session-Controller       |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+-------------------------------------+
| 4 Transportschicht (Transport)      | TCP, UDP, NetBEUI              | Segmente/Datagramme| Firewalls, Gateways      |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+         Host-to-Host Schichten      |
| 3 Vermittlungsschicht (Network)     | IPv4/6, ICMP, IPsec            | Pakete             | Router, Layer-3-Switch   |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+-------------------------------------+
| 2 Sicherungsschicht (Data Link)     | Ehternet, ARP, PPP, MAC        | Frames             | Switches, Bridges        |                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+           Medien-Schichten          |
| 1 Bitübertragungsschicht (Physical) | Ethernet (PHY), WLAN, Bluetooth| Bits               | Kabel, Hubs, WLAN-Adapter|                                     |
+-------------------------------------+--------------------------------+--------------------+--------------------------+-------------------------------------+
```

----

## Schicht 1 🔹 Physikalische Schicht (Physical Layer)

**Aufgabe:** Übertragung von Bits (0/1) über physikalische Medien. Sie definiert die Hardware-Spezifikationen.

- **Einheit:** Bits
- **Komponenten:** Kabel, Stecker, Netzwerkkarten, Repeater, Hubs
- **Beispiele:**
  - Ethernet-Kabel (RJ45, Glasfaser)
  - WLAN (802.11-Standards)
  - Bluetooth
  - USB

```text
Signalübertragung → Elektrisch, Optisch oder Funk
```

----

## Schicht 2 🔹 Sicherungsschicht (Data Link Layer)

**Aufgabe:** Fehlererkennung und Sicherstellung einer zuverlässigen Übertragung zwischen zwei direkt verbundenen Geräten. Sie unterteilt sich in die Unterebenen **LLC (Logical Link Control)** und **MAC (Media Access Control)**.
- **Einheit:** Frames
- **Komponenten:** Switches, Bridges
- **Beispiele:**
  - Ethernet (MAC-Adressen)
  - ARP (Address Resolution Protocol)
  - PPP (Point-to-Poiint Protocol)

```text
Ein Frame besteht aus Header (Ziel-MAC, Quell-MAC), den Daten und einem Trailer (z. B. CRC zur Fehlererkennung).
+----------------------------------------+
| Ziel-MAC | Quell-MAC | Daten | CRC      |
+----------------------------------------+
```
 
----

## Schicht 3 🔹 Vermittlungsschicht (Network Layer)

**Aufgabe:** Routing von **Paketen** über verschiedene Netzwerke hinweg. Sie adressiert Geräte logisch, um Pfade zu finden.
- **Einheit:** Pakete
- **Komponenten:** Router, Layer-3-Switches
- **Beispiele:**
  - IPv4, IPv6
  - ICMP (Ping, Traceroute)
  - IPSec (Security)

```text
Ein Paket-Header enthält die IP-Adressen von Sender und Empfänger.
+----------------------------------------+
| Ziel-IP  | Quell-IP  | Daten            |
+----------------------------------------+
```

-----

## Schicht 4 🔹 Transportschicht (Transport Layer)

**Aufgabe:** Ende-zu-Ende-Kommunikation und Zuverlässigkeit durch Segmentierung und Flusskontrolle. Sie entscheidet, welche Anwendung Daten empfängt.
- **Einheit:** Segmente (TCP) oder Datagramme (UDP)
- **Beispiele:**
  - **TCP (Transmission Control Protocol):** Verbindungsorientiert, zuverlässig, garantiert die Ankunft der Daten.
  - **UDP (User Datagram Protocol):** Verbindungslos, unzuverlässig, aber schnell.
  - TLS/SSL (für sichere Übertragung, wird auch der Anwendungsschicht zugeordnet)

```text
+-----------------------------------------------+
| Src Port | Dst Port | Sequenznr. | Daten ...  |
+-----------------------------------------------+

```

----

## Shicht 5 🔹 Sitzungsschicht (Session Layer)

**Aufgabe:** Steuerung der Dialoge zwischen Anwendungen (Sitzungen aufbauen, verwalten, beenden).
- **Einheit:** Daten
- **Beispiele:**
  - NetBIOS
  - RPC (Remote Procedure Call)
  - SMB (Server Message Block)

```text
Anmeldungen, Sitzungen und Logins zwischen Hosts
```

-----

## Schicht 6 🔹 Darstellungsschicht (Presentation Layer)

**Aufgabe:** Übersetzung, Kompression und Verschlüsselung der Daten, damit Sender und Empfänger sie verstehen.
- **Einheit:** Daten
- **Beispiele:**
  - **Verschlüsselung:** TLS, SSL, AES
  - **Kompression:** JPEG, MPEG
  - **Datenformate:** ASCII, EBCDIC, JSON, XML

```text
Rohdaten <--> Verstehbare Information
```

----

## Schicht 7 🔹 Anwendungsschicht (Application Layer)

**Aufgabe:** Schnittstelle zwischen Benutzer und Netzwerkdiensten. Sie stellt die Anwendung bereit, mit der der Nutzer interagiert.
- **Einheit:** Daten
- **Beispiele:**
  - HTTP/HTTPS (Web)
  - FTP, SFTP (Dateiübertragung)
  - SMTP, IMAP, POP3 (E-Mail)
  - DNS (Namensauflösung)3
 
```text
Browser, Mail-Client, Chat-Apps <--> Netzwerk
```

---

## ℹ️ Encapsulation und Decapsulation

Der Datenfluss im OSI-Modell folgt dem Prinzip der **Kapselung (Encapsulation)** und **Entkapselung (Decapsulation)**.
- **Kapselung (Sender):** Jede Schicht fügt den Daten, die sie von der oberen Schicht empfängt, einen Header (und ggf. Trailer) hinzu und übergibt die neue Daten-Einheit an die darunterliegende Schicht.
- **Entkapselung (Empfänger):** Der Empfänger entfernt in umgekehrter Reihenfolge die Header jeder Schicht, bis die ursprünglichen Anwendungsdaten übrig bleiben.

---

## 💡 Das TCP/IP-Modell

In der Praxis wird häufig das **TCP/IP-Modell** verwendet, das eine vereinfachte Version des OSI-Modells ist. Es fasst die oberen drei Schichten des OSI-Modells in der **Anwendungsschicht** und die unteren beiden Schichten in der **Netzzugangsschicht** zusammen.

### Vergleich der Modelle

```text
|  OSI-Modell	                 |       TCP/IP-Modell              |
-------------------------------|----------------------------------|
|  7. Anwendungsschicht	       |                                  |
|  6. Darstellungsschicht	     |       4. Anwendungsschicht       |
|  5. Sitzungsschicht	         |                                  |
|------------------------------|----------------------------------|
|  4. Transportschicht	       |       3. Transportschicht        |
|------------------------------|----------------------------------|
|  3. Vermittlungsschicht	     |       2. Internetschicht         |
|------------------------------|----------------------------------|
|  2. Sicherungsschicht	       |       1. Netzzugangsschicht      |
|  1. Bitübertragungsschicht	 |                                  |
|------------------------------|----------------------------------|
```

---

## Nützliche Links

- [RFC: Point-to-Point-Protocol (PPP)](https://www.rfc-editor.org/rfc/inline-errata/rfc1661.html)
- [Request for Comments (offizielle Seite)](https://www.rfc-editor.org)
- [Elektronik Kompendium: OSI-Schichtenmodell in der Netzwerktechnik](https://www.elektronik-kompendium.de/sites/net/0706101.htm)
- [Wikipedia: OSI-Model](https://de.wikipedia.org/wiki/OSI-Modell)
