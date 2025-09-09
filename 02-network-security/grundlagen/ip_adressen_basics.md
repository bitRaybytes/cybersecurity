# 🌐 IP-Adressierung in Netzwerken

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [IPv4](#ipv4)
- [IPv6](#ipv6)
- [Subnetting-Tabelle](#subnetting-tabelle)
- [Weitere Hinweise](#weitere-hinweise)
- [Haftungsausschluss](#haftungsausschluss)



## Einführung

IP-Adressen (Internet Protocol) sind einzigartige Kennzeichnungen von Geräten in einem Netzwerk. Sie ermöglichen die Kommunikation zwischen Hosts über verschiedene Netzwerke hinweg und bilden das Fundament des Internets.



## IPv4

### Aufbau
Eine IPv4-Adresse ist 32 Bit lang und wird meist in vier Dezimalzahlen (Oktette) dargestellt, die durch Punkte getrennt sind (z. B. `192.168.0.1`)

- **32 Bit**, unterteilt in **4 Oktette** (z. B. `192.168.0.1`)
- **Dezimalnotation:** `xxx.xxx.xxx.xxx` (je `0–255`)
- **Besteht aus:**
  - **Netzwerkanteil**: Identifiziert das Netzwerk, in dem sich ein Gerät befindet.
  - **Hostanteil**: Identifiziert das spezifische Gerät innerhalb dieses Netzwerks.

### Klassen

| Klasse | Bereich           | Anzahl Hosts | Verwendung              |
|--------|-------------------|--------------|--------------------------|
| A      | 1.0.0.0 – 126.255.255.255 | ca. 16 Mio    | Große Netze (z. B. ISPs) |
| B      | 128.0.0.0 – 191.255.255.255 | ca. 65.000   | Mittlere Netze           |
| C      | 192.0.0.0 – 223.255.255.255 | 254          | Kleine Netze             |
| D      | 224.0.0.0 – 239.255.255.255 | N/A          | Multicast                |
| E      | 240.0.0.0 – 255.255.255.255 | N/A          | Reserviert (Experimentell) |

**Hinweis:** Das klassenbasierte System ist veraltet und wird heute durch CIDR ersetzt, ist aber für das grundlegende Verständnis wichtig.

### Private Adressbereiche (RFC 1918)

Diese Adressen sind für die Verwendung in **lokalen Netzwerken** (**LANs**) reserviert und sind nicht öffentlich im Internet erreichbar. Sie werden durch **NAT** (Network Address Translation) in öffentliche IP-Adressen umgewandelt.

| Klasse | Bereich               | Subnetzmaske      | Hosts      |
|--------|------------------------|-------------------|------------|
| A      | 10.0.0.0 – 10.255.255.255 | 255.0.0.0 (/8)    | 16.777.214 |
| B      | 172.16.0.0 – 172.31.255.255 | 255.240.0.0 (/12) | 1.048.574  |
| C      | 192.168.0.0 – 192.168.255.255 | 255.255.0.0 (/16) | 65.534     |

> Private Adressen sind nicht öffentlich geroutet und benötigen NAT (Network Address Translation), um mit dem Internet zu kommunizieren.



### Subnetting

Subnetting ist die Aufteilung eines IP-Netzes in kleinere Subnetze durch Erweiterung der Subnetzmaske.

#### Beispiel:
- **IP-Adresse:** `192.168.1.0`
- **Subnetzmaske:** `255.255.255.0` → 256 Adressen
- **Benötigte Subnetze:** 4
- **Neue Maske:** `255.255.255.192` (/26) → je 64 Adressen

### CIDR (Classless Inter-Domain Routing)

CIDR ersetzt die Klassen durch Präfixe wie `/24`, `/16`, etc. und erlaubt flexible Netzaufteilung.

| CIDR | Subnetzmaske       | Hosts (−2) |
|------|---------------------|------------|
| /24  | 255.255.255.0       | 254        |
| /25  | 255.255.255.128     | 126        |
| /26  | 255.255.255.192     | 62         |
| /30  | 255.255.255.252     | 2          |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Aufgaben im OSI-Modell

- **Schicht 3 – Netzwerk (OSI-Layer 3):**
  - Routing und Weiterleitung
  - IP-Adressierung
  - Fragmentierung



### IPv4 Sicherheit

- IPv4 war nicht für Sicherheit konzipiert.
- Angriffsszenarien:
  - IP-Spoofing
  - Man-in-the-Middle
  - DoS-Angriffe durch IP-Überflutung
- Schutzmaßnahmen:
  - Firewalls
  - IDS/IPS
  - VPN/IPSec



## IPv6

### Aufbau

- **128 Bit** Adresse in **hexadezimal** Schreibweise.
- **Beispiel:** `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Die Adressen können gekürzt werden, um führende Nullen zu entfernen oder zusammenhängende Null-Blöcke durch `::` zu ersetzen: `2001:db8:85a3::8a2e:370:7334`

### Arten von IPv6-Adressen

| Typ              | Bereich / Präfix   | Bedeutung                            |
|------------------|--------------------|--------------------------------------|
| Global Unicast   | 2000::/3           | Öffentlich erreichbare Adressen      |
| Link-Local       | fe80::/10          | Nur innerhalb des lokalen Netzes     |
| Unique Local     | fc00::/7           | Vergleichbar mit privaten IPv4-Adressen |
| Multicast        | ff00::/8           | Daten an mehrere Hosts gleichzeitig  |
| Anycast          | -                  | Daten an nächstgelegenen Host        |

### Vorteile gegenüber IPv4

- **Kein NAT mehr nötig:** Jedes Gerät kann eine eindeutige, öffentliche Adresse haben.
- Automatische Konfiguration (SLAAC)
- **Eingebaute Sicherheit:** IPSec ist im Standard integriert.
- Mehr Adressen (**2<sup>128</sup>**) gegenüber IPv4 (**2<sup>32</sup>**)

### IPv6 Sicherheit

- IPSec ist Teil des Standards
- Jedes Gerät hat eine eindeutige Adresse – erschwert Anonymität
- Erkennung durch ICMPv6 kann zur Schwachstelle werden



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Subnetting-Tabelle

> Hinweis: Immer **2 Adressen abziehen** – eine für das Netzwerk, eine für Broadcast

### Subnetting Sheet – CIDR → Hosts

| **CIDR** | **Anzahl Adressen**         | **nutzbare Hosts**  | **Subnet Mask (v4)**   |
|----------|-------------------|---------------------|------------------------|
| /1       | 2,147,483,648     | 2,147,483,646       | 128.0.0.0              |
| /2       | 1,073,741,824     | 1,073,741,822       | 192.0.0.0              |
| /3       | 536,870,912       | 536,870,910         | 224.0.0.0              |
| /4       | 268,435,456       | 268,435,454         | 240.0.0.0              |
| /5       | 134,217,728       | 134,217,726         | 248.0.0.0              |
| /6       | 67,108,864        | 67,108,862          | 252.0.0.0              |
| /7       | 33,554,432        | 33,554,430          | 254.0.0.0              |
| /8       | 16,777,216        | 16,777,214          | 255.0.0.0              |
| /9       | 8,388,608         | 8,388,606           | 255.128.0.0            |
| /10      | 4,194,304         | 4,194,302           | 255.192.0.0            |
| /11      | 2,097,152         | 2,097,150           | 255.224.0.0            |
| /12      | 1,048,576         | 1,048,574           | 255.240.0.0            |
| /13      | 524,288           | 524,286             | 255.248.0.0            |
| /14      | 262,144           | 262,142             | 255.252.0.0            |
| /15      | 131,072           | 131,070             | 255.254.0.0            |
| /16      | 65,536            | 65,534              | 255.255.0.0            |
| /17      | 32,768            | 32,766              | 255.255.128.0          |
| /18      | 16,384            | 16,382              | 255.255.192.0          |
| /19      | 8,192             | 8,190               | 255.255.224.0          |
| /20      | 4,096             | 4,094               | 255.255.240.0          |
| /21      | 2,048             | 2,046               | 255.255.248.0          |
| /22      | 1,024             | 1,022               | 255.255.252.0          |
| /23      | 512               | 510                 | 255.255.254.0          |
| /24      | 256               | 254                 | 255.255.255.0          |
| /25      | 128               | 126                 | 255.255.255.128        |
| /26      | 64                | 62                  | 255.255.255.192        |
| /27      | 32                | 30                  | 255.255.255.224        |
| /28      | 16                | 14                  | 255.255.255.240        |
| /29      | 8                 | 6                   | 255.255.255.248        |
| /30      | 4                 | 2                   | 255.255.255.252        |
| /31      | 2 (point-to-point)| 2                   | 255.255.255.254        |
| /32      | 1 (Loopback)      | 1                   | 255.255.255.255        |

**Hinweis:** Bei den meisten Subnetzen werden die erste (Netzwerk-) und die letzte (Broadcast-) Adresse nicht für Hosts genutzt, daher **Anzahl Adressen - 2**. Bei `/31` und `/32` gelten Ausnahmen.


## Die Rolle von Routern und Firewalls
- **Router:** Arbeiten auf **OSI-Schicht 3** und nutzen die IP-Adresse, um Datenpakete zwischen verschiedenen Netzwerken zu routen. Sie entscheiden anhand von Routing-Tabellen, wohin ein Paket gesendet werden muss.

- **Firewalls:** Kontrollieren den Datenverkehr basierend auf IP-Adressen, Ports und Protokollen. Eine Firewall kann so konfiguriert werden, dass sie Datenverkehr von oder zu bestimmten IP-Adressen blockiert (Blacklisting) oder nur von bestimmten Adressen zulässt (Whitelisting).


## Weitere Hinweise

- **Loopback (localhost):** Die Adresse `127.0.0.1` (IPv4) bzw. `::1` (IPv6) verweist immer auf das eigene Gerät und wird für Tests verwendet.
- **Broadcast-Adresse:** letzte Adresse im Subnetz
- **Netzwerkadresse:** erste Adresse im Subnetz
- **Multicast (IPv4):** 224.0.0.0/4
- **DHCP (Dynamic Host Configuration Protocol):** Ein Protokoll, das Geräten in einem Netzwerk automatisch eine IP-Adresse zuweist.
- **ICMP (Internet Control Message Protocol):** Wird für Diagnosewerkzeuge wie `ping` und `traceroute` verwendet.
- **ARP (Address Resolution Protocol):** Übersetzt eine IP-Adresse in die physische **MAC-Adresse** (Layer 2) innerhalb eines lokalen Netzwerks.



**Tipp für Ethical Hacking:**  
Ein tiefes Verständnis von IP-Adressierung, Subnetting und Routing ist unerlässlich für Netzwerk-Scanning, Port-Erkennung, Firewall-Umgehung und die Nutzung von Tools wie Nmap oder Wireshark.



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


🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---


