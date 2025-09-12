# 🛡️ Die CIA-Triade: Das Fundament der Cybersicherheit

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Vertraulichkeit (Confidentiality)](#vertraulichkeit-confidentiality)
- [Integrität (Integrity)](#integrität-integrity)
- [Verfügbarkeit (Availabilty)](#verfügbarkeit-availability)
- [Die CIA-Triade in der Praxis](#die-cia-triade-in-der-praxis)
- [Haftungsausschluss](#haftungsausschluss)

## Einführung
Die CIA-Triade ist ein grundlegendes Konzept der Informationssicherheit. Sie stellt die drei Hauptziele dar, die beim Schutz von Daten und Systemen verfolgt werden: **Vertraulichkeit** (**Confidentiality**), **Integrität** (**Integrity**) und **Verfügbarkeit** (**Availability**)

Zusammen bilden diese drei Prinzipien das Fundament für Sicherheitsrichtlinien, Risikobewertungen und Abwehrmaßnahmen. Wenn nur eines dieser Ziele vernachlässigt wird, kann die Sicherheit der gesamten Information beeinträchtigt werden.

```text
+------------------------------------------+
|  Vertraulichkeit (Confidentiality)       |
|    - Schutz vor unbefugtem Zugriff       |
|    - Verschlüsselung, Zugriffskontrolle  |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
|  Integrität (Integrity)                  |
|    - Schutz vor unbefugter Änderung      |
|    - Hashing, Digitale Signaturen        |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
|  Verfügbarkeit (Availability)            |
|    - Gewährleistung des Zugriffs         |
|    - Backups, Redundanz, DoS-Schutz      |
+------------------------------------------+
```

## Vertraulichkeit (Confidentiality)
**Vertraulichkeit** bedeutet, dass Daten nur für Personen zugänglich sind, die dazu autorisiert wurden. Es geht darum, Informationen vor neugierigen Blicken oder unbefugter Offenlegung zu schützen.

- **Ziel:** Geheimhaltung bewahren.
- **Bedrohung:** Datenleckts, Social Engineering, unbefugte Datenzugriffe.
- **Maßnahmen:**
    - **Verschlüsselung:** Daten werden unlesbar gemacht.
    - **Zugriffskontrollen (Access Control):** Einschränkung von Lesezugriffen auf Dateien oder Systeme.
    - **Authentifizierung:** Sicherstellung der Identität von Nutzern (z. B. Passwörterm, MFA).

**Beispiel:** Ein verschlüsseltes E-Mail-Archiv kann nur von demjenigen entschlüsselt werden, der den richtigen Schlüssel besitzt.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Integrität (Integrity)
**Integrität** stellt sicher, dass Daten korrekt, vollständig und unverändert sind. Sie garantiert, dass Informationen nicht unbefugt oder versehentlich manipuliert wurden.

- **Ziel:** Korrektheit und Vollständigkeit der Daten gewährleisten.
- **Bedrohung:** Datenmanipulation, Malware (Viren, Trojaner), DNS-Spoofing.
- **Maßnahmen:**
    - **Hashing:** Erstellung eines eindeutigen "Fingerabdrucks" von Daten, um jede Änderung sofort zu erkennen.
    - **Digitale Signaturen:** Überprüfung der Herkunft und Authentizität von Dokumenten.
    - **Zugriffskontrollen (Access Control):** Einschränkung von Schreib- und Änderungsrechten.

**Beispiel:** Du lädst eine Software herunter und vergleichst den Hash-Wert mit dem auf der Website des Herstellers. Stimmen sie überein, weißt du, dass die Datei unverändert ist.

## Verfügbarkeit (Availability)
**Verfügbarkeit** bedeutet, dass autorisierte Benutzer bei Bedarf jederzeit auf Daten und Systeme zugreifen können. Es geht darum, Ausfallzeiten zu minimieren und eine unterbrechungsfreie Nutzung zu gewährleisten.

- **Ziel:** Sicherstellung der Nutzbarkeit von Diensten und Informationen.
- **Bedrohung:** Denial-of-Service (DoS) Angriffe, Stromausfälle, Hardwaredefekte.
- **Maßnahmen:**
-   **Backups:** Regelmäßige Datensicherungen zur Wiederherstellung nach einem Verlust.
-   **Redundanz:** Einsatz von Backupsystemen und redundanter Hardware (z. B. RAID, Failover-Cluster).
-   **DDoS-Schutz:** Techniken zur Abwehr von massiven Anfragen, die einen Dienst lahmlegen sollen.

**Beispiel:** Ein Unternehmen betreibt einen Webserver redundant, um sicherzustellen, dass die Webseite auch dann online bleibt, wenn ein Server ausfällt.

## Die CIA-Triade in der Praxis
Die drei Prinzipien sind voneinander abhängig. Ein Angriff auf die Integrität (z. B. die Manipulation einer Konfigurationsdatei) kann auch die Verfügbarkeit eines Dienstes beeinträchtigen. Ein Angriff auf die Verfügbarkeit (z. B. ein DDoS) könnte die Vertraulichkeit oder Integrität indirekt gefährden, indem er die Reaktionszeit der Sicherheitsteams verlangsamt.

Die CIA-Triade ist ein essenzieller Rahmen, um die Risiken von Informationssystemen zu bewerten und ganzheitliche Sicherheitsstrategien zu entwickeln. Sie erinnert daran, dass der Schutz von Daten mehr ist, als nur eine Firewall einzurichten.

## Nützliche Links
- [Wikipedia: Informationssicherheit](https://de.wikipedia.org/wiki/Informationssicherheit)
- [Wikipedia: Vertraulichkeit](https://de.wikipedia.org/wiki/Vertraulichkeit)
- [Wikipedia: Integrität](https://de.wikipedia.org/wiki/Integrit%C3%A4t_(Informationssicherheit))
- [Wikipedia: Verfügbarkeit](https://de.wikipedia.org/wiki/Verf%C3%BCgbarkeit)


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