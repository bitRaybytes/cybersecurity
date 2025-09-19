# 🛰️ Nmap Cheatsheet – Netzwerk- und Portscanner

> Nmap ("Network Mapper") ist ein freier Open-Source-Scanner zur Netzwerkerkundung und Sicherheitsprüfung.  
> ✅ Ideal für Reconnaissance beim Pentesting.




## Inhaltsverzeichnis
- [Installation](#installation)
- [Grundlagen](#grundlagen)
- [Port-Scans](#port-scans)
- [Service & OS-Erkennung](#service--os-erkennung)
- [Scan-Typen (Stealth, TCP, UDP …)](#scan-typen-stealth-tcp-udp-usw)
- [NSE (Nmap Scripting Engine)](#nse-nmap-scripting-engine)
- [Beispiele](#beispiele)
- [Output speichern](#output-speichern)
- [Weitere Tools](#weitere-tools)
- [Ressourcen](#ressourcen)
- [Haftungsausschluss](#haftungsausschluss)





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Installation

**Linux (Debian/Ubuntu):**
```bash
sudo apt update && sudo apt install nmap
```

***Hinweis:*** Unter Kali ist `nmap` bereits vorinstalliert.

**macOS (Homebrew):**
```bash
brew install nmap
```
**Windows:**
➡ [Download von der offiziellen Webseite](https://nmap.org/download.html)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundlagen

| Befehl                 | Beschreibung                           |
| ---------------------- | -------------------------------------- |
| `nmap -h`              | Help Seite von `nmap`                  |
| `nmap -V`              | Version und Infos über `nmap`          |
| `nmap <IP>`            | Standardscan der Ziel-IP               |
| `nmap <IP1> <IP2>`     | Scan mehrerer Ziele                    |
| `nmap 192.168.0.1-50`  | IP-Range scannen                       |
| `nmap -iL targets.txt` | Scan aus Liste von IPs                 |
| `nmap -v <Ziel>`       | Verbose-Modus (detailliertere Ausgabe) |
| `nmap -vv <Ziel>`      | detailliertere Ausgabe als `-v`        |
| `nmap -n <Ziel>`       | Kein DNS-Lookup (schneller)            |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Port-Scans

| Befehl                        | Beschreibung                |
| ----------------------------- | --------------------------- |
| `nmap -p 80,443 <Ziel>`       | Nur bestimmte Ports scannen |
| `nmap -p- <Ziel>`             | Alle 65535 Ports scannen    |
| `nmap --top-ports 100 <Ziel>` | Die 100 häufigsten Ports    |
| `nmap -F <Ziel>`              | Fast Scan (Standardports)   |

Weitere [Nmap Port Scan Techniken](https://nmap.org/book/man-port-scanning-techniques.html).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Service & OS-Erkennung

| Befehl            | Beschreibung                                    |
| ----------------- | ----------------------------------------------- |
| `nmap -sV <Ziel>` | Services und Versionen erkennen                 |
| `nmap -O <Ziel>`  | Betriebssystem-Erkennung                        |
| `nmap -A <Ziel>`  | Aggressiv: OS + Services + Skripte + Traceroute |
| `nmap -sC <Ziel>` | Default NSE-Scripts ausführen                   |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Scan-Typen (Stealth, TCP, UDP, usw.)

| Befehl            | Beschreibung                                 |
| ----------------- | -------------------------------------------- |
| `nmap -sS <Ziel>` | Stealth-Scan (SYN, "Half-Open") (root nötig) |
| `nmap -sT <Ziel>` | TCP-Connect-Scan (kein root nötig)           |
| `nmap -sU <Ziel>` | UDP-Scan                                     |
| `nmap -sN <Ziel>` | Null-Scan (keine Flags)                      |
| `nmap -sX <Ziel>` | Xmas-Scan (alle Flags)                       |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## NSE (Nmap Scripting Engine)

| Befehl                            | Beschreibung                           |
| --------------------------------- | -------------------------------------- |
| `nmap --script help`              | Hilfe zu allen Skripten anzeigen       |
| `nmap --script vuln <Ziel>`       | Verwundbarkeiten testen                |
| `nmap --script smb* <Ziel>`       | Alle SMB-Related-Skripte               |
| `nmap --script http-title <Ziel>` | Seitentitel von Webservern extrahieren |
| `nmap --script default <Ziel>`    | Default-Skripte (häufig nützlich)      |
| `nmap --script exploit <Ziel>`    | Exploit-Angriffe                       |

Weitere NSE-Befehle gibt es auf der offiziellen Homepage von `nmap`: [https://nmap.org/book/nse-usage.html](https://nmap.org/book/nse-usage.html).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Beispiele

**Alle Ports mit Versionsscan:**
`nmap -sV -p- <Ziel>`

**OS- und Service-Erkennung + aggressive NSE-Skripte:**
`nmap -A <Ziel>`

**UDP und TCP kombiniert:**
`nmap -sS -sU -p U:53,161,T:22,80 <Ziel>`

**Scan auf CVEs (Vuln Scripts):**
`nmap --script vuln <Ziel>`




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Output speichern

| Befehl            | Beschreibung                                  |
| ----------------- | --------------------------------------------- |
| `-oN datei.txt`   | Normaler Text-Output                          |
| `-oX datei.xml`   | XML-Output (z. B. für Tools wie Zenmap)       |
| `-oG datei.gnmap` | Grepable Output                               |
| `-oA scan`        | Alle Formate gleichzeitig (`scan.nmap`, etc.) |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Weitere Tools

| Tool       | Funktion                            |
| ---------- | ----------------------------------- |
| `Zenmap`   | GUI für Nmap                        |
| `rustscan` | Superschneller Portscanner          |
| `masscan`  | Ultra-fast Scan ganzer Netzbereiche |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Ressourcen

- 🔗 [NMAP Manpage](https://nmap.org/book/man.html)
- 🌐 [NMAP Offizielle Webseite](https://nmap.org/)
- [YouTube: NMAP Full Guide, Hacker Joe](https://www.youtube.com/watch?v=JHAMj2vN2oU)



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