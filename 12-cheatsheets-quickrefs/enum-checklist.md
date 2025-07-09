# 📋 Enumeration Checklist

## 🧠 Ziel dieser Datei

Diese Datei dient als umfassende Checkliste für die **Enumeration-Phase** in Penetration Tests, Red Team Exercises und CTFs. Ziel ist es, systematisch Informationen über Zielsysteme, Netzwerke und Dienste zu sammeln, um Angriffsvektoren aufzudecken. Sie deckt **lokale**, **Netzwerk-**, **Web-** und **Cloud-spezifische** Enumeration ab.

---

## ✅ Allgemeine Hinweise zur Enumeration

- 📌 Enumeration erfolgt nach der Reconnaissance-Phase
- 🔍 Ziel ist **aktive Informationsgewinnung** (Banner, Benutzer, Shares, Ports etc.)
- ⚠️ Immer mit Genehmigung (Legal Boundaries)
- 📂 Alle Informationen dokumentieren (z. B. in Markdown, CherryTree, Obsidian, KeepNote etc.)

---

## 🔌 Netzwerk- & Port-Enumeration

### 🔍 Ziel: Identifikation erreichbarer Systeme und offener Ports

| Aufgabe                             | Tools/Techniken                          | Check? |
|-------------------------------------|------------------------------------------|--------|
| Ping Sweep (Host Discovery)         | `nmap -sn`, `masscan`, `fping`           | ☐      |
| Portscan TCP/UDP                    | `nmap -sS -sU -p-`, `masscan`            | ☐      |
| Service/Banner-Grabbing             | `nmap -sV`, `netcat`, `telnet`, `curl`   | ☐      |
| OS Detection                        | `nmap -O`, `xprobe2`, `p0f`              | ☐      |
| Version Detection                   | `nmap -sV`                               | ☐      |
| Firewall-Erkennung                  | `nmap -sA`, `hping3`, `traceroute`       | ☐      |

---

## 🖥️ System-/Host-Enumeration (Linux/Windows)

### 🧾 Ziel: Lokale Informationen nach erstem Zugriff oder über RPC/SMB

| Aufgabe                              | Tools/Techniken                           | Check? |
|--------------------------------------|-------------------------------------------|--------|
| Benutzer & Gruppen                   | `whoami`, `id`, `net users`, `dsquery`    | ☐      |
| Prozesse und Dienste                 | `ps`, `tasklist`, `sc query`, `systemctl` | ☐      |
| Installierte Programme               | `dpkg -l`, `wmic product`                 | ☐      |
| Netzwerkverbindungen                 | `netstat`, `ss`, `ip a`, `ifconfig`       | ☐      |
| Freigaben & Laufwerke                | `net share`, `mount`, `lsblk`             | ☐      |
| Umgebungsvariablen                   | `env`, `$PATH`, `$USER`, `set`            | ☐      |
| Zeitpläne / Cronjobs                 | `crontab -l`, `schtasks /query`           | ☐      |
| Schwachstellen-Scanner (lokal)       | `linpeas`, `winPEAS`, `les`, `Seatbelt`   | ☐      |

---

## 🌐 Web-Enumeration

### 🌎 Ziel: Services, APIs, CMS, Parameter und Schwachstellen erkennen

| Aufgabe                              | Tools/Techniken                         | Check? |
|-------------------------------------|------------------------------------------|--------|
| HTTP-Header & Antwortcodes          | `curl -I`, `httpx`, `whatweb`            | ☐      |
| Unterseiten entdecken               | `gobuster`, `dirb`, `feroxbuster`        | ☐      |
| Subdomain-Enumeration               | `assetfinder`, `subfinder`, `amass`      | ☐      |
| CMS-Erkennung                       | `wpscan`, `droopescan`, `whatweb`        | ☐      |
| Login-Portale prüfen                | Manuell, `wfuzz`, `hydra`                | ☐      |
| Parameter-Fuzzing                   | `ffuf`, `paramspider`, `arjun`           | ☐      |
| JS-Parsing auf versteckte Daten     | `LinkFinder`, `JSParser`                 | ☐      |

---

## 🧬 Active Directory Enumeration

| Aufgabe                              | Tools/Techniken                         | Check? |
|-------------------------------------|------------------------------------------|--------|
| Domäneninfos                        | `whoami /all`, `nltest /dclist`          | ☐      |
| Benutzer & Gruppen                  | `net group`, `net user /domain`          | ☐      |
| GPOs & ACLs                         | `gpresult`, `SharpGPOAbuse`, `BloodHound`| ☐      |
| SID & Token-Auswertung              | `whoami /groups`, `whoami /priv`         | ☐      |
| Kerberoasting-Möglichkeiten         | `Rubeus`, `impacket-GetUserSPNs`         | ☐      |

---

## ☁️ Cloud- & API-Enumeration (AWS / Azure / GCP)

| Aufgabe                              | Tools/Techniken                         | Check? |
|-------------------------------------|------------------------------------------|--------|
| AWS Keys prüfen                     | `truffleHog`, `awscli`, `enumerate-iam`  | ☐      |
| IAM-Privilegien prüfen              | `aws iam list-users`, `ScoutSuite`       | ☐      |
| Public Buckets/Blobs suchen         | `s3scanner`, `gobuster`, `azcopy`        | ☐      |
| Serverless & API-Gateways           | `APIsec`, Swagger-Parsing, Burp          | ☐      |

---

## 🔐 Auth-Mechanismen & Bruteforce-Punkte

| Ziel                                 | Tools/Techniken                          | Check? |
|--------------------------------------|------------------------------------------|--------|
| Login-Formulare                      | `hydra`, `wfuzz`, `Burp Intruder`        | ☐      |
| SSH/FTP/SMTP                         | `hydra`, `ncrack`, `medusa`              | ☐      |
| RDP/SMB                              | `crackmapexec`, `xfreerdp`, `hydra`      | ☐      |

---

## 🧩 Checklisten-Snippet zum Mitnehmen

```bash
nmap -sS -sV -p- -T4 10.10.10.0/24
whatweb http://target
gobuster dir -u http://target -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
enum4linux -a target
linpeas.sh (nach Upload)
```

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---

