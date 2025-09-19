# ⚙️ Netzwerkgeräte: Hardware im Überblick

## Inhaltsverzeichnis
- [Hub](#hub)
- [Switch](#switch)
- [Router](#router)
- [Unterschied zwischen Hub und Switch](#unterschied-zwischen-hub-und-switch)
- [Spezialgeräte](#spezialgeräte)
- [Aktive vs. Passive Komponenten](#aktive-vs-passive-komponenten)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Hub
Ein **Hub** ist ein einfaches Netzwerkgerät, das Datenpakete an **alle** angeschlossenen Geräte sendet. Es agiert nach dem Motto: „Es wird schon das richtige Gerät dabei sein!“ Hubs sind weniger effizient und veraltet, da sie die Netzwerkleistung durch unnötigen Traffic (Broadcast) reduzieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Switch
Ein **Switch** ist ein intelligenteres Netzwerkgerät, das Datenpakete nur an das **spezifische Zielgerät** weiterleitet. Er lernt die MAC-Adressen der angeschlossenen Geräte und speichert sie in einer Tabelle. Dies macht die Kommunikation effizienter und sicherer als bei einem Hub.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Router
Ein **Router** ist das zentrale Gerät in einem Heim- oder Unternehmensnetzwerk. Er verbindet verschiedene Netzwerke (z.B. dein lokales LAN mit dem Internet). Anhand der IP-Adressen in den Datenpaketen entscheidet der Router, wie die Daten am effizientesten weitergeleitet werden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Unterschied zwischen Hub und Switch
Der entscheidende Unterschied liegt in der Art, wie sie Datenpakete weiterleiten.

```text
1. Paket versenden mit Hub
+------------+                           +----------+                 +----------+
| PC A sendet|-------------------------->|   Hub    |---------------->| PC B     |
+------------+        Paket an B         +----------+                 | PC C     |
                                              |                       | PC D     |
                                              v                       +----------+
                                [ Paket geht an alle Ports ]


2. Paket versenden mit Switch
+------------+                           +----------+                 +----------+
| PC A sendet|-------------------------->|  Switch  |---------------->| PC B     |
+------------+        Paket an B         +----------+                 | PC C     |
                                                |                     | PC D     |
                                                v                     +----------+
                        [ Paket geht nur an den Port von PC B ]
```





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Spezialgeräte
- **VLAN Switch (Managed Switch):** Ein Switch, der VLANs (Virtual Local Area Networks) unterstützt. Er ermöglicht es, ein physisches Netzwerk in mehrere logische Teilnetze zu unterteilen, was die Sicherheit und Organisation verbessert. Managed Switches haben eine eigene IP-Adresse und können konfiguriert werden.
- **Access Point (AP):** Ein Gerät, das ein drahtloses Netzwerk (WLAN) aufbaut und Geräte kabellos mit einem kabelgebundenen Netzwerk (LAN) verbindet.
- **Netzwerkschrank:** Ein Serverschrank, der Netzwerkgeräte (Switches, Router, Server) vor Beschädigung, Staub und Diebstahl schützt und eine ordentliche Kabelführung ermöglicht.
- **Patchfeld:** Ein Panel mit Ports, das die fest verlegten Netzwerkkabel eines Gebäudes an einem zentralen Ort zusammenführt. Es vereinfacht die Verwaltung und Neuverbindung von Netzwerksegmenten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Aktive vs. Passive Komponenten
- **Aktive Komponenten:** Benötigen eine Stromversorgung, um Signale zu verstärken oder zu verarbeiten. Beispiele sind **Hubs**, **Switches**, **Router**, **Repeater** und **Access Points**.
- **Passive Komponenten:** Benötigen keine Stromversorgung und dienen nur der Übertragung und Verbindung. Beispiele sind **Netzwerkkabel**, **Patchfelder** und **Anschlussdosen**.



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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
