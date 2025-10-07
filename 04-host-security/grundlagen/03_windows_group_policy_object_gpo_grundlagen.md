# 🔐 Gruppenrichtlinien (Group Policy Objects - GPOs)
   
![Windows OS](https://img.shields.io/badge/Windows%20OS-%23334155.svg?style=for-the-badge&logo=windows&logoColor=white)
![Group Policy Objects](https://img.shields.io/badge/Group%20Policy%20Objects-%2300A86B.svg?style=for-the-badge&logo=microsoft&logoColor=white)



## Inhaltsverzeichnis
- [Einleitung: Die Rolle der GPOs](#einleitung-die-rolle-der-gpos)
- [Grundlagen des Active Directory (AD)](#grundlagen-des-active-directory-ad)
- [Was ist eine Gruppenrichtlinie (GPO)?](#was-ist-eine-gruppenrichtlinie-gpo)
- [Aufbau und Struktur einer GPO](#aufbau-und-struktur-einer-gpo)
    - [1. Computerkonfiguration](#1-computerkonfiguration)
    - [2. Benutzerkonfiguration](#2-benutzerkonfiguration)
- [Verarbeitung und Vererbung: Das LSDOU-Prinzip](#verarbeitung-und-vererbung-das-lsdou-prinzip)
    - [Wichtige Prinzipien der Vererbung](#wichtige-prinzipien-der-vererbung)
    - [Erweiterte GPO-Komponenten](#erweiterte-gpo-komponenten)
        - [Administrative Vorlagen (ADMX/ADML)](#administrative-vorlagen-admxadml)
- [Gruppenrichtlinien-Einstellungen (Group Policy Preferences - GPP)](#gruppenrichtlinien-einstellungen-group-policy-preferences---gpp)
- [Präzise Zuweisung durch Filterung](#präzise-zuweisung-durch-filterung)
- [Anwendungsfälle für die IT-Sicherheit](#anwendungsfälle-für-die-it-sicherheit)
- [Sicherheitsrisiken und Angriffe auf GPOs](#sicherheitsrisiken-und-angriffe-auf-gpos)
    - [1. Privilege Escalation über GPO-Manipulation](#1-privilege-escalation-über-gpo-manipulation)
    - [2. SYSVOL-Leak / Password Disclosure](#2-sysvol-leak--password-disclosure)
    - [3. GPO Replikationsmissbrauch](#3-gpo-replikationsmissbrauch)
- [Härtung und Best Practices](#härtung-und-best-practices)
- [Fehlerbehebung und Monitoring](#fehlerbehebung-und-monitoring)
- [Fazit](#fazit)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung: Die Rolle der GPOs

**Gruppenrichtlinien** (**Group Policy Objects** - **GPOs**) sind das zentrale Management-Tool in Windows-Domänen, das es IT-Administratoren ermöglicht, **zentralisierte Konfigurationen** und **Sicherheitseinstellungen** auf Benutzer und Computer anzuwenden. Sie sind der Schlüssel zur Durchsetzung von Unternehmensrichtlinien und zur Erfüllung von Sicherheitsanforderungen in großen und komplexen Umgebungen.

GPOs bieten eine einfache und skalierbare Methode zur Härtung von Systemen und zur Einhaltung von Compliance-Vorgaben.

```text
+------------------+        +----------------------+        +-------------------------+
|  Domänen-Admin   | --->   |  GPO-Server / DC     | --->   |  Domänen-Clients        |
| (Erstellt GPOs)  |        | (Repliziert über AD) |        | (Wenden Richtlinien an) |
+------------------+        +----------------------+        +-------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundlagen des Active Directory (AD)

Um GPOs zu verstehen, sind die folgenden Active Directory-Begriffe essenziell:

| Begriff | Definition | GPO-Bezug |
| :--- | :--- | :--- |
| **Domäne** | Die logische Verwaltungseinheit im AD, die Benutzer, Computer und Ressourcen gruppiert. Sie wird von Domänencontrollern verwaltet. | GPOs werden primär auf Domänenebene oder darunter angewendet. |
| **Domänencontroller (DC)** | Ein Server (meist Windows Server), der das AD hostet, die Anmeldungen verwaltet und für die Replikation zuständig ist. | DCs speichern und replizieren die GPO-Informationen. |
| **Client** | Ein Endgerät (z. B. PC, Laptop), das Mitglied der Domäne ist. | Clients wenden die GPOs während der Anmeldung oder in festgelegten Intervallen an. |
| **Organisationseinheit (OU)** | Ein Container innerhalb der Domäne zur logischen Strukturierung von Benutzern oder Computern (z. B. nach Abteilungen oder Standorten). | Die OU ist der präziseste Ort, um GPOs zuzuweisen. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was ist eine Gruppenrichtlinie (GPO)?
Eine GPO ist eine Sammlung von Konfigurationseinstellungen. Sie ist **keine physische Datei**, sondern besteht aus zwei logischen und physisch getrennten Teilen, die synchronisiert werden:

1.  **Group Policy Container (GPC):** Ein **Container-Objekt im Active Directory**. Enthält Versions-, Status- und Verknüpfungsinformationen (zu welchen OUs sie verknüpft ist).
2.  **Group Policy Template (GPT):** Ein **Ordner auf dem Domänencontroller** (im `SYSVOL`-Freigabeverzeichnis). Enthält die eigentlichen Konfigurationsdateien, Skripte (`.adm`, `.admx`), und die Sicherheitsrichtlinien.

**Zweck:** Sicherstellung eines einheitlichen Zustands aller verwalteten Computer und Benutzer, was die Sicherheit, Compliance und Wartbarkeit massiv verbessert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Aufbau und Struktur einer GPO

Jede GPO ist logisch in zwei Hauptbereiche unterteilt, die auf unterschiedliche Objekte angewendet werden:

### 1. Computerkonfiguration
Dieser Abschnitt enthält Einstellungen, die für den **gesamten Computer** gelten, unabhängig davon, welcher Benutzer sich anmeldet.

* **Anwendung:** Beim Start des Clients.
* **Beispiele:**
    * Firewall-Regeln (Ein- und ausgehend)
    * Sicherheitseinstellungen (z. B. Deaktivierung von USB-Laufwerken)
    * Installierte Software (z. B. automatische Installation von Virenschutz)
    * Netzwerkeinstellungen
    * Zertifikatsrichtlinien

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Benutzerkonfiguration
Dieser Abschnitt enthält Einstellungen, die für **spezifische Benutzer** gelten, unabhängig davon, an welchem Computer sie sich anmelden.

* **Anwendung:** Bei der Benutzeranmeldung.
* **Beispiele:**
    * Desktop-Hintergrund und Bildschirmschoner
    * Einschränkungen der Systemsteuerung
    * Verbindungen zu Netzlaufwerken (Mapping/Drive Mapping)
    * Einstellungen für den Internet Explorer/Edge



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Verarbeitung und Vererbung: Das LSDOU-Prinzip

GPOs werden in einer streng definierten Reihenfolge angewendet. Dieses **Vererbungsprinzip** bestimmt, welche Richtlinie gilt, wenn es Konflikte gibt. Die Regel lautet: **Spätere GPOs überschreiben frühere**.

Das **LSDOU-Prinzip** beschreibt die Hierarchie der Anwendung (von der höchsten zur niedrigsten Priorität, d.h., die letzte angewendete Richtlinie gewinnt):

```text
 +-------------------------------------------------+
 | L - Lokal (Local)    <- Geringste Priorität     |
 | (Auf dem lokalen Client-Computer)               |
 +-------------------------------------------------+
                         |
 +-------------------------------------------------+
 | S - Site (Standort)                             |
 | (Auf AD-Standortebene angewandt)                |
 +-------------------------------------------------+
                         |
 +-------------------------------------------------+
 | D - Domäne (Domain)                             |
 | (Auf die gesamte Domäne angewandt)              |
 +-------------------------------------------------+
                         |
+----------------------------------------------------+
| OU - Organisationseinheit (Organizational Unit)    |
| (Auf OU-Hierarchie angewandt, gewinnt innerste OU) |
|----------------------------------------------------|
| O - Innerste OU      <- Höchste Priorität          |
+----------------------------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Wichtige Prinzipien der Vererbung
- **Enforced (Erzwingen):** Eine GPO kann auf der übergeordneten Ebene als "erzwungen" (`Enforced`) markiert werden. Dies verhindert, dass untergeordnete GPOs diese Einstellungen überschreiben können.
- **Block Inheritance (Vererbung blockieren):** Eine OU kann die Vererbung von Richtlinien der übergeordneten Domäne blockieren. Allerdings kann eine erzwungene (`Enforced`) GPO diese Blockierung ignorieren.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Erweiterte GPO-Komponenten
#### Administrative Vorlagen (ADMX/ADML)
Der Großteil der konfigurierbaren GPO-Einstellungen wird durch administrative Vorlagen definiert:
- **ADMX-Dateien:** Die sprachneutrale XML-Datei, die die eigentlichen Einstellungen und die Struktur der GPO-Ebenen definiert.
- **ADML-Dateien:** Die sprachspezifische XML-Datei, die die Beschreibungstexte der Einstellungen enthält (z. B. Deutsch oder Englisch).

**Zentraler Speicher (Central Store):**   
Um inkonsistente GPO-Verwaltung zu verhindern, werden die ADMX/ADML-Dateien im **Central Store** auf dem Domänencontroller (innerhalb von `SYSVOL`) gespeichert. Dies stellt sicher, dass alle Administratoren immer die gleiche, aktuelle Ansicht der verfügbaren GPO-Einstellungen haben.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### Gruppenrichtlinien-Einstellungen (Group Policy Preferences - GPP)
Die GPPs sind eine modernere Erweiterung der GPOs und dienen zur Konfiguration von Einstellungen, die *nicht* zwingend "Richtlinien" im engeren Sinne sind.
- **Verwendungszweck:** Verteilen von Netzlaufwerken, Druckern, lokalen **Benutzerkonten, Registry-Einstellungen oder Kopieren von Dateien.
Sicherheitsrisiko:** Historisch enthielten GPPs Passwörter in leicht entschlüsselbarer Form (siehe unten). Dies muss vermieden werden.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Präzise Zuweisung durch Filterung
Obwohl GPOs an OUs, Domänen oder Sites verknüpft werden, kann die tatsächliche Anwendung weiter eingeschränkt werden.
1. **Sicherheitsfilterung**
    - **Funktion:** Steuert, welche **Benutzer oder Gruppen** die GPO überhaupt lesen und anwenden dürfen.
    - **Sicherheitsrelevanz:** Eine GPO sollte nur für die Objekte zugänglich sein, die sie tatsächlich betreffen (Prinzip des **Least Privilege**). Standardmäßig wird die GPO auf die Gruppe "Authentifizierte Benutzer" angewendet.
2. **WMI-Filter (Windows Management Instrumentation)**
    - **Funktion:** Stellt sicher, dass eine GPO nur auf Clients angewendet wird, die bestimmte Kriterien erfüllen (z. B. nur Laptops, nur Windows 10, nur Systeme mit einem bestimmten CPU-Typ).
    - **Syntax:** Basierend auf SQL-ähnlichen Abfragen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Anwendungsfälle für die IT-Sicherheit
GPOs sind ein unverzichtbares Werkzeug für die defensive IT-Sicherheit:
- **Passwortrichtlinien (Domänen-Ebene):** Festlegung von Komplexitätsanforderungen, Mindestlänge (z. B. 14 Zeichen) und maximalem Alter von Passwörtern.
- **Kontosperrungsrichtlinien:** Definieren, nach wie vielen fehlgeschlagenen Anmeldeversuchen ein Konto gesperrt wird (Schutz vor Brute-Force-Angriffen).
- **Software-Einschränkungsrichtlinien (SRP/AppLocker):** Verhindert die Ausführung von unsignierter oder unbekannter Software (wichtig gegen Malware).
- **Sicherheitsvorlagen (Security Templates):** Härtung von Betriebssystemen, indem z. B. unnötige Dienste deaktiviert oder Protokollierungsfunktionen aktiviert werden.
- **Überwachungsrichtlinien (Auditing):** Protokollierung von Anmeldeversuchen, Dateizugriffen und administrativen Änderungen (wichtig für die forensische Analyse).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsrisiken und Angriffe auf GPOs
GPOs sind ein **mächtiges Ziel** – sie bieten zentrale Kontrolle über alle Systeme.
Ein kompromittiertes Konto mit GPO-Schreibrechten = **Domänenweiter Angriff**.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. Privilege Escalation über GPO-Manipulation
Angreifer mit Schreibrechten auf GPOs können:

- Startskripte mit **bösartigem Code** einfügen,

- geplante Tasks verteilen,
- Registry-Werte ändern,
- oder Software auf allen Clients installieren.


**Angriffskette (beispielhaft):**
```text
[Angreifer] ──> [GPO Schreibrechte]
        │
        ▼
[SYSVOL\Policies\{GUID}\Scripts\Startup\evil.ps1]
        │
        ▼
[Clients starten] ──> PowerShell läuft mit SYSTEM-Rechten

```

**Resultat:** Domänenweite Codeausführung mit SYSTEM-Rechten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. SYSVOL-Leak / Password Disclosure
Historisch enthalten manche GPOs **XML-Dateien mit Passwörtern in Klartext**, z. B.:

```text
\\domain\SYSVOL\domain.local\Policies\{GUID}\Machine\Preferences\Groups.xml
```

**PowerShell-Abfrage:**
```powershell
Get-ChildItem "\\domain\SYSVOL" -Recurse | Select-String "password"
```

**Resultat:** Offenlegung administrativer Credentials.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. GPO Replikationsmissbrauch
Manipulation einer GPO auf einem DC repliziert sich automatisch über AD.
```text
+-----------+      AD Replikation     +-----------+
| DC1       |  <------------------->  | DC2       |
| (komprom.)|                         | (intakt)  |
+-----------+                         +-----------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Härtung und Best Practices

| Maßnahme                         | Beschreibung                                                                                                   |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Least Privilege**              | Nur Administratoren (z. B. Domänen-Admins) dürfen GPOs erstellen oder verknüpfen. Separate Admin-Konten für GPO-Management nutzen. |                                             
| **Auditing aktivieren**          | Überwachen von GPO-Änderungen im Sicherheitsprotokoll (z. B. Event ID 5136 im Directory Service). |
| **SYSVOL überwachen**            | Datei-Integritätsüberwachung (FIM) für Skripte und Batch-Dateien im SYSVOL-Ordner.  |
| **Kein Klartext in XML/INI**     | Nutzung von **GPPs** zur Speicherung von Passwörtern ist zu unterlassen. |
| **Signierte PowerShell-Skripte** | Skripte mit Code-Signing-Zertifikaten ausführen. |
| **Zentralisierte Kontrolle**     | Einsatz von **AGPM (Advanced Group Policy Management)** zur Versionskontrolle, Genehmigung von Änderungen und Rollback-Fähigkeiten. |
| **CIS Benchmark anwenden**       | Anwendung von Härtungs-Richtlinien basierend auf etablierten Standards (z. B. **CIS Microsoft Windows GPO Benchmark**). |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fehlerbehebung und Monitoring
Als Administrator muss man in der Lage sein, die Anwendung von GPOs zu überprüfen und Fehler zu beheben.

| Befehl | Zweck |
|--------|-------|
| `gpupdate /force` | Erzwingt die sofortige Aktualisierung der GPOs auf dem Client (anstatt auf das reguläre Intervall zu warten). |
| `gpresult /r` | Zeigt eine Zusammenfassung der angewendeten Gruppenrichtlinien und der Sicherheitsgruppen des Benutzers. |
| `gpresult /h report.html` | Erstellt einen detaillierten HTML-Bericht aller angewendeten GPO-Einstellungen (wichtigstes Werkzeug zur Fehleranalyse). |

Der Befehl `gpresult /h` ist das wichtigste Werkzeug für die Fehlerbehebung, da es genau anzeigt, welche GPO welche Einstellung gesetzt hat und welche GPOs aufgrund von Filtern oder Vererbung ignoriert wurden.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fazit
GPOs sind ein mächtiges, aber zweischneidiges Werkzeug:

- Sie ermöglichen systemweite Härtung, stellen aber bei Fehlkonfiguration oder Kompromittierung ein enormes Risiko dar.

- Eine kompromittierte GPO ist gleichbedeutend mit einer kompromittierten Domäne. Daher müssen GPOs wie kritische Infrastruktur behandelt werden – mit Auditing, striktem Least Privilege und klaren Change-Prozessen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links
- [Wikipedia: Group Policy Object](https://de.wikipedia.org/wiki/Group_Policy_Object)
- [Wikipedia: WMI - Windows Management Instrumentation](https://de.wikipedia.org/wiki/Windows_Management_Instrumentation)
- [Microsoft learn: Gruppenrichtlinien](https://learn.microsoft.com/de-de/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview)
- [Mehr zum Thema Active Directory in dieser Repo](/04-host-security/grundlagen/02_windows_active_directory.md)

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

🗓️ **Letzte Aktualisierung:** Oktober 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---