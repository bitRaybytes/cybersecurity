# 📡 Netzwerk-Protokoll Header Cheat Sheet

---

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Layer 2 Protokolle](#layer-2-protokolle)
  - [ARP Header (Layer 2/3)](#arp-header-layer-23)
  - [Ethernet II (Layer 2)](#ethernet-ii-layer-2)
- [Layer 3 Protokolle](#layer-3-protokolle)
  - [IPv4 Header (Layer 3)](#ipv4-header-layer-3)
  - [IPv6 Header (Layer 3)](#ipv6-header-layer-3)
- [Layer 4 Protokolle](#layer-4-protokolle)
  - [TCP Header (Layer 4)](#tcp-header-layer-4)
  - [UDP Header (Layer 4)](#udp-header-layer-4)
- [Layer 5-7 Protokolle](#)
  - [DHCP (über UDP 67/68) (Layer 5)](#dhcp-über-udp-6768-layer-5-7)
  - [DNS Header (UDP/TCP Port 53) (Layer 5)](#dns-header-udptcp-port-53-layer-5-7)
  - [HTTP/HTTPS (Layer 5)](#httphttps-layer-5-7)
  - [ICMP Header (Layer 3)](#icmp-header-layer-3)
  - [NTP (Layer 5)](#ntp)
  - [TLS/SSL Record Layer (Layer 5)](#tlsssl-record-layer-layer-5-7)
  - [Kerberos (Authentication) (Layer 5)](#kerberos-authentication-layer-5-7)
  - [RADIUS (Layer 5)](#radius-layer-5-7)
  - [IPSec (Security for IP) (Layer 5)](#ipsec-internet-procotol-security-layer-5-7)
  - [SMB (Server Message Block) (Layer 5)](#smb-server-message-block)
  - [SNMP (Simple Network Management Protocol) (Layer 5)](#snmp-simple-network-management-protocol)
  - [Syslog (Layer 5)](#syslog)
  - [ARBITRARY PROTOCOLS (SMB, IPSec, Kerberos, SNMP, Syslog) ](#arbitrary-protocols-smb-ipsec-kerberos-snmp-syslog)
- [Vergleichstabelle](#vergleichstabelle)
- [Haftungsausschluss](#haftungsausschluss)

---
## Einleitung

In der Netzwerksicherheit ist es wichtig, die Header der verschiedenen Protokolle zu verstehen.

Sie enthalten die Kontrollinformationen, die Router, Firewalls und Analysetools wie Wireshark oder tcpdump zur Verarbeitung von Paketen nutzen.
Dieses Dokument gibt einen Überblick über die wichtigsten Protokoll-Header mit deren Aufbau und Größen.

---

## Layer 2 Protokolle

###  Ethernet II (Layer 2)

- Zweck: Transport von Frames innerhalb eines LANs.
- Header-Größe: 14 Bytes = 112 Bit

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+--------------------+-------------------+----------------------+
| Ziel-MAC (6b)      | Quell-MAC (6b)    | EtherType  (16b + padding) |
+--------------------+-------------------+----------------------+
```


----


###  ARP Header (Layer 2/3)

- Zweck: Auflösung von IP → MAC-Adressen.
- Header-Größe: 28 Bytes

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+------------------------------+--------------+-----------------+
| Hardware Type 16b / 2B       | Protocol Type (PTYPE 2B / 16b) |
+------------------------------+--------------------------------+
| HW Size (1B)   | Prot Size (1B) | Opcode (2B)                 |
+----------------+----------------+-----------------------------+
| Sender MAC (8B)                                               |
|                                                               |
+---------------------------------------------------------------+
| Sender IP (4B)                                                |
+---------------------------------------------------------------+
| Target MAC (8B)                                               |
|                                                               |
+---------------------------------------------------------------+
| Target IP (4B)                                                |
+---------------------------------------------------------------+
```

----

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Layer 3 Protokolle

### IPv4 Header (Layer 3)

- Zweck: Routing von Paketen über Netzwerke hinweg.
- Header-Größe: 20–60 Bytes (20 Bytes ohne Optionen; 5–15 Words à 32 Bit)

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-------+------+---------------+----------------+---------------+
| Ver(4b)| IHL(4b)| ToS (8b)   | Total Length (16b)             |
+-------+-------+-----------------------------------------------+
| Identification (2 B)         | Flags (3b)|  Fragment Offset (13b)|
+---------------+--------------+---------------+----------------+
| TTL (1 B)     | Protocol (1B)| Header Checksum (2 B)           |
+----------------------------------+---------------+-------------+
| Source IP (4 B)                                                |
+----------------------------------------------------------------+
| Destination IP (4 B)                                           |
+----------------------------------------------------------------+
| Options (0-40 B, optional)                                     |
+----------------------------------------------------------------+
```


---

### ICMP Header (Layer 3)

- Zweck: Diagnose & Fehlerberichte (Ping, Traceroute).
- Header-Größe: min. 8 Bytes (Standard, ohne Zusatzfelder)

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+--------------+---------------+-----------------+-------------+
| Type (1B)    | Code (1B)     | Checksum (16b / 2B)           |
+--------------+---------------+-------------------------------+
| Rest of Header (4 B)                                         |
+--------------------------------------------------------------+
```

---

### IPv6 Header (Layer 3)

- Zweck: Nachfolger von IPv4, 128-Bit Adressen.
- Header-Größe: Fix 40 Bytes (10 Words à 32 Bit)

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+----------------+-------------+--------------------------------+
| Version (4b)   | Traffic Class (8b)| Flow Label (20b)         |
+---------------------------------------------------------------+
| Payload Length (2 B)         | Next Header (1 B) | Hop Limit (8b)|
+------------------------------+--------------+------------------+
| Source Address (16 B)                                          |
+----------------------------------------------------------------+
| Destination Address (16 B)                                     |
+----------------------------------------------------------------+
| Options (0-40 B, optional)                                     |
+----------------------------------------------------------------+
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Layer 4 Protokolle
### TCP Header (Layer 4)

- Zweck: Verbindungsorientierte Kommunikation.
- Header-Größe: 20–60 Bytes (5–15 Words à 32 Bit)

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| Source Port (2 B)            | Dest Port (2 B)               |
+------------------------------+-------------------------------+
| Sequence Number (4 B)                                        |
+--------------------------------------------------------------+
| Acknowledgment Number (4 B)                                  |
+--------------------------------------------------------------+
| Data Offset | Res. | Flags  | Window Size    (16 b)          |  => Data Offset 4b; Reserviert 6b; Flags 6b;
+--------------------------------------------------------------+
| Checksum (2 B)              | Urgent Pointer                 |
+-----------------------------+--------------------------------+
| Options (0-40 B, optional)                                   |
+--------------------------------------------------------------+
```

---

### UDP Header (Layer 4)

- Zweck: Verbindungsloser Transport (z. B. DNS, DHCP).
- Header-Größe: 8 Bytes = 2 Words à 32 Bit

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| Source Port (2 B)            | Dest Port (2 B)               |
+------------------------------+-------------------------------+
| Length (2 B)                 | Checksum (2 B)                |
+------------------------------+-------------------------------+
```

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Layer 5-7 Protokolle

### DHCP (über UDP 67/68) (Layer 5-7)

- Zweck: Automatische Zuweisung von IP, Gateway, DNS.
- Header-Größe: 236 Bytes + Optionen
- Transport: UDP 67/68

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| OP            | HTYPE        | HLEN            | HOPS        |
+---------------+--------------+-----------------+-------------+
| Transaction ID (4 B)                                         |
+--------------------------------------------------------------+
| Seconds (2 B)                | Flags (2 B)                   |
+------------------------------+-------------------------------+
| Client IP (4 B)                                              |
| Your IP (4 B)                                                |
| Server IP (4 B)                                              |
| Gateway IP (4 B)                                             |
+--------------------------------------------------------------+
| Client MAC (16 B)                                            |
+--------------------------------------------------------------+
| Server Name (64 B)                                           |
+--------------------------------------------------------------+
| Boot File (128 B)                                            |
+--------------------------------------------------------------+
| Options (variabel) (64 B)                                    |
+--------------------------------------------------------------+
```

----

### DNS Header (UDP/TCP Port 53) (Layer 5-7)

- Zweck: Namensauflösung (Domain → IP).
- Header-Größe: 12 Bytes + Queries/Responses
- Transport: UDP/TCP 53

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+--------------+---------------+------------------+-------------+
| Transaction ID (2 B)         | Flags (2 B)                    |
+------------------------------+--------------------------------+
| Questions (2 B)              | Answer RRs (2 B)               |
| Authority RRs (2 B)          | Additional RRs (2 B)           |
+------------------------------+--------------------------------+
```

---

### HTTP/HTTPS (Layer 5-7)

- Zweck: Web-Kommunikation (Text-basiert).
- Transport: TCP 80 (HTTP), TCP 443 (HTTPS via TLS/SSL)
- Header: Variabel (Textzeilen mit Feldern wie Host, User-Agent, Cookie, …)

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: curl/7.79.1
Accept: */*
```



----

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### TLS/SSL Record Layer (Layer 5-7)

- Zweck: Verschlüsselte Kommunikation (HTTPS, SMTPS, FTPS).
- Header-Größe: 5 Bytes + Payload

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| Content Type (1B) | Version (2B) | Length (2B)|
+---------+---------+---------+---------+---------+
| Encrypted Payload (variabel)                     |
+-------------------------------------------------+
```

---

### SSH 

- Zweck: Sichere Remote-Verbindungen & Tunnels.
- Transport: TCP 22
- Header: Kein fester Standard wie bei IP/TCP → initiale Pakete:
  - Protokoll-Version-String (SSH-2.0-OpenSSH_8.9)
  - Key Exchange Init (Algorithmen, Nonces)
- Payload: Verschlüsselt nach Schlüsselaustausch.


### NTP

- Zweck: Zeit-Synchronisation.
- Transport: UDP 123
- Header-Größe: 48 Bytes

```text
| LI | VN | Mode | Stratum | Poll | Precision |
| Root Delay (32b) | Root Dispersion (32b)    |
| Reference ID (32b)                          |
| Reference Timestamp (64b)                   |
| Originate Timestamp (64b)                   |
| Receive Timestamp (64b)                     |
| Transmit Timestamp (64b)                    |

```

----

### Kerberos (Authentication) (Layer 5-7)

- Zweck: Netzwerk-Authentifizierung (oft in Windows-AD, SSO).
- Transport: meist über TCP/UDP Port 88
- Header-Größe: variabel (ASN.1-codiert, meist 20–200+ Bytes)

**Kerberos Messages:**
- AS-REQ / AS-REP (Authentication Service)
- TGS-REQ / TGS-REP (Ticket Granting Service)
- AP-REQ / AP-REP (Application Service)

-----

### RADIUS (Layer 5-7)

- Zweck: Authentifizierung, Autorisierung, Accounting (AAA).
- Transport: UDP 1812 (Auth), UDP 1813 (Accounting).
- Header-Größe: 20 Bytes + Attribute

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+--------------+---------------+-----------------+-------------+
| Code  1B     | ID (1B)       | Length (2B)                    |
+--------------+---------------+-------------------------------+
| Authenticator (16B)                                           |
+---------------------------------------------------------------+
| Attributes (variabel)                                         |
+---------------------------------------------------------------+
```

----

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### IPSec (Internet Procotol Security) (Layer 5-7)

- Zweck: Sichere VPN-Kommunikation auf IP-Ebene.
- Header-Typen:
  - AH (Authentication Header): 24 Bytes
  - ESP (Encapsulating Security Payload): variabel (mind. 8 Bytes + Payload + Padding + Auth Data)

#### AH Header

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| Next          | Payload Length| Reserviert                   |
+---------------+--------------+-------------------------------+
SPI (4 B)                                                      |
+--------------------------------------------------------------+
| Sequence Number (4 B)                                        |
+--------------------------------------------------------------+
| Authentication Data (variabel)                               |
+--------------------------------------------------------------+
```

#### ESP Header

```text
0              | 1             | 2               | 3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+---------------+--------------+-----------------+-------------+
| SPI Sercurity Parameter Index (4 B)                          |
+--------------------------------------------------------------+
| Anit-Replay Sequence Number (4 B)                            |
+--------------------------------------------------------------+
| Encrypted Payload (variabel)                                 |
| Padding                     | PadLen     | Next Header       |
+-----------------------------+------------+-------------------+
| Authentication Data (variabel)                               |
+--------------------------------------------------------------+
```

----

### SMB (Server Message Block)

- Zweck: Datei- und Druckerfreigaben, häufiges Angriffsziel (EternalBlue, WannaCry).
- Transport: TCP 445.
- Header-Größe:
  - 32 Bytes (SMBv1),
  - 64 Bytes (SMBv2)

#### SMB-Header Felder (SMBv2):
- Protocol ID
- Structure Size
- Credit Charge
- Command
- Flags
- Message ID
- Tree ID, Session ID, Signature

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### SNMP (Simple Network Management Protocol)

- Zweck: Geräteverwaltung, oft Ziel für Enumeration & Angriffe.
- Transport: UDP 161.
- Header: ASN.1-basiert (variabel, min. ca. 20 Bytes).

#### Struktur:
- Version
- Community String (Passwort)
- PDU (GetRequest, GetResponse, Trap, etc.)


--- 

### Syslog

- Zweck: Zentrale Log-Sammlung.
- Transport: UDP 514 (traditionell), TCP/TLS möglich
- Header: variabel, Basisformat:

```text
<PRI> TIMESTAMP HOSTNAME TAG MESSAGE
```


----

### ARBITRARY PROTOCOLS (SMB, IPSec, Kerberos, SNMP, Syslog)

Diese Protokolle haben sehr variable Header-Aufbauten (häufig mit ASN.1 oder Block-Formaten). 

Eine 32-Bit-Wort-Darstellung wäre extrem komplex und übersteigt den Rahmen.

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vergleichstabelle

| Protokoll   | Port / Typ    | Header-Größe (min) | Header-Größe (max) | Security-Aspekt            |
| ----------- | ------------- | ------------------ | ------------------ | -------------------------- |
| Ethernet II | –             | 14 B               | 14 B               | Basis, VLAN-Hopping        |
| ARP         | –             | 28 B               | 28 B               | ARP-Spoofing               |
| IPv4        | –             | 20 B               | 60 B               | IP-Spoofing                |
| IPv6        | –             | 40 B               | 40 B               | RA-Spoofing, DoS           |
| ICMP        | –             | 8 B                | variabel           | Ping Flood, ICMP Tunnels   |
| TCP         | –             | 20 B               | 60 B               | SYN Flood, Reset Attacks   |
| UDP         | –             | 8 B                | 8 B                | Amplification Attacks      |
| DHCP        | 67/68 UDP     | 236 B              | variabel           | Rogue DHCP                 |
| DNS         | 53 UDP/TCP    | 12 B               | variabel           | DNS-Spoofing, Cache Poison |
| HTTP        | 80 TCP        | variabel           | variabel           | Session Hijacking, XSS     |
| HTTPS       | 443 TCP       | 5 B (TLS Record)   | variabel           | MitM, SSL-Stripping        |
| TLS/SSL     | –             | 5 B                | variabel           | Downgrade, Weak Ciphers    |
| SSH         | 22 TCP        | variabel           | variabel           | Bruteforce, Weak Keys      |
| NTP         | 123 UDP       | 48 B               | 48 B               | NTP Amplification Attacks  |
| Kerberos    | 88 TCP/UDP    | variabel           | variabel           | Pass-the-Ticket/Hash       |
| RADIUS      | 1812/1813 UDP | 20 B               | variabel           | Replay, Bruteforce         |
| IPSec AH    | –             | 24 B               | variabel           | VPN Security               |
| IPSec ESP   | –             | 8 B                | variabel           | VPN Security               |
| SMB         | 445 TCP       | 32 B               | 64 B               | Ransomware, Worms          |
| SNMP        | 161 UDP       | \~20 B             | variabel           | Default Community Strings  |
| Syslog      | 514 UDP/TCP   | variabel           | variabel           | Log Manipulation           |


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

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
