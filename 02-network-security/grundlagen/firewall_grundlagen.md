# 🔥 Firewall – Cheat Sheet



## Inhaltsverzeichnis

- [Firewall-Definition](#firewall-definition)
- [Firewall-Regeln](#firewall-regeln)
- [Firewall-Arten](#firewall-arten)
- [Vergleich nach Schichten](#vergleich-nach-schichten)
- [Wichtige Hinweise und Tools](#wichtige-hinweise-und-tools)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Firewall-Definition
Eine **Firewall** schützt Netzwerke und Systeme vor unautorisierten Zugriffen, indem sie den Datenverkehr nach definierten Regeln filtert. Sie agiert als Barriere und erlaubt nur die **erlaubte Kommunikation** (gemäß den Sicherheitsrichtlinien).

```text
     Internet              |        Internes Netzwerk
     (Unsichere Zone)      |        (Sichere Zone)
                           |
     +-----------+         |        +-------------+
     | Angreifer |         |        | Webserver   |
     +-----------+         |        +-------------+
            |              |              |
            v              |              v
       +---------+         |        +-------------+
       |         |         |        | Client-PC   |
       |  (x)    |<--[ Firewall ]-->|             |
       |         |         |        |    (✓)      |
       +---------+         |        +-------------+
       |         |         |
       |         |         |  

```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Firewall-Regeln
Regelwerke sind das Herzstück jeder Firewall. Sie bestimmen, welcher Datenverkehr basierend auf bestimmten Kriterien zugelassen (`ALLOW`) oder blockiert (`DENY`) wird.

### Kriterien für Regelwerke
  - **Quelladresse / Quellport:** Woher kommt der Verkehr? (z. B. IP-Adressen, Netzwerkbereiche) 
  - **Zieladresse / Zielport:** Wohin geht der Verkehr? (z. B. Server, Dienste, Ports wie 80/443 für HTTP/HTTPS)  
  - **Protokoll:** Welches Protokoll wird verwendet? (TCP, UDP, ICMP usw.)
  - **Richtung:** Ist der Verkehr eingehend (`Inbound`) oder ausgehend (`Outbound`)?
  - **Whitelist:** Nur bekannte Geräte sind berechtigt.
  - **Blacklist:** Alles in der Liste wird geblockt.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Practice
  - **Default Deny:** Standardmäßig wird **alles verboten**, was nicht explizit in einer Regel erlaubt ist. Dies ist das sicherste Vorgehen.
  - **Prinzip des geringsten Privilegs:** Gewähre nur die minimal notwendigen Zugriffsrechte.
  - Regeln von **spezifisch zu allgemein** ordnen  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Firewall-Arten

### 1. Paketfilter-Firewall (Stateless Packet Filtering)
- **Funktionsweise:**
  - Arbeitet auf OSI-Layer 3 (Netzwerk) und Layer 4 (Transport). Sie analysiert nur die Header-Informationen eines IP-Pakets, ohne den Kontext einer Verbindung zu kennen.
  - Kriterien: Quell-/Ziel-IP, Protokoll (TCP/UDP), Port.
  - Keine Zustandsprüfung (keine Session-Verwaltung).
- **Beispiele:** Access Control Lists (ACLs) auf Routern.
- **Vorteile:** 
  - Sehr schnell, geringer Ressourcenverbrauch.
- **Nachteile:**
  - Kennt Verbindungszustände nicht prüfen.
  - Anfällig für Spoofing-Angriffe.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Stateful Packet Inspection (SPI)
- **Funktionsweise:**  
  - Prüft nicht nur Header, sondern auch den **Verbindungszustand**.  
  - Nutzt eine **Connection Table** (Status-Tabelle) mit Zuständen wie `ESTABLISHED`, `NEW`, `RELATED`.  
  - Erlaubt Rückantworten nur zu legitimen Verbindungen (z. B. TCP-Handshake).  
- **Vorteile:**
  - Bessere Sicherheit als reine Paketfilter, Schutz vor Angriffen wie SYN-Flooding   
- **Nachteile:**
  - Kein inhaltliche Analyse des Datenstroms (kein Schutz auf Layer 7).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Application Firewall (Proxy-Firewall)
- **Funktionsweise:**
  - Arbeitet auf **OSI-Layer 7 (Anwendungsebene)**.  
  - Übernimmt Verbindungsaufbau stellvertretend (Proxy-Funktion).  
  - Kann Inhalte analysieren: z. B. HTTP-Header, FTP-Befehle  
- **Einsatzbereiche:**
  - URL-Filterung, Virenscanning, Content-Filter  
  - Unternehmensumgebungen mit granularen Policies  
- **Vorteile:**
  - Tiefere Inhaltskontrolle  
  - Schutz vor Protokoll-Missbrauch  
- **Nachteile:**
  - Höhere Latenz & Ressourcenverbrauch  
  - Komplexere Konfiguration  




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Next Generation Firewall (NGFW)
- **Funktionsweise:**  
  - Kombination klassischer Firewall-Mechanismen mit modernen Sicherheitsfeatures.
- **Typische Merkmale:**  
  - **Deep Packet Inspection (DPI):**  Analyse des Payloads, auch in verschlüsseltem Verkehr (SSL/TLS-Inspection).  
  - **Application Awareness:** Erkennt Anwendungen unabhängig von den verwendeten Ports. 
  - **IDS/IPS:** Erkennt und blockiert bekannte Angriffe (z. B. SQL-Injections).  
  - **Content Filtering:** -> blockiert Malware, Phishing, unerwünschte Webseiten.  
- **Beispiele:**  
  - Palo Alto, Fortinet, Cisco Firepower
- **Vorteile:**  
  - Ganzheitlicher Schutz (Netzwerk + Anwendung)  
  - Geeignet für moderne Bedrohungen  
- **Nachteile:** 
  - Teurer & komplexer  
  - SSL-Inspection kann Datenschutzprobleme verursachen  



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vergleich nach Schichten

```text
+----------------+--------------------+--------------------------+
| OSI-Layer      | Firewall-Typ       | Beispiel                 |
+================+====================+==========================+
|  7             | Application/Proxy  | Web Application Firewall |
| (Anwendung)    |                    | (WAF)                    |
+----------------+--------------------+--------------------------+
|  6             |                    | (entfällt)               |
| (Präsentation) |                    |                          |
+----------------+--------------------+--------------------------+
|  5             | Stateful           | Cisco ASA                |
| (Session)      |                    |                          |
+----------------+--------------------+--------------------------+
|  4             | Stateful / Paket   |                          |
| (Transport)    |                    | Router-ACLs              |
+----------------+--------------------+--------------------------+
|  3             | Paketfilter        |                          |
| (Netzwerk)     |                    |                          |
+----------------+--------------------+--------------------------+
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wichtige Hinweise und Tools
- **Layer 3–4 Firewalls** sind schnell, aber bieten nur grundlegenden Schutz. 
- **Layer 7 Firewalls** sind granular, aber langsamer und komplexer.
- **Hybrid-Ansätze (NGFW)** sind heute Standard in Unternehmen und bieten einen ganzheitlichen Schutz.
- **Firewalls allein reichen nicht:** Sie sollten immer mit anderen Sicherheitslösungen wie IDS/IPS, Logging und SIEM kombiniert werden.  
- **Praktische Tools:**
  - `iptables`/`nftables` (Linux) zur Konfiguration der netfiler-Firewall.
  - `ufw` (Uncomplicated Firewall) für eine einfachere Verwaltung.



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

📅 **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
