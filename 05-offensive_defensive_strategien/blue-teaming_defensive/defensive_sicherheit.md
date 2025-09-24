# Defensive Sicherheits

## Inhaltsverzeichnis

## Einleitung

**Defensive Sicherheit** wird von Blue-Teams genutzt, um Unternehmen proaktiv zu schützen. Dabei verlassen sie sich nicht darauf, dass Angreifer nicht zuschlagen, sondern bilden gemeinsam mit dem Red-Team eine Allianz, um Schwachstellen zu finden und zu beheben.

Das übergeordnete Ziel ist die **Verteidigung der drei grundlegenden Sicherheitsziele (CIA-Triade)**:

- **Confidentiality (Vertraulichkeit):** Schutz sensibler Daten vor unbefugtem Zugriff.

- **Integrity (Integrität):** Sicherstellung, dass Daten nicht unbefugt verändert werden können.

- **Availability (Verfügbarkeit):** Gewährleistung, dass Systeme und Dienste jederzeit für autorisierte Benutzer verfügbar sind.


## Die Säulen der Verteidigung
Um diese Ziele zu erreichen, basiert die defensive Sicherheit auf drei grundlegenden, sich ergänzenden Säulen.

### 1. Prävention (Preventive)
Maßnahmen, um Angriffe von vornherein zu verhindern oder zu erschweren.

- **Cyber Security Awareness:** Schulungen zu Phishing, Social Engineering und sicheren Verhaltensweisen, um die Mitarbeiter als stärkste Verteidigungslinie zu aktivieren.

- **Preventative Security:** Einsatz von Technologien, die Angriffe blockieren. Beispiele:

    - **Firewalls:** Kontrollieren den Netzwerkverkehr.

    - **Intrusion Prevention Systems (IPS):** Blockieren bekannten schädlichen Netzwerkverkehr.

    - **Web Application Firewalls (WAF):** Schützen Webanwendungen vor Angriffsvektoren wie XSS oder SQL Injection.

- **Documenting & Managing Assets:** Eine detaillierte Inventarisierung aller Assets (Server, Endgeräte, Anwendungen, Daten) ist die Grundlage jeder Verteidigung. Man kann nur schützen, was man auch kennt.

- **Frameworks, Policies & Procedures:** Starke Sicherheitsrichtlinien definieren den sicheren Umgang mit Unternehmensressourcen, z. B. Vorgaben zur Passwortstärke, Datenspeicherung oder dem Umgang mit mobilen Geräten.


### 2. Detektion (Detective)
Maßnahmen, um einen laufenden Angriff oder eine Sicherheitsverletzung frühzeitig zu erkennen.

- **Logging & Monitoring:** Das Herzstück der Detektion. Ausführliche Protokolldateien von Systemen, Anwendungen und dem Netzwerk sind unerlässlich, um ungewöhnliche Aktivitäten zu identifizieren. Ein **SIEM-System** (**Security Information and Event Management**) konsolidiert diese Logs und sucht automatisiert nach Anomalien.

### 3. Korrektur (Corrective)
Maßnahmen, um auf einen erkannten Angriff zu reagieren, den Schaden zu minimieren und Systeme wiederherzustellen.

- **Incident Response (IR):** Der klar definierte Prozess zur Reaktion auf einen Sicherheitsvorfall.

- **Digital Forensics:** Die spezialisierte Untersuchung nach einem Vorfall.





Innerhalb dieser Hauptaufgaben gibt es Teilprozesse, die das Blue-Team nutzt, um die Sicherheit aufrecht zu erhalten.

Einige dieser Teilaspekte sind beispielsweise:

- **Cyber Sercurity Awareness:** Es muss auf die Gefahren von potentiellen Angriffen aufmerksam gemacht werden.
    - **Beispiel:** Schulungsvideos zu `phishing` und `social-engineering`, damit Mitarbeiter sensibilisiert werden.

- **Documenting & Managing Assets:** Es ist wichtig, das System bzw. die gesamte Systemstruktur zu kennen. Dabei helfen Dokumentationen und eine Inventarisierung von Assets (alle technologischen und informationellen Ressourcen eines Unternehmens), damit du weißt, was geschützt werden muss.

- **Preventative Security:** Vorbeugende Maßnahmen sollen deine Assets schützen, damit Angriffe erst keine Chance haben, dein System zu korrumpieren.
    - **Beispiel:** Firewall, Intrusion Prevention Systeme.

- **Logging & Monitoring:** Ausführliche Logging-Dateien vom Netzwerk und den Aktivitäten des Systems sind ausschlaggebend und essentiell, um unauthorisierte Zugriffe zu erkennen.

- **Frameworks, Policies & Procedures:** Du solltest starke Sicherheits-Richtlinien haben, damit die Geräte deines Unternehmens sachgemäß genutzt werden.


## Security Operations Center (SOC)
Das **Security Operations Center (SOC)** ist ein Team aus IT-Sicherheitsexperten, die das Netzwerk und die Systeme in Echtzeit überwachen, um Bedrohungen zu erkennen und darauf zu reagieren.

Der SOC-Zyklus:
```text
+-------------------+
|  Threat Hunting   |
| (proaktive Suche) |
+-------------------+
          |
   (Informationen)
          |
+-------------------+
| Log-Analyse &     |
| Alerts            |
| (Überwachung)     |
+-------------------+
          |
 (Erkannter Vorfall)
          |
+-------------------+
| Incident Response |
| (Reaktion)        |
+-------------------+

```

### Trends & Vulnerability Awareness
SOC-Teams müssen den Angreifern stets einen Schritt voraus sein. Dies gelingt nur durch kontinuierliches Lernen und die Beobachtung aktueller Cyber-Bedrohungen, neuer Exploits und Schwachstellen.

### Policy Violations
Eine **Security Policy** ist eine Reihe von Sicherheitsrichtlinien, die den Rahmen der IT-Sicherheit eines Unternehmens vorgeben. Das SOC-Team überwacht die Einhaltung dieser Richtlinien und erkennt, wenn Geräte oder Nutzer sich nicht an die Regeln halten.

### Unauthorised & Illegal Activity
Alle Aktivitäten, die nicht den normalen Betriebsabläufen entsprechen, werden als unbefugt eingestuft und im Monitoring des SOC-Teams gemeldet. Illegale Aktivitäten sind eine enorme Gefahr für Unternehmen, da sie oft zu Datenlecks mit gravierenden Folgen führen.

### Intrusion & Breach Detection
Die Kernaufgabe des SOC ist die Erkennung von Eindringversuchen (`Intrusion`) und erfolgreichen Sicherheitsverletzungen (`Breach`). Hierbei werden technische Indikatoren (Indicators of Compromise, IoC) wie verdächtige IP-Adressen, veränderte Dateien oder untypisches Nutzerverhalten analysiert.

## Digital Forensics

**Digital Forensics**, oder digitale Forensik, ist die Tätigkeit, elektronische Geräte und Daten nach einem Sicherheitsvorfall zu sichern, zu untersuchen und als Beweise aufzubereiten. Für einen Forensiker ist es entscheidend, die digitale Kette der Beweise aufrechtzuerhalten, um sicherzustellen, dass keine Daten manipuliert wurden.

**Ziel:** Die Analyse dient dazu, herauszufinden, was passiert ist, wie der Angreifer eingedrungen ist, welche Daten betroffen sind und wann der Vorfall stattfand.

### Forensische Methoden


- **File System:** Ein forensisches Image des Dateisystems wird erstellt, um die Metadaten zu sichern. Die Low-Level-Analyse kann Aufschluss über gelöschte Dateien, installierte Programme oder veränderte Systemdateien geben.

- **System Memory (RAM):** Die Analyse des flüchtigen Arbeitsspeichers kann Informationen über laufende Prozesse, offene Netzwerkverbindungen und gelöschte Daten liefern, die nicht auf der Festplatte gespeichert wurden.


- **System Logs:** Log-Dateien von Betriebssystemen und Anwendungen liefern wertvolle Hinweise auf verdächtige Aktivitäten, Anmeldeversuche oder Befehle, die ausgeführt wurden.

- **Network Logs (Netzwerklogs):** Die Analyse des Netzwerkverkehrs hilft, die Kommunikationswege des Angreifers zu identifizieren und festzustellen, welche Daten exfiltriert wurden.




## Incident Response
**Incident Response** ist der strukturierte Prozess, den ein Unternehmen nutzt, um auf einen Sicherheitsvorfall zu reagieren. Das Ziel ist es, den Schaden zu minimieren, die Systeme schnell wiederherzustellen und die Ursache zu beheben.



**Vereinfachte Darstellung eines Ablaufs im IR:**

```text
                                +-------------------------------+
                                |                               |
                                |                               |
                                |                               V
+------+-------+       +--------+------+            +-------------------------+       +------------------+
| Vorbereitung |------>| Erkennung und |            | Eindämmung, Beseitigung |------>| Aktivitäten nach |
+--------------+       | Analyse       |            | Wiederherstellung       |       | dem Vorfall      |
       ^               +---------------+            +-----------+-------------+       +---------+--------+
       |                        ^                               |                               |                               
       |                        |                               |                               |
       |                        +-------------------------------+                               |
       |                                                                                        |
       +----------------------------------------------------------------------------------------+                         
```

- **1. Preparation (Vorbereitung):** 
    - Definition eines IR-Teams mit klaren Rollen und Verantwortlichkeiten.
    - Erstellung eines umfassenden IR-Plans, der Checklisten und Kommunikationswege festlegt.
    - Vorbereitung der Infrastruktur, z. B. durch Backups und isolierte Netze für die Untersuchung.

- **2. Detection & Analysis (Erkennung und Analyse):** 
    - Identifizierung des Vorfalls durch Alerts von Überwachungssystemen (z. B. SOC) oder Meldungen von Mitarbeitern.
    - Analyse der Beweise, um den Vorfall zu validieren, zu klassifizieren und den Umfang des Schadens abzuschätzen.

- **3. Containment (Eindämmung):**
    - Das wichtigste Ziel: Die Ausbreitung des Angriffs stoppen.
    - Maßnahmen: Isolierung infizierter Systeme vom Netzwerk, Deaktivierung betroffener Benutzerkonten, Blockieren schädlicher IP-Adressen.


- **4. Eradication (Beseitigung):**
    - Nachdem der Angreifer eingedämmt ist, wird die Grundursache des Angriffs behoben.
    - Maßnahmen: Schließen der Sicherheitslücke, Entfernen des Schadcodes, Löschen aller vom Angreifer hinterlassenen Backdoors.


- **5. Recovery (Wiederherstellung):** 
    - Systeme werden in den sicheren Normalzustand zurückversetzt.
    - Maßnahmen: Wiederherstellung von Daten aus sicheren Backups, erneute Konfiguration der Systeme, Überprüfung auf verbliebene Schwachstellen.


- **6. Post-Incident Activity:** 
    - Dokumentation des gesamten Vorfalls.
    - Erstellung eines "Lessons Learned"-Berichts, um Fehler im Prozess zu identifizieren und die Verteidigung für die Zukunft zu stärken.
    - Information der Stakeholder über den Vorfall und die ergriffenen Maßnahmen.


## Nützliche Links
- [TryHackeMe Raum: Defensive Security Intro](https://tryhackme.com/room/defensivesecurityintroqW)
- [Wikipedia: Security Operations Center ](https://de.wikipedia.org/wiki/Security_Operations_Center)
- [Wikipedia: Sicherheitsrichtlinie ](https://de.wikipedia.org/wiki/Sicherheitsrichtlinie)


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