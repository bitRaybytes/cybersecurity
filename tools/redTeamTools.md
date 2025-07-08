# 🛠️ Penetration Testing Tool-Kit Cheat Sheet

> Tools & Techniken für alle Phasen eines Penetrationstests (Red Team Fokus).
> Es sind selbstverständlich nur einige der vorhandenen Tools für offensive Tests.

## Inhaltsverzeichnis

1) [Informationsbeschaffung](#1️⃣-informationsbeschaffung-reconnaissance)
2) [Vulnerability Analyse](#2️⃣-vulnerability-analyse)
3) [Webb Application Analyse](#3️⃣-web-application-analyse)
4) [Datenbank Assessement](#4️⃣-datenbank-assessment)
5) [Password Attacks](#5️⃣-password-attacks)
6) [Wireless Attacks](#6️⃣-wireless-attacks)
7) [Reverse Engineering](#7️⃣-reverse-engineering)
8) [Exploitation Tools](#8️⃣-exploitation-tools)
9) [Sniffing & Spoofing](#9️⃣-sniffing--spoofing)
10) [Post Exploitation](#-post-exploitation)
11) [Forensics](#1️⃣1️⃣-forensik--log-analyse)
12) [Reporting Tools](#1️⃣2️⃣-reporting-tools)
13) [Social Engineering Tools](#1️⃣3️⃣-social-engineering-tools)
14) [System Services](#1️⃣4️⃣-systemdienste-für-enumeration--missbrauch)

---

## 1️⃣ Informationsbeschaffung (Reconnaissance)

| Tool             | Zweck                                       |
| ---------------- | ------------------------------------------- |
| **Nmap**         | Netzwerk-Scan, Host Discovery, OS Detection |
| **Recon-ng**     | Web-basierte Informationsbeschaffung        |
| **theHarvester** | Emails, Hosts, Namen via OSINT              |
| **Maltego**      | Graphische OSINT-Analyse                    |
| **Shodan**       | IoT-/Internet-Geräte-Suchmaschine           |
| **Amass**        | Subdomain Enumeration                       |
| **FOCA**         | Metadaten in Dokumenten finden              |

---

## 2️⃣ Vulnerability Analyse

| Tool                            | Zweck                                |
| ------------------------------- | ------------------------------------ |
| **Nessus**                      | Kommerzieller Schwachstellenscanner  |
| **OpenVAS**                     | Open-Source-Schwachstellenscanner    |
| **Nikto**                       | Webserver-Schwachstellen finden      |
| **LinEnum / LinPEAS / WinPEAS** | Lokale Privilege Escalation Checks   |
| **Nmap NSE Scripts**            | Sicherheitslücken testen per Skripte |

---

## 3️⃣ Web Application Analyse

| Tool                       | Zweck                                  |
| -------------------------- | -------------------------------------- |
| **Burp Suite**             | Web Proxy, Scanner, Repeater, Intruder |
| **OWASP ZAP**              | Web-Schwachstellenscanner              |
| **Wfuzz**                  | Fuzzing von Parametern & Pfaden        |
| **SQLmap**                 | Automatisierte SQL-Injection           |
| **Dirb / Gobuster / FFUF** | Verzeichnis-Bruteforce                 |
| **Nikto**                  | Webserver-Analyse                      |
| **XSStrike**               | XSS-Finder & Exploiter                 |

---

## 4️⃣ Datenbank Assessment

| Tool                        | Zweck                                    |
| --------------------------- | ---------------------------------------- |
| **SQLmap**                  | Automatisierte SQLi-Erkennung & Exploits |
| **DBPwAudit**               | Datenbankpasswort-Audit                  |
| **Nmap NSE (mysql, mssql)** | Datenbankspezifische Tests               |
| **Metasploit**              | Datenbankmodule                          |


---

## 5️⃣ Password Attacks

| Tool                | Zweck                               |
| ------------------- | ----------------------------------- |
| **John the Ripper** | Offline Passwort-Cracking           |
| **Hashcat**         | GPU-basiertes Passwortknacken       |
| **Hydra**           | Brute-Force über Netzwerkprotokolle |
| **Medusa**          | Schneller Passworttester            |
| **CeWL**            | Passwörter aus Webseiten generieren |
| **Crunch**          | Passwortlisten generieren           |

---

## 6️⃣ Wireless Attacks

| Tool            | Zweck                            |
| --------------- | -------------------------------- |
| **Aircrack-ng** | WPA/WEP-Cracking, Sniffing       |
| **Kismet**      | Wireless Network Detector        |
| **Wireshark**   | WLAN-Paketanalyse                |
| **Wifite**      | Automatisierte Angriffe auf WLAN |
| **Bettercap**   | BLE/WiFi-Angriffe                |
| **Reaver**      | WPS-Angriffe                     |

--- 

## 7️⃣ Reverse Engineering

| Tool                 | Zweck                           |
| -------------------- | ------------------------------- |
| **Ghidra**           | Disassembler & Decompiler (NSA) |
| **IDA Free**         | Interaktiver Disassembler       |
| **Radare2**          | Reverse Engineering Framework   |
| **Binary Ninja**     | Kommerziell, modern             |
| **Cutter**           | GUI für Radare2                 |
| **x64dbg / OllyDbg** | Windows-Debugging               |

---

## 8️⃣ Exploitation Tools

| Tool                      | Zweck                               |
| ------------------------- | ----------------------------------- |
| **Metasploit Framework**  | Exploits, Payloads, Sessions        |
| **ExploitDB**             | Exploit-Datenbank                   |
| **Searchsploit**          | CLI für ExploitDB                   |
| **MSFvenom**              | Payload-Generator                   |
| **Impacket**              | SMB/NTLM-Tools für Lateral Movement |
| **PowerSploit / Nishang** | PowerShell-basierte Exploits        |

---

## 9️⃣ Sniffing & Spoofing

| Tool          | Zweck                        |
| ------------- | ---------------------------- |
| **Wireshark** | Paket-Sniffing               |
| **Ettercap**  | Man-in-the-Middle, Spoofing  |
| **Bettercap** | Advanced Spoofing & Sniffing |
| **ARPspoof**  | ARP-Spoofing                 |
| **Responder** | LLMNR/NBTNS Poisoning        |
| **tcpdump**   | CLI-Sniffer                  |

---

## 🔟 Post Exploitation

| Tool                 | Zweck                                |
| -------------------- | ------------------------------------ |
| **Meterpreter**      | Metasploit Payload (In-Memory Shell) |
| **Empire / Faction** | C2-Frameworks mit PowerShell         |
| **BloodHound**       | AD-Recon & Attack Path Mapping       |
| **Mimikatz**         | Windows-Credentials auslesen         |
| **PowerView**        | AD-Datenabfrage via PowerShell       |
| **SharpHound**       | Daten für BloodHound sammeln         |

--- 

## 1️⃣1️⃣ Forensik & Log-Analyse

| Tool                     | Zweck                     |
| ------------------------ | ------------------------- |
| **Autopsy**              | GUI Forensik-Tool         |
| **Volatility**           | RAM-Analyse               |
| **Sleuth Kit**           | Filesystem Forensik       |
| **Plaso / log2timeline** | Zeitleistenanalyse        |
| **binwalk**              | Binäranalyse von Firmware |
| **ExifTool**             | Metadaten auslesen        |

---

## 1️⃣2️⃣ Reporting Tools

| Tool                   | Zweck                                 |
| ---------------------- | ------------------------------------- |
| **CherryTree**         | Strukturierte Notizen & Reports       |
| **Dradis**             | Reporting & Kollaboration             |
| **MagicTree**          | Automatisierte Report-Zusammenfassung |
| **Faraday**            | Team-Reporting-Plattform              |
| **MS Word / Markdown** | Export von Ergebnissen                |

---

## 1️⃣3️⃣ Social Engineering Tools

| Tool                              | Zweck                          |
| --------------------------------- | ------------------------------ |
| **SET (Social Engineer Toolkit)** | Phishing, Cloning, etc.        |
| **GoPhish**                       | Phishing Kampagnen             |
| **BeEF**                          | Browser Exploitation Framework |
| **King Phisher**                  | Spear-Phishing Tool            |
| **Maltego**                       | OSINT und Personenanalyse      |

---

## 1️⃣4️⃣ Systemdienste (für Enumeration & Missbrauch)

| Service                | Tools                               |
| ---------------------- | ----------------------------------- |
| **SMB**                | enum4linux, smbclient, CrackMapExec |
| **LDAP / AD**          | ldapsearch, BloodHound, PowerView   |
| **RDP / VNC**          | xfreerdp, rdesktop, ncrack          |
| **SNMP**               | snmpwalk, onesixtyone               |
| **FTP / SSH / Telnet** | hydra, medusa, netcat               |

---

**Letzte Aktualisierung:** Juli 2025  
Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!

