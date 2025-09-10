# 🌐 HTTP & HTTPS: Die Sprache des Webs

## Inhaltsverzeichnis
- [Was ist HTTP?](#was-ist-http)
- [Warum HTTPS?](#warum-https)
- [Wie funktioniert HTTPS?](#wie-funktioniert-https)
- [Der "Schlosseffekt"](#der-schlosseffekt)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

## Was ist HTTP?
**HTTP** (**Hypertext Transfer Protocol**) ist das grundlegende Protokoll, das die Kommunikation zwischen deinem Webbrowser (Client) und einem Webserver regelt. Wenn du eine Webseite aufrufst, sendet dein Browser eine HTTP-Anfrage an den Server, und dieser antwortet mit den Daten der Webseite.

Stell dir `HTTP` wie eine Postkarte vor:
```text
+--------------+                                       +--------------+
| Dein Browser |-------------------------------------->| Webserver    |
| (Postkarte)  |                                       | (Webseite)   |
+--------------+                                       +--------------+
                [ Deine Anfrage ist lesbar für Dritte ]
```

Die Übertragung erfolgt im Klartext. Das bedeutet, dass jeder, der den Datenverkehr mitschneiden kann (z. B. in einem öffentlichen WLAN), die gesendeten Informationen lesen kann. Dies ist für sensible Daten wie Passwörter, Kreditkarteninformationen oder persönliche Nachrichten ein enormes Sicherheitsrisiko.

## Warum HTTPS?

**HTTPS** (**Hypertext Transfer Protocol Secure**) ist die sichere Variante von HTTP. Es verwendet eine zusätzliche Verschlüsselungsschicht, um die Vertraulichkeit und Integrität der Kommunikation zu gewährleisten. Der entscheidende Unterschied liegt im `S` am Ende, welches für `Secure` steht.

HTTPS kombiniert **HTTP** mit dem [**TLS-Protokoll**](/06-crypto-stego/grundlagen/01_transport_layer_security_grundlagen.md) (**Transport Layer Security**). TLS ist wie ein sicherer Umschlag für deine Postkarte, der den Inhalt vor neugierigen Blicken schützt.

## Wie funktioniert HTTPS?
HTTPS funktioniert, indem es vor dem eigentlichen Datenaustausch einen **TLS-Handshake** durchführt.   Dieser Handshake hat drei Hauptziele:

1. **Authentifizierung:** Der Server weist seine Identität mit einem digitalen Zertifikat nach. Dein Browser überprüft die Echtheit des Zertifikats.

2. **Schlüsselaustausch:** Client und Server handeln einen gemeinsamen Sitzungsschlüssel aus, der für die schnelle symmetrische Verschlüsselung der Datenübertragung verwendet wird.

3. **Verschlüsselung:** Alle nachfolgenden HTTP-Anfragen und -Antworten werden mit diesem Sitzungsschlüssel verschlüsselt.

Das Ergebnis ist eine sichere Verbindung, die den Schutz deiner Daten gewährleistet.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

**Vereinfachte Darstellung vom HTTP-Protokoll:**

```text
+------------------+                                          +--------------+
| Dein Browser     |----------------------------------------->| Webserver    |
| (Verschlüsselter |                                          | (Webseite)   |
| Umschlag)        |<-----------------------------------------|              |
+------------------+                                          +--------------+
                  [ Kommunikation ist für Dritte nicht lesbar ]
```

👉 **Vertiefung: Was ist ein digitales Zertifikat?**  
Ein [**digitales Zertifikat**](/06-crypto-stego/kryptografie/02_digitale_zertifikate.md) ist ein elektronischer Ausweis, der von einer vertrauenswürdigen Stelle (einer Zertifizierungsstelle oder CA) ausgestellt wird. Es enthält den öffentlichen Schlüssel des Servers und bestätigt dessen Identität. **Ohne** ein gültiges Zertifikat kann **keine** HTTPS-Verbindung aufgebaut werden.


## Der "Schlosseffekt"

Wenn du eine Website mit HTTPS besuchst, siehst du in der Adressleiste deines Browsers ein kleines Schlosssymbol. Das ist das Zeichen dafür, dass die Verbindung sicher ist.

- **Grünes Schloss:** Die Verbindung ist sicher und das Zertifikat ist gültig.

- **Rotes Schloss oder Warnsymbol:** Es gibt ein Problem mit der Sicherheit der Verbindung, z. B. ein abgelaufenes Zertifikat oder eine unsichere Konfiguration.

Immer wenn du sensible Daten wie Passwörter, Finanzinformationen oder persönliche Nachrichten eingibst, solltest du sicherstellen, dass die Verbindung über HTTPS läuft.

## Nützliche Links
- Wikipedia: [https://de.wikipedia.org/wiki/Hypertext_Transfer_Protocol_Secure](https://de.wikipedia.org/wiki/Hypertext_Transfer_Protocol_Secure)
- RFC 9110: [https://datatracker.ietf.org/doc/html/rfc9110](https://datatracker.ietf.org/doc/html/rfc9110)
- RFC 2818: [https://datatracker.ietf.org/doc/html/rfc2818](https://datatracker.ietf.org/doc/html/rfc2818)

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
