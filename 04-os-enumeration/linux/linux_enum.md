# 🐧 Linux Enumeration – Privilege Escalation & Recon

---

## Inhaltsverzeichnis
- [Ziel der Linux-Enumeration](#ziel-der-linux-enumeration)
- [1. Allgemeine Systeminfos](#1-allgemeine-systeminfos)
- [2. Benutzer & Rechte](#2-benutzer--rechte)
- [3. Benutzerrechte & sudo](#3-benutzerrechte--sudo)
- [4. Prozesse & laufende Dienste](#4-prozesse--laufende-dienste)
- [5. Cronjobs & zeitgesteuerte Tasks](#5-cronjobs--zeitgesteuerte-tasks)
- [6. Dateien mit gefährlichen Rechten](#6-dateien-mit-gefährlichen-rechten)
- [7. Interessante Dateien & Pfade](#7-interessante-dateien--pfade)
- [8. SSH-Keys und Zugangsdaten](#8-ssh-keys-und-zugangsdaten)
- [9. Netzwerk-Infos](#9-netzwerk-infos)
- [10. Installierte Pakete & Schwachstellen](#10-installierte-pakete--schwachstellen)
- [11. Passwort & Konfig-Dateien](#11-passwort--konfig-dateien)
- [12. Verbindungen & Weiterleitung](#12-verbindungen--weiterleitung)
- [13. Mounts & Dateisysteme](#13-mounts--dateisysteme)
- [Automatisierte Enumeration](#automatisierte-enumeration)
- [Haftungsausschluss](#haftungsausschluss)

--- 

## Ziel der Linux-Enumeration

Die Linux-Enumeration dient dazu, Informationen über ein kompromittiertes System zu sammeln, um Schwachstellen zu identifizieren und eventuell höhere Rechte (z. B. root) zu erlangen.

> 🎯 Ziel: Zugang, Persistenz, Privilege Escalation, lateral Movement

---

## 1. Allgemeine Systeminfos

```bash
uname -a                 # Kernel-Version
hostname                 # Hostname
whoami                   # Aktueller Benutzer
id                       # UID, Gruppen
cat /etc/issue           # Distribution
cat /etc/*release        # Release-Details
uptime                   # Systemlaufzeit
```
> Achte auf alte Kernel oder nicht gepatchte Versionen!

---

## 2. Benutzer & Rechte

```bash
cat /etc/passwd
cat /etc/shadow          # (nur root-lesbar)
getent passwd
last                    # Login-Historie
who -a
```

- Sind sensible Benutzer vorhanden? (z. B. `test`, `backup`, `admin`)
- Kann man `.bash_history` anderer Benutzer lesen?

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Benutzerrechte & sudo

```bash
sudo -l                 # Liste erlaubter Befehle (wichtig!)
sudo -V                 # sudo-Version (für Exploits)
groups                  # Gruppen des Benutzers
```

- Hat der Benutzer Root-Befehle ohne Passwort?
- Gibt es eine Umgehung wie `sudo` `less`, `nano`, `awk`, `python`, `tar`?

---

## 4. Prozesse & laufende Dienste

```bash
ps aux
top / htop
netstat -tulpen
ss -tuln
```

- Gibt es verdächtige Prozesse?
- Welche Dienste laufen lokal (`127.0.0.1`)?

---

## 5. Cronjobs & zeitgesteuerte Tasks

```bash
crontab -l
ls -la /etc/cron*
cat /etc/crontab
```
> Angriffsvektor: unsichere Skripte oder manipulierte Pfade

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Dateien mit gefährlichen Rechten

```bash
find / -perm -4000 -type f 2>/dev/null     # SUID-Files
find / -perm -2000 -type f 2>/dev/null     # SGID-Files
```

Wichtige SUID-Binaries z. B.:

- `/bin/bash`
- `/usr/bin/nmap`
- `/usr/bin/find`
- `/usr/bin/perl`
- `/usr/bin/python`

> Siehe: [GTFOBins](https://gtfobins.github.io/)

--- 

## 7. Interessante Dateien & Pfade

```bash
ls -la ~
cat ~/.bash_history
find / -name '*.log' 2>/dev/null
find / -name '*.conf' 2>/dev/null
find / -name 'id_rsa' 2>/dev/null
```
- `.bash_history` enthält häufig verwendete Befehle (evtl. Passwörter)
- `id_rsa` = private SSH-Schlüssel

---

## 8. SSH-Keys und Zugangsdaten

```bash
cat ~/.ssh/id_rsa
cat ~/.ssh/authorized_keys
```

> Gibt es andere private Keys auf dem System?

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 9. Netzwerk-Infos

```bash
ip a
ip r
cat /etc/hosts
arp -a
netstat -antup
```

- Gibt es lokale Services oder interne IPs?
- Erreichbare Netzwerke?

---

## 10. Installierte Pakete & Schwachstellen

```bash
dpkg -l       # Debian/Ubuntu
rpm -qa       # CentOS/RHEL
```

> Abgleich mit CVE-Datenbanken → Alte Pakete = Exploit-Möglichkeit

---

## 11. Passwort & Konfig-Dateien

```bash
grep -i "password" /etc/*.conf
find / -name "*.php" | xargs grep -i "db_pass"
```

- Klartext-Passwörter in Skripten, Configs oder Umgebungen?
- `.env` oder `.git/config`?

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 12. Verbindungen & Weiterleitung

```bash
lsof -i
netstat -plant
iptables -L
```
- Ist das System Gateway?
- Gibt es Tunnel, offene Ports oder Weiterleitungen?

---

## 13. Mounts & Dateisysteme

```bash
mount
df -h
lsblk
cat /etc/fstab
```
- Eingehängte Netzlaufwerke?
- Fremde Dateisysteme (z. B. NFS)?

----

## Automatisierte Enumeration

| Tool               | Beschreibung                       |
| ------------------ | ---------------------------------- |
| `LinPEAS.sh`       | Vollautomatische Enumeration       |
| `Linux Smart Enum` | Minimalistisches Bash-Tool         |
| `LinEnum.sh`       | Klassisches Linux-Enum-Tool        |
| `pspy`             | Prozess- und Cronwatcher ohne Root |
| `GTFOBins`         | Exploitable Binaries mit SUID/Sudo |


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