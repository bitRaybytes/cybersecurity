# 🔎 Google Dorking in der Reconnaissance-Phase

## 📘 Ziel dieser Datei

Diese Datei liefert dir ein umfassendes Verständnis von **Google Dorking** als Technik zur Informationsbeschaffung in der **Reconnaissance-Phase** eines Penetration Tests oder Red Team Assessments. Zusätzlich zeigen wir, wie öffentlich zugängliche Daten in der Tiefe analysiert und ggf. für **Reverse Engineering**-Zwecke genutzt werden können.

---

## Inhaltsverzeichnis

1. [Was ist Google Dorking?](#1-was-ist-google-dorking)
2. [Zusammenhang mit der Reconnaissance-Phase](#2-zusammenhang-mit-der-reconnaissance-phase)
3. [Nützliche Dorking-Operatoren](#3-nützliche-google-operatoren)
4. [Beispiele für gezielte Dorks](#4-beispiele-für-gezielte-dorks)
5. [Reverse Engineering durch Dorking](#5-reverse-engineering-durch-google-dorking)
6. [Tools zur Unterstützung](#6-tools-zur-unterstützung)
7. [Rechtlicher Hinweis](#7-rechtlicher-hinweis)
8. [Weiterführende Ressourcen](#8-weiterführende-ressourcen)

---

## 1. Was ist Google Dorking?

**Google Dorking** (auch "Google Hacking" genannt) bezeichnet das gezielte Ausnutzen von **Suchoperatoren**, um mit Hilfe von Suchmaschinen (v.a. Google) **sensible Informationen** auf Webseiten zu finden, die fälschlicherweise öffentlich erreichbar sind.

Dazu gehören unter anderem:
- Konfigurationsdateien
- Backup-Dateien
- Zugangsdaten oder Passwörter
- Fehlermeldungen oder Debug-Informationen
- Admin-Panels
- Offene Verzeichnisse (Index of/)
- PDF-/DOC-/XLS-Dateien mit vertraulichem Inhalt

---

## 2. Zusammenhang mit der Reconnaissance-Phase

Google Dorking ist eine **passive Reconnaissance-Methode**, d.h. du sammelst Informationen, ohne direkt mit dem Zielsystem zu interagieren.

> Ziel: Möglichst viele **Metadaten, technische Infos, Dokumente, IPs, E-Mails, Konfigurationen** etc. finden, um später gezieltere Angriffe durchzuführen.

Beispiele für Informationen, die sich durch Google Dorking finden lassen:
- E-Mail-Adressen (für Phishing)
- API-Keys
- Versteckte Unterverzeichnisse
- Veraltete Softwareversionen
- Dateien mit internen IPs, Ports oder Credentials

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Nützliche Google-Operatoren

| Operator       | Beschreibung                                |
|----------------|---------------------------------------------|
| `site:`        | Nur Ergebnisse einer bestimmten Domain      |
| `intitle:`     | Suchbegriff im Seitentitel                  |
| `inurl:`       | Suchbegriff in der URL                      |
| `filetype:`    | Nach bestimmten Dateitypen suchen           |
| `ext:`         | Alias für `filetype:`                       |
| `intext:`      | Text im Seiteninhalt                        |
| `cache:`       | Zeigt gecachte Versionen an                 |
| `allinurl:`    | Alle Begriffe müssen in der URL vorkommen   |
| `allintitle:`  | Alle Begriffe im Titel                      |
| `link:`        | Seiten, die auf eine bestimmte URL linken   |

---

## 4. Beispiele für gezielte Dorks

```text
# Zugriff auf Login-Panels
inurl:admin site:example.com

# Offene Index-Seiten
intitle:"index of" site:example.com

# Sensible Dokumente
filetype:pdf site:example.com confidential
filetype:xls inurl:"password"

# Backup-Dateien
ext:bak OR ext:old OR ext:backup inurl:"config"

# Git-Repositories offen
inurl:.git site:target.com

# Kamerastreams
inurl:view/index.shtml

# Exposed ENV files (z. B. Laravel)
site:example.com ext:env DB_PASSWORD

# Sensible Fehlermeldungen
intext:"Warning: mysql_fetch_array()" site:example.com
```

--- 

## 5. Reverse Engineering durch Google Dorking

Die Informationen, die durch Dorking gewonnen werden, können im nächsten Schritt analysiert und "rückentwickelt" werden:
Beispiele für Reverse Engineering mit Google Dorking:

- Fehlermeldungen analysieren: Stack Traces offenbaren verwendete Frameworks → Angriffskette bauen.
- Versionsnummern entdecken: Spezifische CVEs identifizieren.
- Skript-Quellcode finden: Offene `.js`, `.env`, `.conf`, `.xml` oder `.php Dateien` → Schwachstellen lokalisieren.
- Veraltete CMS entdecken: z. B. Joomla 3.4 → Reverse Exploit-Suche nach CVEs.
- Offene API-Dokumentationen: Analyse des API-Aufrufs → mögliche Authorization-Bypass testen.

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Tools zur Unterstützung

| Tool                                                       | Zweck                                 |
| ---------------------------------------------------------- | ------------------------------------- |
| Google                                                     | Haupt-Engine                          |
| [GitDorker](https://github.com/obheda12/GitDorker)         | Dorks für GitHub-Suche automatisieren |
| [GHDB](https://www.exploit-db.com/google-hacking-database) | Google Hacking Database               |
| [DorkSearch](https://github.com/ZephrFish/DorkSearch)      | Python-basierte Dork-Suche            |
| [Shodan](https://www.shodan.io)                            | Ergänzung durch Gerätescans           |
| [ZoomEye](https://www.zoomeye.org)                         | Chinesisches Shodan-Äquivalent        |
| [Hunter.io](https://hunter.io)                             | E-Mail-Erkennung                      |
| [FOFA](https://fofa.info)                                  | Suchmaschine für Cybersicherheit      |

---

## 7. Rechtlicher Hinweis

Achtung:
Google Dorking kann schnell rechtlich heikel werden, insbesondere wenn du gezielt Schwachstellen auf fremden Systemen ausnutzt oder automatisierte Scans durchführst.

> Regel: Nur auf eigenen Systemen oder mit schriftlicher Erlaubnis!

- Passives Suchen ist oft nicht strafbar – aber das Sammeln und Weiterverarbeiten kann es werden.
- Bei Missbrauch greift das Computerstrafrecht (§202a StGB, §303b StGB) in Deutschland.

---

## 8. Weiterführende Ressourcen

- [Google Hacking Database – Exploit-DB](https://www.exploit-db.com/google-hacking-database)
- [Recon-NG Framework](https://github.com/lanmaster53/recon-ng)
- [HTB Academy: Information Gathering](https://github.com/lanmaster53/recon-ng)
- [TheHarvester Tool](https://github.com/laramies/theHarvester)

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