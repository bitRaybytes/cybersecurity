# 🔍 Reconnaissance Commands Cheat Sheet

## 📘 Ziel dieser Datei

Diese Datei enthält **wichtige Reconnaissance-Kommandos**, um Ziele in einem Penetration Test, CTF oder bei Red/Blue-Team-Analysen effizient zu identifizieren, zu kartieren und zu analysieren. Die Befehle decken sowohl **aktive als auch passive Recon-Techniken** ab.

> ⚠️ Hinweis: Die hier gelisteten Befehle dürfen **nur in autorisierten, legalen Testumgebungen** eingesetzt werden!

---

## 🧭 Inhaltsverzeichnis

- [1. DNS- & Domain-Recon](#1-dns--domain-recon)
- [2. WHOIS & Zertifikatsinfos](#2-whois--zertifikatsinfos)
- [3. Subdomain Enumeration](#3-subdomain-enumeration)
- [4. Port & Service Scanning](#4-port--service-scanning)
- [5. Web Recon & Fingerprinting](#5-web-recon--fingerprinting)
- [6. Directory & File Brute Forcing](#6-directory--file-brute-forcing)
- [7. E-Mail & Benutzerrecherche](#7-e-mail--benutzerrecherche)
- [8. OSINT & Tools](#8-osint--tools)
- [9. Active Directory Recon](#9-active-directory-recon)
- [10. Nützliche Ressourcen](#10-nützliche-ressourcen)

---

## 1. 🌐 DNS & Domain Recon

```bash
nslookup domain.com
dig domain.com any
dig axfr @ns1.domain.com domain.com     # Zone Transfer
host -t mx domain.com                   # Mailserver prüfen
# DNSDumpster.com (Online)
# crt.sh/?q=domain.com (Zertifikate auslesen)
```

---

## 2. 🌍 WHOIS & Zertifikatsinfos
```bash
whois domain.com
whois -h whois.arin.net <IP>
openssl s_client -connect domain.com:443 -showcerts
```

---

## 3. 🏗️ Subdomain Enumeration
```bash
amass enum -d domain.com
subfinder -d domain.com
assetfinder --subs-only domain.com
crt.sh/?q=%.domain.com
```

---

## 4. 🚪 Port & Service Scanning
```bash
nmap -sC -sV -oA fullscan domain.com
nmap -p- --min-rate 1000 -T4 domain.com
rustscan -a domain.com
```
Nmap Service-Scripts:
```bash
nmap -p 21 --script ftp-anon,ftp-bounce <target>
nmap -p 80,443 --script http-title,http-enum <target>
```

---

## 5. 🌐 Web Recon & Fingerprinting
```bash
whatweb domain.com
wappalyzer -u https://domain.com
curl -I https://domain.com
curl -s -X OPTIONS https://domain.com
```
```bash
# HTTP Header-Infos
curl -s -D- https://domain.com | grep "Server"
```

---

## 6. 📂 Directory & File Brute Forcing
```bash
gobuster dir -u https://domain.com -w /usr/share/wordlists/dirb/common.txt
ffuf -u https://domain.com/FUZZ -w wordlist.txt
dirsearch -u https://domain.com
```

---

## 7. 📧 E-Mail & Benutzerrecherche
```bash
theharvester -d domain.com -b google,bing,linkedin
hunter.io (manuell)
emailrep.io API (E-Mail Reputation)
```

---

## 8. 🕵️ OSINT & Tools

| Tool               | Beschreibung                |
| ------------------ | --------------------------- |
| **Spiderfoot**     | Automatisiertes OSINT-Tool  |
| **Maltego**        | Graphische OSINT-Recon      |
| **Recon-ng**       | Modularer OSINT-Scanner     |
| **Shodan**         | Geräte-/Dienst-Suchmaschine |
| **Censys.io**      | TLS/HTTP/IP Discovery       |
| **HaveIBeenPwned** | Breached E-Mail Check       |
| **GHunt**          | Google Account OSINT        |


---

## . 🧠 Active Directory Recon (intern)
```powershell
# PowerView (PowerShell)
Get-NetUser
Get-NetGroup
Get-NetComputer
Get-DomainTrust
# BloodHound Collection
SharpHound.exe -c All
```
```bash
# ldapsearch (Linux)
ldapsearch -x -h <dc-ip> -b "dc=domain,dc=local"
```

---

## 10. 📚 Nützliche Ressourcen
- [nmap.org](https://nmap.org/)
- [github.com/OWASP/Amass](https://github.com/owasp-amass/amass)
- [hackertarget.com](https://hackertarget.com/)
- [osintframework.com](https://drive.google.com/drive/home)
- [attack.mitre.org](https://attack.mitre.org/)
- [red_team_tools.md](/05-red-teaming/red_team_tools.md)

--- 

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

**Letzte Aktualisierung:** Juli 2025  
Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!
