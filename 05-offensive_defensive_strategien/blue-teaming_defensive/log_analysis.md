# 📊 Log Analysis Guide

## Ziel dieser Datei

Diese Datei bietet eine Übersicht zum Thema **Log-Analyse** im Bereich Cybersecurity. Sie hilft bei der Erkennung von Angriffsmustern, der forensischen Nachverfolgung von Vorfällen und der allgemeinen Sicherheitsüberwachung von Systemen und Netzwerken.



## Inhaltsverzeichnis

- [1. Einführung in Log-Analyse](#1-einführung-in-log-analyse)
- [2. Wichtige Log-Typen](#2-wichtige-log-typen)
- [3. Tools zur Log-Analyse](#3-tools-zur-log-analyse)
- [4. Wichtige Begriffe & Felder](#4-wichtige-begriffe--felder)
- [5. Beispielhafte Angriffserkennung](#5-beispielhafte-angriffserkennung)
- [6. Zentrale Logformate](#6-zentrale-logformate)
- [7. Best Practices](#7-best-practices)
- [8. Nützliche Ressourcen](#8-nützliche-ressourcen)
- [Rechtlicher Hinweis](#rechtlicher-hinweis)
- [Haftungsausschluss](#haftungsausschluss)





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Einführung in Log-Analyse

Logfiles sind Textdateien, die Systemaktivitäten protokollieren. Die Analyse dieser Logs ist essenziell zur:

- Erkennung verdächtiger Aktivitäten
- Nachverfolgung von Angriffsvektoren
- Beweissicherung in der Forensik
- Erfüllung gesetzlicher Anforderungen




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 2. Wichtige Log-Typen

| Logtyp | Beschreibung |
|--------|--------------|
| **Syslog** | Systemereignisse unter Linux/Unix |
| **Windows Event Logs** | Sicherheits-, System- und Anwendungslogs |
| **Webserver Logs** | Zugriffe, Fehler und IP-Adressen |
| **Firewall Logs** | Erlaubte/Blockierte Verbindungen |
| **IDS/IPS Logs** | Intrusion Detection Events |
| **Auth Logs** | Login-Versuche, sudo, SSH etc. |
| **Application Logs** | Fehler und Aktivitäten aus Anwendungen |
| **SIEM Logs** | Korrelierte Ereignisse aus verschiedenen Quellen |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Tools zur Log-Analyse

- **ELK Stack (Elasticsearch + Logstash + Kibana)**
- **Graylog**
- **Splunk**
- **SIEM-Systeme (z. B. Wazuh, QRadar)**
- **Logwatch, GoAccess (CLI)**
- **Linux Tools:** `grep`, `awk`, `cut`, `sed`, `less`, `journalctl`




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 4. Wichtige Begriffe & Felder

- **Timestamp** – Zeitstempel des Events
- **Source IP** – Ursprungs-IP der Anfrage
- **Destination IP** – Zielsystem
- **User-Agent** – Client-Informationen
- **Method** – GET, POST etc.
- **Status-Code** – HTTP-Antwort (z. B. 404, 500)
- **Username** – Eingeloggter oder angefragter Benutzer
- **Process/Service** – Dienst oder ausführende Anwendung




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 5. Beispielhafte Angriffserkennung

### Brute-Force auf SSH (Linux `/var/log/auth.log`)
```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

### Webshell Detection (Apache)
```bash
grep "eval(" /var/log/apache2/access.log
grep ".phtml" /var/log/apache2/access.log
```

### Suspicious User-Agents
```bash
grep -i "curl\|python\|wget" /var/log/apache2/access.log
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Zentrale Logformate

### Apache Access Log
```swift
127.0.0.1 - - [10/Jul/2025:14:23:15 +0200] "GET /index.php HTTP/1.1" 200 2326 "-" "Mozilla/5.0"
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Auth Log (Linux)
```pgsql
Jul 10 12:42:22 hostname sshd[12345]: Failed password for invalid user admin from 192.168.1.50 port 2222 ssh2
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Windows Security Log (Evt/EVTX)
- Event ID 4624 – Erfolgreiche Anmeldung
- Event ID 4625 – Fehlgeschlagene Anmeldung
- Event ID 4688 – Prozess erstellt

 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 7. Best Practices
- Zentralisierung der Logs mit SIEM
- Regelmäßiges Monitoring und Alerts definieren
- Langzeitarchivierung für forensische Analyse
- Log Integrity sicherstellen (Hashing, Audit Logs)
- Automatisierte Regeln für verdächtige Muster (z. B. mit Sigma oder Splunk-Queries)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 8. Nützliche Ressourcen
- [logcheck.org](https://logcheck.org/)
- [wazuh.com](https://wazuh.com/) (Open Source Security Platform)
- [Linux Log Files Overview](https://www.loggly.com/ultimate-guide/linux-logging-basics/)
- [Sigma Rule GitHub](https://github.com/SigmaHQ/sigma)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Rechtlicher Hinweis
Die Analyse und Überwachung von Logs darf nur auf eigenen Systemen oder mit ausdrücklicher Genehmigung erfolgen. Logfiles können personenbezogene Daten enthalten und unterliegen ggf. Datenschutzregelungen.




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