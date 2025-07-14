# 🧁 CyberChef Tricks & Techniken

**CyberChef** – "The Cyber Swiss Army Knife" – ist ein webbasiertes Tool von GCHQ, 
das hunderte Funktionen für die Datenanalyse und -bearbeitung ohne lokale Installation bietet.

🌐 Online: https://gchq.github.io/CyberChef/

---

## 🎯 Anwendungsgebiete (Use Cases)

- 🔐 Entschlüsseln und Dekodieren (Base64, XOR, ROT13, etc.)
- 📄 Analyse von Datenströmen (PCAP, Hex-Dumps)
- 📊 Hash-Erzeugung & Vergleich
- 📤 Encoding & Obfuscation-Erkennung
- 📂 File-Forensik (z. B. Dateiheader identifizieren)
- 🕵️‍♂️ Payload-Rekonstruktion aus Malware
- 💬 JWT/Token-Analyse
- 🧪 Encoding-Ketten (z. B. mehrfach Base64 + URL Encode)

---

## 🛠️ Wichtige Funktionen & Operatoren

| Operator                  | Beschreibung                                      |
|---------------------------|---------------------------------------------------|
| **From Base64**           | Base64-dekodieren                                |
| **To Base64**             | Base64-kodieren                                  |
| **From Hex**              | Hexadezimal zu ASCII                             |
| **To Hex**                | ASCII zu Hex                                     |
| **From Charcode**         | ASCII-Code zu Text                               |
| **To Charcode**           | Text zu ASCII-Code                               |
| **ROT13**                 | Buchstabenverschiebung (klassisch)               |
| **XOR**                   | XOR-Verschlüsselung                              |
| **Gunzip / Gzip**         | gzip-komprimierte Daten entpacken                |
| **Parse JWT**             | JSON Web Token zerlegen und analysieren          |
| **Defang URL**            | URLs entschärfen für Reporting                   |
| **Extract Files**         | Dateien aus Binärdaten extrahieren               |
| **Magic**                 | Automatisierte Operator-Vorschläge               |

---

## 🔍 Praktische Rezepte (Recipes)

### 🔒 2. Hash-Analyse
```text
Input: Hash-Wert
Recipe:
- Identify Hash
- [ggf. in Crackstation.org oder Hashcat prüfen]
```

### 🔑 3. JWT-Token entschlüsseln
```text
Input: JWT Token (eyJhbGciOiJIUzI1...)
Recipe:
- Parse JWT
```

### 🛡️ 4. Obfuscated JavaScript analysieren
```text
Recipe:
- Beautify JavaScript
- Remove null bytes
- Extract strings
- Find / Replace Pattern (z.B. eval, atob)
```

### 🐚 5. Reverse Shell erkennen & dekodieren
```text
Input: encoded payload
Recipe:
- From Base64 → Gunzip → From Hex → XOR
- Beautify
```

### 📤 6. Dateien extrahieren
```text
Input: Binärdump oder Base64-Daten
Recipe:
- From Base64
- Extract Files
- Detect File Type
```

### 🧾 7. Encoding erkennen & automatisieren
```text
Recipe:
- Magic
→ CyberChef erkennt häufige Pattern automatisch
```

---

## 🧠 Tipps & Best Practices
- 🔄 Nutze "Magic" bei unbekannten Inhalten.
- 🔀 Kombiniere mehrere Dekodierungsmethoden.
- 🧪 Überprüfe mehrfach codierte Daten manuell.
- ⚠️ Achte auf Nullbytes (%00) und Obfuskationstricks.
- 🔎 Nutze "Regular Expressions" zum Extrahieren von Infos.
- 📤 Nutze die Export-Funktion, um Ergebnisse weiterzuverarbeiten.

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---

## 📚 Nützliche Links & Quellen

- 🔗 [CyberChef Online Tool](https://gchq.github.io/CyberChef/)
- 📘 [CyberChef Cheatsheet (PDF)](https://malicious.link/file/cyberchef.pdf)
- 📂 [CyberChef GitHub Repo](https://drive.google.com/drive/home)
