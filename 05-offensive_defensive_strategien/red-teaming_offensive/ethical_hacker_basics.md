# Ethical Hacker Basics

## Inhaltsverzeichnis
- [1. Basic Skills](#1-basic-skills)
- [2. Dokumentation](#2-dokumentation)
- [3. Netzwerke](#3-netzwerke)
- [4. Tools und Vorgehensweise im Pentest](#4-tools-und-vorgehensweise-im-pentest)
- [5. Rechtliches (DE/AT/CH)](#5-rechtliches-deatch)
- [6. Weiterführende Lernquellen](#6-weiterführende-lernquellen)
- [Bücher](#bücher)
- [YouTube-Kanäle](#youtube-kanäle)
- [Haftungsausschluss](#haftungsausschluss)

---

## 1. Basic Skills

### Technische Skills

1. Linux (Kali/Parrot bevorzugt)
2. Networking (OSI Modell, Protokolle, etc.)
3. Scripting Erfahrung (Python, Bash, etc.)
4. Solide Hacking Methoden
5. Wissen von Tools (Metasploit, Burp Suite, Nessus, etc.)
6. Umgang mit VMs (z. B. VirtualBox, VMware, Proxmox)
7. Kenntnisse in Webtechnologien (HTML, JavaScript, HTTP/HTTPS, Cookies, Sessions)

### Soft Skills

1. Wissendurstig
2. Socialskills
3. Dokumentation (Blog, GitHub)
4. Ausdauer
5. Kritisches und analytisches Denken

---

## 2. Dokumentation

Am Anfang ist es wichtig, sich Notizen über seine Arbeit und das Gelernte zu machen.

Außerdem ergibt es Sinn, seine Aufschriebe in Themen zu kategoriesieren und je nach Thema eine eigene Datei zu erstellen.

* [KeepNote - Zum Organisieren von Notizen](https://keepnote.org) (optional)
* [Greenshot - Für Screenshots](https://getgreenshot.org/) (optional)
* [Obsidian - Markdown-basierte Notizverwaltung](https://obsidian.md/) (empfohlen)
* GitHub Repositories für Versionierung

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Netzwerke

Ein Ethical Hacker muss im Begriff der folgenden Themen sein.

1. IP Adressen (v4, v6)
2. MAC Adressen
3. TCP, UDP und der Three-Way-Handshake
4. Generelle Ports und Protokolle
5. Das OSI-Modell
6. Subnetting
7. DNS, DHCP, ARP, NAT und VPNs

### IP Adressen

* Es gibt 2 Arten von IP Adressen:

  * IPv4, mit 32 Bit
  * IPv6, mit 128 Bit
* IP Adressen bestehen aus Oktetten

  * IPv4 aus 4
  * IPv6 aus 8
* NAT (Network Adress Translation) ist für Zuweisung von privaten Adressen zuständig.
* private Adressbereiche (nicht öffentlich genutzt, reserviert für private LAN):

  * **Klasse A:** 10.0.0.0/8 - 10.255.255.255/8
  * **Klasse B:** 172.16.0.0/16 - 172.31.255.255/16
  * **Klasse C:** 192.168.0.0/16 - 192.168.255.255/16
  * **Loopback:** (Localhost) 127.0.0.0 - 127.255.255.255
* IP werden im Layer 3 OSI-Modell benötigt und vergeben/angesprochen

#### Bit Liste einer v4 Adresse

Mit Bits werden die Netzwerke und IP Adressen berechnet.
Eine Übersicht über die Bits eines IPv4-Adresse findest du hier:

**Hinweis:** Eine IP Adresse ergibt sich aus einer Kombination im einzelnen Oktett

| Bits |    |    |    |   |   |   |   |
| :--- | -- | -- | -- | - | - | - | - |
| 128  | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| 1    | 1  | 1  | 1  | 1 | 1 | 1 | 1 |

In diesem Beispiel sind alle Bits auf 1 gesetzt.

### MAC Adressen

* MAC Adressen sind gerätespezifische, einmalige Identifikationsnummern.
* Die MAC Adresse ist in OUI (Organizationally Unique Identifier) und einer DI (Device Identifier) gekennzeichnet

  * Beispiel: 1A:2B:3C:4D:5E:6F
* MAC Adressen sind im Layer 2 des OSI-Modell

### TCP/UDP

* **TCP (Transmission Control Protocol)**

  * Verbindungsorientiertes Protokoll
  * Drei-Wege-Handshake (SYN → SYN/ACK → ACK)
  * Zuverlässige Übertragung
  * Beispiele für Ports:

    * FTP 21
    * SSH 22
    * Telnet 23
    * SMTP 25
    * DNS 53
    * HTTP 80 / HTTPS 443
    * POP3 110
    * SMB 139 + 445
    * IMAP 143

* **UDP (User Datagram Protocol)**

  * Verbindungsloses Protokoll
  * Schneller, aber ohne Garantie für Zustellung
  * Beispiele für Ports:

    * DNS 53
    * DHCP 67, 68
    * TFTP 69
    * SNMP 161

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 4. Tools und Vorgehensweise im Pentest

### Vorbereitung

1. Zieldefinition (Scope, Legal Agreement!)
2. Informationssammlung (Reconnaissance)
3. Aktive und passive Erkundung

### Tools für Reconnaissance

* `whois`, `dig`, `nslookup`, `theHarvester`, `Shodan`
* `nmap`, `masscan`, `netcat`, `nikto`

### Schwachstellenscans

* `Nessus`, `OpenVAS`, `Nmap Scripts`, `Burp Suite`

### Exploitation

* `Metasploit`, `sqlmap`, `hydra`, `john`, `crackmapexec`

### Privilege Escalation

* `LinPEAS`, `WinPEAS`, `GTFOBins`, `sudo -l`, `SUID`

### Post Exploitation

* Persistence
* Data Exfiltration
* Lateral Movement (Pivoting)

---

## 5. Rechtliches (DE/AT/CH)

* Penetration Testing darf **nur mit schriftlicher Erlaubnis** erfolgen
* Ohne Genehmigung handelt es sich um **Computerstraftaten (§202a ff. StGB DE)**
* Vereinbare einen **Scope** und eine **Haftungsausschlussvereinbarung** (Letter of Engagement)
* Informiere dich über geltende Gesetze, z. B.:

  * DE: §202a-d StGB, DSGVO, BDSG
  * AT: §118a StGB
  * CH: Art. 143-147 StGB

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Weiterführende Lernquellen

### Plattformen

* [TryHackMe](https://tryhackme.com/)
* [Hack The Box](https://www.hackthebox.com/)
* [PortSwigger Web Security Academy](https://portswigger.net/web-security)
* [CTFtime.org](https://ctftime.org/)

### Bücher

* "The Hacker Playbook" (1–3)
* "Red Team Field Manual (RTFM)"
* "Web Hacking 101"

### YouTube-Kanäle

* NetworkChuck
* The Cyber Mentor
* LiveOverflow
* John Hammond

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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---