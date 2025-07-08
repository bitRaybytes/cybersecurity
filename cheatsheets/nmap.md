# 🛰️ Nmap Cheatsheet – Netzwerk- und Portscanner

> Nmap ("Network Mapper") ist ein freier Open-Source-Scanner zur Netzwerkerkundung und Sicherheitsprüfung.  
> ✅ Ideal für Reconnaissance beim Pentesting.

---

## 📦 Installation

**Linux (Debian/Ubuntu):**
```bash
sudo apt update && sudo apt install nmap
```
**macOS (Homebrew):**
```bash
brew install nmap
```
**Windows:**
➡ [Download von der offiziellen Webseite](https://nmap.org/download.html)

---

## 🔍 Grundlagen

| Befehl                 | Beschreibung                           |
| ---------------------- | -------------------------------------- |
| `nmap <IP>`            | Standardscan der Ziel-IP               |
| `nmap <IP1> <IP2>`     | Scan mehrerer Ziele                    |
| `nmap 192.168.0.1-50`  | IP-Range scannen                       |
| `nmap -iL targets.txt` | Scan aus Liste von IPs                 |
| `nmap -v <Ziel>`       | Verbose-Modus (detailliertere Ausgabe) |
| `nmap -n <Ziel>`       | Kein DNS-Lookup (schneller)            |

---

## 📡 Port-Scans
| Befehl                        | Beschreibung                |
| ----------------------------- | --------------------------- |
| `nmap -p 80,443 <Ziel>`       | Nur bestimmte Ports scannen |
| `nmap -p- <Ziel>`             | Alle 65535 Ports scannen    |
| `nmap --top-ports 100 <Ziel>` | Die 100 häufigsten Ports    |
| `nmap -F <Ziel>`              | Fast Scan (Standardports)   |

---

## 🕵️‍♂️ Service & OS-Erkennung

| Befehl            | Beschreibung                                    |
| ----------------- | ----------------------------------------------- |
| `nmap -sV <Ziel>` | Services und Versionen erkennen                 |
| `nmap -O <Ziel>`  | Betriebssystem-Erkennung                        |
| `nmap -A <Ziel>`  | Aggressiv: OS + Services + Skripte + Traceroute |
| `nmap -sC <Ziel>` | Default NSE-Scripts ausführen                   |

---

## ⚙️ Scan-Typen (Stealth, TCP, UDP …)

| Befehl            | Beschreibung                                 |
| ----------------- | -------------------------------------------- |
| `nmap -sS <Ziel>` | Stealth-Scan (SYN, "Half-Open") (root nötig) |
| `nmap -sT <Ziel>` | TCP-Connect-Scan (kein root nötig)           |
| `nmap -sU <Ziel>` | UDP-Scan                                     |
| `nmap -sN <Ziel>` | Null-Scan (keine Flags)                      |
| `nmap -sX <Ziel>` | Xmas-Scan (alle Flags)                       |

---

## 🧠 NSE (Nmap Scripting Engine)

| Befehl                            | Beschreibung                           |
| --------------------------------- | -------------------------------------- |
| `nmap --script help`              | Hilfe zu allen Skripten anzeigen       |
| `nmap --script vuln <Ziel>`       | Verwundbarkeiten testen                |
| `nmap --script smb* <Ziel>`       | Alle SMB-Related-Skripte               |
| `nmap --script http-title <Ziel>` | Seitentitel von Webservern extrahieren |
| `nmap --script default <Ziel>`    | Default-Skripte (häufig nützlich)      |

---

## Beispiele

**Alle Ports mit Versionsscan:**
`nmap -sV -p- <Ziel>`

**OS- und Service-Erkennung + aggressive NSE-Skripte**
`nmap -A <Ziel>`

**UDP und TCP kombiniert:**
`nmap -sS -sU -p U:53,161,T:22,80 <Ziel>`

**Scan auf CVEs (Vuln Scripts)**
`nmap --script vuln <Ziel>`

---

## 📝 Output speichern

| Befehl            | Beschreibung                                  |
| ----------------- | --------------------------------------------- |
| `-oN datei.txt`   | Normaler Text-Output                          |
| `-oX datei.xml`   | XML-Output (z. B. für Tools wie Zenmap)       |
| `-oG datei.gnmap` | Grepable Output                               |
| `-oA scan`        | Alle Formate gleichzeitig (`scan.nmap`, etc.) |

---

## 🎯 Weitere Tools

| Tool       | Funktion                            |
| ---------- | ----------------------------------- |
| `Zenmap`   | GUI für Nmap                        |
| `rustscan` | Superschneller Portscanner          |
| `masscan`  | Ultra-fast Scan ganzer Netzbereiche |

---

## 📚 Ressourcen

- 🔗 [NMAP Manpage](https://nmap.org/book/man.html)
- 🌐 [NMAP Offizielle Webseite](https://nmap.org/)

---

**Letzte Aktualisierung:** Juli 2025  
Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!