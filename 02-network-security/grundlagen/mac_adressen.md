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





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

Eine **MAC-Adresse (Media Access Control Address)** ist die weltweit eindeutige, physische Adresse eines Netzwerkadapters.
Sie dient zur **Identifikation von Geräten innerhalb eines lokalen Netzwerks (LAN/WLAN)**.

- **Darstellung:** Hexadezimal (z. B. `00:1A:2B:3C:4D:5E`)
- **Länge:** 48 Bit (6 Byte)
- **OSI-Modell:** Layer 2 (Sicherungsschicht)
- **Einzigartigkeit:** Jede Netzwerkkarte hat eine eigene, vom Hersteller vergebene Adresse



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


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





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Visualisierung

```text
+---------------------------------------+
|        MAC-Adresse (48 Bit)           |
+-------------------+-------------------+
| OUI (24 Bit)      | Device ID (24 Bit)|
| Herstellerkennung | Seriennummer      |
+-------------------+-------------------+
Beispiel: 00:1A:2B:3C:4D:5E
```






<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wofür wird eine MAC-Adresse verwendet?

- **Lokale Identifikation** in Netzwerken (Ethernet, WLAN)  
- Kommunikation über **Layer 2** (Frames enthalten Quell- und Ziel-MAC)  
- **Zugriffskontrollen** in Netzwerken (z. B. WLAN-MAC-Filter)  

**Wichtig:** MAC-Adressen werden **nicht im Internet** verwendet!  
Dort übernimmt die **IP-Adresse** die Adressierung.  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## MAC-Adressen herausfinden

### Windows:
```powershell
ipconfig /all
```
→ wird als „Physikalische Adresse“ angezeigt

### Linux/macOS:

```bash
ip a | grep ether
ifconfig -a
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## MAC-Adressen fälschen (Spoofing)

Beim **MAC-Spoofing** wird die Adresse manipuliert, um sich als ein anderes Gerät auszugeben.

- **Gründe:**
    - Umgehung von WLAN-MAC-Filtern
    - Anonymität & Tracking-Schutz (z. B. in öffentlichen WLANs)
    - Angriffe wie [**ARP-Spoofing**](/02-network-security/angriffe/arp_spoofing.md) oder [**Man-in-the-Middle**](/02-network-security/angriffe/mitm_angriff.md).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## MAC-Adresse ändern (Spoofing)

### Windows
```powershell
# Adapterliste anzeigen
getmac
```

Anschließend die MAC-Adresse in den Adaptereigenschaften (Gerätemanager) unter "Netzwerkadresse" manuell ändern.

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




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## MAC-Spoofing verhindern

- **Port-Security auf Switches:** Erlaubt nur eine bestimmte Anzahl oder spezifische MAC-Adressen pro Switch-Port.
- **Dynamic ARP Inspection (DAI):** Blockiert verdächtige ARP-Pakete und verhindert ARP-Spoofing
- **802.1X (Network Access Control):** Erfordert eine Authentifizierung (z.B. Benutzername/Passwort) zusätzlich zur MAC-Adresse.
- **Monitoring:** Erkennung von ungewöhnlichen MAC-Adressen, Wechseln oder Konflikten im Netzwerk.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Ethernet-Frame und MAC-Adressen

Ein Ethernet-Frame ist die Basiseinheit der Kommunikation auf der Sicherungsschicht (Layer 2) und enthält die MAC-Adressen für die lokale Zustellung.
### Aufbau eines Ethernet Frames

```text
+----------------+--------------+--------------+---------------+
| Präambel (7B)  | SFD (1B)     | Ziel-MAC (6B)| Quell-MAC (6B)|
+----------------+--------------+--------------+---------------+
| EtherType (2B) |     Payload (46-1500B)      |   FCS (4B)    |
+----------------+-----------------------------+---------------+
```

| Feld              | Beschreibung                  |
| ----------------- | ----------------------------- |
| Präambel (7 B)    | Synchronisation für Empfänger |
| SFD (1 B)         | Start Frame Delimiter         |
| Ziel-MAC (6 B)    | Empfängeradresse              |
| Quell-MAC (6 B)   | Absenderadresse               |
| EtherType (2 B)   | Protokoll (IPv4, IPv6, ARP …) |
| Daten (46–1500 B) | Nutzlast                      |
| FCS (4 B)         | Fehlerprüfung (CRC)           |

Mehr zu Protokollen und ihren Headern erfährst du [hier](/02-network-security/protokoll_header_cheatsheet.md).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Kommunikationsarten mit MAC-Adressen

- **Unicast:** Ein Frame wird an genau eine spezifische MAC-Adresse gesendet.
- **Broadcast:** Ein Frame wird an alle Geräte im Netzwerk gesendet. Die Ziel-MAC-Adresse ist `FF:FF:FF:FF:FF:FF`
- **Multicast:** Ein Frame wird an eine vordefinierte Gruppe von Geräten gesendet. Die MAC-Adresse beginnt mit `01:00:5E`




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Switches und MAC-Tabellen
Switches speichern die MAC-Adressen der angeschlossenen Geräte in einer **CAM-Tabelle** (**Content Addressable Memory**). Dies ermöglicht eine effiziente Weiterleitung von Frames.

- **Lernprozess:** Empfängt ein Switch einen Frame, speichert er die Quell-MAC-Adresse und den dazugehörigen Port in seiner Tabelle.
- **Weiterleitung:** Ein Frame mit einer bekannten Ziel-MAC wird gezielt an den richtigen Port weitergeleitet (Unicast).
- **Unbekannte Adressen:** Ein Frame mit einer unbekannten Ziel-MAC wird an alle Ports weitergeleitet (**Flooding**), bis der Empfänger antwortet.

### Beispiel einer MAC-Tabelle (CAM-Tabelle)

```text
+---------+-------------------+
| Port    | MAC-Adresse       |
+---------+-------------------+
| Fa0/1   | 00:1A:2B:3C:4D:5E |
| Fa0/2   | 12:34:56:78:9A:BC |
+---------+-------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Präambel im Ethernet

Die Präambel (7 Bytes) und der Start Frame Delimiter (1 Byte) sind die ersten 8 Bytes eines Ethernet-Frames.

Sie dienen zur Synchronisation von Absender und Empfänger und stellen sicher, dass die Netzwerkkarte die folgenden Bits korrekt liest.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## White- und Blacklists mit MAC-Adressen
### Whitelist

Nur Geräte mit einer zugelassenen MAC-Adresse dürfen sich mit dem Netzwerk verbinden.

- **Vorteile:** Bietet eine grundlegende Zugriffskontrolle.
- **Nachteile:** Hoher Verwaltungsaufwand, leicht durch Spoofing zu umgehen.

### Blacklist

Sperrt nur bestimmte, bekannte MAC-Adressen von der Netzwerkverbindung.

- **Vorteile:** Einfach zu pflegen für die Blockierung bekannter Geräte.
- **Nachteile:** Bietet keine Sicherheit gegen unbekannte Angreifen oder gespoofte Adressen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Security-Bezug von MAC-Adressen

- **Tracking in WLANs:** Mobile Geräte senden MAC-Adressen, wenn sie nach WLANs suchen, was von Angreifern oder Unternehmen zum Tracking der Bewegungsprofile genutzt werden kann.
- **Randomized MACs:** Moderne Betriebssysteme (z. B. Android, iOS, Windows) verwenden zufällige MAC-Adressen, um die Privatsphäre zu schützen und Tracking zu erschweren.
- **Angriffe:** 
    - **ARP-Spoofing:** Angreifer manipuliert die ARP-Tabelle, um als Gateway zu agieren und den Datenverkehr umzuleiten (Man-in-the-Middle).
    - **MAC-Flooding:** Überflutung des Switches mit gefälschten Quell-MAC-Adressen, um seine CAM-Tabelle zu überfüllen. Der Switch fällt in den "Fail-Open"-Modus und leitet alle Frames als Broadcast weiter.
    - **Gratuitous ARP:** Ein gefälschtes ARP-Paket sendet eine falsche IP-MAC-Zuordnung in das Netzwerk, um die ARP-Tabellen der anderen Geräte zu manipulieren.
- **Abwehr:** 
    - **Port-Security:** Schützt vor MAC-Flooding.
    - **DAI (Dynamic ARP Inspection):** Verhindert ARP-Spoofing.
    - **802.1X:** Bietet eine robustere Authentifizierung als einfache MAC-Filter.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Zusammenfassung

```text
+-------------+      +---------------+      +-------------+
| Gerät A     |      |     Switch    |      | Gerät B     |
| MAC: ...A   |----->| (CAM-Tabelle) |----->| MAC: ...B   |
+-------------+      +---------------+      +-------------+
      ^                      |                 |
      |                      v                 |
      |                    Unicast             |
      +----------------------------------------+
```
- **MAC-Adressen** sind einzigartige Kennungen für Geräte im lokalen Netzwerk.
- Sie werden auf **Layer 2** verwendet.
- **Switches** nutzen MAC-Adressen und CAM-Tabellen zur gezielten Weiterleitung von Daten.
- **MAC-Spoofing** ist eine gängige Technik, die die Sicherheit von MAC-Filtern untergräbt und für verschiedene Angriffe genutzt wird.
- Der Schut vor MAC-basierten Angriffen erfordert robuste Sicherheitsmaßnahmen wie **Port-Security** und **DAI**.





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

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---