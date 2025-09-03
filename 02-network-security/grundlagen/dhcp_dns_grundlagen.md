# DHCP und DNS: Grundlagen der Netzwerkdienste

## Inhaltsverzeichnis
- [DHCP (Dynamic Host Configuration Protocol)](#dhcp-dynamic-host-configuration-protocol)
    - [Das DORA-Prinzip: Der DHCP-Ablauf](#das-dora-prinzip-der-dhcp-ablauf)
    - [IP-Lease-Verlängerung](#ip-lease-verlängerung)
    - [DHCP-Relay](#dhcp-relay)
    - [Vor- und Nachteile von DHCP](#vor--und-nachteile-von-dhcp)
- [DNS (Domain Name System)](#dns-domain-name-system)
    - [Funktionsweise des DNS]()
    - [DNS-Server](#dns-server)
    - [Dynamisches DNS (DDNS)](#dynamisches-dns-ddns)
        - [Nützliche Befehle](#nützliche-befehle)
- [Sicherheitsaspekte]()
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

## DHCP (Dynamic Host Configuration Protocol)
DHCP ist ein Netzwerkprotokoll, das automatisch die Netzwerkkonfiguration an Clients verteilt. Es macht es unnötig, Geräte wie Computer, Smartphones oder IoT-Geräte manuell mit einer IP-Adresse, Subnetzmaske, Gateway und DNS-Server-Adressen zu konfigurieren.

- Ports: DHCP nutzt die UDP-Ports 67 (Server) und 68 (Client).
- RFC: Das Protokoll ist im RFC 2131 definiert.

> Mehr zum Protokoll Header von DHCP und DNS findest du hier: => [protokoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md)

### Das DORA-Prinzip: Der DHCP-Ablauf

Der Prozess, bei dem ein Client eine IP-Adresse von einem DHCP-Server erhält, folgt dem sogenannten DORA-Prinzip, das aus vier Schritten besteht:

```yaml
  Client                     DHCP-Server
    |                              |
    |------- DHCP DISCOVER ------->|
    |                              |
    |<------ DHCP OFFER -----------|
    |                              |
    |------- DHCP REQUEST -------->|
    |                              |
    |<------ DHCP ACK -------------|
    |                              |
    +-----> Netzwerkzugriff <------+
```

1. **D - Discover:** 
    - Der Client, der eine IP-Adresse benötigt, sendet eine Broadcast-Nachricht (DHCP Discover) an alle Geräte im Netzwerk. Die Absender-IP ist 0.0.0.0, da der Client noch keine eigene hat.

2. **O - Offer:**
    - Ein oder mehrere DHCP-Server antworten mit einem Angebot (DHCP Offer), das eine vorgeschlagene IP-Adresse und andere Konfigurationsdetails wie die Subnetzmaske und das Gateway enthält.

3. **R - Request:**
    - Der Client wählt ein Angebot aus (meist das erste, das er erhält) und sendet eine DHCP Request als Broadcast, um die vorgeschlagene IP-Adresse offiziell anzufordern. Dadurch wissen auch andere Server, dass ihre Angebote nicht angenommen wurden.

4. **A - Acknowledge:**
    - Der ausgewählte DHCP-Server bestätigt die Zuweisung mit einem DHCP Acknowledge (ACK). Die IP-Adresse ist nun für eine bestimmte Lease-Zeit für den Client reserviert.

### IP-Lease-Verlängerung
Die dem Client zugewiesene Adresse gehört ihm nur für eine bestimmte Dauer. Um die Konfiguration beizubehalten, muss der Client die Lease-Zeit verlängern, bevor sie abläuft.

- **T1 - Verlängerungszeitpunkt (50% der Lease-Zeit):** Der Client sendet eine Unicast-Nachricht direkt an den ursprünglichen DHCP-Server, um die Lease zu verlängern.

- **T2 - Wiederbindungszeitpunkt (87,5% der Lease-Zeit):** Wenn der Client keine Antwort vom ursprünglichen Server erhalten hat, sendet er erneut eine Broadcast-Nachricht an alle DHCP-Server im Netzwerk, um die Lease zu erneuern.

- **Lease-Ablauf (100%):** Wenn die Lease-Zeit vollständig abgelaufen ist, gibt der Client seine IP-Adresse frei und beginnt den DORA-Prozess von Neuem, um eine neue IP-Adresse zu erhalten.


### DHCP-Relay
Ein DHCP-Relay ist eine Funktion auf einem Router, die DHCP-Anfragen über Netzwerksegmente hinweg weiterleitet, sodass ein einzelner DHCP-Server mehrere lokale Netzwerke versorgen kann. Das Relay leitet die Anfrage des Clients an den Server weiter und die Antwort vom Server an den Client zurück.

### Vor- und Nachteile von DHCP

| Vorteile | Nachteile |
| -------- | --------- |
**Geringer Verwaltungsaufwand:** Automatische Konfiguration. | **Abhängigkeit vom Server:** Fällt der Server aus, können keine neuen Geräte ins Netz.
**Fehlerminimierung:** Reduziert manuelle Tippfehler. | **Sicherheitsrisiken:** Ein Rogue DHCP-Server könnte falsche Konfigurationen verteilen.
Skalierbarkeit: Ideal für große Netzwerke mit vielen Geräten. | **Lease-Konflikte:** Selten, aber möglich, wenn die Lease-Erneuerung fehlschlägt.
**Mobile Geräte:** Einfache Integration von Laptops, Smartphones, etc. | **Dynamische Adressen:** Ungünstig für Server, die eine feste IP benötigen.

------ 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## DNS (Domain Name System)
DNS ist ein essenzieller Dienst, der Domain-Namen in IP-Adressen umwandelt. Es funktioniert wie ein "Telefonbuch" des Internets, da es für Menschen einfacher ist, sich Namen wie google.com zu merken, als deren zugehörige IP-Adresse.

### Funktionsweise des DNS

Die Namensauflösung im DNS ist ein hierarchischer Prozess. Wenn du eine Webseite besuchst, erfolgt die Auflösung in mehreren Schritten:

1. Lokaler DNS-Server: Dein Gerät fragt den konfigurierten DNS-Server nach der IP-Adresse der gesuchten Domain (www.example.com).

2. Root-Server: Findet der lokale DNS-Server die Information nicht, leitet er die Anfrage an einen Root-Server weiter. Dieser antwortet mit der Adresse des zuständigen TLD-Servers.

3. TLD-Server (Top-Level-Domain): Der TLD-Server (.com) verweist auf den autoritativen Nameserver, der für die Domain example.com zuständig ist.

4. Autoritativer Nameserver: Dieser Server ist die höchste Autorität für die Domain und liefert die gesuchte IP-Adresse zurück.

5. Caching: Der lokale DNS-Server speichert die erhaltene Information, um zukünftige Anfragen zu dieser Domain schneller beantworten zu können.

### DNS-Server
DNS-Server sind die "Telefonbücher" des Internets. Wenn du eine Webseite besuchst, fragt dein Gerät einen DNS-Server nach der passenden IP-Adresse.

**Öffentliche DNS-Server:**

- Cloudflare: 1.1.1.1 & 1.0.0.1
- Google: 8.8.8.8 & 8.8.4.4
- Quad9: 9.9.9.9

#### Dynamisches DNS (DDNS)
Dynamisches DNS ist ein Dienst, der den DNS-Eintrag einer Domain automatisch aktualisiert, wenn sich die öffentliche IP-Adresse ändert. Dies ist besonders nützlich für Heimnetzwerke, bei denen der Internetanbieter die öffentliche IP regelmäßig ändert. So bleibt deine Domain, z. B. meinserver.dyndns.org, immer erreichbar, auch wenn die IP-Adresse im Hintergrund wechselt.

#### Nützliche Befehle
Um DNS-Einträge zu überprüfen, kannst du folgende Befehle im Terminal verwenden:

- Linux/macOS: dig [Domain-Name] oder nslookup [Domain-Name]
- Windows: nslookup [Domain-Name]

Beispiel:
```bash
nslookup google.com
```
----

## Sicherheitsaspekte
**DHCP-Sicherheit**
Die größte Bedrohung für DHCP ist ein Rogue DHCP-Server. Ein Angreifer fügt einen nicht autorisierten DHCP-Server in das Netzwerk ein. Dieser Server kann:

- Clients falsche IP-Adressen, Gateways und DNS-Server zuweisen.
- Den Netzwerkverkehr umleiten und einen **Man-in-the-Middle-Angriff (MitM)** durchführen.

**DNS-Sicherheit**
Die häufigste Bedrohung für DNS ist DNS-Poisoning (auch Cache-Poisoning genannt). Hierbei manipuliert ein Angreifer den Cache eines DNS-Servers, indem er falsche IP-Adressen für legitime Domain-Namen einträgt. Dadurch werden Nutzer, die die korrekte Domain aufrufen, auf eine vom Angreifer kontrollierte Website umgeleitet, was für Phishing-Angriffe genutzt werden kann.

## Nützliche Links
- [protokoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md)
- [Wikipedia: DHCP](https://de.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
- [Wikipedia: DNS](https://de.wikipedia.org/wiki/Domain_Name_System)

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