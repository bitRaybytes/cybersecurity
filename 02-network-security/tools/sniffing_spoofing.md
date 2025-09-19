# 🕵️ Sniffing & Spoofing – Grundlagen, Techniken & Schutz



## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Begriffe & Definitionen](#begriffe--definitionen)
- [Sniffing – Überwachung des Netzverkehrs](#sniffing--überwachung-des-netzverkehrs)
- [Spoofing - Identitätsfälschung im Netzwerk](#spoofing---identitätsfälschung-im-netzwerk)
- [Beispielangriff: ARP Spoofing mit Ettercap](#beispielangriff-arp-spoofing-mit-ettercap)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Lernressourcen](#lernressourcen)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

**Sniffing** und **Spoofing** gehören zu den klassischen Angriffstechniken in der Netzwerksicherheit. Während Sniffing das **Abfangen von Netzwerkdaten** beschreibt, steht Spoofing für das **Fälschen von Identitäten** (z. B. IP, MAC oder DNS).

Diese Techniken sind oft **Basis von Man-in-the-Middle (MITM)-Angriffen** und können schwerwiegende Folgen haben, wenn keine geeigneten Schutzmaßnahmen getroffen werden.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Begriffe & Definitionen

| Begriff         | Beschreibung                                                                 |
|------------------|------------------------------------------------------------------------------|
| Sniffing         | Abhören und Aufzeichnen von Netzwerkverkehr                                 |
| Passive Sniffing | Lauschen im promiskuitiven Modus ohne Paketänderung                         |
| Active Sniffing  | Eingreifen in Netzwerkkommunikation, z. B. durch ARP-Spoofing               |
| Spoofing         | Vortäuschung einer falschen Identität im Netzwerk                          |
| MITM             | Angreifer sitzt zwischen Opfer und Ziel und leitet Daten (manipuliert sie ggf.) |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sniffing – Überwachung des Netzverkehrs

### Voraussetzungen
- Gerät muss sich im selben Netzwerksegment befinden
- Netzwerkkarte im **Promiscuous Mode**
- Switches verhindern Sniffing → muss aktiv umgangen werden (z. B. ARP-Spoofing)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Gängige Tools

| Tool         | Beschreibung                                  |
|--------------|-----------------------------------------------|
| Wireshark    | GUI-Netzwerksniffer mit tiefgehender Analyse  |
| tcpdump      | CLI-Sniffer, extrem mächtig und präzise       |
| Tshark       | CLI-Version von Wireshark                     |
| Ettercap     | MITM-Angriffe + Sniffing + Spoofing           |
| Bettercap    | Moderne Alternative zu Ettercap               |
| MITMf        | Man-In-The-Middle Framework (nicht mehr aktiv gepflegt, aber lehrreich) |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Anwendungsbeispiele

```bash
# Promiscuous Mode aktivieren (Linux)
ip link set eth0 promisc on

# TCPDUMP: Alle HTTP-Pakete mitschneiden
tcpdump -i eth0 -A port 80

# WIRESHARK Filter: Nur HTTP
http.request
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Spoofing - Identitätsfälschung im Netzwerk

Arten von Spoofing

| Spoofing-Typ   | Beschreibung                                       |
| -------------- | -------------------------------------------------- |
| ARP Spoofing   | Fälschen von MAC-IP-Zuordnungen (Layer 2)          |
| IP Spoofing    | Pakete mit gefälschter Quell-IP                    |
| DNS Spoofing   | Falsche DNS-Antworten, z. B. zur Umleitung         |
| MAC Spoofing   | Geräteidentität durch neue MAC-Adresse vortäuschen |
| DHCP Spoofing  | Falsche DHCP-Server-Antworten versenden            |
| Email Spoofing | Absender-Adresse in Mails fälschen                 |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Tools & Techniken

- `arpspoof` (aus dsniff)
- `ettercap` -T -M arp (MITM)
- `bettercap` -iface eth0
- `macchanger` (MAC-Adresse fälschen)
- `dnsspoof` (DNS-Antworten manipulieren)
- `responder` (SMB/NetBIOS-Antwort-Fake für Hash Capture)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Beispielangriff: ARP Spoofing mit Ettercap

```bash
ettercap -T -M arp:remote /192.168.1.10/ /192.168.1.1/
```
→ Der Angreifer täuscht sowohl dem Opfer als auch dem Gateway vor, jeweils das andere zu sein → MITM-Angriff



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schutzmaßnahmen
Gegen Sniffing

- Switches statt Hubs einsetzen
- Port Security aktivieren (z. B. MAC-Locking)
- End-to-End-Verschlüsselung verwenden (HTTPS, SSH, VPN)
- Promiscuous-Mode-Erkennung:
```bash
    ip link show eth0
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Gegen Spoofing

| Maßnahme                      | Wirkung                                    |
| ----------------------------- | ------------------------------------------ |
| ARP-Inspection (Switches)     | Erkennt ARP-Spoofing                       |
| Static ARP Tables             | Kein ARP mehr → keine Manipulation         |
| DHCP Snooping                 | Nur vertrauenswürdige DHCP-Server zulassen |
| DNSSEC                        | Schützt gegen DNS-Spoofing                 |
| Zwei-Faktor-Authentifizierung | Schutz bei kompromittierten Verbindungen   |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Lernressourcen

- TryHackMe: „Packet Analysis“, „Wireshark“, „Network Attacks“
- HackTheBox: Labs mit Spoofing und Sniffing-Szenarien
- Wireshark Labs (kostenlos verfügbar)
- YouTube Channels: John Hammond, NetworkChuck – Sniffing Tutorials
- Buch: „Network Security Assessment“ von Chris McNab




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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
