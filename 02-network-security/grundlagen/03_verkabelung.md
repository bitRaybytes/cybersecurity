# 🔌 Netzwerkkabel: Arten und Eigenschaften

## Inhaltsverzeichnis
- [Ethernet Standards](#ethernet-standards)
- [Kupferkabel-Varianten](#kupferkabel-varianten)
- [Lichtwellenleiter (Glasfaserkabel)](#lichtwellenleiter-glasfaserkabel)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Ethernet Standards
Ethernet ist der am häufigsten verwendete Standard für kabelgebundene Netzwerke.

* **Fast Ethernet (100 Mbit/s):**
    * Basiert auf IEEE 802.3u.
    * Verwendet **Full-Duplex**, d.h. Senden und Empfangen kann gleichzeitig stattfinden.
    * **Autosensing:** Erkennt automatisch die Geschwindigkeit (10 oder 100 Mbit/s).
    * **Autonegotiation:** Bestimmt den Betriebsmodus (Full- oder Half-Duplex).

* **Gigabit Ethernet (1 Gbit/s):**
    * Ermöglicht eine Übertragungsgeschwindigkeit von bis zu 1 Gbit/s.
    * Nutzt den **Burst-Modus**, um mehrere kleine Datenpakete zu einem großen zusammenzufassen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Kupferkabel-Varianten
Kupferkabel werden nach ihrer Leistung in Kategorien (`Cat`) unterteilt. Je höher die Kategorie, desto besser die Übertragungsgeschwindigkeit und Schirmung.

* **Twisted-Pair:** Die Adernpaare in diesen Kabeln sind miteinander verdrillt. Dies reduziert elektromagnetische Störungen und verbessert die Signalqualität.
* **Schirmung:** Schirmungen schützen die Kabel vor externen Störungen (z. B. durch Stromkabel) und reduzieren die Abstrahlung des eigenen Signals.

| Kategorie | Typische Geschwindigkeit | max. Distanz |
|-----------|-------------------------|-------|
| Cat 5e    | bis zu 1 Gbit/s         | 100 Meter |
| Cat 6     | bis zu 1 Gbit/s         | 55 Meter |
| Cat 6a    | bis zu 10 Gbit/s        | 100 Meter |
| Cat 7     | bis zu 10 Gbit/s        | 100 Meter |
| Cat 7a    | bis zu 40 Gbit/s        | 50 Meter |
| Cat 7a    | bis zu 100 Gbit/s       | 15 Meter |
| Cat 8     | bis zu 40 Gbit/s        | 30 Meter |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Unterscheidung der Schirmung

| Schirmung    | Beschreibung             |
|--------------|--------------------------|
| **U**        | Unshielded (ungeschrimt), Keine Abschirmung vorhanden. |
| **F**        | Foiled, Folienabschirmung. |
| **S**        | Shielded, Geflechtsabschirmung. |
| **TP**       | Twisted Pair, verdrillte Adernpaare, um Signalqualität zu verbessern.|


- **Beispiele:**
    - **S/FTP:** Shielded mit Folienabschirmung verdrehten Paaren
    - **F/FTP:** Folienabschirmung mit verdrehten Paaren 
    - **U/FTP:** ungeschirmt mit Folienarbschirmung von verdrehten Paaren


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Kupferkabel Kenngrößen:
- **Dämpfung:** Abnahme des Signalpegels bei zunehmender Länge
- **Übertragungsgeschwindigkeit:** Anzahl der Datenpakete, die in einer bestimmten Zeiteinheit übertragen werden können
- **Reichweite:** bis zu welcher Länge können Daten ohne Verlust übermittelt werden?



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Lichtwellenleiter (Glasfaserkabel)
**Glasfaserkabel** übertragen Daten in Form von Lichtsignalen durch einen dünnen Glas- oder Kunststoffkern. Sie bieten extrem hohe Geschwindigkeiten und eine sehr große Reichweite.

* **Aufbau:**
    * **Kern:** Der innere Kern, durch den das Lichtsignal gesendet wird.
    * **Mantel:** Eine Schicht um den Kern, die das Licht im Kern reflektiert.
    * **Schutzschicht:** Eine äußere Schicht, die das Kabel schützt.

* **Singlemode vs. Multimode:**
    * **Singlemode:** Sehr dünner Kern. Nur ein Lichtstrahl wird geleitet. Ideal für **weite Strecken** (bis zu 40 km oder mehr).
    * **Multimode:** Größerer Kern. Mehrere Lichtstrahlen werden gleichzeitig geleitet. Ideal für **kurze Strecken** (bis zu 2 km).




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
