# DVWA - Damn Vulnerable Web Application 🐞💻

## Was ist DVWA? 🤔

Die **Damn Vulnerable Web Application (DVWA)** ist eine bewusst unsichere Webanwendung, die für **Schulungs- und Testzwecke** entwickelt wurde. Ziel ist es, **Sicherheitsexperten, Entwicklern und Studenten** die Möglichkeit zu geben, **verschiedene Webanwendungs-Schwachstellen** in einer kontrollierten Umgebung zu üben und zu verstehen – ohne dabei echte Systeme zu gefährden.

🔎 [Was ist die DVWA? - Google-Suche](https://www.google.com/search?q=was+ist+die+dvwa&client=firefox-b-d&sca_esv=45ade08aecea71dd&sxsrf=AE3TifNZWbMyTSVuoLgFF1l0m5ggqQv_Ew%3A1753627769172&ei=eTyGaNOZCt3d7_UP4e2IoQs)

---

## DVWA im Docker unter Kali Linux installieren 🐳📦

Folgende Schritte zeigen dir, wie du DVWA über Docker auf Kali Linux installieren kannst:

### 🧰 Schritt 1: Kali Linux updaten

Öffne dein Terminal und führe aus:

```bash
sudo apt update -y
```

![Schritt 1: Update Kali Linux](/10-practice-labs/ressources/pictures/step1UpdateKali.png)

### 🔧 Schritt 2: Kali Linux upgraden

Anschließend führst du das Upgrade durch:

```bash
sudo apt upgrade -y
```

![Schritt 2: Upgrade Kali Linux](/10-practice-labs/ressources/pictures/step2UpgradeKali.png)

💡 **Tipp:** Das `-y` steht für "yes" – es bestätigt alle Rückfragen automatisch.

🔐 **Hinweis:** Falls du zur Eingabe eines Passworts aufgefordert wirst, gib dein **Root- oder Sudo-Passwort** ein.

### 🐳 Schritt 3: Docker installieren

Eine ausführliche Anleitung zur Docker-Installation findest du hier: [Docker Guide](/09-tools-cheatsheet/docker-infos.md).

Hier die Kurzfassung:

1. Systemabhängigkeiten installieren ✅
2. Docker GPG-Schlüssel importieren 🔑
3. Docker-Repository einrichten 📦
4. Docker installieren 🐳
5. Testlauf mit `hello-world` 🔄

![Schritt 3: Abhängigkeiten installieren](/10-practice-labs/ressources/pictures/step3installDependencies.png)

### 🔥 Schritt 4: DVWA starten

Nachdem Docker installiert ist, kannst du DVWA starten. Die offizielle Image-Dokumentation findest du hier:
📚 [Docker Hub – DVWA](https://hub.docker.com/r/vulnerables/web-dvwa)

👉 Führe den folgenden Befehl im Terminal aus:

```bash
docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

📂 Dieser Befehl lädt das DVWA-Image (falls noch nicht vorhanden), startet einen Container, leitet den Webserver-Port 80 weiter und startet die Anwendung direkt.

🌐 Öffne jetzt deinen Browser und rufe auf:

```http
http://localhost
```

Oder – falls du DVWA auf einer virtuellen Maschine nutzt – verwende die entsprechende **IP-Adresse deiner VM**:

```http
http://<IP-Adresse>
```

![DVWA starten](10-practice-labs/ressources/pictures/step4installDocker.png)

> Wenn wir erfolgreich gewesen sind, dann sollten wir nun genau diese Seite (links) vorfinden. Das heißt, wir haben die DVWA erfolgreich installiert.

> Um die Installation komplett abzuschließen, müssen wir unsere Datenbank registrieren. Ab hier kannst du die gesamte Seite versuchen zu inspizieren. Brauchst du Hilfe bei dem Them SQL-Injection? [Hier geht es zum SQL-Injection Cheat Sheet](/03-web-security/sql-injection/sql-injection-cheatsheet.md)


---

## 🧠 Nützliche Hinweise

* Falls Port 80 bereits verwendet wird, nutze z. B. `-p 8080:80` und rufe DVWA über `http://localhost:8080` auf.
* Standard-Zugangsdaten für DVWA:

  * **Benutzer:** `admin`
  * **Passwort:** `password`
* In DVWA musst du unter `DVWA Security` eventuell zuerst die Sicherheitsstufe auf **low** stellen, um alle Tests zu ermöglichen.

---

## ✅ Fazit

Mit DVWA kannst du realistische Web-Angriffe wie SQL-Injection, XSS oder Command Injection **risikofrei testen und verstehen**. In Kombination mit Docker auf Kali Linux erhältst du eine flexible, wiederverwendbare Umgebung für dein Cybersecurity-Training. 🔐🧑‍💻

> 🚀 **Nächster Schritt:** Übe mit Tools wie Burp Suite, OWASP ZAP oder Nikto gegen deine DVWA-Instanz!


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
