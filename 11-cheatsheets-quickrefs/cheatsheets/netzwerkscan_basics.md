# ⚙️ Kommandozeilen Basics auf Linux

## Inhaltsverzeichnis
- [Netzwerk Scans (Klassisch)](#netzwerk-scans-klassisch)
- [Netzwerk Scans (Moderne Tools)](#netzwerk-scans-moderne-tools)
- [Erweiterte Tools für Netzwerk-Discovery](#erweiterte-tools-für-netzwerk-discovery)
- [Tipps zur Netzwerkanalyse](#tipps-zur-netzwerkanalyse)
- [Bonus: Interface-Troubleshooting](#bonus-interface-troubleshooting)
- [Empfohlene Reihenfolge für Beginner](#empfohlene-reihenfolge-für-beginner)
- [Haftungsausschluss](#haftungsausschluss)

---

## Netzwerk Scans (Klassisch)

| Befehl       | Beschreibung                                                                           |
| ------------ | -------------------------------------------------------------------------------------- |
| `ifconfig`   | Zeigt IP-Adressen, Netzwerkkarten, MAC-Adressen. (deprecated, aber oft noch vorhanden) |
| `iwconfig`   | Wireless-Konfiguration anzeigen (SSID, Mode, Signalstärke etc.)                        |
| `ping [IP]`  | ICMP Echo Request an Host senden – funktioniert Ziel?                                  |
| `arp`        | Zeigt ARP-Tabelle (MAC-Adressen zu IPs im lokalen Netz)                                |
| `netstat`    | Listet offene Verbindungen und Ports (deprecated, ersetzt durch `ss`)                  |
| `route`      | Zeigt die Routing-Tabelle (Gateway etc.)                                               |
| `traceroute` | Zeigt den Weg (Hops) zum Zielhost im Netzwerk                                          |
| `tracert`    | Windows-Version von `traceroute`                                                       |

---

## Netzwerk Scans (Moderne Tools)

| Befehl | Beschreibung                                                      |
| ------ | ----------------------------------------------------------------- |
| `ip a` | Zeigt Netzwerkschnittstellen und IP-Adressen (ersetzt `ifconfig`) |
| `ip n` | Zeigt ARP-Nachbarn (wie `arp -a`)                                 |
| `ip r` | Zeigt Routing-Tabelle (ersetzt `route`)                           |

---

## Erweiterte Tools für Netzwerk-Discovery

### `netdiscover`

Automatischer ARP-Scan im lokalen Netzwerk zur Erkennung aktiver Hosts.

```bash
netdiscover -i eth0
```

### `nmap`

Vielseitiges Netzwerkscan-Tool:

```bash
# Ping Scan – Zeigt aktive Hosts:
nmap -sn 192.168.1.0/24

# Standard Scan mit Skripten & Versionsinfos:
nmap -sC -sV 192.168.1.102

# Alle 65535 TCP-Ports scannen:
nmap -p- 192.168.1.102

# Betriebssystem erkennen:
nmap -O 192.168.1.102
```

-> [Mehr Infos zu `nmap` hier](/02-network-security/tools/nmap.md)

### `arp-scan`

Ein schneller ARP-basiertes Tool für lokale Netzwerke:

```bash
sudo arp-scan --interface=eth0 192.168.1.0/24
```

---

## Tipps zur Netzwerkanalyse

- Stelle sicher, dass du Root-Rechte hast (`sudo`) bei Tools wie `nmap`, `arp-scan` oder `netdiscover`.
- Nutze `ip a`, um deine eigene IP und Subnetz zu identifizieren.
- Ziel: Aktive Hosts finden, deren offene Ports identifizieren, Dienste erkennen, Exploits vorbereiten.
- Kombination aus: **Ping**, **ARP**, **DNS**, **Portscanning**, **Banner-Grabbing** = perfekte Recon.

---

## Bonus: Interface-Troubleshooting

```bash
# Liste aller Netzwerkschnittstellen:
ip link show

# Interface aktivieren:
sudo ip link set eth0 up

# Neue IP per DHCP beziehen:
sudo dhclient eth0
```

---

## Empfohlene Reihenfolge für Beginner

1. `ip a` -> Eigene IP & Subnetz erkennen
2. `netdiscover` oder `nmap -sn` -> Aktive Hosts finden
3. `nmap -sC -sV` -> Dienste & Versionen erkennen
4. `traceroute` -> Netzwerkstruktur analysieren
5. `arp -a` oder `ip n` -> MAC-zu-IP-Zuordnung verstehen

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
