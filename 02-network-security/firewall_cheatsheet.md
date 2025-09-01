# 🔥 Firewall – Cheat Sheet

---

## Inhaltsverzeichnis

- [Firewall-Definition](#firewall-definition)
- [Firewall-Regeln](#firewall-regeln)
- [Firewall-Arten](#firewall-arten)
- [Vergleich nach Schichten](#vergleich-nach-schichten)
- [Wichtige Hinweise](#wichtige-hinweise)
- [Haftungsausschluss](#haftungsausschluss)

---

## Firewall-Definition
Eine **Firewall** schützt Netzwerke und Systeme vor unautorisierten Zugriffen, indem sie den Datenverkehr nach definierten Regeln filtert.  
Nur **erlaubte Kommunikation** (gemäß Sicherheitsrichtlinien) wird durchgelassen.

---

## Firewall-Regeln

**Kriterien für Regelwerke:**
- **Quelladresse / Quellport** -> z. B. IP-Adressen oder Netzwerkbereiche  
- **Zieladresse / Zielport** -> z. B. Server, Dienste oder Ports (80/443 für HTTP/HTTPS)  
- **Protokoll** -> TCP, UDP, ICMP usw.  
- **Richtung** -> eingehend (Inbound) oder ausgehend (Outbound)
- **Whitelist** -> Nur bekannte Geräte sind berechtigt
- **Blacklist** -> Alles in der Liste wird geblockt

👉 **Best Practice:**  
- **Default Deny** (alles verbieten, nur explizit erlaubtes zulassen)  
- Regeln von **spezifisch -> allgemein** ordnen  
- Dokumentation & regelmäßige Überprüfung  

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Firewall-Arten

### 1. Paketfilter-Firewall (Stateless Packet Filtering)
- **Funktionsweise:**  
  - Analysiert nur **Header-Informationen** eines IP-Pakets  
  - Kriterien: Quell-/Ziel-IP, Protokoll (TCP/UDP), Port  
  - Keine Zustandsprüfung (keine Session-Verwaltung)  
- **Beispiele:** Access Control Lists (ACLs) auf Routern  
- ✅ Vorteile:  
  - Sehr schnell, geringer Ressourcenverbrauch  
- ❌ Nachteile:  
  - Kennt Verbindungszustände nicht  
  - Anfällig für Spoofing / Replay-Angriffe  

---

### 2. Stateful Packet Inspection (SPI)
- **Funktionsweise:**  
  - Prüft nicht nur Header, sondern auch den **Verbindungszustand**  
  - Nutzt eine **Connection Table** (Status-Tabelle) mit Zuständen wie `ESTABLISHED`, `NEW`, `RELATED`  
  - Erlaubt Rückantworten nur zu legitimen Verbindungen (z. B. TCP-Handshake)  
- ✅ Vorteile:  
  - Bessere Sicherheit als reine Paketfilter, Schutz vor SYN-Flooding  
  - Erkennt gefälschte / unautorisierte Pakete  
- ❌ Nachteile:  
  - Kein inhaltliches Verständnis (kein Schutz auf Layer 7)  
  - Schwierigkeiten mit verschlüsseltem Traffic  

---

### 3. Application Firewall (Proxy-Firewall)
- **Funktionsweise:**  
  - Arbeitet auf **OSI-Layer 7 (Anwendungsebene)**  
  - Übernimmt Verbindungsaufbau stellvertretend (Proxy-Funktion)  
  - Kann Inhalte analysieren: z. B. HTTP-Header, FTP-Befehle  
- **Einsatzbereiche:**  
  - URL-Filterung, Virenscanning, Content-Filter  
  - Unternehmensumgebungen mit granularen Policies  
- ✅ Vorteile:  
  - Tiefere Inhaltskontrolle  
  - Schutz vor Protokoll-Missbrauch  
- ❌ Nachteile:  
  - Höhere Latenz & Ressourcenverbrauch  
  - Komplexere Konfiguration  

---

### 4. Next Generation Firewall (NGFW)
- **Funktionsweise:**  
  - Kombination klassischer Firewall-Mechanismen + moderne Sicherheitsfeatures  
- **Typische Merkmale:**  
  - 🔍 **Deep Packet Inspection (DPI)** -> Analyse über Header hinaus, inkl. SSL-Inspection  
  - 📡 **Application Awareness** -> erkennt Anwendungen unabhängig von Portnummern  
  - 🚨 **IDS/IPS** -> erkennt & blockiert Angriffe (SQLi, XSS, Exploits)  
  - 🛑 **Content Filtering** -> blockiert Malware, Phishing, unerwünschte Webseiten  
  - 🧪 **Sandboxing** (je nach Hersteller) -> verdächtige Dateien in isolierten Umgebungen ausführen  
- **Beispiele:**  
  - Palo Alto, Fortinet, Cisco Firepower, Check Point, Sophos XG  
- ✅ Vorteile:  
  - Ganzheitlicher Schutz (Netzwerk + Anwendung)  
  - Geeignet für moderne Bedrohungen  
- ❌ Nachteile:  
  - Teurer & komplexer  
  - SSL-Inspection kann Datenschutzprobleme verursachen  

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vergleich nach Schichten

| Firewall-Typ             | OSI-Layer | Prüfungsebene | Beispiele |
|---------------------------|-----------|---------------|-----------|
| Paketfilter (Stateless)   | 3–4       | IP / Port     | Router ACLs |
| Stateful Inspection       | 3–4       | Verbindungsstatus | Cisco ASA |
| Application Firewall      | 7         | Anwendungsebene | Proxy-Server |
| Next-Gen Firewall (NGFW)  | 3–7       | Header + Payload + App | Palo Alto, Fortinet |

---

## Wichtige Hinweise
- **Layer 3–4 Firewalls** -> schnell, aber weniger sicher  
- **Layer 7 Firewalls** -> granular, aber langsamer  
- **Hybrid-Ansätze (NGFW)** sind heute Standard in Unternehmen  
- Firewalls allein reichen nicht -> immer mit IDS/IPS, Logging und SIEM kombinieren  

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

📅 **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
