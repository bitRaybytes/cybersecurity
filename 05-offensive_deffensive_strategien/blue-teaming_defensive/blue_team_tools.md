# 🛡️ Blue Team Tool-Kit Cheat Sheet

> Tools & Techniken für die Verteidigung, Erkennung, Forensik & Reaktion auf Angriffe.  

---

## Inhaltsverzeichnis

1. [Asset Discovery & Network Mapping](#asset-discovery--network-mapping)
2. [Vulnerability Management](#vulnerability-management)
3. [Patch Management](#patch-management)
4. [Endpoint Detection & Response (EDR)](#endpoint-detection--response-edr)
5. [Security Information & Event Management (SIEM)](#security-information--event-management-siem)
6. [Intrusion Detection & Prevention (IDS / IPS)](#intrusion-detection--prevention-ids--ips)
7. [Log Management & Monitoring](#log-management--monitoring)
8. [Threat Intelligence](#threat-intelligence)
9. [Vulnerability Intelligence & Attack Surface Management](#vulnerability-intelligence--attack-surface-management)
10. [Digital Forensics & Incident Response (DFIR)](#digital-forensics--incident-response-dfir)
11. [Malware Analysis](#malware-analysis)
12. [Email Security & Phishing Defense](#email-security--phishing-defense)
13. [Firewall & Network Security](#firewall--network-security)
14. [Awareness & Simulation](#awareness--simulation)
15. [Backup & Recovery](#backup--recovery)
16. [Empfehlungen](#empfehlungen)
17. [Haftungsausschluss](#haftungsausschluss)

---

## Asset Discovery & Network Mapping

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Nmap**          | Netzwerk-Scan, Service-Erkennung           |
| **NetBox**        | Netzwerk-Inventarisierung                  |
| **Spiceworks**    | IT-Asset-Management                        |
| **Lansweeper**    | Netzwerkgeräte-Discovery                   |
| **Open-AudIT**    | Automatische Inventarisierung              |

---

## Vulnerability Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Nessus**        | Schwachstellenscanner                      |
| **OpenVAS**       | Open-Source-Scanner                        |
| **Qualys**        | Cloud-basierte Schwachstellenverwaltung    |
| **Rapid7 InsightVM** | Risikobasiertes VM-Management           |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Patch Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **WSUS**          | Windows-Patch-Verteilung                   |
| **PDQ Deploy**    | Softwareverteilung & Patchen               |
| **Ansible / Puppet / Chef** | Automatisierte Updates           |
| **ManageEngine Patch Manager Plus** | Patchverwaltung         |

---

## Endpoint Detection & Response (EDR)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **CrowdStrike Falcon** | Cloud-EDR-Plattform                   |
| **Microsoft Defender for Endpoint** | Windows-EDR              |
| **Elastic EDR**   | Open-Source EDR (Elastic Stack)            |
| **SentinelOne**   | KI-gestützte Bedrohungserkennung           |

---

## Security Information & Event Management (SIEM)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Splunk**        | Ereignisanalyse & Korrelationsregeln       |
| **ELK Stack (Elasticsearch, Logstash, Kibana)** | Logmanagement |
| **Graylog**       | Open-Source SIEM                           |
| **Wazuh**         | Hostüberwachung + SIEM-Funktionalität      |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Intrusion Detection & Prevention (IDS / IPS)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Snort**         | Netzwerkbasiertes IDS                      |
| **Suricata**      | IDS/IPS mit hoher Leistung                 |
| **Zeek (Bro)**    | Netzwerküberwachung & Analyse              |
| **Security Onion**| IDS/NSM-Plattform mit Snort, Suricata etc. |

---

## Log Management & Monitoring

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Syslog-ng / Rsyslog** | Zentrale Logaufnahme                |
| **Logwatch**      | Tägliche Log-Reports                       |
| **Kibana**        | Visualisierung von Logs                    |
| **Fluentd / Logstash** | Logaggregation                       |

---

## Threat Intelligence

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **MISP**          | Threat Sharing Plattform                   |
| **AlienVault OTX**| Open Threat Exchange                       |
| **Cisco Talos**   | Bedrohungsdatenbank                        |
| **VirusTotal**    | Dateianalyse & Reputation                  |
| **AbuseIPDB**     | IP-Reputationsdatenbank                    |

---

## Vulnerability Intelligence & Attack Surface Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Shodan Monitor**| Externe Angriffsfläche überwachen          |
| **Attack Surface Analyzer** | Microsoft Tool zur Analyse       |
| **Censys**        | Angriffsflächen-Scan                       |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Digital Forensics & Incident Response (DFIR)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Velociraptor**  | Endpoint-DFIR                              |
| **Autopsy**       | Forensik-GUI                               |
| **Volatility**    | RAM-Analyse                                |
| **The Sleuth Kit**| Datei-Forensik                             |
| **GRR Rapid Response** | Remote Incident Response              |

---

## Malware Analysis

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Cuckoo Sandbox**| Dynamische Malwareanalyse                  |
| **Any.Run**       | Online Sandbox                             |
| **REMnux**        | Malware-Analyse-Linux-Toolkit              |
| **IDA Free / Ghidra** | Reverse Engineering                    |

---

## Email Security & Phishing Defense

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Proofpoint**    | Anti-Phishing & Email Security             |
| **Mimecast**      | Mailfilter & Schutz                        |
| **MailCleaner**   | Open-Source Gateway                        |
| **PhishTool**     | Analyse von verdächtigen E-Mails           |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Firewall & Network Security

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **pfSense / OPNsense** | Open-Source Firewalls                |
| **iptables / nftables** | Paketfilter für Linux                |
| **Cisco ASA / Fortinet** | Kommerzielle Firewalls              |

---

## Awareness & Simulation

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Gophish**       | Phishing-Simulation                        |
| **KnowBe4**       | Security-Awareness-Training                |
| **PhishSim**      | Benutzer-Schulung                          |

---

## Backup & Recovery

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Veeam**         | Backup & Wiederherstellung                 |
| **BorgBackup**    | Open-Source Backup                         |
| **Restic**        | Schnelles Backup CLI                       |
| **Rsync / Duplicity** | Scriptbare Backuplösungen              |

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Empfehlungen

- Nutze **Wazuh + ELK + Suricata** für eine Open-Source Blue-Team-Pipeline.
- Kombiniere mit **MISP** und **Velociraptor** für erweiterte Threat Detection & IR.

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