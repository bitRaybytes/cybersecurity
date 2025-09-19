# 🛠️ Penetration Testing Tool-Kit Cheat Sheet

> Tools & Techniken für alle Phasen eines Penetrationstests (Red Team Fokus).
> Es sind selbstverständlich nur einige der vorhandenen Tools für offensive Tests.

## Inhaltsverzeichnis

1. [Informationsbeschaffung (Reconnaissance)](#informationsbeschaffung-reconnaissance)
2. [Vulnerability Analyse](#vulnerability-analyse)
3. [Web Application Analyse](#web-application-analyse)
4. [Datenbank Assessement](#datenbank-assessment)
5. [Password Attacks](#password-attacks)
6. [Wireless Attacks](#wireless-attacks)
7. [Reverse Engineering](#reverse-engineering)
8. [Exploitation Tools](#exploitation-tools)
9. [Sniffing & Spoofing](#sniffing--spoofing)
10. [Post Exploitation](#post-exploitation)
11. [Forensics](#forensik--log-analyse)
12. [Reporting Tools](#reporting-tools)
13. [Social Engineering Tools](#social-engineering-tools)
14. [Systemdienste (für Enumeration & Missbrauch) ](#systemdienste-für-enumeration--missbrauch)
15. [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Informationsbeschaffung (Reconnaissance)

| Tool             | Zweck                                       |
| ---------------- | ------------------------------------------- |
| **Nmap**         | Netzwerk-Scan, Host Discovery, OS Detection |
| **Recon-ng**     | Web-basierte Informationsbeschaffung        |
| **theHarvester** | Emails, Hosts, Namen via OSINT              |
| **Maltego**      | Graphische OSINT-Analyse                    |
| **Shodan**       | IoT-/Internet-Geräte-Suchmaschine           |
| **Amass**        | Subdomain Enumeration                       |
| **FOCA**         | Metadaten in Dokumenten finden              |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Vulnerability Analyse

| Tool                            | Zweck                                |
| ------------------------------- | ------------------------------------ |
| **Nessus**                      | Kommerzieller Schwachstellenscanner  |
| **OpenVAS**                     | Open-Source-Schwachstellenscanner    |
| **Nikto**                       | Webserver-Schwachstellen finden      |
| **LinEnum / LinPEAS / WinPEAS** | Lokale Privilege Escalation Checks   |
| **Nmap NSE Scripts**            | Sicherheitslücken testen per Skripte |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Web Application Analyse

| Tool                       | Zweck                                  |
| -------------------------- | -------------------------------------- |
| **Burp Suite**             | Web Proxy, Scanner, Repeater, Intruder |
| **OWASP ZAP**              | Web-Schwachstellenscanner              |
| **Wfuzz**                  | Fuzzing von Parametern & Pfaden        |
| **SQLmap**                 | Automatisierte SQL-Injection           |
| **Dirb / Gobuster / FFUF** | Verzeichnis-Bruteforce                 |
| **Nikto**                  | Webserver-Analyse                      |
| **XSStrike**               | XSS-Finder & Exploiter                 |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Datenbank Assessment

| Tool                        | Zweck                                    |
| --------------------------- | ---------------------------------------- |
| **SQLmap**                  | Automatisierte SQLi-Erkennung & Exploits |
| **DBPwAudit**               | Datenbankpasswort-Audit                  |
| **Nmap NSE (mysql, mssql)** | Datenbankspezifische Tests               |
| **Metasploit**              | Datenbankmodule                          |





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Password Attacks

| Tool                | Zweck                               |
| ------------------- | ----------------------------------- |
| **John the Ripper** | Offline Passwort-Cracking           |
| **Hashcat**         | GPU-basiertes Passwortknacken       |
| **Hydra**           | Brute-Force über Netzwerkprotokolle |
| **Medusa**          | Schneller Passworttester            |
| **CeWL**            | Passwörter aus Webseiten generieren |
| **Crunch**          | Passwortlisten generieren           |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wireless Attacks

| Tool            | Zweck                            |
| --------------- | -------------------------------- |
| **Aircrack-ng** | WPA/WEP-Cracking, Sniffing       |
| **Kismet**      | Wireless Network Detector        |
| **Wireshark**   | WLAN-Paketanalyse                |
| **Wifite**      | Automatisierte Angriffe auf WLAN |
| **Bettercap**   | BLE/WiFi-Angriffe                |
| **Reaver**      | WPS-Angriffe                     |

 

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Reverse Engineering

| Tool                 | Zweck                           |
| -------------------- | ------------------------------- |
| **Ghidra**           | Disassembler & Decompiler (NSA) |
| **IDA Free**         | Interaktiver Disassembler       |
| **Radare2**          | Reverse Engineering Framework   |
| **Binary Ninja**     | Kommerziell, modern             |
| **Cutter**           | GUI für Radare2                 |
| **x64dbg / OllyDbg** | Windows-Debugging               |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Exploitation Tools

| Tool                      | Zweck                               |
| ------------------------- | ----------------------------------- |
| **Metasploit Framework**  | Exploits, Payloads, Sessions        |
| **ExploitDB**             | Exploit-Datenbank                   |
| **Searchsploit**          | CLI für ExploitDB                   |
| **MSFvenom**              | Payload-Generator                   |
| **Impacket**              | SMB/NTLM-Tools für Lateral Movement |
| **PowerSploit / Nishang** | PowerShell-basierte Exploits        |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sniffing & Spoofing

| Tool          | Zweck                        |
| ------------- | ---------------------------- |
| **Wireshark** | Paket-Sniffing               |
| **Ettercap**  | Man-in-the-Middle, Spoofing  |
| **Bettercap** | Advanced Spoofing & Sniffing |
| **ARPspoof**  | ARP-Spoofing                 |
| **Responder** | LLMNR/NBTNS Poisoning        |
| **tcpdump**   | CLI-Sniffer                  |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Post Exploitation

| Tool                 | Zweck                                |
| -------------------- | ------------------------------------ |
| **Meterpreter**      | Metasploit Payload (In-Memory Shell) |
| **Empire / Faction** | C2-Frameworks mit PowerShell         |
| **BloodHound**       | AD-Recon & Attack Path Mapping       |
| **Mimikatz**         | Windows-Credentials auslesen         |
| **PowerView**        | AD-Datenabfrage via PowerShell       |
| **SharpHound**       | Daten für BloodHound sammeln         |

 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Forensik & Log-Analyse

| Tool                     | Zweck                     |
| ------------------------ | ------------------------- |
| **Autopsy**              | GUI Forensik-Tool         |
| **Volatility**           | RAM-Analyse               |
| **Sleuth Kit**           | Filesystem Forensik       |
| **Plaso / log2timeline** | Zeitleistenanalyse        |
| **binwalk**              | Binäranalyse von Firmware |
| **ExifTool**             | Metadaten auslesen        |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Reporting Tools

| Tool                   | Zweck                                 |
| ---------------------- | ------------------------------------- |
| **CherryTree**         | Strukturierte Notizen & Reports       |
| **Dradis**             | Reporting & Kollaboration             |
| **MagicTree**          | Automatisierte Report-Zusammenfassung |
| **Faraday**            | Team-Reporting-Plattform              |
| **MS Word / Markdown** | Export von Ergebnissen                |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Social Engineering Tools

| Tool                              | Zweck                          |
| --------------------------------- | ------------------------------ |
| **SET (Social Engineer Toolkit)** | Phishing, Cloning, etc.        |
| **GoPhish**                       | Phishing Kampagnen             |
| **BeEF**                          | Browser Exploitation Framework |
| **King Phisher**                  | Spear-Phishing Tool            |
| **Maltego**                       | OSINT und Personenanalyse      |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Systemdienste (für Enumeration & Missbrauch)

| Service                | Tools                               |
| ---------------------- | ----------------------------------- |
| **SMB**                | enum4linux, smbclient, CrackMapExec |
| **LDAP / AD**          | ldapsearch, BloodHound, PowerView   |
| **RDP / VNC**          | xfreerdp, rdesktop, ncrack          |
| **SNMP**               | snmpwalk, onesixtyone               |
| **FTP / SSH / Telnet** | hydra, medusa, netcat               |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---