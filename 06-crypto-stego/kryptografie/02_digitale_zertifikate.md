# 🔐 Digitale Zertifikate: Grundlagen der Authentifizierung

## Inhaltsverzeichnis
- [Was ist ein digitales Zertifikat?](#was-ist-ein-digitales-zertifikat)
- [Wichtige Bestandteile](#wichtige-bestandteile)
    - [Die Rolle der PKI](#die-rolle-der-pki)
- [Wie funktioniert die Authentifizierung?](#wie-funktioniert-die-authentifizierung)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## Was ist ein digitales Zertifikat?
Ein **digitales Zertifikat** ist ein elektronischer Nachweis, der die Identität einer Person, eines Servers oder einer Organisation in einem Netzwerk bestätigt. Es dient als digitaler Ausweis und wird von einer vertrauenswürdigen Stelle, der sogenannten **Zertifizierungsstelle** (**Certificate Authority - CA**), ausgestellt.

Stell dir ein digitales Zertifikat wie einen Personalausweis vor:

- Der Ausweis (das Zertifikat) bestätigt, wer du bist.
- Er wird von einer offiziellen Behörde (der CA) ausgestellt.

Jeder kann die Gültigkeit deines Ausweises überprüfen, indem er bei der ausstellenden Behörde nachfragt.

Das Zertifikat bindet einen **öffentlichen kryptografischen Schlüssel** an die Identität des Besitzers. Dieser öffentliche Schlüssel wird dann verwendet, um die vom Besitzer signierten Daten zu überprüfen.

## Wichtige Bestandteile
Jedes digitale Zertifikat enthält mehrere kritische Informationen, um seine Authentizität zu gewährleisten:

- **Öffentlicher Schlüssel:** Der Schlüssel, der zum Ver- und Entschlüsseln von Daten verwendet wird.
- **Seriennummer:** Eine eindeutige Identifikationsnummer, die von der CA vergeben wird.
- **Gültigkeitsdauer:** Der Zeitraum, in dem das Zertifikat gültig ist (von-bis).
- **Aussteller:** Die Zertifizierungsstelle (CA), die das Zertifikat ausgestellt hat.
- **Betreff:** Der Name der Person, des Geräts oder der Organisation, für das das Zertifikat ausgestellt wurde.
- **Digitale Signatur:** Eine von der CA mit ihrem privaten Schlüssel erstellte Signatur, die die Integrität des Zertifikats bestätigt und eine Manipulation verhindert.

### Die Rolle der PKI
Die **Public Key Infrastructure** (**PKI**) ist das umfassende System, das für die Erstellung, Verwaltung, Verteilung und den Widerruf von digitalen Zertifikaten verantwortlich ist. Die PKI bildet die Vertrauenskette, die es ermöglicht, die Echtheit eines Zertifikats zu überprüfen.

Die wichtigsten Komponenten einer PKI sind:

- **Zertifizierungsstelle (CA):** Die vertrauenswürdige Instanz, die Zertifikate ausstellt und digital signiert.
- **Registrierungsstelle (RA):** Überprüft die Identität des Anfragenden, bevor die CA ein Zertifikat ausstellt.
- **Zertifikatsspeicher:** Ein Verzeichnis, in dem Zertifikate gespeichert und öffentlich zugänglich gemacht werden.
- **Sperrliste (CRL):** Eine Liste der ungültigen oder widerrufenen Zertifikate.

## Wie funktioniert die Authentifizierung?
Digitale Zertifikate werden in zahlreichen Szenarien zur Authentifizierung eingesetzt, beispielsweise bei der verschlüsselten Kommunikation über HTTPS.

**Ablauf bei einer HTTPS-Verbindung:**

1. Ein Client (z. B. dein Browser) möchte eine sichere Verbindung zu einem Server herstellen.

2. Der Server sendet sein digitales Zertifikat an den Client.

3. Der Client überprüft das Zertifikat:

    - Ist die digitale Signatur der CA gültig?

    - Ist das Zertifikat noch gültig und nicht abgelaufen?

    - Ist das Zertifikat nicht auf der Sperrliste (CRL) der CA?

4. Wenn alle Prüfungen erfolgreich sind, vertraut der Client dem Server. Die Kommunikation kann nun sicher und verschlüsselt beginnen.

Durch diesen Prozess wird sichergestellt, dass der Client tatsächlich mit dem beabsichtigten Server kommuniziert und nicht mit einem Angreifer, der sich als der Server ausgibt.

## Nützliche Links
- [https://de.wikipedia.org/wiki/Digitales_Zertifikat](https://de.wikipedia.org/wiki/Digitales_Zertifikat)

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