# 🔐 Encoding vs. Encryption vs. Hashing

## 📘 Ziel dieser Datei

Diese Datei erklärt die Unterschiede und Anwendungsfälle von **Encoding**, **Encryption** und **Hashing** – drei häufig verwendete, aber oft verwechselte Techniken in der Cybersecurity. Das Verständnis dieser Begriffe ist essenziell für jeden, der sich mit Netzwerksicherheit, Datenverarbeitung oder Kryptographie beschäftigt.

Links zu den Tools findest du [hier](#8-nützliche-links)



## Inhaltsverzeichnis

- [1. Encoding](#1-encoding-umwandlung)
    - [Arten des Encodings](#arten-des-encodings)
    - [Beispiele](#beispiele)
- [2. Encryption (Verschlüsselung)](#2-encryption-verschlüsselung)
    - [Symmetrisches Verschlüsselung](#symmetrisches-verschlüsselung)
        - [Blockchiffre](#blockchiffre)
        - [Stromchiffre](#stromchiffre)
        - [Typische Anwendungsbeispiele](#typische-anwendungsbeispiele)
    - [Asymmetrisches Verschlüsselung](#asymmetrisches-verschlüsselung)
        - [Funktionsweise asymmetrischer Verschlüsselung](#funktionsweise-asymmetrischer-verschlüsselung)
        - [Anwendungsbereich](#anwendungsbereich)
        - [Algorithmen](#algorithmen)
    - [hybride Verschlüsselung](#hybride-verschlüsselung)
    - [Beispiele](#beispiele-1)
- [3. Hashing](#3-hashing)
    - [Sicherheit erhöhen durch Salting und Peppering](#sicherheit-erhöhen-durch-salting-und-peppering)
    - [Salting vs. Peppering](#salting-vs-peppering)
- [4. Vergleichstabelle](#4-vergleichstabelle)
    - [Praktische Beispiele](#praktische-beispiele)
- [5. Typische Einsatzszenarien](#5-typische-einsatzszenarien)
- [6. Häufige Missverständnisse](#6-häufige-missverständnisse)
- [7. Tools und Befehle](#7-tools-und-befehle)
- [8. Nützliche Links](#8-nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Encoding (Umwandlung)

In der Informatik werden Zeichen und Symbole durch eine Zeichenkodierung in Zahlenwerte übersetzt. Diese sind die Grundlage für die Speicherung und Übertragung von Daten. Die Umwandlung erfolgt mithilfe von Regeln oder Algorithmen.

> kurz gesagt: Encoding ist die Technik, die zur Umwandlung von Informationen von einem Format in ein anderes Format genutzt wird.

So ermöglicht es beispielsweise, dass URLs das Leerzeichen nicht falsch interpretieren und mit dem Zeichen `%20` ersetzen oder Texte, die verschiedene Sprachen und Sonderzeichen enthalten, korrekt angezeigt werden. 

Encoding verläuft in den meisten Fälle **verlustfrei**, das heißt, dass nur das Dateiformat umgewandelt wird, der eigentliche Inhalt der Nachricht bleibt jedoch gleich.
Darüber hinaus ist es nicht für Sicherheit, sondern für die Kompatibiltät gedacht.

- **Ziel:** Lesbare Daten in ein bestimmtes Format umwandeln, um Kompatibilität und Transport zu ermöglichen.
- **Zweck:** Datenübertragung oder -speicherung (nicht sicherheitsrelevant)
- **Beispiele:** Base64, URL-Encoding, ASCII, HTML-Entities
- **Reversibel:** Ja, ohne Schlüssel
- **Sicherheit:** **Keine**


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Arten des Encodings

- **Zeichenkodierung:** Oder Character Encoding genannt, verarbeitet die erhaltenen Informationen mit genutztem Zeichensatz:
    - `UTF-8` - Sehr verbreitetes Encoding im Internet, das Unicode verwendet, um Zeichen aus fast allen Sprachen der Welt darzustellen.
    - `ISO 8859-1 (Latin-1)` - Ältere Zeichenkodierung, hauptsächlich unterstützend in westeuropäischen Sprachen.
- **URL-Encoding:** Oder Prozentkodierung genannt, ersetzt in einer URL bestimmte Zeichen mit speziellen Zeichenfolgen:
    - z. B.: ein `Leerzeichen` wird ersetzt durch `%20`, für korrekte Übertragung.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Beispiele
**Beispiel (Base64 Encoding):**
```bash
echo -n "password123" | base64
# cGFzc3dvcmQxMjM=
```

**Beispiel (Base64 Decoding):**
```bash
echo "cGFzc3dvcmQxMjM=" | base64 -d
# password123
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 2. Encryption (Verschlüsselung)

Die Encryption ist die Umwandlung von Informationen (auf seite des Senders), die in Klartext vorhanden sind, in einen Geheimtext, sodass sie nur vom Empfänger mit dem richtigen Schlüssel entschlüsselt werden können.

**Auszug aus *Wikipedia*:**
> Verschlüsselung (auch: Chiffrierung oder Kryptierung)[1] ist die Umwandlung von Informationen, genannt Klartext, 
> in einen Geheimtext (auch Chiffrat oder Schlüsseltext genannt). Dabei wird ein geheimzuhaltender Schlüssel verwendet, 
> der nur den befugten Personen bekannt sein darf.
> *https://de.wikipedia.org/wiki/Verschl%C3%BCsselung*



- **Ziel:** Schutz von **Vertraulichkeit** und **Integrität** durch Umwandlung von Daten mithilfe eines Schlüssels.
- **Zweck:** Vertrauliche Kommunikation oder Datenspeicherung
- **Reversibel:** Ja, aber nur mit dem richtigen Schlüssel
- **Sicherheit:** Hoch (je nach Algorithmus und Schlüssellänge)
- **Arten:**
    - **Symmetrisch:** Gleicher Schlüssel zum Ver- und Entschlüsseln (z. B. AES, ChaCha20)
    - **Asymmetrisch:** Öffentlich/privat Schlüsselpaar (z. B. RSA)
    - **Hybrid:** Praxis: asymmetrisch zum Schlüsselaustausch, symmetrisch für die eigentlichen Daten → z. B. TLS.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Symmetrisches Verschlüsselung

Symmetrische Verschlüsselung ist ein kryptografisches Verfahren, bei dem derselbe geheime Schlüssel sowohl zum Ver- als auch zum Entschlüsseln von Daten verwendet wird. Dies macht sie schnell und effizient, weshalb sie sich gut für die Verschlüsselung großer Datenmengen eignet.

Die größte Herausforderung ist die sichere Verteilung des geheimen Schlüssels an den Empfänger. Ein Angreifer könnte den Schlüssel abfangen und alle damit verschlüsselten Daten entschlüsseln.

Die Verarbeitung der Daten zur Verschlüsselung hin erfolgt entweder als **Block-** oder als **Stromchiffren**-Verfahren.

Während **Blockchiffren** wie `AES` (Advanced Encryption Standard) Daten im **CBC/GCM-Modus** in festen Blöcken verarbeiten, verarbeiten **Stromchiffren** wie `ChaCha20` Daten Bit für Bit.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


```text
Das Verfahren zur Ver- und Entschlüsselung von Informationen vereinfacht dargesetellt:

Verschlüsselungsverfahren:

                      geheimer Schlüssel
                      zum verschlüsseln
 ________________             |              ____________
| Nachricht, die |            V             |            |
| verschlüsselt  |  ====================>   | Geheimtext |
| werden soll    |    Verschlüsselung       |            |
|________________|                          |____________|
|_______________/                           |___________/


Entschlüsselungsverfahren:

                    geheimer Schlüssel
                    zum entschlüsseln
 ____________             |              ________________ 
|            |            V             | Nachricht, die |
| Geheimtext |  ====================>   | entschlüsselt  |
|            |                          | wurde          |
|____________|                          |________________|
|___________/                           |_______________/ 
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Blockchiffre
- **Prinzip:** `AES` verarbeitet Daten Blockweise und in festen Größen 16 Byte (128 Bit).
- **Betriebsmodi:** Um mit beliebigen Datenmengen umzugehen, wird `AES` in verschiedenen Betriebsmodi eingesetzt:
    - **CBC (Cipher Block Chaining):** Jeder Block wird mit dem vorherigen Block `XOR` verknüpft, was serielle Verarbeitung erfordert und Leistung beeinträchtigen kann.
    - **GCM (Galois/Counter Mode):** Eine sicherere und oft perfomantere Variante, die parallele Verarbeitung ermöglicht.
- **Eigenschaften:** `AES` ist ein etablierter, weiter verbreiteter Standard.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Stromchiffre
- **Prinzip:** `ChaCha20` ist eine Stromchiffre, die einen Keystrem aus zufälligen Bytes generiert.
- **Verarbeitung:** Keystream wird dann mit Daten `XOR` verknüpft, um sie zu verschlüsseln.
- **Eigenschaften:** 
    - `ChaCha20` oft schneller als `AES`, insbesondere auf Geräten mit eingeschränkten Ressourcen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Typische Anwendungsbeispiele 
- Verschlüsselung von Dateien und Daten auf einer Festplatte (z. B. durch ein Passwort).
- Verschlüsselung in WLAN-Netzwerken.
- Verschlüsselung in Cloud-Speichern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Asymmetrisches Verschlüsselung
Bei asymmetrischer Verschlüsselung, wird ein sehr langer Schlüssel erzeugt, aus dem der privater Schlüssel, auch `private key` und ein öffentlicher Schlüssel, auch `public key` aufgeteilt werden.

Der `private key` dient dazu, die Information zu verschlüsseln und darf nicht an die Öffentlichkeit gelangen. Er ist quasi der Mechanismus, der genutzt werden muss, damit die Verschlüsselung überhaupt vollzogen werden kann, denn ohne `private key` auch kein `public key`.

Der `public key` hingegen, kann an x-beliebig Empfänger verteilt werden, die berechtigt sind, den Inhalt der Information zu verschlüsseln und auch zu verschlüsseln. 
Heißt, dass der Empfänger, der den `public key` hat, eine Nachricht verschlüsselt an den Sender mit dem eigentlichen `private key` sendet, der diese dann mit seinem `private key` entschlüsselt.

Der `public key` kann ebenso zur Verifizierung digitaler Signaturen verwendet werden, während der `private key` die Information entschlüsselt.

- **Vorteil:** Schlüsselaustausch ist sicher, da nur der öffentliche Schlüssel geteilt wird.
- **Nachteil:** Das Verfahren ist deutlich langsamer und rechenintensiver fals die symmetrische Verschlüsselung.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Funktionsweise asymmetrischer Verschlüsselung
1. Ein Paar von mathematisch verbundenen Schlüsseln wird erstellt:
    - Ein `public key` - kann frei verteilt werden, z.B. über einen Webserver.
    - Ein `private key` - darf nicht geteilt werden! 
2. Öffentlicher Schlüssel wird mit berechtigten Personen geteilt.
3. Verschlüsselung einer Information mit dem öffentlichen Schlüssel des Empfängers.
4. Entschlüsselung mit passendem privaten Schlüssel.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Anwendungsbereich:
- **Sichere Online-Kommunikation:** sichert Verbindungen zwischen Webbrowsern und Servern via `SSL/TLS`-Protokollen.
- **Authentifizierung:** Bestätigt die Identität eines Absenders.
- **Datenintegrität:** Stellt sicher, dass Daten nach Übertragung nicht manipuliert wurden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Algorithmen
- `RSA` - weit verbreitetes und sicheres System, das sowohl für Versclhüsselung als auch für Signaturen verwendet wird.
- `ECC` - Elliptische Kurven Kryptografie bitet hohe Sicherheit mit kürzeren Schlüssellängen, was es effizient für mobile Geräte macht.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### Visualisierung: Asymmetrische Verschlüsselung
```text
+-------------+                             +-------------+
|    Alice    |                             |     Bob     |
|  (Sender)   |                             | (Empfänger) |
+-------------+                             +-------------+
       |                                            ^
       |   Bobs öffentlicher Schlüssel              |
       |------------------------------------------->|
       |                                            |
       |--[ Verschlüsseln mit Bobs Public Key ]---->|
       |                                            |
       |<---(Nur Bob kann entschlüsseln)------------|
       |                                            |
+------------------+                 +-------------------------+
|  (Verschlüsselte |                 | Bobs privater Schlüssel |
|    Nachricht)    |                 | (nur bei Bob)           |
+------------------+                 +-------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### hybride Verschlüsselung

Die hybride Verschlüsselung kombiniert die Vorteile der **symmetrischen Verschlüsselung** (schnell, aber schwierige Schlüsselverteilung) und die **asymmetrische Verschlüsselung** (langsame, aber sichere Verschlüsselung).

1. Der Sender generiert einen einmaligen, zufälligen **symmetrischen Session-Schlüssel**.
2. Dieser Session-Schlüssel wird mit dem öffentlichen Schlüssel des Empfängers asymmetrisch verschlüsselt und gesendet.
3. Die eigentlichen Daten werden mit dem schnellen symmetrischen Session-Schlüssel verschlüsselt und übertragen.


**Beispiele, die hybride Verschlüsselung nutzen:**
- `HTTPS` - nutzt hybride Verschlüsselung über TLS (ehemals SSL).
- `PGP / GnuPG` - Standards für die Email-Verschlüsselung.
- `IPsec` - Protokoll-Suite, um sichere Verbindung über potenziell unsicheres Internet zu ermöglichen (OSI Layer 3).


### Beispiele
**Beispiel (OpenSSL AES-256):**
```bash
echo -n "secrettext" | openssl enc -aes-256-cbc -base64 -pass pass:MyKey
```

 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Hashing

Hashing ist die Umwandlung von Daten beliebiger Größe in eine Zeichenkette fester Länge, einen sogenannten Hashwert oder Hash, mithilfe einer Hashfunktion. Der Prozess ist einseitig und somit nahezu unumkehrbar, was Hashing ideal für die sichere Speicherung von Passwörtern und die Überprüfung der Datenintegrität macht.

Verwechsle daher das Hashen eines Passworts nicht mit der Verschlüsselung. Die Verschlüsselung ist umkehrbar, der Hash ist eine **Einweg-Funktion**.


- **Ziel:** Erzeugung eines eindeutigen, festen Ausgabewertes (Digest) zur Überprüfung von Integrität oder Identität.

- **Zweck:** Fingerabdruck von Daten, z. B. für Passwörter, Datei-Integritätsprüfung
- **Reversibel:** Nein (Einweg-Funktion)
- **Sicherheit:** Stark gegen Kollisionen und Vorabberechnung (je nach Algorithmus)

Verfahren wie `MD5`, `SHA-1`, `SHA-256` (allgemein `SHA`-Familie), `bcrypt`, `scrypt`, `Argon2`, usw. machen deutlich, wie viele es zum hashen von Informationen gibt.

**Schnelles Hashing** wie es bei `CityHash64` oder `xxHash` ist, ist auf hohe Geschwindigkeit für Datenverarbeitung optimiert. Damit lassen sich durch nicht-kryptografische Algorithmen große Datenmengen schnell indizieren oder vergleichen. 

Im Gegenzug dazu sind **langsamere Hashes**, zu denen kryptografische Algorithmen wie bspw. `Argon2` oder `MD5` gehören, bestens geeignet, um Passwörter sicher zu speichern. 

> Kyptografische Algorithmen, die nicht einfach reversibel sind, sind für Sicherheit und Integrität konzipiert und werden dafür genutzt, Passwörter sicher zu speichern.
> Nicht-kryptografische Algorithem werden dafür genutzt, um große Datenmengen schnell zu indizieren und zu vergleichen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Sicherheit erhöhen durch Salting und Peppering
Sicherheitstechniken wie Salting oder Peppering werden beim Hashen dafür verwendet, die Sicherheit der Passwörter noch einmal zu erhöhen.

- **Salting:** Ein zufälliger, einzigartiger Wert (`Salt`) wird jedem Passwort vor dem Hashing hinzugefügt. Der Hash wird zusammen mit dem Salt in der Datenbank gespeichert. Dies verhindert, dass Angreifer Rainbow-Tables verwenden können, da dasselbe Passwort für jeden Benutzer einen anderen Hash erzeugt.

- **Peppering:** Ein geheimer, statischer Wert (`Pepper`) wird zu allen Passwörtern vor dem Hashing hinzugefügt. Im Gegensatz zum Salt wird der Pepper nicht in der Datenbank gespeichert, sondern an einem externen, sicheren Ort. Dies schützt die Passwörter auch dann, wenn die gesamte Datenbank gestohlen wird.

Die Kombination von **Hashing**, **Salting** und **Peppering** ist eine sehr wirksame Methode, um Passwörter sicher zu speicher.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Salting vs. Peppering

|               |   **Salting**    |   **Peppering**   |
| ------------- | ---------------- | ----------------- |
| **Was ist es?** | Ein zufälliger, einzigartiger Wert, der jedem Passwort vor dem Hash hinzugefügt wird. | Eine zusätzliche Zeichenkette, die vor dem Hashing zum Passwort hinzugefügt wird. |
| **Funktionsweise**       | Der Salt wird zusammen mit dem Passwort-Hash gespeichert. Wenn du dich anmeldest, wird das eingegebene Passwort mit demselben Salt gehasht und mit dem gespeicherten Wert verglichen. | Im Gegensatz zum Salt wird der Pepper nicht zusammen mit dem Passwort-Hash gespeichert. Er wird getrennt davon an einem sicheren Ort, wie z. B. einer Konfigurationsdatei oder einem Hardware-Sicherheitsmodul, aufbewahrt. |
| **Zweck** | Verhindert, dass Angreifer Rainbow-Tabellen verwenden können, da jedes Passwort einen anderen Hash erzeugt, selbst wenn dasselbe Passwort verwenden wird. | Bietet eine zusätzliche Sicherheitsebene, da ein Angreifer, selbst wenn er die Datenbank mit den Passwort-Hashes besitzt, immer noch den geheimen Pepper benötigt, um die Passwörter zu knacken. |


**Beispiel (SHA256):**
```bash
echo -n "secrettext" | sha256sum
# 8b2c99e5c489b9677872f22e51f06c87178d24...
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 4. Vergleichstabelle

| Eigenschaft            | Encoding    | Encryption           | Hashing            |
| ---------------------- | ----------- | -------------------- | ------------------ |
| Ziel                   | Lesbarkeit  | Vertraulichkeit      | Integritätsprüfung |
| Reversibel             | ✅ Ja        | ✅ Ja (mit Schlüssel) | ❌ Nein             |
| Sicherheitsrelevant    | ❌ Nein      | ✅ Ja                 | ✅ Ja               |
| Schlüssel erforderlich | ❌ Nein      | ✅ Ja                 | ❌ Nein             |
| Beispiele              | Base64, URL | AES, RSA             | SHA-256, bcrypt    |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Praktische Beispiele

| Typ        | Beispiel in der Praxis       |
| ---------- | ---------------------------- |
| Encoding   | Bild in Base64 für HTML-Mail |
| Encryption | HTTPS (TLS)                  |
| Hashing    | Passwort-Datenbank (bcrypt)  |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 5. Typische Einsatzszenarien

- Encoding:
    - VPN (IPsec, WireGuard)
    - E-Mail-Anhänge (Base64)
    - URL-Parameter (URL-Encoding)
    - HTML in Webseiten
- Encryption:
    - HTTPS-Kommunikation (TLS)
    - Festplattenverschlüsselung
    - E-Mails mit GPG
- Hashing:
    - Passwortspeicherung (bcrypt)
    - Integritätsprüfung (SHA256 Checksums)
    - Digitale Signaturen (Signatur basiert auf Hash)
    - Blockchain (SHA-256 in Bitcoin)

 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Häufige Missverständnisse

- Base64 ist KEINE Verschlüsselung!
- Hashes sind NICHT rückrechenbar – außer bei Rainbow Tables oder schwachen Algorithmen (MD5).
- Hashing ist keine Verschlüsselung!
- Hashing schützt keine Daten, sondern prüft nur Integrität/Identität.
- Encryption schützt Daten nur solange der Schlüssel geheim bleibt.
- Encryption ohne sichere Schlüsselspeicherung ist nutzlos.
- Encoding ist keine Sicherheitsschicht – jeder kann es zurückwandeln.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 7. Tools und Befehle

| Tool        | Verwendung                                  |
| ----------- | ------------------------------------------- |
| `base64`    | Encoding/Decoding                           |
| `openssl`   | Verschlüsselung/Entschlüsselung             |
| `gpg`       | Asymmetrische Verschlüsselung               |
| `sha256sum` | Hashing von Dateien/Text                    |
| `CyberChef` | Kombiniert Encoding/Hashing/Verschlüsselung |
| `hashcat`   | Passwortwiederherstellungs-Tool             |



**Beispiel: GPG**
```bash
gpg --encrypt --recipient user@webseite.com
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 8. Nützliche Links

- [CyberChef – The Cyber Swiss Army Knife](https://gchq.github.io/CyberChef/)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [Introduction to Modern Cryptography (Stanford)](https://crypto.stanford.edu/~dabo/courses/OnlineCrypto/)
- [Wikepdia: Base64](https://de.wikipedia.org/wiki/Base64)
- [Wikipedia: Zeichenkodierung](https://de.wikipedia.org/wiki/Zeichenkodierung)
- [Wikipedia: Verschlüsselung](https://de.wikipedia.org/wiki/Verschl%C3%BCsselung)
- [Wikipedia: Kryptografische Hashfunktion](https://de.wikipedia.org/wiki/Kryptographische_Hashfunktion)
- [Wikipedia: Hybride Verschlüsselung](https://de.wikipedia.org/wiki/Hybride_Verschl%C3%BCsselung)
- [Wikipedia: Salt (Kryptologie)](https://de.wikipedia.org/wiki/Salt_(Kryptologie))
- [Wikipedia: Secure Hash Algorithm](https://de.wikipedia.org/wiki/Secure_Hash_Algorithm)
- [Wikipedia: Message-Digest Algorithm 5 (MD5)](https://de.wikipedia.org/wiki/Message-Digest_Algorithm_5)
- [Ubuntu Manual für sha256sum](https://manpages.ubuntu.com/manpages/plucky/en/man1/sha256sum.1.html)
- [Hashcat](https://hashcat.net/hashcat/)
- [GnuPG.org](https://gnupg.org/)
- [openssl.org](https://www.openssl.org/)


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