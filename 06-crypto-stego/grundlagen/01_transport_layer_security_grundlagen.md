# TSL/SSL, Transport Layer Security (TLS) und Secure Sockets Layer (SSL)

## Inhaltsverzeichnis
- [Was ist TLS/SSL](#was-ist-tlsssl)
- [Der TLS-Handshake: Herzstück der Verschlüsselung](#der-tls-handshake-herzstück-der-verschlüsselung)
- [TLS-Record-Protokoll: Die Datenübertragung](#tls-record-protokoll-die-datenübertragung)
- [Wichtige Begriffe einfach erklärt](#wichtige-begriffe-einfach-erklärt)
    - [Session Key](#session-key)
    - [Diffie-Hellmann-Schlüsselaustausch](#diffie-hellmann-schlüsselaustausch)
    - [SSL vs. TLS](#ssl-vs-tls)
- [TLS in der Praxis: HTTPS](#tls-in-der-praxis-https)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## Was ist TLS/SSL

**TLS** (**Transport Layer Security**), der Nachfolger von **SSL** (**Secure Sockets Layer**), ist ein kryptografisches Protokoll, das die Kommunikation über ein Computernetzwerk sichert. Es wird verwendet, um sicherzustellen, dass die Daten, die zwischen einem Client (z. B. deinem Webbrowser) und einem Server ausgetauscht werden, **vertraulich**, **authentisch** und **integer** sind.

Kurz gesagt, TLS baut einen verschlüsselten "Tunnel" auf, durch den der Datenverkehr vor Dritten geschützt wird. Es ist das Protokoll, das du jedes Mal verwendest, wenn du eine Website mit `https://` aufrufst.

| Aspekt | Beschreibung |
|--------|--------------|
| **Zweck** | Absicherung der Kommunikation im Internet. |
| **OSI-Schicht** | Schicht 5, wo es unterhalb des Anwendungsprotokolls (z.B HTTP) agiert und Daten für dieses verschlüsselt. |
| **Verschlüsselung** | Nutzt eine Kombination aus **symmetrischer** und **asymmetrischer** Kryptografie. |
| **Aktueller Stand** | **TLS 1.3** (2018); viele Sicherheitsverbesserungen gegenüber Vorgänger. |

Die beiden Hauptkomponenten, die TLS zum Funktionieren bringen, sind der **TLS-Handshake** und das **TLS-Record-Protokoll**.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Der TLS-Handshake: Herzstück der Verschlüsselung

Der **TLS-Handshake** ist der anfängliche Prozess, bei dem sich Client und Server authentifizieren, die Verschlüsselungsalgorithmen aushandeln und einen gemeinsamen **Session Key** festlegen. Dieser Prozess findet jedes Mal statt, wenn eine neue TLS-Verbindung aufgebaut wird.

Das ist der Ablauf einer typischen TLS 1.3-Verbindung:

1. **Client Hello:** 
    - Dein Browser (Client) sendet eine Nachricht an den Server, in der er die zu verwendende **TLS-Version**, die unterstützten **Verschlüsselungs-Suiten** (z.B. `AES`, `ChaCha20`) und eine **zufällige Zahl** angibt. 

2. **Server Hello:** 
    - Der Server antwortet und wählt aus den Vorschlägen des Clients eine bevorzugte TLS-Version und eine Verschlüsselungs-Suite aus und sendet ebenfalls eine zufällige Zahl.

3. **Zertifikat des Servers:** 
    - Der Server sendet sein digitales Zertifikat. Dein Browser validiert dieses Zertifikat und stellt sicher, dass er wirklich mit dem beabsichtigten Server kommuniziert.

4. **Schlüsselaustausch:** 
    - Client und Server führen nun den **Diffie-Hellman-Schlüsselaustausch** durch. Jeder generiert einen privaten Schlüssel und teilt einen öffentlichen Wert. Aus diesen beiden Werten leiten beide Seiten unabhängig voneinander den gleichen geheimen **Session Key** ab, ohne dass dieser je über das Netzwerk gesendet wird.

5. **Finish:** 
    - Client und Server senden eine abschließende Nachricht, die mit dem neuen Session Key verschlüsselt ist. Diese Nachricht bestätigt, dass beide Parteien denselben Session Key abgeleitet haben und die Verbindung nun sicher ist.


**Vereinfachte Darstellung des TLS-Handshakes:**
```text
                        Client                         Server
                              |                        |
               Client Hello   |----------------------->|
    (Version, Suites, Random) |                        |
                              |                        |
                              |<-----------------------| Server Hello
                              |                        |
(Auswahl, Random, Zertifikat) |                        |
                              |                        |
                              |----------------------->| Schlüsselmaterial
                              |<-----------------------| Schlüsselmaterial
                              |                        |
                              |----------------------->| Finish
                              |<-----------------------| Finish
                              |                        |
                              <======= GESICHERT ======>
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## TLS-Record-Protokoll: Die Datenübertragung
Nachdem der Handshake erfolgreich abgeschlossen wurde, übernimmt das **TLS-Record-Protokoll**. Es ist verantwortlich für die eigentliche Übertragung der Anwendungsdaten.

Das Protokoll teilt die zu übertragenden Daten in kleinere Blöcke (**Records**) auf, verschlüsselt diese mithilfe des im Handshake ausgehandelten Session Keys und versieht sie mit einem **Message Authentication Code** (**MAC**), um die Integrität zu gewährleisten. Der MAC stellt sicher, dass die Daten während der Übertragung nicht manipuliert wurden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wichtige Begriffe einfach erklärt

### Session Key: 
Ein temporärer, symmetrischer Schlüssel, der nur für eine einzige Verbindung (Session) gültig ist. 

TLS handelt diesen für jede neue Verbindung aus, was **Forward Secrecy** gewährleistet. 
Das bedeutet, selbst wenn ein Angreifer den privaten Schlüssel des Servers stiehlt, kann er keine alten, aufgezeichneten Verbindungen entschlüsseln, da der jeweilige Session Key geheim bleibt.

### Diffie-Hellmann-Schlüsselaustausch
Ein Algorithmus, der es zwei Parteien ermöglicht, einen gemeinsamen Schlüssel zu erstellen, indem sie lediglich öffentliche Informationen austauschen. Der private Schlüssel des Servers wird dabei nie direkt übertragen.

### SSL vs. TLS
SSL ist der Vorläufer von TLS. Die Versionen `SSL 1.0`, `2.0` und `3.0` sind als unsicher eingestuft und sollten nicht mehr verwendet werden.

`TLS 1.0` und `1.1` sind ebenfalls veraltet, weshalb heute fast ausschließlich `TLS 1.2` und der aktuelle Standard `TLS 1.3` verwendet werden.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## TLS in der Praxis: HTTPS
Das bekannteste Anwendungsbeispiel für TLS ist **HTTPS** (**Hypertext Transfer Protocol Secure**). Es kombiniert das standardmäßige HTTP-Protokoll mit der Sicherheit von TLS. 

Immer wenn du `https://` in der Adressleiste siehst und ein kleines Schlosssymbol erscheint, bedeutet das, dass deine Verbindung durch **TLS** abgesichert ist.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links
- [https://de.wikipedia.org/wiki/Transport_Layer_Security](https://de.wikipedia.org/wiki/Transport_Layer_Security)
- [https://de.wikipedia.org/wiki/Perfect_Forward_Secrecy](https://de.wikipedia.org/wiki/Perfect_Forward_Secrecy)


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