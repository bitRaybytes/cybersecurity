# Address Resolution Protocol (ARP)

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Wofür wird ARP gebraucht?](#wofür-wird-arp-gebraucht)
- [Aufbau von ARP](#aufbau-von-arp)
- [Beispiel einer ARP-Tabelle](#beispiel-einer-arp-tabelle)
- [Ablauf einer ARP-Anfrage](#ablauf-einer-arp-anfrage)
- [ARP in der Praxis](#arp-in-der-praxis)
- [Sicherheitsaspekte & Spoofing](#sicherheitsaspekte--spoofing)
- [Zusammenfassung](#zusammenfassung)

---

## Einleitung

- **ARP (Address Resolution Protocol)** ist ein Protokoll auf **OSI-Schicht 2/3**.  
- Aufgabe: Übersetzung von **IP-Adressen (Layer 3, logisch)** in **MAC-Adressen (Layer 2, physisch)**.  
- Nur innerhalb eines lokalen Netzwerks (LAN) relevant – im Internet übernimmt dies Routing und DNS.  

---

## Wofür wird ARP gebraucht?

Ein Computer kennt meist nur die **IP-Adresse** des Ziels (z. B. 192.168.0.1).  
Damit das Paket im LAN zugestellt werden kann, benötigt er jedoch die **MAC-Adresse** des Zielgeräts.  

👉 Lösung: **ARP-Anfrage („Who has?“)**  

---

## Aufbau von ARP

Ein ARP-Paket ist im Ethernet-Frame eingebettet und hat folgende Struktur:  

```yaml
+------------------------+---------------------------------+
| Feld                   | Größe                           |
+------------------------+---------------------------------+
| Hardware-Typ (HTYPE)   | 16 Bit (z. B. 1 = Ethernet)     |
| Protokoll-Typ (PTYPE)  | 16 Bit (z. B. 0x0800 = IP)      |
| Hardware-Länge (HLEN)  | 8 Bit  (z. B. 6 für MAC)        |
| Protokoll-Länge (PLEN) | 8 Bit (z. B. 4 für IPv4)        |
| Opcode                 | 16 Bit (1 = Request, 2 = Reply) |
| Sender MAC-Adresse     | 48 Bit (6 Byte)                 |
| Sender IP-Adresse      | 32 Bit (4 Byte)                 |
| Ziel MAC-Adresse       | 48 Bit (6 Byte)                 |
| Ziel IP-Adresse        | 32 Bit (4 Byte)                 |
+------------------------+---------------------------------+
```

Mehr zu Protokollen findest du in der Datei [protkoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md)

## Beispiel einer ARP-Tabelle

Ein Gerät speichert bekannte IP ↔ MAC-Zuordnungen in einer ARP-Tabelle:

```yaml
IP-Adresse     MAC-Adresse
192.168.0.1    00:1A:2B:3C:4D:5E   # Router
192.168.0.10   12:34:56:78:9A:BC   # Laptop
192.168.0.20   AA:BB:CC:DD:EE:FF   # Smartphone
```

**Abruf unter Windows/Linux/macOS:**
```bash
arp -a
```

## Ablauf einer ARP-Anfrage
1. Anfrage (Broadcast)
```yaml
PC (192.168.0.10) → LAN-Broadcast:
"Wer hat 192.168.0.1? Bitte sende mir deine MAC!"
```

2. Antwort (Unicast)
```yaml
Router (192.168.0.1) → PC (192.168.0.10):
"192.168.0.1 hat MAC 00:1A:2B:3C:4D:5E"
```
Der PC trägt diese Zuordnung in seine ARP-Tabelle ein.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## ARP in der Praxis

**Befehl unter Windows:**
```bash
arp -a
```

**Befehl unter Linux/macOS:**
```bash
ip neigh show
```

**Typisch Ausgabe:**
```yaml
192.168.0.1   ether 00:1A:2B:3C:4D:5E   C
192.168.0.20  ether AA:BB:CC:DD:EE:FF   C
```

(`C` = complete, gültiger Eintrag)


## Sicherheitsaspekte & Spoofing

Da ARP **keine Authentifizierung** vorsieht:
- Jeder Host im LAN kann **falsche ARP-Antworten** senden.
- Diese Schwachstelle ermöglicht **ARP-Spoofing/Poisoning** (→ siehe [arp-spoofing.md](/02-network-security/arp_spoofing.md)).

```yaml
[Normales ARP]
- ARP löst IP → MAC innerhalb eines LAN auf
- Speicherung in einer ARP-Tabelle
- Kommunikation funktioniert ohne DNS/Internet

[Problem]
- Keine Authentifizierung → Angriffe durch Spoofing möglich
```


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