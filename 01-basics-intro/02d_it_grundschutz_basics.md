# IT-Grundschutz Grundlagen

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [BSI-Standards](#bsi-standards)
    - [BSI-Standard 200-1 Managementsysteme für Informationssicherheit (ISMS)](#bsi-standard-200-1-managementsysteme-für-informationssicherheit-isms)
    - [BSI-Standard 200-2: IT-Grundschutz Methodik](#bsi-standard-200-2-it-grundschutz-methodik)
    - [BSI-Standard 200-3: Risikoanalyse](#bsi-standard-200-3-risikoanalyse)
    - [BSI-Standards 200-4: Business Continuity Management (BCM)](#bsi-standards-200-4-business-continuity-management-bcm)
- [IT-Grundschutz-Kompendium](#it-grundschutz-kompendium)
    - [Die Baustein-Typen](#die-baustein-typen)
    - [Die Anforderungen](#die-anforderungen)
    - [Der Muster-Prozess im Überblick](#der-muster-prozess-im-überblick)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung
Das Konzept des **IT-Grundschutzes** zählt zum De-Facto-Standard für die IT-Sicherheit in Deutschland. Die Grundlagen bilden die Standards des **Bundesamts für Sicherheit in der Informationstechnik** (**BSI**) und dem IT-Grundschutz-Kompendium.

Die IT-Grundschutz-Methode ist darauf ausgelegt, ein angemessenes und umsetzbares Sicherheitsniveau zu schaffen, das die drei Schutzziele Vertraulichkeit, Integrität und Verfügbarkeit effektiv adressiert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## BSI-Standards
Ein elementarer Bestandteil des IT-Grundschutzes bilden die BSI-Standards, die die Methoden, Prozesse, Verfahren sowie Empfehlungen beinhalten, die im Rahmen unterschiedlicher Aspekte der Informationssicherheit beachtet werden sollten, damit ein ganzheitlicher Schutz der IT-Systeme gewährleistet werden kann.

Die BSI-Standards hat das BSI auf seiner Homepage, welche du [hier findest](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/IT-Grundschutz/BSI-Standards/bsi-standards_node.html).

### BSI-Standard 200-1 Managementsysteme für Informationssicherheit (ISMS)

- Beschreibt, welche grundlegenden Anforderungen ein ISMS erfüllen muss.
- Erläutert, welche Komponenten ein ISMS enthalten sollte und welche Aufgaben Leitungsebenen übernehmen müssen.
- **Ziel:** Organisatonen dabei zu unterstützen, die zunehmenden Risiken von Cyberangriffen zu reduzieren, indem geeignete Maßnahmen ergriffen werden.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### BSI-Standard 200-2: IT-Grundschutz Methodik
Der BSI-Standard 200-2 bildet die Basis der notwendigen Maßnahmen zum Aufbau eines ISMS. Zur Umsetzung dieser Absicherung gibt es drei Vorgehensweisen:

- BSI-Standard 200-2 bildet die Basis der notwendigen Maßnahmen zum Aufbau eines ISMS.
- Zur Umsetzung dieser Absicherung gibt es drei Vorgehensweisen:
    - **Basis-Absicherung:** Ein ISMS wird mit einem reduzierten Aufwand implementiert, um schnell ein grundlegendes Sicherheitsniveau zu erreichen.
    - **Standard-Absicherung:** Diese Vorgehensweise implementiert einen kompletten Sicherheitsprozess auf Basis der im Kompendium definierten Bausteine.
    - **Kern-Absicherung:** Wie die Basis-Absicherung, aber mit Fokus auf besonders schützenswerte Kerndaten oder -prozesse.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### BSI-Standard 200-3: Risikoanalyse

- Der BSI-Standard 200-3 beschreibt die Methodik der Risikoanalyse auf Basis des IT-Grundschutzes.
- Der Vorteil liegt hier im deutlich reduzierten Aufwand, da Risiken nur für Bereiche bewertet werden, in denen der Standard-Schutz nicht ausreicht oder spezielle Bedrohungen bestehen.
- Diese Methodik ist besonders nützlich, sobald ein Unternehmen mit der Implementierung eines ISMS begonnen hat und darüber hinausgehende Risiken absichern möchte.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### BSI-Standards 200-4: Business Continuity Management (BCM)

- BSI-Standard 200-4 beinhaltet eine praxisnahe Anleitung, um ein BCM-System (BCMS) in der eigenen Institution aufzubauen.
- Es geht auf potentielle Synergiemöglichkeiten mit Themen der IT-Sicherheit und des Krisenmanagements ein.
- Gut für unerfahrene BCM-Anwender, da es einen leichten Einstieg in die Thematik ermöglicht.
- Erfahrene Anwender erhalten einen Anforderungskatalog, der ein Mapping mit der internationalen Norm ISO/IEC 22301:2019 ermöglicht.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## IT-Grundschutz-Kompendium
Das IT-Grundschutz-Kompendium stellt zusammen mit den BSI-Standards die methodische Basis der IT-Sicherheit dar. Sein Fokus liegt auf den sogenannten Bausteinen, die in einem hierarchischen System organisiert sind.

Die Struktur des Kompendiums lässt sich so darstellen:

```text
+------------------------------------+
|            Bausteine               | <--- Oberste Ebene (z.B. ORP, SYS)
+------------------------------------+
       |          |          |
       V          V          V
+------------------------------------+
|           Anforderungen            | <--- Detaillierte Sicherheitsanweisungen
| (M - Muss, S - Soll, H - Hilfreich)|
+------------------------------------+
       |          |          |
       V          V          V
+------------------------------------+
|        Umsetzungshinweise          | <--- Konkrete Empfehlungen zur Umsetzung
+------------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Die Baustein-Typen
Jeder Baustein gehört einem Themenbereich an, die mit einem spezifischen Präfix gekennzeichnet sind. Zum Beispiel gibt es:

- **ORP (Organisation und Personal):**

    - **Aufgabe:** Definiert organisatorische Richtlinien und Prozesse.

    - **Beispiel:** ORP.1 `Sicherheitsmanagement`, ORP.2 `Notfallmanagement`, ORP.3 `Aus- und Fortbildung`.

- **SYS (Systeme):**

    - **Aufgabe:** Behandelt allgemeine Systemkomponenten, die in der Organisation verwendet werden.

    - **Beispiel:** SYS.1 `Allgemeine Server`, SYS.2 `Clients`, SYS.3 `Mobile Endgeräte`.

- **NET (Netzwerke und Infrastruktur):**

    - **Aufgabe:** Behandelt alle Aspekte der Netzwerksicherheit.

    - **Beispiel:** NET.1 `Netzwerkmanagement`, NET.2 `Wireless` `LAN`, NET.3 `Fernzugriff`.

- **APP (Anwendungen):**

    - **Aufgabe:** Behandelt die Sicherheit von Software-Anwendungen.

    - **Beispiel:** APP.1 `Allgemeine Anwendungen`, APP.2 `Web-Anwendungen`, APP.3 `Datenbanken`.

Jeder Baustein enthält eine Reihe von **Anforderungen** und **Umsetzungshinweisen**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Die Anforderungen
Die Anforderungen sind die konkreten Sicherheitsmaßnahmen, die umgesetzt werden müssen. Sie sind in drei Kategorien unterteilt, um eine Priorisierung zu ermöglichen:

- **M (Muss):** Obligatorische Maßnahmen, die unbedingt umgesetzt werden müssen, um das angestrebte Sicherheitsniveau zu erreichen.

- **S (Soll):** Wichtige Maßnahmen, deren Umsetzung dringend empfohlen wird, wenn der Aufwand vertretbar ist.

- **H (Hilfreich):** Zusätzliche Maßnahmen, die die Sicherheit weiter erhöhen, aber nicht zwingend sind.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Der Muster-Prozess im Überblick
Die Umsetzung des IT-Grundschutzes folgt einem klaren, iterativen Prozess, der in der Praxis angewendet wird.

```text
+------------------------------+
|     1. ISMS-Einrichtung      |
|    - Festlegung des Ziels    |
|    - Ein Team bilden         |
+------------------------------+
         |
         V
+------------------------------+
|   2. Strukturanalyse         |
|   - Informationsverbund      |
|   - Assets inventarisieren   |
+------------------------------+
         |
         V
+------------------------------+
|  3. Modellierung             |
|  - Passende Bausteine        |
|  - Schutzbedarf bestimmen    |
+------------------------------+
         |
         V
+----------------------------------+
|  4. Basis-Sicherheitscheck       |
|  - Umsetzung der M-Anforderungen |
|  - Dokumentation                 |
+----------------------------------+
         |
         V
+-------------------------------+
|  5. Risikoanalyse             |
|  - Bedrohungen identifizieren |
|  - Restrisiken bewerten       |
+-------------------------------+
         |
         V
+------------------------------+
|   6. Umsetzung & Wartung     |
|   - Maßnahmen implementieren |
|   - Kontinuierliche Prüfung  |
+------------------------------+
```


## Nützliche Links

- [Wikipedia: IT-Grundschutz](https://de.wikipedia.org/wiki/IT-Grundschutz)
- [Wikipedia: IT-Grundschutz-Kompendium](https://de.wikipedia.org/wiki/IT-Grundschutz-Kompendium)

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