# 🌐 TCP/IP Basics – Grundlagen der Netzwerkkommunikation



## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Das TCP/IP-Modell – Schichtenüberblick](#das-tcpip-modell---schichtenüberblick)
- [Protokolle & Funktionen im Detail](#protokolle--funktionen-im-detail)
- [IPv4 – Adressierung & Aufbau](#ipv4---adressierung--aufbau)
- [CIDR (Classless Inter-Domain Routing)](#cidr-classless-inter-domain-routing)
- [Wichtige Protokolle & Tools](#wichtige-protokolle--tools)
- [TCP-Verbindungsaufbau: Der 3-Way Handshake](#tcp-verbindungsaufbau-der-3-way-handshake)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

**TCP/IP** ist das Rückgrat der modernen digitalen Kommunikation – es ermöglicht den Austausch von Daten über das Internet und private Netzwerke. Es handelt sich dabei um ein Protokoll-Stack, also ein Schichtenmodell aus mehreren Netzwerkprotokollen, die zusammenarbeiten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das TCP/IP-Modell - Schichtenüberblick

Das TCP/IP-Modell besteht aus **4 Schichten**, die dem OSI-Modell ähnlich sind:

| TCP/IP-Schicht       | Vergleich OSI | Aufgabe                                      |
|----------------------|---------------|----------------------------------------------|
| 1. Anwendungsschicht | 7–5           | Stellt Dienste für Programme bereit          |
| 2. Transportschicht  | 4             | End-to-End-Kommunikation (TCP/UDP)           |
| 3. Internetschicht   | 3             | Routing und IP-Adressen (IP, ICMP)           |
| 4. Netzzugangsschicht| 2–1           | Physikalischer Zugang zum Netzwerk           |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Protokolle & Funktionen im Detail

### 1. Anwendungsschicht
- Protokolle: HTTP, FTP, SMTP, DNS, DHCP, SSH
- Stellt Netzfunktionen für Benutzeranwendungen bereit


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Transportschicht
- **TCP (Transmission Control Protocol)**: Verbindungsorientiert, zuverlässig (z. B. Web, E-Mail)
- **UDP (User Datagram Protocol)**: Verbindungsfrei, schneller, aber unsicherer (z. B. VoIP, DNS)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### TCP Merkmale:
- 3-Way Handshake (SYN → SYN/ACK → ACK)
- Sequenznummern, Bestätigungen
- Flusskontrolle (Windowing)
- Fehlererkennung & Wiederholung


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Internetschicht
- **IP (Internet Protocol)**: Routing der Datenpakete über Netzwerke hinweg
- **ICMP**: Diagnose (z. B. Ping, Traceroute)
- IPv4 (32 Bit) & IPv6 (128 Bit)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Netzzugangsschicht
- MAC-Adressen, Switches, ARP
- Standards: Ethernet, WLAN, PPP



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## IPv4 - Adressierung & Aufbau

- **IPv4-Adresse:** 32 Bit → 4 Dezimalzahlen (z. B. `192.168.1.1`)
- Besteht aus:
  - **Netzanteil**: Identifiziert das Subnetz
  - **Hostanteil**: Identifiziert das Gerät im Subnetz
- **Subnetzmaske:** Definiert, wie viele Bits für das Netz verwendet werden (`255.255.255.0` = /24)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### IP-Klassen (veraltet, aber grundlegend)
| Klasse | Bereich           | Standard-Subnetzmaske | Verwendung        |
|--------|-------------------|------------------------|--------------------|
| A      | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 (/8)         | Große Netzwerke     |
| B      | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 (/16)      | Mittlere Netze      |
| C      | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 (/24)    | Kleine Netze        |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Private IPv4-Bereiche
- Klasse A: `10.0.0.0/8`
- Klasse B: `172.16.0.0/12`
- Klasse C: `192.168.0.0/16`

> [Mehr zum Thema IP-Adressen findest du hier](/02-network-security/ip_adressen_basics.md)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## CIDR (Classless Inter-Domain Routing)

- Ermöglicht flexible Subnetzbildung mit **/Notation** (z. B. `/24 = 255.255.255.0`)
- Beispiel: `192.168.1.0/24` → 256 Adressen (254 nutzbar)
- `/30`, `/29`, `/28`, ... = kleinere Subnetze



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wichtige Protokolle & Tools

| Tool / Protokoll | Zweck                              |
|------------------|-------------------------------------|
| **Ping**         | Prüft Erreichbarkeit via ICMP       |
| **Traceroute**   | Zeigt den Pfad durch Netzwerke      |
| **NSLookup/Dig** | DNS-Auflösung testen                |
| **Netstat**      | Zeigt aktive Verbindungen & Ports   |
| **Wireshark**    | Netzwerk-Traffic analysieren        |
| **Nmap**         | Portscanning und Netzwerkerkennung  |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## TCP-Verbindungsaufbau: Der 3-Way Handshake

```text
Client                    Server
  |  SYN  ----------------->|
  |<-------------- SYN-ACK  |
  |  ACK  ----------------->|
  
      Verbindung steht!
```


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

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---