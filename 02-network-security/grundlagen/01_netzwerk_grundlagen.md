# 🌐 Netzwerk - Grundlagen

## Inhaltsverzeichnis
- [Was ist ein Netzwerk?](#was-ist-ein-netzwerk)
- [Abgrenzung von Netzen](#abgrenzung-von-netzen)
- [Was ist ein Server / Client?](#was-ist-ein-server--client)
- [Peer-to-Peer-Netzwerk](#peer-to-peer-netzwerk)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist ein Netzwerk?
Ein Netzwerk verbindet zwei oder mehr Geräte, um Daten auszutauschen und gemeinsame Ressourcen zu nutzen. Geräte in einem Netzwerk sind nicht nur Computer, sondern können auch Sensoren, Drucker, Scanner und sogar Aktoren sein.

**Wichtige Merkmale:**
* Geräte werden durch Adressen (IPv4, IPv6, MAC) eindeutig adressiert.
* Größere Netzwerke können Dienste wie Lastenverteiler (Load Balancer) über mehrere Rechner verteilen.
* Geräte innerhalb eines Netzwerks können unterschiedliche Aufgaben erfüllen.

```text
+-----------+                 +------------+
| PC Client |-----------------|  Router    |
+-----------+                 +------------+
                                  |
                                  v
                            +---------------+
                            |  Drucker      |
                            +---------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Abgrenzung von Netzen
Netzwerke werden oft nach ihrer geografischen Reichweite kategorisiert. Für Fachinformatiker sind **LAN** und **WAN** die wichtigsten Konzepte.

| Abkürzung | Name                         | Reichweite                                  |
|-----------|------------------------------|---------------------------------------------|
| **PAN** | Personal Area Network        | Direktverbindungen (z.B. Bluetooth)         |
| **LAN** | Local Area Network           | Lokales Netzwerk (Gebäude, Campus)          |
| **MAN** | Metropolitan Area Network    | Stadt oder Metropolregion                   |
| **WAN** | Wide Area Network            | Weitreichend (Länder, Kontinente)           |
| **GAN** | Global Area Network          | Weltweite Reichweite (z.B. Internet)        |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist ein Server / Client?
### Server
Ein **Server** ist ein leistungsstarker Computer oder eine Software, die Ressourcen für andere Computer im Netzwerk bereitstellt. Er ermöglicht verschiedene Dienste und speichert Daten, auf die mehrere Nutzer zugreifen können.

**Typische Serverdienste:**
* **Webserver:** Stellt Webseiten zur Verfügung.
* **Mailserver:** Verwaltet den E-Mail-Verkehr.
* **Dateiserver:** Speichert und verwaltet Dateien (z.B. über FTP oder SMB).
* **Druckserver:** Verwaltet Druckaufträge im Netzwerk.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Client
Ein **Client** ist ein Computer oder eine Software, der mit einem Server kommuniziert, um dessen Daten und Dienste zu nutzen. Dein Webbrowser ist ein Client, der eine Webseite von einem Webserver anfordert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Peer-to-Peer-Netzwerk
In einem **Peer-to-Peer (P2P)-Netzwerk** haben alle Teilnehmer die gleichen Rechte. Die Kommunikation erfolgt direkt zwischen den Computern, ohne einen zentralen Server. Dieses Modell steht im Gegensatz zum **Client-Server-Modell**, wo ein Client immer von einem Server abhängt.


```text
    Client-Server-Modell:                            Peer-to-Peer-Modell:

        +---------+                                      +-------+
        |  Server |                                      | PC A  |
        +---------+                                      +---+---+
            / | \                                           /  | 
           /  |  \                                         /   |
          /   |   \                                       /    |   
         /    |    \                                     /     |
        +---+--+----+                                +---+---+-+---+-+
        | Clients   |                                | PC B |  | PC C |
        +-----------+                                +------+  +------+
```



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
