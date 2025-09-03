# ARP-Spoofing

**ARP-Spoofing (auch ARP-Poisoning)** ist ein Angriff auf die **Adress Resolution Protocol (ARP)-Tabelle** in einem lokalen Netzwerk (LAN).  
Der Angreifer manipuliert ARP-Antworten, sodass Datenverkehr zwischen zwei Geräten über ihn selbst umgeleitet wird.  

Mehr zum Thema `ARP` erfährst du hier: [arp_grundlagen.md](/02-network-security/arp_grundlagen.md)

## Inhaltsverzeichnis
- [Grundlagen: Was ist ARP?](#grundlagen-was-ist-arp)
- [Wie funktioniert ARP-Spoofing?](#wie-funktioniert-arp-spoofing)
- [Ziele von ARP-Spoofing](#ziele-von-arp-spoofing)
- [Beispiel: ARP-Spoofing mit `arpspoof`](#beispiel-arp-spoofing-mit-arpspoof)
- [Erkennen von ARP-Spoofing](#erkennen-von-arp-spoofing)
- [Schutzmaßnahmen gegen ARP-Spoofing](#schutzmaßnahmen-gegen-arp-spoofing)
- [Zusammenfassung](#zusammenfassung)
- [Haftungsausschluss](#haftungsausschluss)

---

## Grundlagen: Was ist ARP?

- **ARP (Address Resolution Protocol)** ordnet **IP-Adressen (Layer 3)** den **MAC-Adressen (Layer 2)** zu.  
- Jedes Gerät im LAN führt eine **ARP-Tabelle**, in der IP ↔ MAC-Zuordnungen gespeichert werden.  

### Beispiel einer ARP-Tabelle
```yaml
IP-Adresse MAC-Adresse
192.168.0.1 00:1A:2B:3C:4D:5E # Router
192.168.0.10 12:34:56:78:9A:BC # Laptop
192.168.0.20 AA:BB:CC:DD:EE:FF # Smartphone
```

---

## Wie funktioniert ARP-Spoofing?

Der Angreifer sendet **gefälschte ARP-Antworten** ins Netzwerk:  
- Er behauptet z. B., die **IP des Routers** (`192.168.0.1`) hätte seine eigene **MAC-Adresse**.  
- Dadurch leitet das Opfer alle Pakete an den Angreifer statt an den Router.  

### Visualisierung

```yaml
Opfer-PC (192.168.0.10)       Angreifer (192.168.0.66)       Router (192.168.0.1)
         |                             |                            |
         | ----> gefälschte ARP ------>|                            |
         | <---- gefälschte ARP -------|                            |
         |                             |                            |
         |---------------------------->|--------------------------->|
         |       (Datenverkehr)        |        (weitergeleitet)    |

+---------------- Manipulierte ARP-Tabelle Opfer ----------------+
192.168.0.1   →   MAC: Angreifer

+---------------- Manipulierte ARP-Tabelle Router ---------------+
192.168.0.10  →   MAC: Angreifer
```


---

## Ziele von ARP-Spoofing

- **Man-in-the-Middle (MitM)**: Angreifer kann Datenverkehr abhören oder verändern  
- **Session Hijacking**: Cookies, Tokens oder Anmeldedaten abfangen  
- **Denial of Service (DoS)**: Daten werden einfach verworfen  
- **Traffic Manipulation**: Opfer auf Fake-Webseiten oder Malware-Server umleiten  

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Beispiel: ARP-Spoofing mit `arpspoof`

**Linux (Kali, Parrot, etc.)**
```bash
# Ziel: Umleiten von Opfer (192.168.0.10) und Router (192.168.0.1)
sudo arpspoof -i eth0 -t 192.168.0.10 192.168.0.1
sudo arpspoof -i eth0 -t 192.168.0.1 192.168.0.10
```

Damit sitzt der Angreifer „in der Mitte“ und kann den gesamten Traffic mitschneiden.

## Erkennen von ARP-Spoofing

Doppelte MAC-Adressen in der ARP-Tabelle
```bash
arp -a
```
→ Wenn die IP des Routers plötzlich die gleiche MAC wie ein anderer Client hat → verdächtig.

- Tools wie `arpwatch` oder `Wireshark`:
    - ungewöhnlich viele ARP-Replies
    - sich ständig ändernde Zuordnungen

## Schutzmaßnahmen gegen ARP-Spoofing

- Statische ARP-Einträge:
    - Router-IP fest einer MAC zuordnen (`arp -s 192.168.0.1 00:1A:2B:3C:4D:5E`)
    - Nur praktikabel in kleinen Netzen.
- Dynamic ARP Inspection (DAI):
    - Switch prüft ARP-Pakete gegen eine vertrauenswürdige IP-MAC-Liste.
- Port-Security:
    - begrenzt die Anzahl der MAC-Adressen pro Switch-Port.
- Verschlüsselte Protokolle nutzen:
    - HTTPS, SSH, VPNs → selbst wenn Traffic abgefangen wird, bleibt er verschlüsselt.
- Monitoring:
    - IDS/IPS-Systeme (z. B. Snort, Suricata) erkennen ARP-Anomalien.

## Zusammenfassung

```css
[Normales ARP]                 [ARP-Spoofing]
192.168.0.1 → Router-MAC       192.168.0.1 → Angreifer-MAC
192.168.0.10 → Laptop-MAC      192.168.0.10 → Opfer-MAC

Folge:
Opfer sendet Daten → Angreifer → Router
Angreifer kann lesen, manipulieren, blockieren
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