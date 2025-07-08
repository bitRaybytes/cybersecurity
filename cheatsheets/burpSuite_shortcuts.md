# 🔧 Burp Suite Shortcuts & Quick Workflow Guide

> Übersicht nützlicher Shortcuts und Tipps für effizientes Arbeiten mit Burp Suite (Community & Pro).  
> Für Linux/Windows – macOS-User nutzen `Cmd` statt `Ctrl`.

---

## ⌨️ Tastenkombinationen (Default)

| Shortcut             | Beschreibung                                         |
|----------------------|------------------------------------------------------|
| `Ctrl + Tab`         | Zwischen geöffneten Tabs wechseln                   |
| `Ctrl + W`           | Aktuellen Tab schließen                             |
| `Ctrl + Shift + T`   | Geschlossenen Tab wiederherstellen                  |
| `Ctrl + F`           | Suchen im aktuellen Fenster                         |
| `Ctrl + J`           | Wiederhole letzte Aktion (z. B. Request senden)     |
| `Ctrl + U`           | URL decodieren (Encoder/Decoder)                   |
| `Ctrl + Shift + U`   | URL encodieren                                     |
| `Ctrl + Shift + H`   | Hex-Ansicht umschalten (z. B. im Repeater)          |
| `Ctrl + Enter`       | Anfrage im Repeater senden                         |
| `Ctrl + R`           | Send to Repeater                                    |
| `Ctrl + B`           | Send to Intruder                                    |
| `Ctrl + E`           | Send to Comparer                                    |
| `Ctrl + Shift + L`   | Send to Logger++ (wenn installiert)                 |

---

## 🚦 Workflow Shortcuts (via Kontextmenü / GUI)

| Funktion              | Beschreibung                                         |
|-----------------------|------------------------------------------------------|
| Rechter Mausklick → *"Send to Repeater"* | Anfrage direkt zum Repeater senden         |
| Rechter Mausklick → *"Engagement tools → Generate CSRF PoC"* | CSRF-Proof-of-Concept generieren |
| Strg + Doppelklick auf Header / Param. | Kopiert Namen/Wert in Zwischenablage       |
| Doppelklick auf `Param-Wert` im Repeater | Bearbeitung im Inline-Modus                |

---

## 🧪 Nützliche Tools innerhalb von Burp

| Tool            | Zweck                                         |
|------------------|-----------------------------------------------|
| Repeater         | Manuelle Tests / Payload-Variationen          |
| Intruder         | Automatisiertes Fuzzing                       |
| Decoder          | Kodierung/Decodierung (Base64, URL, Hex)      |
| Comparer         | Unterschiede zwischen Requests/Responses      |
| Logger++         | Erweiterter Traffic-Logger (extensibles Plugin) |
| Collaborator     | Out-of-Band-Angriffe testen                   |

---

## 🛠️ Erweiterungen (empfohlen)

| Extension        | Funktion                                     |
|------------------|----------------------------------------------|
| Logger++         | Erweiterte Protokollierung                   |
| Autorize         | Zugriffskontrolle testen                     |
| Retire.js        | Veraltete JS-Bibliotheken erkennen           |
| Active Scan++    | Erweiterter aktiver Scan (nur Pro)           |
| Param Miner      | Versteckte Parameter entdecken               |
| Turbo Intruder   | Hochperformantes Fuzzing                     |

---

## 📁 Tipps zur Organisation

- Nutze `Burp Project Files (.burp)` zur Speicherung kompletter Sessions
- Setze in **Proxy → Options** gezielte Scope-Filter
- Verwende "Target → Site Map" zur strukturierten Übersicht der gescannten Seiten

---

## 🔒 Quick Commands für Decoder

| Eingabe              | Ergebnis                           |
|----------------------|-------------------------------------|
| `Ctrl + U`           | URL-Decodierung                     |
| `Ctrl + Shift + U`   | URL-Encoding                        |
| `Base64 → Decode`    | Base64-Decode im Decoder-Tab       |
| `Ctrl + Shift + H`   | Wechsel zur Hex-Darstellung         |

---

## 📚 Weitere Ressourcen

- [PortSwigger Academy](https://portswigger.net/web-security) – Kostenlos lernen!
- [Burp Suite Docs](https://portswigger.net/burp/documentation)
- [CheatSheetSeries: Burp Suite](https://cheatsheetseries.owasp.org/)

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!