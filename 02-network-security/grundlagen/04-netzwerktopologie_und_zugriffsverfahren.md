# 🗺️ Netzwerktopologien & Zugriffsverfahren

## Inhaltsverzeichnis
- [Was ist eine Netzwerktopologie?](#was-ist-eine-netzwerktopologie)
- [Physikalische Topologien](#physikalische-topologien)
- [Zugriffsverfahren](#zugriffsverfahren)
- [Haftungsausschluss](#haftungsausschluss)



## Was ist eine Netzwerktopologie?
Die **Topologie** beschreibt die Anordnung der Verbindungen in einem Netzwerk. Es ist sozusagen die "Landkarte", die den physischen oder logischen Aufbau des Netzwerks darstellt.

## Physikalische Topologien

* **Stern-Topologie:** Alle Geräte sind sternförmig mit einem zentralen Knoten (z. B. Switch oder Hub) verbunden.
    ```
                  +-----------+
                 /             \
        +---+---+              +---+---+
        | PC A  |---(Switch)---| PC B  |
        +-------+              +---+---+
                \              /
                 +------------+
                       |
                    +------+
                    | PC C |
                    +------+
    ```

* **Ring-Topologie:** Jedes Gerät ist mit seinem Vorgänger und Nachfolger verbunden, sodass ein geschlossener Ring entsteht. Daten fließen in der Regel nur in eine Richtung.
* **Mesh-Topologie:** Geräte sind über mehrere Pfade miteinander verbunden. Ein vollständig vermaschtes Netz hat Verbindungen zwischen allen Knoten, was es sehr ausfallsicher, aber auch teuer macht.
* **Bus-Topologie:** Alle Geräte sind über ein einziges, gemeinsames Kabel verbunden.



## Zugriffsverfahren
Zugriffsverfahren regeln, wie Geräte im Netzwerk auf das Übertragungsmedium zugreifen, um Kollisionen zu vermeiden.

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
Wird in **kabelgebundenen Ethernet-Netzwerken** verwendet.
1.  **Abhören:** Bevor ein Gerät sendet, horcht es, ob das Kabel frei ist.
2.  **Senden:** Ist das Kabel frei, beginnt die Übertragung.
3.  **Kollisionserkennung:** Kommt es trotzdem zu einer Kollision, stoppen alle beteiligten Geräte die Übertragung.
4.  **Warten & Wiederholen:** Jedes Gerät wartet eine zufällige Zeit, bevor es erneut versucht zu senden.

### CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)
Wird in **drahtlosen WLAN-Netzwerken** verwendet.
1.  **Abhören:** Das Gerät horcht, ob der Funkkanal frei ist.
2.  **Anfrage senden (RTS):** Wenn der Kanal frei ist, sendet es ein **RTS**-Signal (Request to Send) an den Access Point.
3.  **Bestätigung (CTS):** Der Access Point antwortet mit einem **CTS**-Signal (Clear to Send).
4.  **Datenübertragung:** Erst nach Erhalt des CTS-Signals beginnt das Gerät mit der Übertragung der Daten.
Dieses Verfahren versucht, Kollisionen **im Voraus zu vermeiden**, was in drahtlosen Netzen effektiver ist.



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
