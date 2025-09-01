# 🔐 Encoding vs. Encryption vs. Hashing

## 📘 Ziel dieser Datei

Diese Datei erklärt die Unterschiede und Anwendungsfälle von **Encoding**, **Encryption** und **Hashing** – drei häufig verwendete, aber oft verwechselte Techniken in der Cybersecurity. Das Verständnis dieser Begriffe ist essenziell für jeden, der sich mit Netzwerksicherheit, Datenverarbeitung oder Kryptographie beschäftigt.

---

## Inhaltsverzeichnis

- [1. Encoding](#1-encoding)
- [2. Encryption (Verschlüsselung)](#2-encryption-verschlüsselung)
- [3. Hashing](#3-hashing)
- [4. Vergleichstabelle](#4-vergleichstabelle)
- [5. Typische Einsatzszenarien](#5-typische-einsatzszenarien)
- [6. Häufige Missverständnisse](#6-häufige-missverständnisse)
- [7. Tools und Befehle](#7-tools-und-befehle)
- [8. Nützliche Links](#8-nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

---

## 1. Encoding

**Ziel:** Lesbare Daten in ein bestimmtes Format umwandeln, um Kompatibilität und Transport zu ermöglichen.

- **Zweck:** Datenübertragung oder -speicherung (nicht sicherheitsrelevant)
- **Beispiele:** Base64, URL-Encoding, ASCII, HTML-Entities
- **Reversibel:** Ja, ohne Schlüssel
- **Sicherheit:** **Keine**

**Beispiel (Base64 Encoding):**
```bash
echo -n "password123" | base64
# cGFzc3dvcmQxMjM=
```

---

## 2. Encryption (Verschlüsselung)
**Ziel:** Schutz von Vertraulichkeit und Integrität durch Umwandlung von Daten mithilfe eines Schlüssels.

- **Zweck:** Vertrauliche Kommunikation oder Datenspeicherung
- **Reversibel:** Ja, aber nur mit dem richtigen Schlüssel
- **Sicherheit:** Hoch (je nach Algorithmus und Schlüssellänge)
- **Arten:**
    - **Symmetrisch:** Gleicher Schlüssel zum Ver- und Entschlüsseln (z. B. AES)
    - **Asymmetrisch:** Öffentlich/privat Schlüsselpaar (z. B. RSA)

Beispiel (OpenSSL AES-256):
```bash
echo -n "secrettext" | openssl enc -aes-256-cbc -base64 -pass pass:MyKey
```

--- 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Hashing
**Ziel:** Erzeugung eines eindeutigen, festen Ausgabewertes (Digest) zur Überprüfung von Integrität oder Identität.

- **Zweck:** Fingerabdruck von Daten, z. B. für Passwörter, Datei-Integritätsprüfung
- **Reversibel:** Nein (Einwegfunktion)
- **Sicherheit:** Stark gegen Kollisionen und Vorabberechnung (je nach Algorithmus)

Beispiele: MD5, SHA-1, SHA-256, bcrypt, Argon2

Beispiel (SHA256):
```bash
echo -n "secrettext" | sha256sum
# 8b2c99e5c489b9677872f22e51f06c87178d24...
```

---

## 4. Vergleichstabelle

| Eigenschaft            | Encoding    | Encryption           | Hashing            |
| ---------------------- | ----------- | -------------------- | ------------------ |
| Ziel                   | Lesbarkeit  | Vertraulichkeit      | Integritätsprüfung |
| Reversibel             | ✅ Ja        | ✅ Ja (mit Schlüssel) | ❌ Nein             |
| Sicherheitsrelevant    | ❌ Nein      | ✅ Ja                 | ✅ Ja               |
| Schlüssel erforderlich | ❌ Nein      | ✅ Ja                 | ❌ Nein             |
| Beispiele              | Base64, URL | AES, RSA             | SHA-256, bcrypt    |

---

## 5. Typische Einsatzszenarien

- Encoding:
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

--- 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Häufige Missverständnisse

- Base64 ist KEINE Verschlüsselung!
- Hashes sind NICHT rückrechenbar – außer bei Rainbow Tables oder schwachen Algorithmen (MD5).
- Encryption schützt Daten nur solange der Schlüssel geheim bleibt.

---

## 7. Tools und Befehle

| Tool        | Verwendung                                  |
| ----------- | ------------------------------------------- |
| `base64`    | Encoding/Decoding                           |
| `openssl`   | Verschlüsselung/Entschlüsselung             |
| `gpg`       | Asymmetrische Verschlüsselung               |
| `sha256sum` | Hashing von Dateien/Text                    |
| `CyberChef` | Kombiniert Encoding/Hashing/Verschlüsselung |


---

## 8. Nützliche Links

- [CyberChef – The Cyber Swiss Army Knife](https://gchq.github.io/CyberChef/)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [Wikipedia: Zeichenkodierung](https://de.wikipedia.org/wiki/Zeichenkodierung)
- [Wikipedia: Verschlüsselung](https://de.wikipedia.org/wiki/Verschl%C3%BCsselung)
- [Wikipedia: Kryptgrafische Hashfunktion](https://de.wikipedia.org/wiki/Kryptographische_Hashfunktion)

---

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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---