# 🧪 Labnet Testumgebung


## Inhaltsverzeichnis
- [Aufbau eines Test-Labnets](#aufbau-eines-test-labnets)
- [Architekturübersicht](#architekturübersicht)
- [Sicherheitskonzept](#sicherheitskonzept)
- [Tools & Rollenverteilung](#tools--rollenverteilung)
- [Nächste Verbesserungen](#nächste-verbesserungen)
- [Weitere Ideen](#weitere-ideen)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Aufbau eines Test-Labnets

Aufbau eines Test-Labs mit **3-virutellen Maschinen und einer Gatewayverbindung über pfSense** (Firewall/NAT/Tor).

Die virtuellen Maschinen sind vom Host abgeschottet. Drag&Drop-Funktionen sowie Gasterweiterungen wurden bidirektional deaktiviert.

Die VMs greifen über Tor-Tunnelung und Proxychains auf das Internet zu.

Alle VMs haben eine statische IP-Adresse und sind im gleichen Subnetz.


| Bereich                                           | Kommentar                                                                                   |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Netzwerkisolation (Host <-> VM)**               | Kein Drag & Drop, keine Gasterweiterung – das minimiert Risiko von Datenlecks.              |
| **Internes Netzwerk (VirtualBox)**                | „Internes Netzwerk“ verhindert automatisch Kommunikation mit dem Host und externen Geräten. |
| **Internetzugang via Proxychains & Tor**          | Ideal für anonymisierte Recon & Angriffe.                                                   |
| **pfSense-Firewall als zentrale Kontrollinstanz** | Zentralisierung der Internetfreigabe erhöht Kontrolle und Logging-Möglichkeiten.            |
| **Keine Internetverbindung ohne Kontrolle**       | Das verhindert unbemerkte Outbound-Calls.                                                   |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

 
## Architekturübersicht

```text
┌──────────────────────────────┐
│   Windows 10 Host (isoliert) │
│ - Kein Netzwerk zu VMs       │
│ - Drag&Drop/Clipboard: aus   │
│ - Keine Gasterweiterungen    │
└────────────┬─────────────────┘
             │
             ▼
┌───────────────────────────────────────────┐
│   Interne VirtualBox-Umgebung (Labnet)    │
│                                           │
│  ┌────────────┐     ┌──────────────────┐  │
│  │    Kali    │ <-> │  Metasploitable  │  |
│  └─────▲──────┘     └────▲─────────────┘  │
│        │                 │                │
│        ▼                 ▼                │
│     ┌──────────────────────────┐          │
│     │   Parrot OS (Red/Blue)   │          │
│     └──────────────────────────┘          │
│             │                             │
│             ▼                             │
│      ┌──────────────────────┐             │
│      │   pfSense Gateway    │             │
│      │ - Firewall/NAT/DHCP  │             │
│      │ - ProxyChains + Tor  │             │
│      └──────────────────────┘             │
└───────────────────────────────────────────┘

```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitskonzept
| Feature                                    | Status                          |
| ------------------------------------------ | --------------------------------|
| **Host-VM-Kommunikation**                  | ❌ Verboten (internes Netzwerk) |
| **Copy/Paste & Gasterweiterung**           | ❌ Deaktiviert                  |
| **Internetzugang**                         | ✔️ Nur über pfSense / Tor        |
| **VM-IP-Konfiguration**                    | ✔️ Statisch (nicht DHCP-basiert) |
| **Externe Erreichbarkeit**                 | ❌ Komplett unterbunden         |
| **Interne Kommunikation (z.B. Exploits)**  | ✔️ Erlaubt                       |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Tools & Rollenverteilung

| VM             | Betriebssystem | Rolle                | Tools                              |
| -------------- | -------------- | -------------------- | ---------------------------------- |
| Kali           | Linux          | Angreifer            | `nmap`, `metasploit`, `gobuster`   |
| Metasploitable | Linux          | Zielsystem           | Verwundbare Dienste                |
| Parrot         | Linux          | Dualrolle (Red/Blue) | `wireshark`, `burpsuite`, `splunk` |
| pfSense        | BSD            | Gateway/Firewall     | DNS Forwarding, NAT, Tor Routing   |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nächste Verbesserungen

| Thema                                         | Verbesserung                                                       | Warum?                                                                           |
| --------------------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| **Tor-Exit-Nodes**                            | Logging auf pfSense + DNS-Traffic analysieren                      | Tor kann geleakt werden (z.B. bei DNS-Leaks). `dnsproxy`/`dnscrypt` hinzufügen.  |
| **VM Snapshots**                              | Dokumentieren & regelmäßig nutzen                                  | Falls Malware getestest, kannst verseuchte Umgebungen schnell zurückrollen.      |
| **Logging & Monitoring**                      | Nutzen von Security Onion oder ELK Stack in einer VM               | Angriffe besser nachvollziehen und lernen können.                                |
| **Bridging für gezielte Angriffe (optional)** | Isolierte „Opfer-VMs“ über anderes Subnetz bridgen (z.B. IoT-VMs)  | Für fortgeschrittene Tests von Netzwerk-Pivoting.                                |
| **Firmware/BIOS-Zugriff**                     | Host absichern (BIOS-Passwort, USB-Blocking)                       | Wenn Angriffe simuliert werden: Schutz für physischen Layer.                     |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Monitoring

- ELK oder Security Onion in einer VM für Netzwerk- und Angriffsanalyse installieren.
- Zeek/Bro für Netzwerk-Traffic nutzen.
- Tor- & Proxy-Verbindungen, z.B. per torsocks-Integration loggen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Weitere Ideen

- Integration eines DNS-Sinkholes
- Integration von intentionally vulnerable Web-Apps (z.B. DVWA, Juice Shop)
- Reverse Engineering Umgebung (Ghidra, Radare2) auf Parrot VM
- File Server mit SMB für Lateral Movement Tests
- Integration von Active Directory in Windows VM (für Realismustests)




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
