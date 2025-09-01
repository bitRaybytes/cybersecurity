# 🌐 Ports & ihre Funktionsweise – Cheat Sheet

---

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Funktionsweise von Ports](#funktionsweise-von-ports)
- [Port-Bereiche](#port-bereiche)
- [Wichtige Standard-Ports (Well-Known)](#wichtige-standard-ports-well-known)
- [Ports & Firewalls](#ports--firewalls)
- [Ports & Sicherheit](#ports--sicherheit)
- [Beispiel: Verbindungsaufbau TCP vs. UDP](#beispiel-verbindungsaufbau-tcp-vs-udp)
- [Zusammenfassung](#zusammenfassung)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

---

## Einführung
Ein **Port** ist eine logische Schnittstelle in einem Betriebssystem, die als **Kommunikationsendpunkt** für Anwendungen dient.  
Zusammen mit einer IP-Adresse bildet ein Port eine **Socket-Adresse** (`IP:Port`).  
Dadurch können mehrere Dienste gleichzeitig auf einer Maschine laufen, auch wenn nur eine IP vorhanden ist.

👉 Beispiel:  
- `192.168.0.10:80` -> HTTP-Webserver  
- `192.168.0.10:22` -> SSH-Verbindung  

---

## Funktionsweise von Ports
- Ports sind **16-Bit-Werte** (0–65535).  
- Sie ermöglichen **Multiplexing**, d. h. mehrere Anwendungen können gleichzeitig Netzwerkressourcen nutzen.  
- Betriebssysteme unterscheiden:  
  - **TCP-Ports** -> verbindungsorientiert (SYN/ACK-Handshake)  
  - **UDP-Ports** -> verbindungslos (schneller, aber unsicherer)  

---

## Port-Bereiche

| Bereich             | Nummern        | Zweck |
|---------------------|----------------|-------|
| **Well-Known Ports** | 0 – 1023       | Standard-Dienste (HTTP, FTP, DNS, SSH) |
| **Registered Ports** | 1024 – 49151   | Von Software/Herstellern registriert (z. B. MySQL 3306, RDP 3389) |
| **Dynamic/Ephemeral Ports** | 49152 – 65535 | Temporär von Clients für ausgehende Verbindungen genutzt |

👉 Hinweis: Ephemere Ports werden vom **Client** geöffnet, um eine **Antwort** vom Server zu ermöglichen.

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wichtige Standard-Ports (Well-Known)

| Port | Protokoll | Dienst |
|------|-----------|--------|
| 20 | TCP | FTP-Datenpbertragung |
| 21 | TCP/UDP | FTP-Steuerkanal |
| 22 | TCP/UDP | SSH, Secure Shell (Konsolensteuerung verschlüsselt) |
| 23 | TCP/UDP | Telnet (unsicher) |
| 25 | TCP | SMTP (E-Mail Versand) |
| 53 | UDP | DNS |
| 67/68 | UDP | DHCP (Client/Server) |
| 80 | TCP | HTTP (Webseiten) |
| 110 | TCP | POP3 (E-Mail Abruf) |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 389 | TCP/UDP | LDAP |
| 443 | TCP | HTTPS (verschlüsselt) |
| 445 | TCP | SMB (Dateifreigaben) |
| 909 | TCP | POP3 (verschlüsselt) |
| 993 | TCP | IMAP (verschlüsselt) |
| 1433 | TCP | MS SQL Server |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP (Remote Desktop) |
| 5900 | TCP | VNC |
| 8080 | TCP | HTTP Proxy / Alternative zu Port 80 |

> Well-Know-Ports sind **standardisierte Ports** im Bereich von 0 bis 1023, die von der **IANA (Internet Assigned Numbers Authority)** verwaltet werden.
> **Ports sind reserviert** und sollten nicht von eigenen Serverdiensten für andere Zwecke verwendet werden.

[WikiPedia: Liste der Portnummern](https://de.wikipedia.org/wiki/Liste_der_Portnummern)

---

## Ports & Firewalls
- Firewalls kontrollieren, welche Ports **eingehend/ausgehend** genutzt werden dürfen.  
- Beispiel:  
  - Eingehend auf Port 22 offen -> Remote-Login erlaubt  
  - Alles andere blockiert (Default-Deny)  

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Ports & Sicherheit
- **Port Scanning** (z. B. mit `nmap`) identifiziert offene Ports und Dienste.  
- **Gefahren offener Ports:**  
  - Angriffsvektor (z. B. SMB auf Port 445 → WannaCry)  
  - Schwachstellen durch alte/unsichere Dienste  
- **Best Practice:**  
  - Nur benötigte Ports öffnen  
  - Dienste aktuell halten  
  - IDS/IPS & Firewall einsetzen  

---

## Beispiel: Verbindungsaufbau TCP vs. UDP

### TCP (z. B. HTTP, SSH)
1. Client öffnet **ephemeren Port** (z. B. 50000)  
2. Verbindung zu Zielserver:Port (z. B. 192.168.0.1:80)  
3. **3-Way-Handshake**: SYN -> SYN/ACK -> ACK  
4. Datenübertragung  
5. Verbindung wird geordnet geschlossen  

### UDP (z. B. DNS, VoIP)
1. Client sendet Paket von **ephemerem Port** (z. B. 55000) an Zielserver:Port (z. B. 8.8.8.8:53)  
2. Server antwortet direkt zurück -> kein Handshake, keine Zustandsverwaltung  

---

## Zusammenfassung
- Ports sind **16-Bit-Nummern (0–65535)**, die Anwendungen eindeutige Kommunikationskanäle bereitstellen.  
- Es gibt drei Bereiche: **Well-Known, Registered, Ephemeral**.  
- TCP = verbindungsorientiert, UDP = verbindungslos.  
- Offene Ports müssen durch Firewalls und Patch-Management abgesichert werden.  

----

## Nützliche Links
- [firewall_cheatsheet.md](02-network-security/firewall_cheatsheet.md) - Einführung in Firwalls
- [tcp_ip_basics.md](tcp_ip_basics.md) - Einführung in TCP/IP
- [osi_schichtenmodell.md](osi_schichtenmodell.md) - Einführung in das OSI-Schichtenmodell
- [ip_adressen_basics.md](ip_adressen_basics.md) - Einführung in das Thema IP-Adressen

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

📅 **Letzte Aktualisierung:** August 2025  
🤝 Ergänzungen und Pull Requests sind willkommen!
