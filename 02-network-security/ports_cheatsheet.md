# 🌐 Ports & ihre Funktionsweise – Cheat Sheet


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



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einführung
Ein **Port** ist eine logische Schnittstelle in einem Betriebssystem, die als **Kommunikationsendpunkt** für Anwendungen dient.
Zusammen mit einer IP-Adresse bildet ein Port eine **Socket-Adresse** (`IP:Port`).
Dadurch können mehrere Dienste gleichzeitig auf einer Maschine laufen, auch wenn nur eine IP vorhanden ist.

**Beispiel:**
- `192.168.0.10:80` -> HTTP-Webserver
- `192.168.0.10:22` -> SSH-Verbindung



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Funktionsweise von Ports
- Ports sind **16-Bit-Werte** (0–65535).
- Sie ermöglichen **Multiplexing**, d. h. mehrere Anwendungen können gleichzeitig Netzwerkressourcen nutzen, indem sie über unterschiedliche Ports kommunizieren.
- Betriebssysteme unterscheiden:
  - **TCP-Ports** -> verbindungsorientiert (SYN/ACK-Handshake)
  - **UDP-Ports** -> verbindungslos (schneller, aber unsicherer)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Port-Bereiche

| Bereich             | Nummern        | Zweck |
|---------------------|----------------|-------|
| **Well-Known Ports** | 0 – 1023       | Standard-Dienste (HTTP, FTP, DNS, SSH), oft Root-Rechte erforderlich |
| **Registered Ports** | 1024 – 49151   | Von Software/Herstellern registriert (z. B. MySQL 3306, RDP 3389) |
| **Dynamic/Ephemeral Ports** | 49152 – 65535 | Temporär von Clients für ausgehende Verbindungen genutzt |

**Hinweis:** Ephemere Ports werden vom **Client** automatisch zugewiesen, um eine **Antwort** vom Server empfangen zu können.



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
| 53 | UDP | DNS (Domain Name System) |
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


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Ports & Firewalls
- Firewalls kontrollieren, welche Ports für **eingehenden und ausgehenden** Datenverkehr genutzt werden dürfen.
- Eine Firewall kann Ports öffnen (`allow`), schließen (`deny`) oder den Datenverkehr einschränken (`rate-limit`)
- **Beispiel:**
  - **Regel:** Eingehender Traffic auf Port 22 ist erlaubt.
  - **Ergebnis:** Remote-Anmeldungen per SSH sind möglich.
  - **Regel:** Alle anderen Ports blockieren (Default-Deny).
  - **Ergebnis:** Das System ist vor unnötigen Angriffsvektoren geschützt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Ports & Sicherheit
- **Port Scanning** (z. B. mit `nmap`) ist eine gängige Technik, um offene Ports auf einem Zielsystem zu identifizieren und die darauf laufenden Dienste zu erkennen.
- **Gefahren offener Ports:**  
  - **Angriffsvektor:** Ein offener, unsicherer Dienst (z. B. SMB auf Port 445) kann ausgenutzt werden, wie es bei der Ransomware WannaCry der Fall war.
  - **Unnötige Exposition:** Jeder offene Port ist ein potenzieller Eintrittspunkt für Angreifer.
- **Best Practice:**  
  - **Default-Deny:** Schließe alle Ports, die nicht explizit für einen Dienst benötigt werden.
  - **Patch-Management:** Halte alle auf den Ports laufenden Dienste und deren Software auf dem neuesten Stand.
  - **Monitoring:** Setze Intrusion Detection/Prevention Systeme (IDS/IPS) ein, um ungewöhnliche Zugriffe zu erkennen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Beispiel: Verbindungsaufbau TCP vs. UDP

### TCP (z. B. HTTP, SSH)
- **Verbindungsorientiert** und zuverlässig. Es findet ein "**Handshake**" statt, um sicherzustellen, dass die Verbindung aufgebaut ist und Daten korrekt ankommen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Der 3-Wege-Handshake
1. Client sendet ein **SYN-Paket** vom ephemeren Port an den Server-Port.
2. Server empfängt SYN, antwortet mit **SYN-ACK**.
3. Client empfängt SYN-ACK und bestätigt mit **ACK**.

```text
+-------------+                     +-------------+
|    Client   |                     |    Server   |
+-------------+                     +-------------+
      |                                   |
      |------ SYN (Synchronize) --------->|
      |                                   |
      |<---- SYN-ACK (Sync-Acknowledge)---|
      |                                   |
      |------ ACK (Acknowledge) --------->|
      |                                   |
(Verbindung steht, Datenübertragung kann beginnen)
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### UDP (z. B. DNS, VoIP)
- **Verbindungslos** und schnell. Es gibt keinen Handshake oder eine Bestätigung, was die Datenübertragung sehr effizient macht.

1. Client sendet ein UDP-Paket vom ephemeren Port an den Server-Port.
2. Server empfängt die Anfrage und antwortet direkt mit einem UDP-Paket.
3. Es gibt keine Garantie, dass das Paket ankommt oder in der richtigen Reihenfolge empfangen wird.

```text
+-------------+                     +-------------+
|    Client   |                     |    Server   |
+-------------+                     +-------------+
      |                                   |
      |------ UDP-Paket (Anfrage) ------->|
      |                                   |
      |<----- UDP-Paket (Antwort) --------|
      |                                   |
(Es gibt keine Bestätigung, dass das Paket ankam)
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Zusammenfassung
- Ports sind **16-Bit-Nummern (0–65535)**, die Anwendungen eindeutige Kommunikationskanäle bereitstellen.  
- Es gibt drei Bereiche: **Well-Known**, **Registered**, **Ephemeral**.  
- **TCP** ist zuverlässig und verbindungsorientiert, während **UDP** schnell und verbindungslos ist.  
- Offene Ports müssen durch Firewalls und Patch-Management abgesichert werden, um Angriffsvektoren zu minimieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [firewall_cheatsheet.md](02-network-security/firewall_cheatsheet.md) - Einführung in Firwalls
- [tcp_ip_basics.md](tcp_ip_basics.md) - Einführung in TCP/IP
- [osi_schichtenmodell.md](osi_schichtenmodell.md) - Einführung in das OSI-Schichtenmodell
- [ip_adressen_basics.md](ip_adressen_basics.md) - Einführung in das Thema IP-Adressen


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

📅 **Letzte Aktualisierung:** August 2025  
🤝 Ergänzungen und Pull Requests sind willkommen!
