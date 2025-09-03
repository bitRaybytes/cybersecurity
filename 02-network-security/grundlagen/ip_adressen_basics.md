# 🌐 IP-Adressierung in Netzwerken

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [IPv4](#ipv4)
- [IPv6](#ipv6)
- [Subnetting-Tabelle](#subnetting-tabelle)
- [Weitere Hinweise](#weitere-hinweise)
- [Haftungsausschluss](#haftungsausschluss)

---

## Einführung

IP-Adressen (Internet Protocol) sind eindeutige Kennzeichnungen von Geräten in einem Netzwerk. Sie ermöglichen die Kommunikation zwischen Hosts über verschiedene Netzwerke hinweg.

---

## IPv4

### Aufbau

- **32 Bit**, unterteilt in **4 Oktette** (z. B. 192.168.0.1)
- Dezimalnotation: `xxx.xxx.xxx.xxx` (je 0–255)
- Besteht aus:
  - **Netzwerkanteil**: Bestimmt das Subnetz
  - **Hostanteil**: Identifiziert das Gerät im Subnetz

### Klassen

| Klasse | Bereich           | Anzahl Hosts | Verwendung              |
|--------|-------------------|--------------|--------------------------|
| A      | 1.0.0.0 – 126.255.255.255 | ca. 16 Mio    | Große Netze (z. B. ISPs) |
| B      | 128.0.0.0 – 191.255.255.255 | ca. 65.000   | Mittlere Netze           |
| C      | 192.0.0.0 – 223.255.255.255 | 254          | Kleine Netze             |
| D      | 224.0.0.0 – 239.255.255.255 | -            | Multicast                |
| E      | 240.0.0.0 – 255.255.255.255 | -            | Reserviert (Experimentell) |

### Private Adressbereiche (RFC 1918)

| Klasse | Bereich               | Subnetzmaske      | Hosts      |
|--------|------------------------|-------------------|------------|
| A      | 10.0.0.0 – 10.255.255.255 | 255.0.0.0 (/8)    | 16.777.214 |
| B      | 172.16.0.0 – 172.31.255.255 | 255.240.0.0 (/12) | 1.048.574  |
| C      | 192.168.0.0 – 192.168.255.255 | 255.255.0.0 (/16) | 65.534     |

> Private Adressen sind nicht öffentlich geroutet und benötigen NAT (Network Address Translation), um mit dem Internet zu kommunizieren.



### Subnetting

Subnetting ist die Aufteilung eines IP-Netzes in kleinere Subnetze durch Erweiterung der Subnetzmaske.

#### Beispiel:
- IP-Adresse: `192.168.1.0`
- Subnetzmaske: `255.255.255.0` → 256 Adressen
- Benötigte Subnetze: 4
- Neue Maske: `255.255.255.192` (/26) → je 64 Adressen

### CIDR (Classless Inter-Domain Routing)

CIDR ersetzt die Klassen durch Präfixe wie `/24`, `/16`, etc. und erlaubt flexible Netzaufteilung.

| CIDR | Subnetzmaske       | Hosts (−2) |
|------|---------------------|------------|
| /24  | 255.255.255.0       | 254        |
| /25  | 255.255.255.128     | 126        |
| /26  | 255.255.255.192     | 62         |
| /30  | 255.255.255.252     | 2          |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Aufgaben im OSI-Modell

- **Schicht 3 – Netzwerk (OSI-Layer 3):**
  - Routing und Weiterleitung
  - IP-Adressierung
  - Fragmentierung

---

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

---

## IPv6

### Aufbau

- **128 Bit** Adresse, Hexadezimal-Schreibweise:
  - Beispiel: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Reduktion durch Regeln erlaubt Kürzung:
  - `2001:db8::8a2e:370:7334`

### Arten von IPv6-Adressen

| Typ              | Bereich / Präfix   | Bedeutung                            |
|------------------|--------------------|--------------------------------------|
| Global Unicast   | 2000::/3           | Öffentlich erreichbare Adressen      |
| Link-Local       | fe80::/10          | Nur innerhalb des lokalen Netzes     |
| Unique Local     | fc00::/7           | Vergleichbar mit privaten IPv4-Adr.  |
| Multicast        | ff00::/8           | Daten an mehrere Hosts gleichzeitig  |
| Anycast          | -                  | Daten an nächstgelegenen Host        |

### Vorteile gegenüber IPv4

- Kein NAT mehr nötig
- Automatische Konfiguration (SLAAC)
- Eingebaute Sicherheit (IPSec)
- Mehr Adressen (2^128)

### IPv6 Sicherheit

- IPSec ist Teil des Standards
- Jedes Gerät hat eine eindeutige Adresse – erschwert Anonymität
- Erkennung durch ICMPv6 kann zur Schwachstelle werden

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Subnetting-Tabelle

> Hinweis: Immer **2 Adressen abziehen** – eine für das Netzwerk, eine für Broadcast

### Subnetting Sheet – CIDR → Hosts

| **CIDR** | **Hosts**         | **Subnet Mask (v4)**  |
|----------|-------------------|------------------------|
| /1       | 2,147,483,648     | 128.0.0.0              |
| /2       | 1,073,741,824     | 192.0.0.0              |
| /3       | 536,870,912       | 224.0.0.0              |
| /4       | 268,435,456       | 240.0.0.0              |
| /5       | 134,217,728       | 248.0.0.0              |
| /6       | 67,108,864        | 252.0.0.0              |
| /7       | 33,554,432        | 254.0.0.0              |
| /8       | 16,777,216        | 255.0.0.0              |
| /9       | 8,388,608         | 255.128.0.0            |
| /10      | 4,194,304         | 255.192.0.0            |
| /11      | 2,097,152         | 255.224.0.0            |
| /12      | 1,048,576         | 255.240.0.0            |
| /13      | 524,288           | 255.248.0.0            |
| /14      | 262,144           | 255.252.0.0            |
| /15      | 131,072           | 255.254.0.0            |
| /16      | 65,536            | 255.255.0.0            |
| /17      | 32,768            | 255.255.128.0          |
| /18      | 16,384            | 255.255.192.0          |
| /19      | 8,192             | 255.255.224.0          |
| /20      | 4,096             | 255.255.240.0          |
| /21      | 2,048             | 255.255.248.0          |
| /22      | 1,024             | 255.255.252.0          |
| /23      | 512               | 255.255.254.0          |
| /24      | 256               | 255.255.255.0          |
| /25      | 128               | 255.255.255.128        |
| /26      | 64                | 255.255.255.192        |
| /27      | 32                | 255.255.255.224        |
| /28      | 16                | 255.255.255.240        |
| /29      | 8                 | 255.255.255.248        |
| /30      | 4                 | 255.255.255.252        |
| /31      | 2 (point-to-point)| 255.255.255.254        |
| /32      | 1 (Loopback)      | 255.255.255.255        |

---

## Weitere Hinweise

- **Loopback (localhost):** 127.0.0.1 (IPv4), ::1 (IPv6)
- **Broadcast-Adresse:** letzte Adresse im Subnetz
- **Netzwerkadresse:** erste Adresse im Subnetz
- **Multicast (IPv4):** 224.0.0.0/4
- **DHCP (IPv4):** Port 67/68 UDP
- **ICMP (Ping, Traceroute):** Layer 3-Protokoll, wichtig für Diagnosen

---

**Tipp für Ethical Hacking:**  
Ein tiefes Verständnis von IP-Adressierung, Subnetting und Routing hilft dir enorm bei Netzwerk-Scanning, Port-Erkennung, Firewall-Umgehung und bei der Nutzung von Tools wie Nmap, Wireshark oder Metasploit.

---

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

> Erstellt von Ray – für das Cybersecurity Lern- und Pentest-Repository  
> 🛡️ Stay curious. Stay safe.
Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---


