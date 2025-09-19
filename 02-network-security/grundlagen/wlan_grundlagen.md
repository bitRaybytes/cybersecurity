# 🛡️ WLAN-Sicherheit – Grundlagen und Angriffsvektoren
## Inhaltsverzeichnis
- [WLAN vs. Wi-Fi](#wlan-vs-wi-fi)
- [WLAN-Betriebsmodi](#wlan-betriebsmodi)
- [Die wichtigsten WLAN-Sicherheitsstandards](#die-wichtigsten-wlan-sicherheitsstandards)
- [Grundbegriffe aus der Praxis](#grundbegriffe-aus-der-praxis)
- [Frequenzbänder und ihre Bedeutung](#frequenzbänder-und-ihre-bedeutung)
- [Gefahren und typische Angriffsvektoren](#gefahren-und-typische-angriffsvektoren)
- [Best Practices für die WLAN-Absicherung](#best-practices-für-die-wlan-absicherung)
- [Haftungsausschluss](#haftungsausschluss)

## WLAN vs. Wi-Fi
In der Praxis werden die Begriffe **WLAN** (**Wireless Local Area Network**) und **Wi-Fi** oft synonym verwendet. Aus technischer und professioneller Sicht gibt es jedoch einen wichtigen Unterschied:

- **WLAN** ist der allgemeine Überbegriff für ein drahtloses lokales Netzwerk, das beliebige Technologien für die Funkverbindung nutzen kann.

- **Wi-Fi** ist ein Zertifizierungslabel der **Wi-Fi Alliance**. Dieses Label wird nur an Geräte vergeben, die den technischen Standard **IEEE 802.11** erfüllen und erfolgreich auf Kompatibilität getestet wurden.

Für IT-Sicherheitsexperten ist es wichtig, die genaue Spezifikation (`802.11a/b/g/n/ac/ax`) zu kennen, da jede Version unterschiedliche Geschwindigkeiten, Frequenzen und Sicherheitsfunktionen mit sich bringt.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## WLAN-Betriebsmodi
Die Art und Weise, wie Geräte in einem drahtlosen Netzwerk kommunizieren, wird durch den Betriebsmodus bestimmt.

### Infrastructure-Modus
Dies ist der häufigste Modus in Unternehmen und Privathaushalten. WLAN-Geräte kommunizieren über einen zentralen **Access Point** (**AP**), der ihnen Zugang zu einem kabelgebundenen Netzwerk (LAN) ermöglicht. Dies ist die einzige sichere Option für Unternehmensumgebungen, da der AP als Gatekeeper und zentrale Schnittstelle dient.

### Ad-Hoc-Modus (Peer-to-Peer)
Geräte verbinden sich direkt miteinander, ohne einen Access Point. Sie bilden ein temporäres Netzwerk. Dieser Modus ist in puncto Sicherheit und Verwaltung kaum kontrollierbar und wird selten genutzt.

### Mesh-Modus
Hierbei fungieren mehrere Access Points als eine Art "Maschennetz". Sie kommunizieren untereinander und leiten den Traffic weiter. Dies bietet eine hohe Ausfallsicherheit und Reichweite, da das Netzwerk intelligent über die verschiedenen Knotenpunkte verteilt wird.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die wichtigsten WLAN-Sicherheitsstandards
Die Sicherheit eines WLANs hängt primär von seinem Verschlüsselungsstandard ab. Hier ist eine chronologische Übersicht:

| Standard | Jahr | Kryptografie | Sicherheitslage |
|----------|------|--------------|-----------------|
| **WEP**  | 1999 | RC4 mit statischem Schlüssel | **Unsicher**. Schlüssel kann innerhalb von Minuten geknackt werden. Sollte niemals mehr verwendet werden. | 
| **WPA**  | 2003 | TKIP (Temporal Key Integrity Protocol) | **Veraltet**. War eine Übergangslösung. Anfällig für Angriffe, kann heute ebenfalls schnell umgangen werden. |
| **WPA2** | 2004 | **AES-CCMP** (Advanced Encryption Standard) | **Aktueller Standard**. Bietet eine starke Verschlüsselung. Die `WPA2-PSK` Variante ist jedoch anfällig für Offline-Brute-Force-Angriffe auf den 4-Wege-Handshake. |
| **WPA3** | 2018 | **SAE** (Simultaneous Authentication of Equals) | **Zukünftiger Standard**. Erschwert das Knacken von Passwörtern erheblich, da es Angriffe mittels Offline-Brute-Force-Techniken abwehrt. Bietet zudem stärkere Sicherheit in öffentlichen WLANs. |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundbegriffe aus der Praxis
- **SSID (Service Set Identifier):** Der Name eines drahtlosen Netzwerks. Das Verstecken der SSID bietet keinerlei Sicherheit, da sie einfach über einen Netzwerkanalyse-Traffic identifiziert werden kann.

- **PSK (Pre-Shared Key):** Ein einzelnes Passwort, das von allen Geräten im Netzwerk geteilt wird. Dies ist der typische Anmeldemechanismus in privaten Netzwerken.

- **WPS (Wi-Fi Protected Setup):** Eine Methode, um Geräte einfach mittels Knopfdruck oder einer PIN ins Netzwerk zu integrieren. Die WPS-PIN kann jedoch mit Tools wie `Reaver` innerhalb weniger Stunden per Brute-Force-Angriff geknackt werden. **WPS sollte daher in sicherheitsrelevanten Umgebungen immer deaktiviert werden**.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Frequenzbänder und ihre Bedeutung
WLAN-Geräte funken primär auf zwei Frequenzbändern, die jeweils Vor- und Nachteile mit sich bringen:

| **Band** | 2,4 GHz | 5 GHz |
|----------|---------|-------|
| Vorteil  | Höhere Reichweite, besserer Empfang durch Wände | Höhere Geschwindigkeit, weniger Überlastung, mehr Kanäle |
| Nachteil | Geringere Geschwindigkeit, viele Störquellen (Bluetooth, Mikrowellen) | Geringere Reichweite, weniger durchdringend (Wände) |
   

Neuere Standards wie **Wi-Fi 6E** und **Wi-Fi 7** nutzen auch das **6-GHz-Band**, das noch mehr Kanäle und eine höhere Geschwindigkeit bietet, um die Überlastung in dicht besiedelten Gebieten zu reduzieren.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Gefahren und typische Angriffsvektoren
Gerade in offenen oder schlecht gesicherten Netzwerken lauern viele Gefahren. Es gibt unter Anderem folgende Angriffstypen:

### 1. Evil-Twin-Angriff (Rogue Access Point)
Ein Angreifer richtet einen gefälschten Access Point mit dem gleichen Namen (SSID) wie ein legitimes Netzwerk ein. Der Angreifer nutzt eine stärkere Signalstärke oder eine Deauthentication-Attacke, um Opfer auf seinen gefälschten AP zu locken. Wenn der Nutzer sich unwissentlich verbindet, kann der Angreifer den gesamten Datenverkehr abfangen, manipulieren oder Anmeldedaten stehlen.

### 2. Deauthentication-Attacke (DoS)
Diese Art des Angriffs sendet gefälschte "Deauthentifizierungs-Pakete" an einen oder alle Clients eines Netzwerks. Der Client verliert die Verbindung zum Access Point und versucht, sich erneut zu verbinden. Diese Attacke kann genutzt werden, um eine Denial-of-Service-Attacke zu initiieren oder um ein Opfer zu zwingen, sich mit einem schädlichen "Evil Twin" zu verbinden.

### 3. WPA/WPA2-PSK Handshake-Angriff
Auch wenn WPA2-PSK als sicher gilt, kann ein Angreifer das Passwort offline knacken. Er fängt den sogenannten **4-Wege-Handshake** ab, der beim Verbindungsaufbau zwischen Client und Access Point stattfindet. Mit einem Wörterbuch- oder Brute-Force-Angriff auf den aufgezeichneten Handshake kann der Angreifer das Passwort offline entschlüsseln.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Best Practices für die WLAN-Absicherung
Ein robustes drahtloses Netzwerk ist entscheidend für die Sicherheit einer Organisation. Die folgenden Maßnahmen sind unerlässlich:

- **Verwendung von WPA2-PSK oder WPA3:** Wähle immer den stärksten verfügbaren Verschlüsselungsstandard.

- **Komplexes und einzigartiges Passwort:** Verwende ein langes, komplexes WLAN-Passwort (mindestens 20 Zeichen) und ändere es regelmäßig.

- **Firmware-Updates:** Halte die Firmware deines Routers oder Access Points stets auf dem neuesten Stand, um bekannte Sicherheitslücken zu schließen.

- **Deaktiviere WPS:** Die WPS-Funktion ist eine erhebliche Sicherheitslücke und sollte immer ausgeschaltet werden.

- **Netzwerk-Segmentierung:** Richte separate Netzwerke für Gäste, Firmen-Geräte und IoT-Geräte ein. Dies verhindert, dass ein Angreifer, der sich in das Gastnetzwerk hackt, Zugriff auf das Unternehmensnetzwerk erhält.

- **Enterprise-Modus für Unternehmen:** Statt eines geteilten PSK sollte in Unternehmen der **WPA2/WPA3-Enterprise-Modus** genutzt werden. Hierbei authentifiziert sich jeder Nutzer individuell über einen zentralen **RADIUS-Server**.

### Schematische Darstellung der Enterprise-Authentifizierung

```text
+----------+      +------------+         +-----------------+
| Benutzer | ---> | Access     | ------> | RADIUS          |
+----------+      | Point (AP) |         | Server          |
                  |            |         +-----------------+
                  |            |                  ^
                  |            |                  |
                  |            |           Authentifizierungs-
                  |            |           Anfrage/Antwort
                  |            |                  |
                  v            v                  v
                +-------------------+        +--------------+
                | WLAN-Anmeldedaten | <----> | Verzeichnis- |
                | (Login/Zertifikat)|        | dienst (AD)  |
                +-------------------+        +--------------+
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