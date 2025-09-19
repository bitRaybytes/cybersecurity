# Rechtliche Rahmenbedingungen der IT-Sicherheit
Der Schutz von Informationen und IT-Systemen wird in Deutschland und der EU durch eine Reihe von Gesetzen und Richtlinien geregelt. Diese bilden die Grundlage für die rechtssichere Umsetzung von Sicherheitsmaßnahmen.

## Inhaltsverzeichnis
- [1. Gesetze und Verordnungen](#1-gesetze-und-verordnungen)
- [2. Das Bundesamt für Sicherheit in der Informationstechnik (BSI)](#2-das-bundesamt-für-sicherheit-in-der-informationstechnik-bsi)
- [3. Wichtige Prinzipien der DSGVO](#3-wichtige-prinzipien-der-dsgvo)
- [4. Technische und organisatorische Maßnahmen (TOMs)](#4-technische-und-organisatorische-maßnahmen-toms)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Gesetze und Verordnungen
Die wichtigsten rechtlichen Grundlagen, die den Schutz personenbezogener Daten und die IT-Sicherheit im Allgemeinen regeln, sind auf europäischer und nationaler Ebene verankert:

- **DSGVO (Datenschutz-Grundverordnung):** Eine EU-weite Verordnung, die seit 2018 verbindlich ist und die Regeln zur Verarbeitung personenbezogener Daten vereinheitlicht. Sie hat direkten Rechtscharakter und steht über nationalen Gesetzen.

- **BDSG (Bundesdatenschutzgesetz):** Das deutsche Gesetz, das die DSGVO ergänzt und präzisiert, wo die Verordnung sogenannte „Öffnungsklauseln“ für nationale Regelungen vorsieht (z. B. bei der Bestellung eines Datenschutzbeauftragten).

- **LDSG (Landesdatenschutzgesetz):** Gesetze auf Ebene der einzelnen Bundesländer, die ergänzende Regelungen für staatliche Stellen festlegen können.

- **IT-Sicherheitsgesetz (IT-SiG 2.0):** Fokussiert sich auf die Sicherheit kritischer Infrastrukturen (KRITIS), wie Energieversorger, Telekommunikation und Gesundheitswesen. Es verpflichtet Betreiber, Mindeststandards zu erfüllen und Cyberangriffe dem BSI zu melden.

Die Beziehung zwischen diesen Gesetzen kann wie folgt dargestellt werden:
```text
                             EU-Recht
         +-------------------------------------------------+
         | DSGVO (EU-weit, vorrangig)                      |
         |-------------------------------------------------|
         | IT-Sicherheitsgesetz (D-weit, KRITIS)           |
         +-------------------------------------------------+
                                 |
         +-------------------------------------------------+
         | BDSG (Deutschland, ergänzend zur DSGVO)         |
         |-------------------------------------------------|
         | Landesgesetze (z.B. LDSG, für Länderbehörden)   |
         +-------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 2. Das Bundesamt für Sicherheit in der Informationstechnik (BSI)
Das BSI ist die zentrale Behörde für Cybersicherheit in Deutschland. Seine Aufgaben sind im BSI-Gesetz sowie im IT-Sicherheitsgesetz festgelegt, das sich speziell auf kritische Infrastrukturen (KRITIS) bezieht.

Die Hauptaufgaben des BSI sind:
- Entwicklung von IT-Sicherheitsstandards und Empfehlungen.
- Unterstützung und Beratung von Behörden, Unternehmen und Bürgern.
- Analyse und Abwehr von Cyber-Angriffen.

Wichtige Veröffentlichungen des BSI:

- **IT-Grundschutzkompendium:** Ein jährlich aktualisiertes Werk, das Gefährdungen und Bausteine zur Erstellung eines **Informationssicherheitsmanagementsystems (ISMS)** beschreibt.

- **BSI-Standards:** Spezifikationen für Methoden und Prozesse.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Was ist ein ISMS?

Ein **Information Security Management System (ISMS)** ist ein Satz von Verfahren und Regeln, mit denen eine Organisation die Informationssicherheit systematisch definieren, steuern, kontrollieren und kontinuierlich verbessern kann. Die Vorgehensweise folgt oft dem **PDCA-Zyklus:** **P**lan, **D**o, **C**heck, **A**ct.

```text
                            +-----------+                   
                            |   Plan    |                   
        +------------------>+           +-------------------+
        |                   | (Planen)  |                   |
        |                   +-----------+                   |
+------------+                                          +------------+
|    Act     |                  PDCA                    |     Do     |
|  (Handeln) |            (Demening-Zyklus)             |(Ausführen) |
+------------+                                          +------------+              
        ^                   +-----------+                   |
        |                   |   Check   |                   |
        +-------------------+           +<------------------+
                            |  (Prüfen) |
                            +-----------+
``` 

- **Plan (planen):** Ziele, Richtlinien, Prozesse, Risikoanalyse, Maßnahmen planen.
- **Do (ausführen):** Maßnahmen umsetzen, Schulungen, Prozesse einführen.
- **Check (prüfen):** Audits, Überwachung, Reports, Wirksamkeit der Maßnahmen prüfen.
- **Act (handeln):** Verbesserungen vornehmen, aus den Fehlern lernen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 3. Wichtige Prinzipien der DSGVO
Artikel 5 Absatz 1 der DSGVO legt die grundlegenden Prinzipien fest, die bei der Verarbeitung personenbezogener Daten zu beachten sind:

```text
      Datensicherheit (Art. 32)
        +-----------------+
        |    Integrität   |
+-------> Vertraulichkeit <-------+
|       +-----------------+       |
|                ^                |
|                |                v
+----------------+----------------+
|  Rechtmäßigkeit, Transparenz    |
|       Datenminimierung          |
|         Zweckbindung            |
+---------------------------------+
```

- **Rechtmäßigkeit, Verarbeitung nach Treu und Glauben, Transparenz:** Die Datenverarbeitung muss rechtlich zulässig, nachvollziehbar und fair sein. Die Betroffenen müssen wissen, was mit ihren Daten geschieht.

- **Zweckbindung:** Daten dürfen nur für festgelegte, eindeutige und legitime Zwecke erhoben und weiterverarbeitet werden.

- **Datenminimierung:** Es dürfen nur die Daten verarbeitet werden, die für den jeweiligen Zweck unbedingt erforderlich sind.

- **Richtigkeit:** Daten müssen sachlich korrekt und auf dem neuesten Stand sein.

- **Speicherbegrenzung:** Daten dürfen nur so lange gespeichert werden, wie es für den Zweck der Verarbeitung erforderlich ist.

- **Integrität und Vertraulichkeit:** Durch geeignete technische und organisatorische Maßnahmen (**TOMs**) müssen die Daten vor unbefugter oder versehentlicher Änderung und Offenlegung geschützt werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 4. Technische und organisatorische Maßnahmen (TOMs)
Artikel 32 DSGVO fordert die Umsetzung von **TOMs**, um ein dem Risiko angemessenes Schutzniveau zu gewährleisten. Die Maßnahmen sollen die Vertraulichkeit, Integrität, Verfügbarkeit und Belastbarkeit der Systeme sicherstellen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### **Technische Maßnahmen:**
- **Verschlüsselung:** Absicherung von Daten im Ruhezustand (Data at rest) und bei der Übertragung (Data in transit).
- **Firewalls und IDS/IPS:** Schutz von Netzwerken und Systemen.
- **Zugriffskontrollen:** Einsatz von starken Passwörtern, Multi-Faktor-Authentifizierung (MFA) und Least-Privilege-Prinzip.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Organisatorische Maßnahmen:
- **Datenschutz-Schulungen:** Sensibilisierung der Mitarbeiter für Datensicherheitsrisiken.
- **Sicherheitsrichtlinien:** Klare Regeln für den Umgang mit Daten und IT-Systemen.
- **Notfallpläne:** Strategien für den Umgang mit Datenpannen und Sicherheitsvorfällen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Grundlagen der IT-sicherheit](/01-basics-intro/it_sicherheit_grundlagen.md)
- [Mehr zum Thema Cyberangriffe](/02-network-security/angriffe/cyberangriffe_grundlagen.md)



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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---