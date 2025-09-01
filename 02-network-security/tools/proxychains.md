# 🧰 Proxychains – Anonymität durch Kaskadierung von Proxys

---

## Inhaltsverzeichnis
- [Was ist Proxychains?](#was-ist-proxychains)
- [Aufbau & Funktionsweise](#aufbau--funktionsweise)
- [Installation](#installation)
- [Konfiguration – /etc/proxychains.conf](#konfiguration--etcproxychainsconf)
- [Nutzung](#nutzung)
- [Beispiel: Konfiguration für Tor](#beispiel-konfiguration-für-tor)
- [Tipps & Best Practices](#tipps--best-practices)
- [Weitere Ressourcen](#weitere-ressourcen)
- [Haftungsausschluss](#haftungsausschluss)

---

## Was ist Proxychains?

**Proxychains** ist ein Unix-basiertes Tool, mit dem du beliebige Programme über eine oder mehrere Proxys (z. B. SOCKS5, HTTP) ins Internet tunneln kannst – etwa über Tor. Dadurch bleibt die eigene **echte IP-Adresse verborgen**, was es ideal für anonymes Scanning, Browsing oder Datenabfragen macht.

Proxychains *hängt sich vor die Netzwerkaufrufe eines Programms* und zwingt es, nur über die definierten Proxys zu kommunizieren.

---

## Aufbau & Funktionsweise

Proxychains funktioniert über drei zentrale Komponenten:

1. **Wrapper**: ruft Programme wie `proxychains firefox` oder `proxychains nmap` mit vorangestelltem Proxy auf.
2. **Konfigurationsdatei**: `/etc/proxychains.conf` steuert das Verhalten und die eingesetzten Proxyserver.
3. **Proxy-Reihenfolge (Kaskadierung)**: einzelne oder mehrere Proxys seriell nutzen (Chain).

---

## Installation

Auf Debian-basierten Systemen (z. B. Kali, Parrot OS):

```bash
sudo apt update
sudo apt install proxychains
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Konfiguration – /etc/proxychains.conf

Die Konfigurationsdatei steuert den Ablauf der Weiterleitung. 
Beispielsweise die Verkettung der Proxys (strict/dnymic/random...)
Um sie zu bearbeiten:

```bash
sudo nano /etc/proxychains.conf
```

### 🔧 Wichtige Abschnitte:
1. Chain-Typ
Wähle den Weiterleitungsmodus – üblich ist:
```ini
dynamic_chain
```

**weitere Optionen**
- `strict_chain` - feste Reihenfolge aller Proxys, bricht ab wenn einer ausfällt
- `dynamic_chain` – nutzt die erreichbaren Proxys in der angegebenen Reihenfolge
- `random_chain` – nutzt zufällige Reihenfolge bei jedem Aufruf

2. DNS über Proxy weiterleiten

Damit auch DNS-Abfragen über den Proxy laufen (zur Vermeidung von DNS-Leaks):

```ini
proxy_dns
```
3. Proxy-Liste am Ende der Datei bearbeiten

einen lokalen Tor-SOCKS5-Proxy hinzu:
```ini
[ProxyList]
socks5 127.0.0.1 9050
```
(Wenn Tor installiert ist und der Dienst läuft, ist Port 9050 der Standard für den lokalen SOCKS5-Zugang.)

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nutzung

Einfach vor ein Kommando setzen, z. B.:

```bash
proxychains curl https://ifconfig.me
proxychains nmap -sT example.com
proxychains firefox
```
Wenn alles funktioniert, wird die eigene IP-Adresse nicht die echte sein, sondern die des letzten Proxys (z. B. eines Tor-Exit-Nodes).

---

## Beispiel: Konfiguration für Tor

1. Konfigurationen vornehmen:

```ini
# /etc/proxychains.conf
strict_chain
proxy_dns
tcp_read_time_out 15000
tcp_connect_time_out 8000

[ProxyList]
socks5 127.0.0.1 9050
```

2. Tor-Dienst starten:

```bash
sudo service tor start
```

3. Über Proxychains verbinden:

```bash
proxychains curl ifconfig.io
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tipps & Best Practices

| Tipp                             | Beschreibung                                                            |
| -------------------------------- | ----------------------------------------------------------------------- |
| `proxy_dns` aktivieren           | Schützt vor DNS-Leaks, unbedingt aktivieren                             |
| `dynamic_chain` bevorzugen       | Flexibler, wenn ein Proxy ausfällt                                      |
| Tor + Proxychains = starke Kombi | Ideal für Recon-Tools, bei denen Anonymität wichtig ist                 |
| Logs prüfen                      | `proxychains -f <config> <tool>` verwenden für benutzerdefinierte Konfig |
| Parallel mit Wireshark sniffen   | Zur Kontrolle, ob Verkehr wirklich nur durch den Proxy läuft            |

---

## Weitere Ressourcen

- 🔗 [Proxychains GitHub](https://github.com/rofl0r/proxychains-ng)
- 🔐 [Tor Project](https://www.torproject.org/)
- 📖 `man proxychains`

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