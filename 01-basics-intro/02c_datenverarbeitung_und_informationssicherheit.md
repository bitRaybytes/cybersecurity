# 💻 Datenverarbeitung und Informationssicherheit

Die **Informations- und Telekommunikationstechnik** (**ITK**) bildet das Rückgrat der modernen Gesellschaft. Als IT-Sicherheitsexperten müssen wir verstehen, wie Daten erfasst, gespeichert, verarbeitet und übertragen werden, um sie effektiv schützen zu können.

## Inhaltsverzeichnis
- [Das Ziel der Datenverarbeitung](#das-ziel-der-datenverarbeitung)
- [Arten der Datenverarbeitung](#arten-der-datenverarbeitung)
- [Datenschutz vs. Datensicherheit](#datenschutz-vs-datensicherheit)
- [Die drei Grundpfeiler der Informationssicherheit: Die CIA-Triade](#die-drei-grundpfeiler-der-informationssicherheit-die-cia-triade)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das Ziel der Datenverarbeitung

Das Hauptziel der Datenverarbeitung ist es, aus Rohdaten nützliche Informationen zu gewinnen, um effiziente Kommunikation, Automatisierung und Problemlösungen zu ermöglichen. Für uns in der Cybersicherheit bedeutet dies, dass wir die Prozesse nicht nur vor Angriffen schützen, sondern auch ihre Integrität und Verfügbarkeit sicherstellen müssen.

Die Datenverarbeitung lässt sich in vier Hauptphasen unterteilen:

- **Datenerfassung:** Sammeln von Rohdaten aus verschiedenen Quellen.
- **Datenverarbeitung:** Anwendung von Algorithmen zur Umwandlung in verwertbare Informationen.
- **Datenspeicherung:** Lang- oder kurzfristige Ablage auf Speichermedien.
- **Datenübertragung:** Verschieben von Daten zwischen Systemen oder Netzwerken.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Arten der Datenverarbeitung

Die Art der Verarbeitung hat direkte Auswirkungen auf die Sicherheitsanforderungen.

| Art | Bechreibung | Sicherheitsrelevanz|
|-----|-------------|--------------------|
| **Batch-Verarbeitung** | Verarbeitung großer Datenmengen in geplanten Blöcken. Beispiel: Monatsabschlüsse im Finanzwesen. | Angreifer könnten geplante Prozesse sabotieren, um die Integrität oder Verfügbarkeit zu stören. |
| **Echtzeit-Verarbeitung** | Unmittelbare Verarbeitung von Daten, die eine sofortige Reaktion erfordern. Beispiel: Verkehrsüberwachung. | Hier ist die **Verfügbarkeit** der Daten kritisch. Jeder Ausfall kann katastrophale Folgen haben. |
| **Online-Verarbeitung** | Interaktive Verarbeitung von Daten während der Benutzereingabe. Beispiel: Online-Buchungssysteme. | Es besteht eine hohe Gefahr durch **Man-in-the-Middle-Angriffe** oder **Datenlecks** während der Transaktion. |
| **Verteilte-Verarbeitung** | Verarbeitung über mehrere vernetzte Computer hinweg (z. B. in der Cloud). | Bietet Skalierbarkeit, vergrößert aber die **Angriffsfläche**. Jede Komponente im Netzwerk ist ein potenzielles Ziel. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Datenschutz vs. Datensicherheit

Diese beiden Begriffe werden oft verwechselt, sind aber klar voneinander abzugrenzen.

- **Datenschutz (Privacy):** Bezieht sich auf den Schutz personenbezogener Daten und das Recht von Personen auf informationelle Selbstbestimmung. Es geht um die Frage: "Was darf geschützt werden?"
    - Beispiel: Einhaltung der DSGVO (Datenschutz-Grundverordnung) in Europa.

- **Datensicherheit (Security):** Bezieht sich auf den Schutz von Daten vor Verlust, Beschädigung oder Manipulation, unabhängig davon, ob sie personenbezogen sind oder nicht. Es geht um die Frage: "Wie werden die Daten geschützt?"
    - **Beispiel:** Einsatz von Verschlüsselung, Firewalls und Backups.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Die drei Grundpfeiler der Informationssicherheit: Die CIA-Triade

Das grundlegende Modell, um die Qualität von Informationssystemen zu beurteilen, ist die **[CIA-Triade](/01-basics-intro/02b_cia_triade.md)**.

- **Vertraulichkeit (Confidentiality):**

    - **Ziel:** Sicherstellen, dass Daten nur für berechtigte Personen zugänglich sind.

    - **Schutzmaßnahmen:** Zugriffskontrollen, Authentifizierung, Autorisierung und Verschlüsselung.

    - **Gefahr:** Unbefugter Zugriff, Datenlecks.

- **Integrität (Integrity):**

    - **Ziel:** Gewährleisten, dass die Daten korrekt, vollständig und unverändert sind.

    - **Schutzmaßnahmen:** Hash-Funktionen, digitale Signaturen und Zugriffsprotokolle.

    - **Gefahr:** Unbefugte Manipulation von Daten (z. B. Trojaner).

- **Verfügbarkeit (Availability):**

    - **Ziel:** Sicherstellen, dass die Daten und Systeme für berechtigte Nutzer jederzeit zugänglich sind.

    - **Schutzmaßnahmen:** Redundante Systeme, regelmäßige Backups und Schutz vor DDoS-Angriffen.

    - **Gefahr:** Denial-of-Service (DoS)-Angriffe, Hardware-Ausfälle, Naturkatastrophen.

Jede Sicherheitsmaßnahme, die wir implementieren, zielt darauf ab, mindestens eine dieser drei Eigenschaften zu stärken.



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