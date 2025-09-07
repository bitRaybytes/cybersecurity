# 🛡️ Brute-Force-Angriff – Cheat Sheet

Ein **Brute-Force-Angriff** ist eine Methode zum Entschlüsseln von Passwörtern, Chiffren oder Schlüsseln, indem systematisch alle möglichen Kombinationen ausprobiert werden, bis die richtige gefunden wird.
Der Angreifer nutzt die immense Rechenleistung moderner Computer oder Botnets, um Millionen oder Milliarden von Kombinationen pro Sekunde durchzuprobieren.

## Inhaltsverzeichnis
- [Funktionsweise](#funktionsweise)
- [Arten von Brute-Force-Angriffen](#arten-von-brute-force-angriffen)
- [Schutzmaßnahmen](#schutzmaßnahmen)
    - [1. Rate Limiting (Login-Versuche begrenzen)](#1-rate-limiting-login-versuche-begrenzen)
    - [2. Multi-Faktor-Authentifizierung (MFA)](#2-multi-faktor-authentifizierung-mfa)
    - [3. Komplexität von Passwörtern](#3-komplexität-von-passwörtern)
    - [4. Passwort-Hashing + Salting](#4-passwort-hashing--salting)
    - [5. Captcha](#5-captcha)
    - [6. Monitoring & Logging](#6-monitoring--logging)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

## Funktionsweise

Das Prinzip ist einfach:
- „Versuche jede Möglichkeit, bis du Erfolg hast.“
- Der Angreifer hat eine Liste möglicher Zeichen (z. B. Klein-/Großbuchstaben, Zahlen, Sonderzeichen).
- Er kombiniert diese systematisch in allen möglichen Varianten.
- Je länger und komplexer das Passwort, desto länger dauert der Angriff.

**Beispiel:** Ein Passwort mit nur **zwei Kleinbuchstaben** (`a–z`):

```yaml
aa
ab
ac
...
az
ba
...
zz
```

- Angriffszeit hängt von:
    - Länge des Passworts
    - Zeichenraum (Alphabetgröße)
    - Geschwindigkeit der Hash- oder Login-Prüfung

## Arten von Brute-Force-Angriffen

1. **Simple Brute-Force:**

    - **Beschreibung:** Der klassische, systematische Ansatz, der jede mögliche Kombination ausprobiert.
    - **Anwendung:** Geeignet für kurze oder einfache Passwörter. Bei komplexen Passwörtern ist dieser Angriff oft zu zeitaufwändig.

2. **Wörterbuchangriff (Dictionary Attack):**

    - **Beschreibung:** Eine spezialisierte Form, die keine zufälligen Kombinationen testet, sondern eine Liste gängiger Wörter, Phrasen und bekannter Passwörter verwendet. Diese Liste wird als **"Wörterbuch"** (Dictionary) bezeichnet.
    - **Vorteil:** Viel schneller und effektiver, wenn das Passwort ein echtes Wort (z.B. `Schmetterling`), ein häufiges Passwort (z.B. `123456`) oder ein persönlicher Name ist.
    - **Beispielwörter:** `password`, `123456`, `admin`, `qwerty`.

3. **Hybride Angriffe:**

    - **Beschreibung:** Kombination aus Wörterbuch und Brute-Force.
    - **Beispiel:** Aus „password“ wird `Password123`, `P@ssword`, `password!` .

4. **Reverse Brute-Force:**

    - **Beschreibung:** Anstatt ein Passwort für einen einzelnen Benutzernamen zu erraten, wird ein **häufiges Passwort** (z.B. 12345678) genommen und gegen eine große Liste von Benutzernamen getestet.
    - **Anwendung:** Besonders effektiv bei Massen-Hacks, bei denen die Angreifer Zugriff auf eine Liste von Benutzernamen haben.
    - **Effektiv bei:** Leaks von Standardpasswörtern.

5. **Credential Stuffing**

    - **Beschreibung:** Nutzung gestohlener Login-Daten aus Leaks (z. B. von „Have I Been Pwned?“).
    - **Effektiv bei:** Benutzern, die Passwörter mehrfach verwenden.

## Gängige Tools

- **`Hydra` (THC-Hydra):** Login-Bruteforcer für viele Protokolle (SSH, FTP, HTTP).
```bash
hydra -l admin -P rockyou.txt ssh://192.168.0.10
```

- **`John` John the Ripper:** Passwort-Cracker für Hashes.
```bash
john --wordlist=rockyou.txt hashfile.txt
```

- **`hashcat`:** GPU-beschleunigtes Passwort-Cracking:
```bash
hascat -a 3 -m 0 hash.txt ?a?a?a?a?a
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schutzmaßnahmen

### 1. Rate Limiting (Login-Versuche begrenzen)

Die wichtigste Maßnahme ist die Begrenzung der Anmeldeversuche. Nach einer bestimmten Anzahl von falschen Versuchen (z.B. 3 bis 5) wird der Zugriff für eine bestimmte Zeit blockiert oder der Account gesperrt.

- Accounts nach X fehlgeschlagenen Versuchen sperren.
- Zeitverzögerung einbauen.

```text
+--------------------------+
|  Benutzer meldet sich an |
+--------------------------+
          |
          V
+---------------------------------+
|  1. Versuch (falsches PW)       |
|  2. Versuch (falsches PW)       |
|  3. Versuch (falsches PW)       |
+---------------------------------+
          |
          V
+-------------------------------------+
|  ❌ Zugriff gesperrt für 30 Minuten |
+-------------------------------------+
```

### 2. Multi-Faktor-Authentifizierung (MFA)

Selbst wenn ein Angreifer das Passwort errät, kann er sich ohne den zweiten Faktor (z.B. ein Code von einer App oder SMS) nicht anmelden.

### 3. Komplexität von Passwörtern

Längere und komplexere Passwörter mit einer Mischung aus Groß- und Kleinbuchstaben, Zahlen und Sonderzeichen machen Brute-Force-Angriffe exponentiell langsamer.

- Ein Passwort mit 6 Kleinbuchstaben (`a-z`) hat 26^6 = 308.915.776 Möglichkeiten.
- Ein Passwort mit 8 Zeichen (`a-z` (26), `A-Z` (26), `0-9`, Sonderzeichen) hat 94^8 ≈ 6,1 * 10^15 Möglichkeiten.
- Nicht das gleiche Passwort nutzen.

```yaml
                                                    +-----------------------------+
6 Kleinbuchstaben (a-z):    26^6  ≈ 3,1 * 10^8      | Ergebnisse gerundet nach    |
8 gemischte Zeichen (~94):  94^8  ≈ 6,1 * 10^15     | wissenschaftlicher Notation |
12 gemischte Zeichen:       94^12 ≈ 4,7 * 10^23     +-----------------------------+

[ Passwortlänge ↑ ]   =  exponentielle Steigerung der Möglichkeiten
a-z  →  schwach
a-zA-Z0-9 → stärker
a-zA-Z0-9!$% → sehr stark

```

Hier erfährst du mehr zur wissenschaftlichen Notation: [google: Was ist wissenschaftliche Notation?](https://www.google.com/search?client=firefox-b-d&q=Was+ist+wissenschaftliche+Notation%3F)

### 4. Passwort-Hashing + Salting

Passwörter **niemals** im Klartext speichern.

- Hashing-Algorithmen wie `bcrypt`, `scrypt`, `Argon2`.
- `Salt` schützt vor Rainbow-Tables.

### 5. Captcha

Eine Herausforderung, die nur von Menschen gelöst werden kann, stoppt automatisierte Skripte von Brute-Force-Angriffen.


### 6. Monitoring & Logging

- **IDS/IPS-Systeme** → erkennen Login-Anomalien.
- **Brute-Force-Versuche** → Security-Alert.

## Nützliche Links

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