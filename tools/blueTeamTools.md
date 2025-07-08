# 🛡️ Blue Team Tool-Kit Cheat Sheet

> Tools & Techniken für die Verteidigung, Erkennung, Forensik & Reaktion auf Angriffe.  

---

## Inhaltsverzeichnis

1. [1️⃣ Asset Discovery & Network Mapping](#1️⃣-asset-discovery--network-mapping)
2. [2️⃣ Vulnerability Management](#2️⃣-vulnerability-management)
3. [3️⃣ Patch Management](#3️⃣-patch-management)
4. [4️⃣ Endpoint Detection & Response (EDR)](#4️⃣-endpoint-detection--response-edr)
5. [5️⃣ Security Information & Event Management (SIEM)](#5️⃣-security-information--event-management-siem)
6. [6️⃣ Intrusion Detection & Prevention (IDS / IPS)](#6️⃣-intrusion-detection--prevention-ids--ips)
7. [7️⃣ Log Management & Monitoring](#7️⃣-log-management--monitoring)
8. [8️⃣ Threat Intelligence](#8️⃣-threat-intelligence)
9. [9️⃣ Vulnerability Intelligence & Attack Surface Management](#9️⃣-vulnerability-intelligence--attack-surface-management)
10. [🔟 Digital Forensics & Incident Response (DFIR)](#-digital-forensics--incident-response-dfir)
11. [1️⃣1️⃣ Malware Analysis](#1️⃣1️⃣-malware-analysis)
12. [1️⃣2️⃣ Email Security & Phishing Defense](#1️⃣2️⃣-email-security--phishing-defense)
13. [1️⃣3️⃣ Firewall & Network Security](#1️⃣3️⃣-firewall--network-security)
14. [1️⃣4️⃣ Awareness & Simulation](#1️⃣4️⃣-awareness--simulation)
15. [1️⃣5️⃣ Backup & Recovery](#1️⃣5️⃣-backup--recovery)
16. [📋 Empfehlungen](#-empfehlungen)

---

## 1️⃣ Asset Discovery & Network Mapping

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Nmap**          | Netzwerk-Scan, Service-Erkennung           |
| **NetBox**        | Netzwerk-Inventarisierung                  |
| **Spiceworks**    | IT-Asset-Management                        |
| **Lansweeper**    | Netzwerkgeräte-Discovery                   |
| **Open-AudIT**    | Automatische Inventarisierung              |

---

## 2️⃣ Vulnerability Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Nessus**        | Schwachstellenscanner                      |
| **OpenVAS**       | Open-Source-Scanner                        |
| **Qualys**        | Cloud-basierte Schwachstellenverwaltung    |
| **Rapid7 InsightVM** | Risikobasiertes VM-Management           |

---

## 3️⃣ Patch Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **WSUS**          | Windows-Patch-Verteilung                   |
| **PDQ Deploy**    | Softwareverteilung & Patchen               |
| **Ansible / Puppet / Chef** | Automatisierte Updates           |
| **ManageEngine Patch Manager Plus** | Patchverwaltung         |

---

## 4️⃣ Endpoint Detection & Response (EDR)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **CrowdStrike Falcon** | Cloud-EDR-Plattform                   |
| **Microsoft Defender for Endpoint** | Windows-EDR              |
| **Elastic EDR**   | Open-Source EDR (Elastic Stack)            |
| **SentinelOne**   | KI-gestützte Bedrohungserkennung           |

---

## 5️⃣ Security Information & Event Management (SIEM)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Splunk**        | Ereignisanalyse & Korrelationsregeln       |
| **ELK Stack (Elasticsearch, Logstash, Kibana)** | Logmanagement |
| **Graylog**       | Open-Source SIEM                           |
| **Wazuh**         | Hostüberwachung + SIEM-Funktionalität      |

---

## 6️⃣ Intrusion Detection & Prevention (IDS / IPS)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Snort**         | Netzwerkbasiertes IDS                      |
| **Suricata**      | IDS/IPS mit hoher Leistung                 |
| **Zeek (Bro)**    | Netzwerküberwachung & Analyse              |
| **Security Onion**| IDS/NSM-Plattform mit Snort, Suricata etc. |

---

## 7️⃣ Log Management & Monitoring

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Syslog-ng / Rsyslog** | Zentrale Logaufnahme                |
| **Logwatch**      | Tägliche Log-Reports                       |
| **Kibana**        | Visualisierung von Logs                    |
| **Fluentd / Logstash** | Logaggregation                       |

---

## 8️⃣ Threat Intelligence

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **MISP**          | Threat Sharing Plattform                   |
| **AlienVault OTX**| Open Threat Exchange                       |
| **Cisco Talos**   | Bedrohungsdatenbank                        |
| **VirusTotal**    | Dateianalyse & Reputation                  |
| **AbuseIPDB**     | IP-Reputationsdatenbank                    |

---

## 9️⃣ Vulnerability Intelligence & Attack Surface Management

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Shodan Monitor**| Externe Angriffsfläche überwachen          |
| **Attack Surface Analyzer** | Microsoft Tool zur Analyse       |
| **Censys**        | Angriffsflächen-Scan                       |

---

## 🔟 Digital Forensics & Incident Response (DFIR)

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Velociraptor**  | Endpoint-DFIR                              |
| **Autopsy**       | Forensik-GUI                               |
| **Volatility**    | RAM-Analyse                                |
| **The Sleuth Kit**| Datei-Forensik                             |
| **GRR Rapid Response** | Remote Incident Response              |

---

## 1️⃣1️⃣ Malware Analysis

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Cuckoo Sandbox**| Dynamische Malwareanalyse                  |
| **Any.Run**       | Online Sandbox                             |
| **REMnux**        | Malware-Analyse-Linux-Toolkit              |
| **IDA Free / Ghidra** | Reverse Engineering                    |

---

## 1️⃣2️⃣ Email Security & Phishing Defense

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Proofpoint**    | Anti-Phishing & Email Security             |
| **Mimecast**      | Mailfilter & Schutz                        |
| **MailCleaner**   | Open-Source Gateway                        |
| **PhishTool**     | Analyse von verdächtigen E-Mails           |

---

## 1️⃣3️⃣ Firewall & Network Security

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **pfSense / OPNsense** | Open-Source Firewalls                |
| **iptables / nftables** | Paketfilter für Linux                |
| **Cisco ASA / Fortinet** | Kommerzielle Firewalls              |

---

## 1️⃣4️⃣ Awareness & Simulation

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Gophish**       | Phishing-Simulation                        |
| **KnowBe4**       | Security-Awareness-Training                |
| **PhishSim**      | Benutzer-Schulung                          |

---

## 1️⃣5️⃣ Backup & Recovery

| Tool              | Zweck                                      |
|-------------------|--------------------------------------------|
| **Veeam**         | Backup & Wiederherstellung                 |
| **BorgBackup**    | Open-Source Backup                         |
| **Restic**        | Schnelles Backup CLI                       |
| **Rsync / Duplicity** | Scriptbare Backuplösungen              |

---

## 📋 Empfehlungen

- Nutze **Wazuh + ELK + Suricata** für eine Open-Source Blue-Team-Pipeline.
- Kombiniere mit **MISP** und **Velociraptor** für erweiterte Threat Detection & IR.

---

**Letzte Aktualisierung:** Juli 2025  
Pull Requests für neue Shortcuts, Plugins oder Verbesserungen willkommen!