# 🧠 CTF Resources & Learning Labs

**Zweck dieser Datei:**  
Diese Übersicht richtet sich an alle, die systematisch in die Welt der CTFs, Cybersicherheit und des ethischen Hackings einsteigen möchten – insbesondere Anfänger, Fortgeschrittene oder Teilnehmer einer IT-Umschulung.  
Sie enthält Links zu Plattformen, Trainings, Tools, Repositories und Communities.

> **Disclaimer:** Alle hier aufgeführten Ressourcen dienen ausschließlich zu legalen, schulischen oder schulungsbezogenen Zwecken. Die Nutzung von Exploits, Recon-Techniken oder Hacking-Tools ist nur in autorisierten Testumgebungen erlaubt.

---

## Inhaltsverzeichnis
- [CTF-Plattformen & Übungslabore](#ctf-plattformen--übungslabore)
- [Nützliche Tools (Pentest & CTF)](#nützliche-tools-pentest--ctf)
- [GitHub Repositories](#github-repositories)
- [Communitys & Discord-Server](#communitys--discord-server)
- [Lernempfehlungen nach Themengebiet](#lernempfehlungen-nach-themengebiet)
- [Für den Einstieg empfohlen](#für-den-einstieg-empfohlen)
- [Tägliche Lern-Routine (Beispiel)](#tägliche-lern-routine-beispiel)
- [Haftungsausschluss](#haftungsausschluss)

## CTF-Plattformen & Übungslabore

| Plattform      | Beschreibung                                                   |
|----------------|----------------------------------------------------------------|
| [Hack The Box (HTB)](https://www.hackthebox.com/) | Realistische Labs (PEN-200, OSCP-Level) mit Maschinen, Challenges und Academy |
| [TryHackMe](https://tryhackme.com/)        | Sehr anfängerfreundlich, gamifiziert, mit Lernpfaden z. B. Pre-Security, Offensive Pentesting |
| [PicoCTF](https://picoctf.org)             | CTF von der Carnegie Mellon University. Ideal für Einsteiger & Schüler |
| [OverTheWire](https://overthewire.org/wargames/) | Klassische Linux-/CTF-Wargames (Bandit, Narnia, Leviathan) |
| [Root-Me](https://www.root-me.org/)        | Viele Challenges & Systeme in versch. Schwierigkeitsgraden |
| [CTFtime.org](https://ctftime.org/)        | Kalender & Rankingsystem für aktuelle CTF-Wettbewerbe weltweit |
| [CyberDefenders](https://cyberdefenders.org/) | DFIR & Blue Team Labs, gute Ergänzung zu Red-Team-Plattformen |

---

## Nützliche Tools (Pentest & CTF)

| Tool           | Zweck                                         |
|----------------|-----------------------------------------------|
| Burp Suite     | Web Application Proxy für Tests & Exploits    |
| Nmap           | Netzwerk-Scanning & Host Discovery            |
| Nikto / Dirb   | Webserver-Recon, Directory Brute Force        |
| John / Hashcat | Passwort-Cracking                            |
| LinPEAS / WinPEAS | Privilege Escalation Scripts für Linux/Windows |
| Gobuster / ffuf| Fuzzing & Directory Discovery                 |
| Metasploit     | Exploitation Framework                        |
| Ghidra         | Reverse Engineering                           |
| CyberChef      | Encoding/Decoding & Data-Transformation       |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## GitHub Repositories

| Repo                                                   | Inhalt                                               |
|--------------------------------------------------------|------------------------------------------------------|
| [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) | Umfangreiche Payload-Sammlung für Pentests/Exploits |
| [GTFOBins](https://gtfobins.github.io/)                | Linux PrivEsc mit legitimen Binaries                |
| [LOLBAS](https://lolbas-project.github.io/)            | Windows PrivEsc mit legitimen Systemdateien         |
| [HackTricks](https://book.hacktricks.xyz/)             | Sehr gute Sammlung für Exploits, Tricks, Tipps      |
| [LinEnum](https://github.com/rebootuser/LinEnum)       | Automatisierte Enumeration für Linux PrivEsc        |
| [awesome-hacking](https://github.com/Hack-with-Github/Awesome-Hacking) | Riesige Linkliste zu Tools, CTFs, Lernmaterialien   |

---

## Communitys & Discord-Server

| Name                  | Fokus                            |
|-----------------------|----------------------------------|
| TryHackMe Discord     | Anfängerhilfe, Community         |
| HTB Discord           | Tipps, Lösungen, Networking      |
| InfoSec Prep Discord  | Unterstützung für OSCP & andere Zertifikate |
| CTFtime Discord       | CTF-Turniere, Organisation       |
| r/netsec (Reddit)     | Allgemeine IT-Security Themen    |
| r/AskNetsec           | Fragen zu Tools & Vorgehensweisen|

---

## Lernempfehlungen nach Themengebiet

### Netzwerk & Recon

- Nmap, Wireshark
- DNS/WHOIS Recon
- Subdomain-Enumeration (amass, subfinder)

### Web Exploitation

- Burp Suite Basics
- OWASP Top 10
- SQLi, XSS, SSTI, LFI, RCE

### Binary Exploitation

- Buffer Overflows
- Format String Attacks
- pwntools, GDB

### Cryptography

- Caesar, XOR, AES Challenges
- RSA Crack (small e/n), FactorDB
- CyberChef zum Testen

### Steganographie

- Analyse mit `binwalk`, `steghide`, `zsteg`, `exiftool`

### Forensik / Blue Team

- Memory Analysis mit Volatility
- Packet Captures (PCAP) analysieren
- CyberDefenders Challenges

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Für den Einstieg empfohlen:

1. **TryHackMe: "Pre-Security" oder "Complete Beginner" Pfad**  
2. **HTB Academy: "Linux Fundamentals", "Web Requests", "Enumeration"**
3. **OverTheWire: "Bandit" bis Level 26**
4. **Root-Me: Einsteiger-Challenges aus Web & Network**
5. **YouTube-Kanäle** wie:
   - The Cyber Mentor (TCM)
   - IppSec (HackTheBox-Lösungen)
   - John Hammond
   - LiveOverflow

---

## Tägliche Lern-Routine (Beispiel)

| Tag | Inhalt                            |
|-----|-----------------------------------|
| Mo  | 1x TryHackMe Room (Web Exploit)   |
| Di  | Linux Enumeration (HTB Academy)   |
| Mi  | Crypto + Stego Challenge (PicoCTF)|
| Do  | RootMe + Dirbusting üben          |
| Fr  | YouTube-Video + Notizen           |
| Sa  | CTF-Challenge mit Writeup         |
| So  | Zusammenfassung + Schwachstellen lesen |

---

**Letzter Tipp:**  
**Führe ein CTF-Tagebuch.** Notiere:
- verwendete Tools
- Commands
- Lösungswege
- neue Erkenntnisse

> Wer schreibt, der bleibt – und wiederholt Gelerntes schneller.

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
