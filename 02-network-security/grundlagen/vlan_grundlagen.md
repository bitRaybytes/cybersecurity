# 🌐 VLAN – Virtual Local Area Network



## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Grundlagen](#grundlagen)
- [Funktionsweise](#funktionsweise)
- [VLAN-Typen](#vlan-typen)
  - [Portbasierte VLANs](#portbasierte-vlans)
  - [Tagged VLANs (Trunk-Ports)](#tagged-vlans-trunk-ports)
  - [Statische VLANs](#statische-vlans)
  - [Dynamische VLANs](#dynamische-vlans)
- [Implementierung & Nutzen](#implementierung--nutzen)
- [Schutz & Sicherheit](#schutz--sicherheit)
- [Wichtige Protokolle](#wichtige-protokolle)
- [Grafiken](#grafiken)
- [Fazit](#fazit)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



## Einleitung
Ein **Virtual Local Area Network (VLAN)** unterteilt ein physisches Netzwerk in mehrere **logische Teilnetze**.  
Dadurch lassen sich Netzwerke segmentieren, Sicherheit erhöhen und Datenströme optimieren.  

- Jedes VLAN ist eine eigene **Broadcast-Domain**  
- VLANs können sich über mehrere Switches hinweg erstrecken  
- VLANs arbeiten im **OSI-Modell auf Layer 2 (Sicherungsschicht)**  



## Grundlagen
- Ohne VLAN: alle Geräte im LAN sind in derselben Broadcast-Domain.  
- Mit VLAN: Geräte können logisch getrennt werden, auch wenn sie am selben Switch hängen.  
- Standard: **IEEE 802.1Q** – definiert VLAN-Tagging (Trunking).  



## Funktionsweise
- VLANs „teilen“ einen Switch virtuell in mehrere kleinere Switches.  
- Geräte im gleichen VLAN können direkt miteinander kommunizieren.  
- Kommunikation zwischen VLANs erfordert einen **Router oder Layer-3-Switch** („Inter-VLAN Routing“).  



## VLAN-Typen

### Portbasierte VLANs
- Jeder **Port** des Switches ist einem VLAN zugeordnet.  
- Geräte können nur mit anderen Geräten im gleichen VLAN kommunizieren.  

👉 Beispiel:  
- Ports 1–4 = VLAN 10 (Grün)  
- Ports 5–8 = VLAN 20 (Rot)  



### Tagged VLANs (Trunk-Ports)
- Ein **Trunk-Port** kann mehrere VLANs gleichzeitig übertragen.  
- VLANs werden durch einen **802.1Q-Tag** im Ethernet-Frame unterschieden.  
- Wird genutzt, um VLANs über mehrere Switches hinweg zu verbinden.  



### Statische VLANs
- VLAN wird **fest** einem Switch-Port zugewiesen.  
- Unabhängig davon, welches Gerät angeschlossen wird.  
- Vorteil: stabil, sicher, einfach.  


### Dynamische VLANs
- VLAN-Zuordnung erfolgt automatisch anhand von Attributen:  
  - MAC-Adresse,  
  - Protokolle,
  - Benutzer-Authentifizierung (z. B. via RADIUS).
- Vorteil: flexibel und skalierbar.  
- Einsatz: große Netzwerke, BYOD-Umgebungen.  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Implementierung & Nutzen
- **Logische Aufteilung** von Datenströmen.
- **QoS (Quality of Service)**: Priorisierung von Datenverkehr (z. B. VoIP vor normalem Traffic).
- **Sicherheit**: Trennung sensibler Systeme (z. B. Server, Gäste-WLAN).
- **Effizienz**: Reduzierung von Broadcast-Traffic.



## Schutz & Sicherheit
- VLANs allein sind **keine vollständige Sicherheitslösung**.  
- Angriffsvektoren:  
  - **VLAN Hopping** (Manipulation von Tags, Trunking-Angriffe).
  - **Rogue DHCP Server** in fremden VLANs.
- Schutzmaßnahmen:  
  - Unbenutzte Ports deaktivieren oder auf „Blackhole VLAN“ legen.
  - Trunk-Ports **explizit konfigurieren**, nicht automatisch aushandeln (kein DTP nutzen).
  - VLANs für **Management-Traffic** (SSH, SNMP) strikt trennen.



## Wichtige Protokolle
- **IEEE 802.1Q** – VLAN-Tagging.
- **VTP (VLAN Trunking Protocol, Cisco)** – automatisiert VLAN-Verteilung (Achtung: oft unsicher!).
- **RSTP / STP (Spanning Tree Protocol)** – verhindert Schleifen, wichtig bei VLANs über mehrere Switches.
- **Inter-VLAN Routing** – ermöglicht Kommunikation zwischen VLANs (Layer 3).



## Grafiken

### VLAN ohne Trunking
```text
   +----------+       +----------+
   | Switch A |       | Switch B |
   |          |       |          |
   | VLAN 10  |-------| VLAN 10  |
   | VLAN 20  |-------| VLAN 20  |
   +----------+       +----------+

=> Für jedes VLAN ein separates Kabel nötig
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### VLAN mit Trunk-Port (802.1Q)
```text
   +----------+         +----------+
   | Switch A |=========| Switch B |
   |          |  Trunk  |          |
   | VLAN 10  |=========| VLAN 10  |
   | VLAN 20  |=========| VLAN 20  |
   +----------+         +----------+

=> Ein Kabel, mehrere VLANs über 802.1Q Tags
```

## Fazit

- VLANs sind ein zentraler Baustein moderner Netzwerke.
- Sie erhöhen Sicherheit, Effizienz und Skalierbarkeit.
- Besonders in Unternehmensnetzen und Rechenzentren unverzichtbar.
- Achtung: VLANs ersetzten keine Firewalls – sie sind ein Sicherheits- und Management-Werkzeug, aber kein Schutz vor allen Angriffen.


## Nützliche Links

- [OSI-Schichtenmodell](/02-network-security/osi_schichtenmodell.md)
- [Protokoll Header Cheatsheet](/02-network-security/protokoll_header_cheatsheet.md)



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