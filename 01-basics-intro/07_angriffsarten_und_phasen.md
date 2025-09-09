# ⚔️ Angriffsarten & -phasen
## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Häufige Angriffsvektoren](#häufige-angriffsvektoren)
- [Der Ablauf eines Angriffs: Die Cyber Kill Chain](#der-ablauf-eines-angriffs-die-cyber-kill-chain)
    - [Phase 1: Aufklärung (Reconnaissance)](#phase-1-aufklärung-reconnaissance)
    - [Phase 2: Bewaffnung (Weaponization)](#phase-2-bewaffnung-weaponization)
    - [Phase 3: Auslieferung (Delivery)](#phase-3-auslieferung-delivery)
    - [Phase 4: Ausnutzung (Exploitation)](#phase-4-ausnutzung-exploitation)
    - [Phase 5: Installation (Installation)](#phase-5-installation-installation)
    - [Phase 6: Befehls- & Kontroll-Kommunikation (Command & Control / C2)](#phase-6-befehls---kontroll-kommunikation-command--control--c2)
    - [Phase 7: Ziele auf dem System (Actions on Objectives)](#phase-7-ziele-auf-dem-system-actions-on-objectives)
- [Praktische Anwendung](#praktische-anwendung)
- [Haftungsausschluss](#haftungsausschluss)

## Einführung
Um sich effektiv gegen Cyberangriffe zu verteidigen, muss man verstehen, wie Angreifer vorgehen. Ein Angriff ist selten ein einzelner, zufälliger Akt, sondern folgt meist einem strukturierten Prozess. Modelle wie die **Cyber Kill Chain** helfen, diese Phasen zu visualisieren und ermöglichen es Verteidigern, an jedem Punkt gezielte Gegenmaßnahmen zu ergreifen.

## Häufige Angriffsvektoren
Ein **Angriffsvektor** ist die Methode oder der Pfad, den ein Angreifer nutzt, um in ein System einzudringen oder es zu schädigen.

- **Social Engineering:** Manipulationsversuche, bei denen menschliche Schwächen ausgenutzt werden, um an vertrauliche Informationen zu gelangen. Ein Angreifer gibt sich beispielsweise als IT-Support aus, um Passwörter zu erfragen.

- **Phishing:** Eine Art von Social Engineering, bei der gefälschte E-Mails, SMS oder Websites verwendet werden, um Benutzerdaten wie Passwörter oder Kreditkartennummern zu stehlen.

- **Malware-basierte Angriffe:** Ein System wird mit Schadsoftware (Malware) infiziert, um Daten zu stehlen, das System zu manipulieren oder zu zerstören.

- **Denial-of-Service (DoS) / Distributed-Denial-of-Service (DDoS):** Angriffe, bei denen ein Dienst oder Server mit so vielen Anfragen überflutet wird, dass er zusammenbricht und für legitime Nutzer nicht mehr erreichbar ist.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Der Ablauf eines Angriffs: Die Cyber Kill Chain
Das **Cyber Kill Chain-Modell** von Lockheed Martin beschreibt die sieben Phasen eines zielgerichteten Cyberangriffs. Jede Phase ist ein potenzieller Punkt, an dem der Angreifer aufgehalten werden kann.

### Phase 1: Aufklärung (Reconnaissance)
Der Angreifer sammelt Informationen über das Ziel. Dies kann passiv (z. B. durch die Suche im Internet oder in sozialen Medien) oder aktiv (z. B. durch Port-Scanning oder das Senden von Pings) geschehen.

- **Ziel des Angreifers:** Schwachstellen und Eintrittspunkte finden.

- **Verteidigung:** Überwachung von externen Diensten und Netzwerken, um ungewöhnliche Aktivitäten zu erkennen.

### Phase 2: Bewaffnung (Weaponization)
Der Angreifer kombiniert einen **Exploit** (z. B. ein Skript) mit einer **Malware** (z. B. ein Trojaner) zu einer ausführbaren "Waffe", die per E-Mail oder auf einem USB-Stick bereitgestellt werden kann.

- **Ziel des Angreifers:** Ein Paket schnüren, das bereit zur Auslieferung ist.

- **Verteidigung:** Einsatz von Antiviren-Software und E-Mail-Filtern.

### Phase 3: Auslieferung (Delivery)
Die Waffe wird an das Zielsystem gesendet. Gängige Methoden sind E-Mail-Anhänge, bösartige Websites oder infizierte USB-Sticks.

- **Ziel des Angreifers:** Die Waffe an das Ziel zu bringen.

- **Verteidigung:** Netzwerk-Monitoring, Firewalls und Intrusion Prevention Systeme (IPS).

### Phase 4: Ausnutzung (Exploitation)
Der Exploit-Code wird auf dem Zielsystem ausgeführt, indem eine Schwachstelle ausgenutzt wird (z. B. durch einen Buffer-Overflow).

- **Ziel des Angreifers:** Die Kontrolle über das System erlangen.

- **Verteidigung:** Schnelles Patch-Management, um bekannte Schwachstellen zu schließen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Phase 5: Installation (Installation)
Nach der erfolgreichen Ausnutzung installiert der Angreifer die Malware, um sich eine dauerhafte Präsenz im System zu sichern.

**Ziel des Angreifers:** Eine Hintertür oder ein Backdoor zu schaffen.

**Verteidigung:** Ständige Überwachung von Systemen auf unerwartete Prozesse und Dateien.

### Phase 6: Befehls- & Kontroll-Kommunikation (Command & Control / C2)
Die installierte Malware kommuniziert mit dem Server des Angreifers, um Anweisungen zu empfangen und gestohlene Daten zu senden.

- **Ziel des Angreifers:** Das System fernsteuern.

- **Verteidigung:** Überwachung des Netzwerkverkehrs, um verdächtige Kommunikationsmuster zu erkennen.

### Phase 7: Ziele auf dem System (Actions on Objectives)
In dieser letzten Phase führt der Angreifer seine eigentlichen Ziele aus, wie Datendiebstahl, Datenverschlüsselung, Manipulation oder die Zerstörung des Systems.

- **Ziel des Angreifers:** Das eigentliche Ziel des Angriffs erreichen.

- **Verteidigung:** Datensicherung (Backups) und schnelle Reaktion auf Vorfälle.

```yaml
       Angreifer                                      Zielsystem

       1. Aufklärung  --->  2. Bewaffnung  --->  3. Auslieferung
       (Info sammeln)       (Waffe erstellen)     (Waffe senden)
              |                      |                    |
              V                      V                    V
        |-----+-----|          |-----+-----|        |-----+-----|
        |  Scanner  |          |  Exploit  |        | Phishing- |
        |  & OSINT  |          | & Malware |        | E-Mail    |
        |-----+-----|          |-----+-----|        |-----+-----|
                                                          |
                                                          V
                                                +----------------+
                                                |   Netzwerk     |
                                                +----------------+
                                                        |
                                                        V
                                     +----------------------------------+
                                     |  4. Ausnutzung (Exploitation)    |
                                     |  5. Installation (Installation)  |
                                     |  6. C2 (Command & Control)       |
                                     |  7. Ziele auf dem System (A-o-O) |
                                     +----------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Praktische Anwendung
Das Verständnis der **Cyber Kill Chain** ermöglicht es, effektive Gegenmaßnahmen zu planen. Anstatt nur die finale Aktion (Phase 7) zu bekämpfen, kann man den Angriff in den früheren Phasen unterbrechen.

- **Vor Phase 1:** Sensibilisierung der Mitarbeiter, um Social Engineering zu erkennen.
- **In Phase 3:** Starke E-Mail-Filter und Web-Filter, um die Auslieferung von Malware zu blockieren.
- **In Phase 6:** Firewalls und Intrusion Detection Systeme, die den C2-Verkehr erkennen und blockieren

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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---