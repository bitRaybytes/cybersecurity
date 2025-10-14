# 📡 Netzwerk-Protokoll Header Cheat Sheet


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Layer 2 Protokolle](#layer-2-protokolle)
  - [Ethernet II (Layer 2)](#ethernet-ii-layer-2)
  - [ARP Header (Layer 2/3)](#arp-header-layer-23)
- [Layer 3 Protokolle](#layer-3-protokolle)
  - [IPv4 Header (Layer 3)](#ipv4-header-layer-3)
  - [IPv6 Header (Layer 3)](#ipv6-header-layer-3)
  - [ICMP Header (Layer 3)](#icmp-header-layer-3)
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



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

In der Netzwerksicherheit ist es entscheidend, die Header der verschiedenen Protokolle zu verstehen. Sie enthalten die Kontrollinformationen, die Router, Firewalls und Analysetools wie `Wireshark` oder `tcpdump` zur Verarbeitung von Paketen nutzen.

Dieses Dokument gibt einen Überblick über die wichtigsten Protokoll-Header mit deren Aufbau und Größen.

Weitere technische Informationen und Dokumentationen zu Protokollen findest du unter [https://www.rfc-editor.org/](https://www.rfc-editor.org/).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Layer 2 Protokolle


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


###  Ethernet II (Layer 2)

- **Zweck:** Transport von Frames innerhalb eines LANs.
- **Header-Größe:** 14 Bytes = 112 Bit

```text
+----------------+----------------+----------------+
| Ziel-MAC (6B)  | Quell-MAC (6B) | EtherType (2B) |
+----------------+----------------+----------------+
```





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


###  ARP Header (Layer 2/3)

- **Zweck:** Auflösung von IP-Adressen zu MAC-Adressen. Obwohl es die IP-Adresse verwendet, ist ARP ein Layer-2-Protokoll, da es direkt auf der Sicherungsschicht operiert.
- **Header-Größe:** 28 Bytes

```text
+----------------------+---------------------------+
| Hardware Type (2B)   | Protocol Type (2B)        |
+----------------------+---------------------------+
| HW Size (1B) | Prot Size (1B) | Opcode (2B)      |
+----------------------+---------------------------+
| Sender MAC Address (6B)                          |
+--------------------------------------------------+
| Sender IP Address (4B)                           |
+--------------------------------------------------+
| Target MAC Address (6B)                          |
+--------------------------------------------------+
| Target IP Address (4B)                           |
+--------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Layer 3 Protokolle

### IPv4 Header (Layer 3)

- **Zweck:** Routing von Paketen über Netzwerke hinweg.
- **Header-Größe:** 20-60 Bytes (Minimum 20 Bytes, da die "Options" optional sind).

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |   TOS/DSCP    |         Total Length          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|        Identification         | Flags |    Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      TTL      |    Protocol   |        Header Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Source IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Destination IP Address                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Options                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### IPv6 Header (Layer 3)

- **Zweck:** Nachfolger von IPv4, 128-Bit Adressen.
- **Header-Größe:** Fix 40 Bytes (10 Words à 32 Bit)

```text
0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Vers.| Traffic Class (8b)| Flow Label (20b)                    |
+---------------------------------------------------------------+
| Payload Length (2 B)         | Next Header(1B)| Hop Limit (8b)|
+------------------------------+----------------+---------------+
| Source Address (16 B)                                         |
+---------------------------------------------------------------+
| Destination Address (16 B)                                    |
+---------------------------------------------------------------+
| Options (0-40 B, optional)                                    |
+---------------------------------------------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### ICMP Header (Layer 3)

- **Zweck:** Diagnose & Fehlerberichte (Ping, Traceroute).
- **Header-Größe:** min. 8 Bytes (Standard, ohne Zusatzfelder)

```text
0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Type (1B)     | Code (1B)     | Checksum (16b / 2B)           |
+---------------+---------------+-------------------------------+
| Rest of Header (4 B)                                          |
+---------------------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Layer 4 Protokolle


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### TCP Header (Layer 4)

- **Zweck:** Verbindungsorientierte Kommunikation.
- **Header-Größe:** 20–60 Bytes (5–15 Words à 32 Bit)

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+---------------------------------------------------------------+
|                        Sequence Number                        |
+---------------------------------------------------------------+
|                     Acknowledgment Number                     |
+---------------------------------------------------------------+
| Data Offset | Reserved  |C|E|U|A|P|R|S|F|     Window Size     |
+---------------------------------------------------------------+
|         Checksum              |      Urgent Pointer           |
+---------------------------------------------------------------+
|                        Options (if any)                       |
+---------------------------------------------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### UDP Header (Layer 4)

- **Zweck:** Verbindungsloser Transport (z. B. DNS, DHCP).
- **Header-Größe:** 8 Bytes = 2 Words à 32 Bit

```text
0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Source Port (2 B)             | Dest Port (2 B)               |
+-------------------------------+-------------------------------+
| Length (2 B)                  | Checksum (2 B)                |
+-------------------------------+-------------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Layer 5-7 Protokolle



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### DHCP (über UDP 67/68) (Layer 5-7)

- **Zweck:** Automatische Zuweisung von IP-Adressen und Netzwerkkonfiguration.
- **Header-Größe:** 236 Bytes + Optionen.
- **Transport:** UDP 67 (Server) und 68 (Client).

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     op (1B)   |   htype (1B)  |   hlen (1B)   |   hops (1B)   |
+---------------+---------------+---------------+---------------+
|                            xid (4B)                           |
+---------------------------------------------------------------+
|           secs (2B)           |           flags (2B)          |
+---------------------------------------------------------------+
|                          ciaddr  (4B)                         |
+---------------------------------------------------------------+
|                          yiaddr  (4B)                         |
+---------------------------------------------------------------+
|                          siaddr  (4B)                         |
+---------------------------------------------------------------+
|                          giaddr  (4B)                         |
+---------------------------------------------------------------+
|                                                               |
|                          chaddr  (16B)                        |
|                                                               |
|                                                               |
+---------------------------------------------------------------+
|                                                               |
|                          sname   (64B)                        |
+---------------------------------------------------------------+
|                                                               |
|                          file    (128B)                       |
+---------------------------------------------------------------+
|                                                               |
|                          options (variabel)                   |
+---------------------------------------------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### DNS Header (UDP/TCP Port 53) (Layer 5-7)

- Zweck: Namensauflösung (Domain → IP).
- Header-Größe: 12 Bytes + Queries/Responses
- Transport: UDP/TCP 53

```text
+---------------------------------------------------------------+
|            Transaction ID (2B)           |       Flags (2B)   |
+---------------------------------------------------------------+
|            Questions (2B)                |    Answer RRs (2B) |
+---------------------------------------------------------------+
|           Authority RRs (2B)             | Additional RRs (2B)|
+---------------------------------------------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### HTTP/HTTPS (Layer 5-7)

- **Zweck:** Web-Kommunikation (Text-basiert).
- **Transport:** TCP 80 (HTTP), TCP 443 (HTTPS via TLS/SSL)
- **Header:** Variabel (Textzeilen mit Feldern wie Host, User-Agent, Cookie, …)

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: */*
```





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### TLS/SSL Record Layer (Layer 5-7)

- **Zweck:** Verschlüsselte Kommunikation (HTTPS, SMTPS, FTPS).
- **Header-Größe:** 5 Bytes + Payload
- **Transport:** Wird über TCP getunnelt, typischerweise auf Port 443.

```text
+-------------------+----------------+---------------------+
| Content Type (1B) | Version (2B)   |     Length (2B)     |
+-------------------+----------------+---------------------+
|                     Encrypted Payload (variabel)         |
+----------------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### SSH 

- **Zweck:** Sichere Remote-Verbindungen und Tunneling.
- **Transport:** TCP 22
- **Header:** Kein fester Header wie bei IP oder TCP. Der erste Teil des Protokolls ist ein einfacher Versionsstring (SSH-2.0-OpenSSH_...), gefolgt von einer binären Nachrichtenstruktur, die in der Regel nach dem Schlüsselaustausch verschlüsselt ist.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### NTP (Network Time Protocol)

- **Zweck:** Synchronisation der Systemzeit von Computern.
- **Transport:** UDP 123
- **Header-Größe:** 48 Bytes

```text
+-----+-----+------+----------+----------+----------+
| LI  | VN  | Mode | Stratum  |  Poll    | Precision|
+-----+-----+------+----------+----------+----------+
|        Root Delay (4B)      | Root Dispersion (4B)|
+---------------------------------------------------+
|               Reference ID (4B)                   |
+---------------------------------------------------+
|               Reference Timestamp (8B)            |
+---------------------------------------------------+
|               Originate Timestamp (8B)            |
+---------------------------------------------------+
|               Receive Timestamp (8B)              |
+---------------------------------------------------+
|               Transmit Timestamp (8B)             |
+---------------------------------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Kerberos (Authentication) (Layer 5-7)

- **Zweck:** Netzwerk-Authentifizierung (oft in Windows-AD, SSO).
- **Transport:** meist über TCP/UDP Port 88
- **Header-Größe:** variabel (ASN.1-codiert, meist 20–200+ Bytes)

**Kerberos Messages:**
- AS-REQ / AS-REP (Authentication Service)
- TGS-REQ / TGS-REP (Ticket Granting Service)
- AP-REQ / AP-REP (Application Service)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### RADIUS (Layer 5-7)

- **Zweck:** Zentrale Authentifizierung, Autorisierung und Accounting (AAA).
- **Transport:** UDP 1812 (Auth), UDP 1813 (Accounting).
- **Header-Größe:** 20 Bytes + Attribute

```text
+--------------+---------------+--------------------+
| Code (1B)    | ID (1B)       |    Length (2B)     |
+--------------+---------------+--------------------+
|                    Authenticator (16B)            |
+---------------------------------------------------+
|                 Attributes (variabel)             |
+---------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### IPSec (Internet Procotol Security) (Layer 5-7)

- **Zweck:** Bietet Authentifizierung und/oder Verschlüsselung auf IP-Ebene.
- **Transport:** Integriert in IP (Protokollnummern 50 für ESP und 51 für AH).
- **Header-Typen:**
  - **AH (Authentication Header):** 24 Bytes
  - **ESP (Encapsulating Security Payload):** variabel (mind. 8 Bytes + Payload + Padding + Auth Data)

#### AH Header

```text
+---------------+---------------+-------------------+
| Next Header(1B) | Payload Len(1B) | Reserved (2B) |
+---------------------------------------------------+
|                    SPI (4B)                       |
+---------------------------------------------------+
|             Sequence Number (4B)                  |
+---------------------------------------------------+
|        Authentication Data (variabel)             |
+---------------------------------------------------+
```

#### ESP Header
- **Header-Größe:** Mindestens 8 Bytes + verschlüsselte Nutzdaten + optionale Padding und Authentifizierungsdaten.

```text
+---------------------------------------------------+
|                    SPI (4B)                       |
+---------------------------------------------------+
|               Sequence Number (4B)                |
+---------------------------------------------------+
|                Payload (variabel)                 |
+---------------------------------------------------+
|        Padding | Pad Len | Next Header (1B)       |
+---------------------------------------------------+
|           Authentication Data (variabel)          |
+---------------------------------------------------+

```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### SMB (Server Message Block)

- **Zweck:** Datei- und Druckerfreigaben, häufiges Angriffsziel (EternalBlue, WannaCry).
- **Transport:** TCP 445.
- **Header-Größe:**
  - 32 Bytes (SMBv1),
  - 64 Bytes (SMBv2/3)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### SNMP (Simple Network Management Protocol)

- **Zweck:** Überwachung und Verwaltung von Netzwerkgeräten.
- **Transport:** UDP 161 (Manager zu Agent), UDP 162 (Traps).
- **Header:** ASN.1-basiert (daher variabel, min. ca. 20 Bytes).

 
 
<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Syslog

- **Zweck:** Standard für die zentrale Übertragung von Log-Nachrichten.
- **Transport:** UDP 514 (traditionell), TCP/TLS möglich
- **Header:** Eine einfache Textzeile, variabel.

```text
<PRI> TIMESTAMP HOSTNAME TAG: MESSAGE
```





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### ARBITRARY PROTOCOLS (SMB, IPSec, Kerberos, SNMP, Syslog)

Diese Protokolle haben sehr variable Header-Aufbauten (häufig mit ASN.1 oder Block-Formaten). 

Eine 32-Bit-Wort-Darstellung wäre extrem komplex und übersteigt den Rahmen.



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
