# MAC-Adressen

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Aufbau einer MAC-Adresse](#aufbau-einer-mac-adresse)
- [Visualisierung](#visualisierung)
- [Wofür wird eine MAC-Adresse verwendet?](#wofür-wird-eine-mac-adresse-verwendet)
- [MAC-Adressen herausfinden](#mac-adressen-herausfinden)
- [MAC-Adressen fälschen (Spoofing)](#mac-adressen-fälschen-spoofing)
- [MAC-Adresse ändern (Spoofing)](#mac-adresse-ändern-spoofing)
- [MAC-Spoofing verhindern](#mac-spoofing-verhindern)
- [Ethernet-Frame und MAC-Adressen](#ethernet-frame-und-mac-adressen)
- [Kommunikationsarten mit MAC-Adressen](#kommunikationsarten-mit-mac-adressen)
- [Switches und MAC-Tabellen](#switches-und-mac-tabellen)
- [Präambel im Ethernet](#präambel-im-ethernet)
- [White- und Blacklists mit MAC-Adressen](#white--und-blacklists-mit-mac-adressen)
- [Security-Bezug von MAC-Adressen](#security-bezug-von-mac-adressen)
- [Zusammenfassung](#zusammenfassung)
- [Haftungsausschluss](#haftungsausschluss)

---

## Einleitung

Eine **MAC-Adresse (Media Access Control Address)** ist die weltweit eindeutige, physische Adresse eines Netzwerkadapters.  
Sie dient zur **Identifikation von Geräten innerhalb eines lokalen Netzwerks (LAN/WLAN)**.  

- Darstellung: Hexadezimal (z. B. `00:1A:2B:3C:4D:5E`)  
- Länge: **48 Bit (6 Byte)**  
- OSI-Modell: **Layer 2 (Sicherungsschicht)**  
- Einzigartigkeit: Jede Netzwerkkarte hat eine eigene, vom Hersteller vergebene Adresse  

## Aufbau einer MAC-Adresse

Eine MAC-Adresse besteht aus **zwei Hauptteilen**:

| Teil | Beschreibung |
|------|--------------|
| **OUI (Organizationally Unique Identifier)** | Die ersten 24 Bit (3 Byte) geben den Hersteller an |
| **Gerätekennung (Device Identifier)** | Die letzten 24 Bit (3 Byte) sind die eindeutige Seriennummer |

**Beispiel:**  
`00:1A:2B:3C:4D:5E`  
- `00:1A:2B` → Hersteller (OUI)  
- `3C:4D:5E` → Individuelle Gerätekennung  

---

## Visualisierung

```yaml
MAC-Adresse (48 Bit)
+-------------------+--------------------+
| OUI (24 Bit)      | Device ID (24 Bit) |
| Herstellerkennung | Seriennummer       |
+-------------------+--------------------+
Beispiel: 00:1A:2B:3C:4D:5E
```


---

## Wofür wird eine MAC-Adresse verwendet?

- Lokale Identifikation in Netzwerken (Ethernet, WLAN)  
- Kommunikation über Layer 2 (Frames enthalten Quell- und Ziel-MAC)  
- Zugriffskontrollen in Netzwerken (z. B. WLAN-MAC-Filter)  

**Wichtig:** MAC-Adressen werden **nicht im Internet** verwendet!  
Dort übernimmt die **IP-Adresse** die Adressierung.  

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## MAC-Adressen herausfinden

**Windows:**
```powershell
ipconfig /all
```
→ wird als „Physikalische Adresse“ angezeigt

Linux/macOS:

```bash
ip a | grep ether
ifconfig -a
```

## MAC-Adressen fälschen (Spoofing)

Beim MAC-Spoofing wird die Adresse manipuliert, um sich als ein anderes Gerät auszugeben.

Gründe:
- Umgehung von WLAN-MAC-Filtern
- Anonymität & Tracking-Schutz (z. B. in öffentlichen WLANs)
- Angriffe wie ARP-Spoofing oder Man-in-the-Middle


## MAC-Adresse ändern (Spoofing)

### Windows
```powershell
# Adapterliste anzeigen
getmac

# In den Adaptereigenschaften (Gerätemanager) -> "Netzwerkadresse" manuell ändern
```

### Linux
```bash
# Temporär ändern
sudo ip link set dev eth0 down
sudo ip link set dev eth0 address 12:34:56:78:9A:BC
sudo ip link set dev eth0 up

# Mit macchanger
sudo apt install macchanger
sudo macchanger -r eth0
```

### macOS

```bash
sudo ifconfig en0 ether 12:34:56:78:9A:BC
```


## MAC-Spoofing verhindern

- Port-Security auf Switches: nur bestimmte MAC-Adressen pro Port zulassen
- Dynamic ARP Inspection (DAI): blockiert verdächtige Adressen im LAN
- 802.1X (Network Access Control): Authentifizierung zusätzlich zur MAC-Adresse
- Monitoring: ungewöhnliche MAC-Wechsel oder Konflikte erkennen


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Ethernet-Frame und MAC-Adressen

Ein Ethernet-Frame ist die Basiseinheit der LAN-Kommunikation.

| Feld              | Beschreibung                  |
| ----------------- | ----------------------------- |
| Präambel (7 B)    | Synchronisation für Empfänger |
| SFD (1 B)         | Start Frame Delimiter         |
| Ziel-MAC (6 B)    | Empfängeradresse              |
| Quell-MAC (6 B)   | Absenderadresse               |
| EtherType (2 B)   | Protokoll (IPv4, IPv6, ARP …) |
| Daten (46–1500 B) | Nutzlast                      |
| FCS (4 B)         | Fehlerprüfung (CRC)           |

Mehr zu Protokollen und ihren Headern erfährst du [hier](/02-network-security/protokoll_header_cheatsheet.md)

## Kommunikationsarten mit MAC-Adressen

- **Unicast** → Frame an genau eine MAC-Adresse
- **Broadcast** → an alle Geräte (MAC: `FF:FF:FF:FF:FF:FF`)
- **Multicast** → an eine Gruppe von Geräten (MAC beginnt mit `01:00:5E`)


## Switches und MAC-Tabellen

- Switches speichern MAC-Adressen in einer **CAM-Tabelle** (Content Addressable Memory)
- Frames werden gezielt an den Port weitergeleitet (Unicast)
- Unbekannte Adressen → **Broadcast**

```text
+---------+-------------------+
| Port    | MAC-Adresse       |
+---------+-------------------+
| Fa0/1   | 00:1A:2B:3C:4D:5E |
| Fa0/2   | 12:34:56:78:9A:BC |
+---------+-------------------+
```

## Präambel im Ethernet

Die Präambel (7 Bytes) ist ein "**Achtung, gleich geht’s los!**"-Signal für den Empfänger.
Sie dient zur Synchronisation und stellt sicher, dass Netzwerkkarten die kommenden Bits korrekt lesen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## White- und Blacklists mit MAC-Adressen
### Whitelist

Nur Geräte mit zugelassener MAC-Adresse dürfen ins Netzwerk.

- ✅ Hohe Sicherheit
- ❌ Hoher Verwaltungsaufwand
- ❌ Spoofing möglich

### Blacklist

Nur gesperrte Geräte werden blockiert.

- ✅ Einfach zu pflegen
- ❌ Keine Sicherheit gegen unbekannte Angreifer


## Security-Bezug von MAC-Adressen

- **Tracking in WLANs:** Smartphones senden MAC-Adressen bei der Suche nach Netzwerken → Tracking möglich
- **Randomized MACs:** Moderne Systeme (Android, iOS, Windows) nutzen zufällige MACs in WLANs zum Schutz der Privatsphäre
- **Angriffe:** ARP-Spoofing, MAC-Flooding, DoS durch MAC-Spoofing
- **Abwehr:** Port-Security, DAI, 802.1X


## Zusammenfassung

```less
Gerät A (MAC: 00:11:22:33:44:55) ---> Switch ---> Gerät B (MAC: 66:77:88:99:AA:BB)

Unicast: direkt an B
Broadcast: an alle Geräte
Multicast: an definierte Gruppe
```

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