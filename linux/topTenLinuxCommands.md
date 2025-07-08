# 🧰 Top 10 Useful Linux Commands

**Zweck dieser Datei:**  
Diese Datei enthält eine kompakte Auswahl der nützlichsten Linux-Kommandos für Security-Analysten, Pentester, Systemadministratoren und CTF-Spieler. Sie helfen beim Navigieren, Analysieren und Kontrollieren eines kompromittierten oder fremden Systems – etwa nach erfolgreicher Exploitation oder während der Post-Exploitation-Phase.

> ⚠️ **Disclaimer:** Die hier beschriebenen Techniken und Kommandos sind ausschließlich für Schulungs- und Testzwecke gedacht – z. B. in CTF-Umgebungen oder autorisierten Penetrationstests. Der Missbrauch in realen, nicht autorisierten Systemen ist strafbar.

---

## 🔟 Die wichtigsten Linux-Kommandos

### 1. `grep` – Textsuche in Dateien

```bash
grep 'root' /etc/passwd
grep -R 'password' /var/www/
```
➡️ Durchsuche Dateien nach bestimmten Strings, Passwörtern oder Konfigurationen.

### 2. find – Dateien nach Kriterien suchen

```bash
find / -type f -name "*.conf" 2>/dev/null
find / -perm -4000 2>/dev/null
```
➡️ Suche nach konfigurationsrelevanten Dateien oder SUID-Binaries.

### 3. awk – Text extrahieren, Spaltenweise analysieren

```bash
cat /etc/passwd | awk -F ':' '{print $1}'
```
➡️ Extrahiere Benutzernamen aus Systemdateien.

### 4. chmod – Berechtigungen ändern

```bash
chmod +x exploit.sh
chmod 777 shell.php
```
➡️ Setze ausführbare Rechte oder öffne Berechtigungen für Datei-Uploads.

### 5. scp – Dateien über SSH übertragen

```bash
scp localfile.txt user@host:/tmp/
scp root@target:/etc/shadow .
```
➡️ Ziehe Dateien vom oder auf das Zielsystem (Reverse Shells, Loot, etc.).

### 6. rsync – Synchronisiere Dateien oder ziehe ganze Verzeichnisse

```bash
rsync -avz /var/www/html user@host:/backup/
```
➡️ Ideal, um Dateien schnell und rekursiv zu sichern oder zu extrahieren.

### 7. tar – Packen und Entpacken von Archiven

```bash
tar -czf www.tar.gz /var/www
tar -xvf backup.tar
```
➡️ Packe komplette Webroots zur Analyse oder übertrage sie effizient.

### 8. netstat oder ss – Netzwerke analysieren

```bash
netstat -tuln
ss -tuln
```
➡️ Welche Dienste lauschen auf welchen Ports?

### 9. whoami, id, hostname, uname – Systeminformationen

```bash
whoami
id
uname -a
hostname
```
➡️ Ermittle User- und Systemkontext für Rechteeskalation.

### 10. ps, top, pstree – Prozesse beobachten

```bash
ps aux | grep apache
top
pstree
```
➡️ Finde laufende Prozesse oder potenzielle Schwachstellen in Services.

### 🧪 Bonus: Praktische Kombinationen

```bash
find / -type f -name "*.php" 2>/dev/null | xargs grep -i 'db_password'
grep -r "api_key" /var/www/html
```
➡️ Kombiniere Tools für effiziente Enumeration und Looting.

--- 

## 📎 Weitere nützliche Kommandos

| Kommando        | Zweck                                     |
| --------------- | ----------------------------------------- |
| `nc` / `netcat` | Shells, Port-Scan, einfache HTTP-Anfragen |
| `curl`          | Web-Anfragen über CLI                     |
| `wget`          | Dateien herunterladen                     |
| `ls -la`        | Berechtigungen und versteckte Dateien     |
| `crontab -l`    | Geplante Tasks analysieren                |


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

## 📚 Quellen & Empfehlungen
- [explainshell.com – Erklärt Bash-Befehle im Detail](https://explainshell.com/)
- [GTFOBins – Nützliche Binaries für Privilege Escalation](https://gtfobins.github.io/)
- [HackTricks Linux – Post-Exploitation Tipps](https://book.hacktricks.xyz/linux-hardening/)