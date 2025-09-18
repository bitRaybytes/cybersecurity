# 🤖 MQTT-Protokoll – Das Rückgrat der IoT-Kommunikation

## Inhaltsverzeichnis
- [MQTT-Protokoll](#mqtt-protokoll)
- [Kommunikation der Geräte mit MQTT](#kommunikation-der-geräte-mit-mqtt)
- [Begriffe des MQTT-Protokolls](#begriffe-des-mqtt-protokolls)
- [Regeln zu Topics](#regeln-zu-topics)
- [Quality of Service (QoS) und Last Will Testament](#quality-of-service-qos-und-last-will-testament)
- [Message Persistence (Beharrlichkeit einer Nachricht)](#message-persistence-beharrlichkeit-einer-nachricht)
- [Eigenschaften von MQTT](#eigenschaften-von-mqtt)
- [Datenformat und Ablauf mit JSON](#datenformat-und-ablauf-mit-json)
- [Sicherheit des MQTT-Protokolls](#sicherheit-des-mqtt-protokolls)
- [Fazit und Absicherungsempfehlungen](#fazit-und-absicherungsempfehlungen)
- [Gängige MQTT-Broker](#gängige-mqtt-broker)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## MQTT-Protokoll
Das **Message Queuing Telemetry Transport** (**MQTT**)-Protokoll ist ein schlankes Übertragungsprotokoll für die **Machine-to-Machine** (**M2M**)-Kommunikation. Es wurde speziell für Umgebungen mit geringer Bandbreite und hoher Latenz entwickelt.

MQTT nutzt ein Publisher-Subscriber-Muster (Senden und Abonnieren von Daten). Der Kern dieses Modells ist der **MQTT-Broker**, der die Kommunikation zwischen Sendern (Publishern) und Empfängern (Subscribern) entkoppelt.

```text
+-------------------+      (Publish)      +-------------------------+      (Subscribe)    +-------------------+
|      Client       |-------------------->|          Broker         |-------------------->|       Client      |
|    (Publisher)    |                     |       (Der Server)      |                     |    (Subscriber)   |
+-------------------+                     |                         |                     +-------------------+
                                          | - Leitet Nachrichten an |
                                          |   alle Subscriber eines |
                                          |   Topics weiter.        |
                                          +-------------------------+

```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Kommunikation der Geräte mit MQTT
Die Kommunikation zwischen IoT-Geräten und dem Broker kann über verschiedene Wege erfolgen:

- **Mobilfunk:** GSM, 5G, LTE

- **Drahtlose Kommunikation:** WLAN, Bluetooth
- **Nahfeldkommunikation:** RFID, NFC



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Begriffe des MQTT-Protokolls

| Begriff | Beschreibung |
|---------|--------------|
| **MQTT-Broker** | Empfängt Nachrichten von Publishern und leitet sie an alle Subscriber weiter, die das entsprechende Topic abonniert haben. Man unterscheidet zwischen nativen **Brokern** (eigens entwickelt) und **konnektorunterstützenden Brokern**. |
| **MQTT-Client** | Jedes Gerät, das sich mit dem Broker verbindet. Ein Client kann sowohl ein Publisher (Nachrichtensender) als auch ein Subscriber (Nachrichtenempfänger) sein, von einem kleinen Mikrocontroller bis zu einem ganzen Server. |
| **MQTT-Topic** | Eine Art Adressierung, die vom Broker verwendet wird, um Nachrichten der Clients zu filtern und zuzustellen. Topics sind hierarchisch und durch einen Schrägstrich (`/`) getrennt. Sie sind Case-Sensitive. Das `$`-Zeichen am Anfang eines Topics ist für interne Broker-Statistiken reserviert. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Regeln zu Topics

- **Vermeide einen Schrägstrich (`/`) am Beginn eines Topics**. Dies erzeugt ein leeres Topic-Level.

- **Vermeide Leerzeichen in Topics.**

- **Halte Topics kurz und aussagekräftig.**

- **Verwende keine Sonderzeichen** (z. B. `ä`, `ö`, `ü`).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Topic Wildcards – einstufig
Mit dem `+`-Zeichen können Sie ein beliebiges einzelnes Topic-Level ersetzen.

- **Beispiel-Topic:** `Buerogebaeude/Erdgeschoss/+/Temperatur`

- **Passende Topics:** `Buerogebaeude/Erdgeschoss/Serverraum/Temperatur`, `Buerogebaeude/Erdgeschoss/Besprechungszimmer/Temperatur`



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Topic Wildcards – mehrstufig
Mit dem `#`-Zeichen werden alle nachfolgenden Topic-Level ersetzt, unabhängig von ihrer Tiefe. Das `#` muss immer am Ende eines Topics stehen.

- **Beispiel-Topic:** `Buerogebaeude/Erdgeschoss/#`

- **Passende Topics:** `Buerogebaeude/Erdgeschoss/Serverraum/Temperatur`, `Buerogebaeude/Erdgeschoss/Besprechungszimmer/CO2`





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Quality of Service (QoS) und Last Will Testament
Das MQTT-Protokoll bietet verschiedene Quality of Service (QoS)-Stufen, die definieren, wie zuverlässig eine Nachricht zugestellt wird.

| Stufe | Beschreibung |
|-------|--------------|
| **QoS0: At most once** | Nachricht wird **höchstens einmal** gesendet. Es gibt keine Garantie, dass sie ankommt. Der Publisher kümmert sich nach dem Senden nicht mehr um die Nachricht. |
| **QoS1: At least once** | **Garantiert**, dass die Nachricht **mindestens einmal** beim Empfänger ankommt. Es ist möglich, dass Duplikate eintreffen. |
| **QoS2: Exactly once** | **Garantiert**, dass die Nachricht **genau einmal** beim Empfänger ankommt. Dies erfordert einen zweistufigen Bestätigungsablauf und ist die sicherste, aber auch ressourcenintensivste Stufe. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Last Will Testament (LWT)

Das **Last Will Testament** ist eine Funktion, bei der ein Publisher eine "letzte Nachricht" definiert. Wenn die Verbindung des Publishers zum Broker unerwartet unterbrochen wird, sendet der Broker diese vordefinierte Nachricht an alle Subscriber, die das LWT-Topic abonniert haben. Dies ist nützlich, um den Ausfall eines Geräts zu signalisieren.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Message Persistence (Beharrlichkeit einer Nachricht)
- **Retained Messages:** Wenn ein Publisher eine Nachricht mit dem `retained`-Flag sendet, speichert der Broker die letzte Nachricht für das Topic. Wenn ein neuer Subscriber das Topic abonniert, erhält er sofort die letzte gespeicherte Nachricht.

- **Persistent Sessions:** Clients können eine dauerhafte Verbindung (`Clean Session = false`) aufbauen, die auch bei physischer Unterbrechung bestehen bleibt. Dies ermöglicht es einem Client, Nachrichten zu erhalten, die während seiner Abwesenheit gesendet wurden.

- **Keep Alive:** Clients können dem Broker einen Zeitraum mitteilen, nach dem inaktive Verbindungen automatisch getrennt werden sollen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Eigenschaften von MQTT

| Eigenschaft | Beschreibung |
|-------------|--------------|
| **Performance** | MQTT ist sehr performant und effizient. Die Header sind binär und klein, was es ideal für den schnellen Austausch kleiner Nachrichten (wenige Kilobyte) macht. |
| **Skalierbarkeit** | Durch das Publisher-Subscriber-Prinzip ist MQTT sehr einfach skalierbar, da die Geräte nicht direkt miteinander kommunizieren müssen. |
| **Sicherheit** | MQTT unterstützt von Haus aus nur eine Authentifizierung mittels Benutzername und Passwort. Eine sichere Kommunikation erfordert zusätzliche Maßnahmen. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Datenformat und Ablauf mit JSON
Oft werden Nachrichten im JSON-Format (JavaScript Object Notation) über MQTT versendet.

- **Ablauf:**

    1. Ein Sensor misst z. B. die Temperatur und erstellt ein JSON-Objekt mit den Daten.
    
    2. Ein MQTT-Client (Publisher) sendet die JSON-Daten an ein entsprechendes Topic.
    
    3. Der MQTT-Broker empfängt die Nachricht und leitet sie an alle Subscriber weiter.
    
    4. Ein Subscriber (z. B. ein Server) empfängt die JSON-Nachricht, parst und verarbeitet die Daten.

- **Vorteile:**

    - **Menschlesbar:** JSON ist eine leicht verständliche Datenstruktur.

    - **Zuverlässigkeit:** MQTT ermöglicht eine zuverlässige Übertragung der JSON-Nachrichten.

    - **Plattformunabhängig:** JSON kann in nahezu jeder Programmiersprache einfach verarbeitet werden.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheit des MQTT-Protokolls
Die Sicherheit des MQTT-Protokolls ist ein kritischer Aspekt, der oft vernachlässigt wird. Von Haus aus bietet MQTT nur eine schwache Absicherung, weshalb zusätzliche Maßnahmen unabdingbar sind.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 1. Authentifizierung & Autorisierung
Die Standard-Authentifizierung über Benutzername und Passwort ist die erste Verteidigungslinie. Ein Broker muss aber auch die **Autorisierung** der Clients überprüfen, um zu steuern, welche Clients Nachrichten veröffentlichen oder abonnieren dürfen.

- **Schwachstelle:** Unsichere Konfigurationen erlauben anonymen Zugriff (`allow_anonymous true`). Dies ermöglicht es jedem, sich mit dem Broker zu verbinden, Nachrichten zu senden oder zu empfangen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Verschlüsselung auf der Transportebene
Ohne Verschlüsselung werden die Daten im Klartext übermittelt. Dies macht sie anfällig für das Abhören (`sniffing`) und die Manipulation. Die Absicherung der Kommunikation über **SSL/TLS** ist daher zwingend erforderlich.

- **Schwachstelle:** Ein Angreifer kann die unverschlüsselten Pakete abfangen und die Sensordaten manipulieren oder Befehle an Geräte senden.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Zugriffskontrolle über Topics
Broker können eine Zugriffsverwaltung auf Topic-Ebene (`Topic ACLs - Access Control Lists`) implementieren. So wird sichergestellt, dass Clients nur auf die Topics zugreifen, für die sie berechtigt sind.

```text
+---------------------+      (Publish)       +-------------------------------------+     (Subscribe)      +----------------------+
|  Client A           |--------------------->|   Broker mit ACLs                   |--------------------->|   Client B           |
|  (Autorisiert für   | (Topic: Sensor/Temp) |                                     | (Topic: Sensor/Temp) |   (Autorisiert für   |
|  Topic Sensor/Temp) |                      | - Client C (nicht autorisiert) wird |                      |   Topic Sensor/Temp) |
+---------------------+                      |   Zugriff auf Topic Sensor/Temp     |                      +----------------------+
                                             |   verwehrt.                         |
                                             +-------------------------------------+
```

- **Schwachstelle:** Eine fehlende oder fehlerhafte ACL kann dazu führen, dass ein Angreifer, der sich als einfacher Client ausgibt, Nachrichten an kritische Topics (`Zuhause/Licht/Kueche/An`) senden oder sie abhören kann.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Zertifikate und gegenseitige Authentifizierung
Für eine maximale Sicherheit sollte der Broker Clients nicht nur über Benutzername/Passwort, sondern auch über **Client-Zertifikate** authentifizieren. Die **gegenseitige TLS-Authentifizierung** (**mTLS**) sorgt dafür, dass sowohl der Client den Broker als auch der Broker den Client verifiziert, bevor eine Kommunikation stattfindet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Fazit und Absicherungsempfehlungen
Die Sicherheit eines MQTT-Systems hängt weniger vom Protokoll selbst ab, sondern vielmehr von der **Konfiguration des Brokers**. Jede der oben genannten Schwachstellen kann durch bewährte Praktiken abgewendet werden:

- **Aktiviere SSL/TLS:** Verschlüssel die Kommunikation zwischen Client und Broker.

- **Deaktiviere anonymen Zugriff:** Setze `allow_anonymous false` im Broker.

- **Implementiere Topic ACLs:** Definiere genau, welche Clients welche Topics nutzen dürfen.

- **Verwende starke Authentifizierung:** Nutze komplexe Passwörter oder, falls möglich, Zertifikate.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Gängige MQTT-Broker
| Anbieter | Beschreibung |
|----------|--------------|
| **Eclipse Mosquitto** | Ein weit verbreiteter, quelloffener Broker. Einfach zu installieren und ideal für kleine bis mittlere Anwendungen. |
| **HiveMQ** | Ein kommerzieller Broker, der für Skalierbarkeit und Zuverlässigkeit in Unternehmen konzipiert ist. |
| **VerneMQ** | Ein hoch skalierbarer Broker, der auf verteilten Architekturen basiert. Geeignet für sehr hohes Datenaufkommen. |
| **Cloud-Anbieter** | Anbieter wie Amazon Web Services (AWS), Microsoft Azure und Google Cloud bieten verwaltete MQTT-Broker als Teil ihrer IoT-Plattformen an. |



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