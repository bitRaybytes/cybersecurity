# OWASP Juice Shop Installation

## 1. Was ist OWASP Juice Shop

`OWASP Juice Shop` ist eine Webanwendung mit sehr vielen Sicherheitslücken wie es beispielsweise [DVWA](/10-practice-labs/dvwa-lab/dvwa-lab.md) ist.

Das Ziel dieser Anwendung ist es, die [OWASP Top 10](https://owasp.org/www-project-top-ten/) einmal selbst auszunutzen, ohne sich sofort strafbar zu machen. 

Über die Webseite von OWASP Juice Shop kannst du dir im Reiter `Challenges` ansehen, welche Herausforderungen es gibt. Unter anderem `Brute Force`, `Broken Anti Automation`, `Cryptographic Issues` usw. 


Mehr Informationen zum OWASP Juice Shop erhältst du hier: [OWASP Juice Shop (offizielle Webseite)](https://owasp.org/www-project-juice-shop/)

## 2. System Updates vorbereiten und durchführen

Gib im Termial deiner Kali folgenden Befehle ein, um dein System und Programme zu aktualisieren:

```bash
sudo apt update && sudo apt upgrade -y
```

## 3. Docker mit OWASP Juice Shop starten

**Hinweis:** Um Docker zu starten, ist es notwendig, das Programm herunterzuladen und zu installieren. Wie du Docker und alle Abhängigkeiten auf Kali Linux herunterlädts, [erfährst du hier](/09-tools-cheatsheet/docker-infos.md) (Debian basierte Installation)

Wir arbeiten mit Docker, um `OWASP Juice Shop` in einer sicherer Umgebung zu testen.

Teste, ob `Docker` auf deiner Kali installiert ist.
Dazu gibst du in deinem Kali Linux Terminal folgenden Befehl ein:

```bash
docker --version
```

Solltest du nun eine Version deines Docker-Programms erhalten, hast du Docker bei dir bereits erfolgreich installiert. Andernfalls musst du Docker zuerst installieren.

Du kannst jetzt dein Docker mit folgendem Container starten und gleichzeitig die aktuelle Version des OWASP Juice Shop herunterladen.

Gib dazu in deinem Kali Linux Terminal folgenden Befehle ein:

### 3.1 OWASP Juice Shop in Docker installieren
```bash
docker pull bkimminich/juice-shop
```
### 3.2 Docker und OWASP Juice Shop starten
```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
```
- `-d`: Startet den Container im Hintergrund (detached mode)
- `-p 3000:3000`: Öffnet Port 3000 auf deiner Maschine und verknüpft sie mit Port 3000 im Container.
- `--name juice-shop`: Benennt den Container `juice-shop`


Eine ausführliche Installationsanleitung erhältst du im [OWASP Juice Shop Companion](https://pwning.owasp-juice.shop/companion-guide/latest/part1/running.html).

### 3.3 Docker Container auflisten (optional)
```bash
docker ps
```
Hier sollte `juice-shop` auftauchen. Und vielleicht andere Container, die du am Laufen hast.

### 3.4 Juice Shop im Webbrowser öffnen

Um auf die OWASP Juice Shop Web Anwendung zuzugreifen öffnest du einfach deinen bevorzugten Browser und gibst die folgende URL ein:
```http
http://localhost:3000
```

### 3.5 Juice Shop Containerisierung beenden
Gib im Kali Linux Terminal folgenden Befehl ein:

```bash
docker stop juice-shop
```

### 3.6 Juice Shop Neustart
```bash
docker start juice-shop
```

## Nützliche Links

- [PWNING OWASP Juice Shop](https://pwning.owasp-juice.shop/companion-guide/latest/)
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [GitHub Juice-Shop](https://github.com/juice-shop/juice-shop)

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
