# 🚪 Port Scanning – Grundlagen, Techniken & Tools

## 🧭 Was ist Port Scanning?

Port Scanning ist eine Methode, um **offene, geschlossene oder gefilterte Ports** auf einem Zielsystem zu identifizieren. Ziel ist es herauszufinden:
- Welche **Dienste** (z. B. Webserver, Datenbank) aktiv sind
- Welche **Ports** offen oder verwundbar sind
- Welche Sicherheitsmaßnahmen aktiv sind (z. B. Firewalls)

---

## ⚙️ Grundlagen: Ports und Protokolle

### Was ist ein Port?

- Ein **Port** ist eine virtuelle Schnittstelle, über die Netzwerkdienste kommunizieren.
- Jeder Port hat eine **Portnummer** (0–65535)

### Portbereiche

| Bereich              | Nummern          | Beschreibung                       |
|----------------------|------------------|------------------------------------|
| Well-Known Ports     | 0–1023           | Reserviert für Standarddienste     |
| Registered Ports     | 1024–49151       | Für benutzerdefinierte Dienste     |
| Dynamic/Private Ports| 49152–65535      | Für temporäre Verbindungen         |

### Wichtige Ports

| Dienst      | Port | Protokoll |
|-------------|------|-----------|
| HTTP        | 80   | TCP       |
| HTTPS       | 443  | TCP       |
| SSH         | 22   | TCP       |
| FTP         | 21   | TCP       |
| DNS         | 53   | UDP/TCP   |
| SMTP        | 25   | TCP       |
| RDP         | 3389 | TCP       |

---

## 🔍 Arten von Port Scans

| Scan-Typ        | Beschreibung                                          | Detektion möglich? |
|------------------|------------------------------------------------------|---------------------|
| TCP Connect Scan | Vollständiger TCP-Handshake (3-Way)                  | Ja (am auffälligsten) |
| SYN Scan         | Nur SYN → SYN/ACK → RST (kein vollständiger Handshake) | Teilweise (stealthy) |
| UDP Scan         | Sendet UDP-Pakete an Zielport, Antwortanalyse        | Ja/Nein (langsamer, unzuverlässiger) |
| Xmas Scan        | TCP-Flags FIN, URG, PSH gesetzt → Reaktion prüfen    | Ja (nur auf manchen Systemen wirksam) |
| FIN Scan         | Sendet TCP-Paket mit FIN-Flag                        | Ja/Nein             |
| NULL Scan        | Kein gesetztes TCP-Flag → Fehlerantwort provozieren  | Teilweise           |

---

## 🧪 Tool: Nmap – Der Klassiker

**Nmap (Network Mapper)** ist das bekannteste Tool für Netzwerkerkennung und Port Scanning.

### 🔧 Installation

- **Linux/Debian:** `sudo apt install nmap`
- **Windows:** [nmap.org](https://nmap.org/download.html)

### 📋 Beispiele

```bash
# Alle 1000 Standard-Ports scannen
nmap 192.168.1.10

# TCP-SYN-Scan (stealthy)
nmap -sS 192.168.1.10

# UDP-Scan
nmap -sU 192.168.1.10

# Alle Ports (0-65535)
nmap -p- 192.168.1.10

# Betriebssystemerkennung + Services
nmap -A 192.168.1.10

# Nur bestimmte Ports scannen (z. B. SSH & HTTP)
nmap -p 22,80 192.168.1.10

# Subnetz scannen
nmap -sP 192.168.1.0/24
```

---

## 🧱 Scan-Ergebnisse verstehen

| Status         | Bedeutung                                         |
| -------------- | ------------------------------------------------- |
| **open**       | Ein Dienst antwortet aktiv auf dem Port           |
| **closed**     | Port ist erreichbar, aber kein Dienst antwortet   |
| **filtered**   | Firewall oder Filter blockiert die Anfrage        |
| **unfiltered** | Port erreichbar, aber keine klare Aussage möglich |


---

## 🧑‍⚖️ Rechtliche Hinweise ⚠️

> Port Scanning ist in vielen Ländern ohne Erlaubnis illegal!

### Grundregel:

> Immer nur in Testumgebungen, Laboren oder mit schriftlicher Genehmigung des Besitzers!

In Deutschland kann ein nicht autorisierter Scan gegen §202c StGB ("Vorbereiten des Ausspähens von Daten") verstoßen.

---

## 🔐 Verteidigung gegen Port Scans (Blue Team)

- Firewall einsetzen: Blockiert unnötige Ports (z. B. nur Port 443 offen lassen)
- IDS/IPS: Tools wie Snort oder Suricata erkennen Scans
- Port-Knocking: Dienste nur nach „geheimer Portfolge“ öffnen
- Honeypots: Absichtlich offene Ports zur Täuschung von Angreifern

---

## 🧠 Lern-Tipps
Empfohlene Ressourcen:

- Plattformen: TryHackMe („Nmap Room“), HackTheBox, Offensive Security Labs
- YouTube: NetworkChuck – „Nmap Tutorial for Beginners“
- Bücher: „Nmap Network Scanning“ (Gordon Lyon aka Fyodor)

---

## Checkliste für Port Scanner:

✅ Zielsystem identifizieren
✅ Scantyp wählen
✅ Ports definieren
✅ Firewalls/Filter beachten
✅ Ergebnisse dokumentieren

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