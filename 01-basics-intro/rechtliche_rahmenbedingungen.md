# Rechtliche Rahmenbedingungen der IT-Sicherheit
Der Schutz von Informationen und IT-Systemen wird in Deutschland und der EU durch eine Reihe von Gesetzen und Richtlinien geregelt. Diese bilden die Grundlage für die rechtssichere Umsetzung von Sicherheitsmaßnahmen.

## Inhaltsverzeichnis
1. Gesetze und Verordnungen
2. Das Bundesamt für Sicherheit in der Informationstechnik (BSI)
3. Wichtige Prinzipien der DSGVO
Nützliche Links
Haftungsausschluss

## 1. Gesetze und Verordnungen
Die wichtigsten rechtlichen Grundlagen, die den Schutz personenbezogener Daten regeln, sind auf europäischer und nationaler Ebene verankert:

- **DSGVO (Datenschutz-Grundverordnung):** Eine EU-weite Verordnung, die seit 2018 verbindlich ist und die Regeln zur Verarbeitung personenbezogener Daten vereinheitlicht.

- **BDSG (Bundesdatenschutzgesetz):** Das deutsche Gesetz, das die DSGVO ergänzt. Es darf der Verordnung nicht widersprechen, sondern konkretisiert die Regelungen dort, wo die DSGVO sogenannte „Öffnungsklauseln“ für nationale Gesetze vorsieht.

- **LDSG (Landesdatenschutzgesetz):** Gesetze auf Ebene der einzelnen Bundesländer, die ergänzende Regelungen für staatliche Stellen festlegen können.

Die Beziehung zwischen diesen Gesetzen kann wie folgt dargestellt werden:
```yaml
+-------------------------------------------------+
| DSGVO (EU-weit, vorrangig)                      |
|                                                 |
|        +-----------------------------------+    |
|        | BDSG (Deutschland, ergänzend)     |    |
|        |                                   |    |
|        |    +-------------------------+    |    |
|        |    | LDSG (Bundesländer)     |    |    |
|        |    +-------------------------+    |    |
|        +-----------------------------------+    |
+-------------------------------------------------+
```
Es gibt zudem weitere Gesetze, die für den Datenschutz relevant sind, wie das **Telekommunikationsgesetz (TKG)** und das **Telemediengesetz (TMG)**.

## 2. Das Bundesamt für Sicherheit in der Informationstechnik (BSI)
Das BSI ist die zentrale Behörde für Cybersicherheit in Deutschland. Seine Aufgaben sind im BSI-Gesetz sowie im IT-Sicherheitsgesetz festgelegt, das sich speziell auf kritische Infrastrukturen (KRITIS) bezieht.

Die Hauptaufgaben des BSI sind:
- Entwicklung von IT-Sicherheitsstandards und Empfehlungen.
- Unterstützung und Beratung von Behörden, Unternehmen und Bürgern.
- Analyse und Abwehr von Cyber-Angriffen.

Wichtige Veröffentlichungen des BSI:

- **IT-Grundschutzkompendium:** Ein jährlich aktualisiertes Werk, das Gefährdungen und Bausteine zur Erstellung eines **Informationssicherheitsmanagementsystems (ISMS)** beschreibt.

- **BSI-Standards:** Spezifikationen für Methoden und Prozesse.

**Was ist ein ISMS?**

Ein **Information Security Management System (ISMS)** ist ein Satz von Verfahren und Regeln, mit denen eine Organisation die Informationssicherheit systematisch definieren, steuern, kontrollieren und kontinuierlich verbessern kann. Die Vorgehensweise folgt oft dem **PDCA-Zyklus:** **P**lan, **D**o, **C**heck, **A**ct.

```yaml
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

## 3. Wichtige Prinzipien der DSGVO
Artikel 5 Absatz 1 der DSGVO legt die grundlegenden Prinzipien fest, die bei der Verarbeitung personenbezogener Daten zu beachten sind:

- **Rechtmäßigkeit, Verarbeitung nach Treu und Glauben, Transparenz:** Die Datenverarbeitung muss rechtlich zulässig, nachvollziehbar und fair sein.

- **Zweckbindung:** Daten dürfen nur für festgelegte, eindeutige und legitime Zwecke erhoben werden.

- **Datenminimierung:** Es dürfen nur die Daten verarbeitet werden, die für den jeweiligen Zweck unbedingt erforderlich sind.

- **Richtigkeit:** Daten müssen sachlich korrekt und auf dem neuesten Stand sein.

- **Speicherbegrenzung:** Daten dürfen nur so lange gespeichert werden, wie es für den Zweck der Verarbeitung erforderlich ist.

- **Integrität und Vertraulichkeit:** Durch geeignete technische und organisatorische Maßnahmen (**TOMs**) müssen die Daten vor unbefugter oder versehentlicher Änderung und Offenlegung geschützt werden.

## Nützliche Links
- [Grundlagen der IT-sicherheit](/01-basics-intro/it_sicherheit_grundlagen.md)
- [Mehr zum Thema Cyberangriffe](/02-network-security/angriffe/cyberangriffe_grundlagen.md)

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