# 🔑 Zugriffs- und Zugangsmöglichkeiten

## Ziel dieser Datei

Diese Datei fasst die wichtigsten Konzepte rund um **Authentifizierung**, **Zugriffskontrolle**, **Passwörter** und **Verschlüsselung** zusammen.
Sie ist als **Cheat Sheet** und **Nachschlagewerk** gedacht – für Lernende oder Security-Interessierte in der IT-Sicherheit.

## Inhaltsverzeichnis

- [1. Authentifizierungsmaßnahmen](#1-authentifizierungsmaßnahmen)
- [2. Rechtevergabe](#2-rechtevergabe)
- [3. Passwortrichtlinien](#3-passwortrichtlinien)
- [4. Zugriffskontrollmodelle](#4-zugriffskontrollmodelle)
- [5. Verschlüsselung](#5-verschlüsselung)
- [6. Best Practices](#6-best-practices)
- [7. Nützliche Links](#7-nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## 1. Authentifizierungsmaßnahmen

Authentifizierung ist der Prozess, durch den ein Benutzer oder ein System nachweist, dass es berechtigt ist, auf bestimmte Ressourcen zuzugreifen.

### Typen der Authentifizierung

- **Wissensbasiert (Knowledge-based, KBA)**
    - Passwort, PIN, Sicherheitsfragen
- **Besitzbasiert (Token-based)**
    - SmartCards, Hardware-Token, OTP-Apps (Google Authenticator, Authy), YubiKey
- **Biometrisch**
    - Fingerabdruck, Gesicht, Iris, Stimme, Tippverhalten
- **Multi-Faktor-Authentifizierung (MFA)**
    - Kombination mehrerer Faktoren (z. B. Passwort + SMS-Code oder Passwort + Fingerabdruck)
- **Adaptive Authentifizierung**
    - Kontextbasiert (Ort, Gerät, Uhrzeit, Risikoanalyse)

## 2. Rechtevergabe
Die Vergabe von Zugriffsrechten ist ein entscheidender Bestandteil der IT-Sicherheit.

### Prinzip der minimalen Rechte (Least Privilege)

- Jeder Benutzer erhält nur die Rechte, die für seine Aufgaben erforderlich sind.
- Reduziert Angriffsfläche und Schadenspotenzial.

### Dateirechte (Beispiel Unix/Linux)
- **Benutzerklassen:** Eigentümer (user), Gruppe (group), Sonstige (others).
- **Rechte:** Lesen (r), Schreiben (w), Ausführen (x).


```bash
# Beispiel: Dateirechte ändern
chmod 750 script.sh   # User: rwx, Group: r-x, Others: ---
ls -l script.sh
-rwxr-x---  1 user group 1234 Aug 28 12:00 script.sh
```

### Best Practices
- Regelmäßige Überprüfung und Bereinigung von Rechten.
- Entfernen inaktiver Accounts.
- Logging & Monitoring von Zugriffen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Passwortrichtlinien
Passwörter sind nach wie vor eines der am häufigsten genutzten, aber auch am meisten angegriffenen Sicherheitsmittel.

### Anforderungen an sichere Passwörter

- **Länge:** mindestens 12–16 Zeichen
- **Komplexität:** Groß- und Kleinbuchstaben, Zahlen, Sonderzeichen
- **Keine Wiederverwendung** alter Passwörter
- **Kein Bezug zu persönlichen Daten** (Name, Geburtsdatum, Haustiere etc.)
- **Passphrasen bevorzugt:** „Haus!Apfel-99Wolke?“

### Passwortmanager

- Speichern & generieren sicherer Passwörter
- Verhindern Wiederverwendung schwacher Passwörter
- **Beispiele:** KeePass, Bitwarden, 1Password

### Zusätzliche Schutzmaßnahmen

- Zwei-Faktor-Authentifizierung (2FA)
- Automatische Sperrung nach mehrfachen Fehlversuchen
- CAPTCHAs gegen Brute-Force


## 4. Zugriffskontrollmodelle
| Modell                                    | Erklärung                                                          | Beispiel                                 |
| ----------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------- |
| **DAC** (Discretionary Access Control)    | Eigentümer entscheidet, wer Zugriff hat                            | Windows-Dateifreigabe                    |
| **MAC** (Mandatory Access Control)        | Zugriff durch zentrale Richtlinien, oft in hochsicheren Umgebungen | Militärische Systeme                     |
| **RBAC** (Role-Based Access Control)      | Rechte nach Rolle zugewiesen                                       | Admin, User, Gast                        |
| **ABAC** (Attribute-Based Access Control) | Kontextabhängig (Ort, Zeit, Gerät)                                 | Cloud-Zugriff nur aus bestimmtem Land/IP |

## 5. Verschlüsselung
Verschlüsselung schützt Daten bei Übertragung und Speicherung.

- **Symmetrische Verschlüsselung**
    - Gleicher Schlüssel für Ver- und Entschlüsselung
    - Sehr schnell, aber Schlüsselverteilung problematisch
    - Beispiel: AES

```bash
echo "secret" | openssl enc -aes-256-cbc -a -salt -pass pass:geheim
```

- **Asymmetrische Verschlüsselung**
    - Schlüsselpaare: Private Key & Public Key
    - Sicherer Schlüsselaustausch
    - Beispiele: RSA, ECC

```bash
# RSA-Key erzeugen
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem
```

- **Hybride Verschlüsselung**
    - Kombination beider Verfahren
    - Typisch in TLS (HTTPS):
        - Asymmetrisch für Schlüsselaustausch
        - Symmetrisch für eigentliche Daten

Mehr zum Thema Verschlüsselung erfährst du [hier](/07-crypto-stego/encoding_vs_encryption.md).

## 6. Best Practices

- MFA aktivieren für alle kritischen Konten
- Passwortmanager verwenden
- Rechte nach dem Least Privilege Prinzip
- Regelmäßige Security Audits & Monitoring
- Verschlüsselung für Daten bei Übertragung und Speicherung
- Mitarbeiter schulen (Phishing, Social Engineering)


## 7. Nützliche Links

- [BSI: Sichere Passwörter erstellen](https://www.bsi.bund.de/DE/Themen/Verbraucherinnen-und-Verbraucher/Informationen-und-Empfehlungen/Cyber-Sicherheitsempfehlungen/Accountschutz/Sichere-Passwoerter-erstellen/sichere-passwoerter-erstellen_node.html)
- [NIST Guidelines: Digital Identity](https://pages.nist.gov/800-63-3/)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)


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