# DHCP (Dynamic Host Configuration Protocol)
## Inhaltsverzeichnis
- [Was ist DHCP (Dynamic Host Configuration Protocol)](#was-ist-dhcp-dynamic-host-configuration-protocol)
- [Das DORA-Prinzip: Der DHCP-Ablauf](#das-dora-prinzip-der-dhcp-ablauf)
    - [Der DORA-Handshake](#der-dora-handshake)
    - [DHCP-Lease-Verlängerung](#dhcp-lease-verlängerung)
    - [DHCP-Relay](#dhcp-relay)
- [Vor- und Nachteile von DHCP](#vor--und-nachteile-dhcp)
- [Sicherheitsaspekte](#sicherheitsaspekte)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist DHCP (Dynamic Host Configuration Protocol)
**DHCP** ist ein Netzwerkprotokoll, das automatisch die Netzwerkkonfiguration an Clients verteilt. Es macht es unnötig, Geräte wie Computer, Smartphones oder IoT-Geräte manuell mit einer **IP-Adresse**, **Subnetzmaske**, **Gateway** und **DNS-Server-Adressen** zu konfigurieren.

- **Ports:** DHCP nutzt die UDP-Ports **67** (**Server**) und **68** (**Client**).

- **RFC:** Das Protokoll ist im [RFC 2131](https://www.rfc-editor.org/rfc/rfc2131.html) definiert.

Weitere Infos zum Protokoll Header von DHCP und DNS findest du hier: => [protokoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Das DORA-Prinzip: Der DHCP-Ablauf
Der Prozess, bei dem ein Client eine IP-Adresse von einem DHCP-Server erhält, folgt dem sogenannten DORA-Prinzip, das aus vier Schritten besteht:

### Der DORA-Handshake
```text
+----------+                           +-------------+
|  Client  |                           | DHCP-Server |
+----------+                           +-------------+
    |                                        |
    |------ DHCP DISCOVER (Broadcast) ------>|  D
    |          (Absender: 0.0.0.0)           |  
    |                                        |  
    |<--- DHCP OFFER (Unicast/Broadcast) ----|  O
    |        (Angebot der IP-Adresse)        |
    |                                        |
    |------- DHCP REQUEST (Broadcast)  ----->|  R
    |       (Anforderung des Angebots)       |
    |                                        |
    |<-------- DHCP ACK (Unicast) -----------|  A
    |       (Bestätigung der Zuweisung)      |
    |                                        |
    |----------- Netzwerkzugriff ----------->|
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### D - Discover:

- Der Client, der eine IP-Adresse benötigt, sendet eine Broadcast-Nachricht (`DHCP Discover`) an alle Geräte im Netzwerk. Die Absender-IP ist `0.0.0.0`, da der Client noch keine eigene hat.

#### O - Offer:

- Ein oder mehrere DHCP-Server antworten mit einem Angebot (`DHCP Offer`), das eine vorgeschlagene IP-Adresse und andere Konfigurationsdetails wie die Subnetzmaske und das Gateway enthält.

#### R - Request:

- Der Client wählt ein Angebot aus und sendet eine `DHCP Request` als Broadcast, um die vorgeschlagene IP-Adresse offiziell anzufordern. Dadurch wissen auch andere Server, dass ihre Angebote nicht angenommen wurden.

#### A - Acknowledge:

- Der ausgewählte DHCP-Server bestätigt die Zuweisung mit einem `DHCP Acknowledge` (`ACK`). Die IP-Adresse ist nun für eine bestimmte Lease-Zeit für den Client reserviert.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### DHCP-Lease-Verlängerung
Die dem Client zugewiesene Adresse gehört ihm nur für eine bestimmte Dauer (Lease-Zeit). Um die Konfiguration beizubehalten, muss der Client die Lease-Zeit verlängern, bevor sie abläuft.

- **T1 - Verlängerungszeitpunkt (50% der Lease-Zeit):** Der Client sendet eine Unicast-Nachricht direkt an den ursprünglichen DHCP-Server, um die Lease zu verlängern.

- **T2 - Wiederbindungszeitpunkt (87,5% der Lease-Zeit):** Wenn der Client keine Antwort vom ursprünglichen Server erhalten hat, sendet er erneut eine Broadcast-Nachricht an alle DHCP-Server im Netzwerk, um die Lease zu erneuern.

- **Lease-Ablauf (100%):** Wenn die Lease-Zeit vollständig abgelaufen ist, gibt der Client seine IP-Adresse frei und beginnt den DORA-Prozess von Neuem, um eine neue IP-Adresse zu erhalten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### DHCP-Relay
Ein **DHCP-Relay** ist eine Funktion auf einem Router, die DHCP-Anfragen über Netzwerksegmente hinweg weiterleitet. Dies ist nützlich, wenn ein einziges DHCP-Subnetz für mehrere physische Netzwerke konfiguriert werden soll.

- Der Client sendet eine Broadcast-Anfrage (`DHCP Discover`) im lokalen Netzwerk.
- Der Router (`DHCP-Relay-Agent`) empfängt die Anfrage und leitet sie als Unicast an den konfigurierten DHCP-Server im entfernten Netzwerk weiter.
- Der DHCP-Server sendet seine Antwort (`DHCP Offer`) an das DHCP-Relay, das die Antwort dann an den ursprünglichen Client im lokalen Netzwerk weiterleitet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Vor- und Nachteile DHCP

| **Vorteile** | **Nachteile** |
|--------------|---------------|
| **Geringer Verwaltungsaufwand:** Automatische Konfiguration. | **Abhängigkeit vom Server:** Fällt der Server aus, können keine neuen Geräte ins Netz. |
| **Fehlerminimierung:** Reduziert manuelle Tippfehler. | **Sicherheitsrisiken:** Ein Rogue DHCP-Server könnte falsche Konfigurationen verteilen. |
| **Skalierbarkeit:** Ideal für große Netzwerke mit vielen Geräten. | **Lease-Konflikte:** Selten, aber möglich, wenn die Lease-Erneuerung fehlschlägt. |
| **Mobile Geräte:** Einfache Integration von Laptops, Smartphones, etc. |**Dynamische Adressen:** Ungünstig für Server, die eine feste IP benötigen. |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsaspekte
Die größte Bedrohung für DHCP ist ein **Rogue DHCP-Server**. Ein Angreifer fügt einen nicht autorisierten DHCP-Server in das Netzwerk ein. Dieser Server kann:

- Clients falsche IP-Adressen, Gateways und DNS-Server zuweisen.
- Den Netzwerkverkehr umleiten und einen [Man-in-the-Middle-Angriff](/02-network-security/angriffe/mitm_angriff.md) (**MitM**) durchführen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Maßnahmen

- **DHCP Snooping:** Eine Sicherheitsfunktion auf Switches, die legitime DHCP-Server identifiziert und nur deren Antworten zulässt.
- **Port-Security:** Begrenzung der Anzahl von MAC-Adressen pro Switch-Port, um unautorisierte Geräte zu verhindern.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links
- [protokoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md)
- [Wikipedia: DHCP](https://de.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)



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