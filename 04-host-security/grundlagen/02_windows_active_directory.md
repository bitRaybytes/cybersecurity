# 💻 Active Directory – Grundlagen und Sicherheit

![Windows OS](https://img.shields.io/badge/Windows%20OS-%23334155.svg?style=for-the-badge&logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-%2300A86B.svg?style=for-the-badge&logo=microsoft&logoColor=white)


## Inhaltsverzeichnis
- [Was ist Active Directory?](#was-ist-active-directory)
- [Grundkonzepte: Objekte und Attribute](#grundkonzepte-objekte-und-attribute)
- [Die Rolle des Domain Controllers](#die-rolle-des-domain-controllers)
- [Aufbau des Active Directory](#aufbau-des-active-directory)
- [Zwei Schlüsselkonzepte der Sicherheit](#zwei-schlüsselkonzepte-der-sicherheit)
- [Wichtige Abwehrmechanismen](#wichtige-abwehrmechanismen)
- [Gefahren und Angriffsvektoren](#gefahren-und-angriffsvektoren)
- [Haftungsausschluss](#haftungsausschluss)


## Was ist Active Directory?
**Active Directory** (**AD**) ist ein Verzeichnisdienst von Microsoft, der als zentrale Datenbank für die Verwaltung von Ressourcen und Identitäten in einem Netzwerk dient. Es organisiert alle relevanten Informationen über die IT-Infrastruktur – von Benutzern und Computern über Drucker und Anwendungen bis hin zu Gruppen und Standorten.

Das übergeordnete Ziel ist es, die Verwaltung zu zentralisieren und zu vereinfachen. Das Active Directory ist somit das Nervenzentrum jeder Windows-basierten Umgebung und spielt eine entscheidende Rolle für die **Autorisierung** und **Authentifizierung** von Nutzern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundkonzepte: Objekte und Attribute
Im Active Directory wird alles als **Objekt** betrachtet. Jedes Objekt ist eine Ressource innerhalb des Netzwerks, z. B. ein Benutzer, ein Computer, eine Gruppe oder eine Organisationseinheit (OU). Jedes dieser Objekte hat spezifische **Attribute**, die seine Eigenschaften beschreiben, wie zum Beispiel den Namen, die E-Mail-Adresse oder die Abteilung eines Benutzers.

- **Objekte** werden durch eine eindeutige **SID** (**Security Identifier**) identifiziert, die es dem System ermöglicht, Berechtigungen exakt zuzuweisen.

- **Gruppen** sind eine Sammlung von Objekten und dienen dazu, Berechtigungen effizient zu verwalten. Anstatt jedem Benutzer einzeln Zugriff zu gewähren, weist man die Berechtigung der Gruppe zu, und alle Gruppenmitglieder erben sie automatisch.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die Rolle des Domain Controllers
Der **Domain Controller** (**DC**) ist der zentrale Server in einer Active-Directory-Domäne. Er speichert eine Kopie der AD-Datenbank und ist für folgende Aufgaben zuständig:

- **Authentifizierung:** Überprüft die Identität von Benutzern und Computern beim Anmeldevorgang.

- **Autorisierung:** Gewährt oder verweigert den Zugriff auf Ressourcen, basierend auf den Berechtigungen der Objekte.

- **Zentrale Verwaltung:** Stellt sicher, dass alle Änderungen an Objekten und Richtlinien innerhalb der Domäne repliziert und durchgesetzt werden.

Ohne den Domain Controller könnten sich Benutzer nicht am Netzwerk anmelden und nicht auf die freigegebenen Ressourcen zugreifen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Aufbau des Active Directory
Die Struktur des Active Directory ist sowohl **logisch** als auch **hierarchisch** aufgebaut. Dies ermöglicht eine flexible Organisation, die sich an der Struktur eines Unternehmens orientiert.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 1. Organisationseinheiten (OUs)
Eine **Organisationseinheit** (**OU**) ist eine logische Unterteilung innerhalb einer Domäne. Sie dient als Container für Benutzer, Gruppen und andere OUs. OUs spiegeln oft die Abteilungs- oder geografische Struktur eines Unternehmens wider (z. B. eine OU für die „Marketing-Abteilung“ oder den „Standort Hamburg“).

- **Sicherheitsrelevanz:** Berechtigungen und Gruppenrichtlinien können direkt auf eine OU angewendet werden und gelten automatisch für alle enthaltenen Objekte. Dies macht die Verwaltung von Berechtigungen deutlich sicherer und effizienter.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Domäne (Domain)
Eine Domäne ist die grundlegende Verwaltungseinheit im Active Directory. Sie bildet eine logische Sicherheitsgrenze, innerhalb derer alle Objekte eine gemeinsame Datenbank und Sicherheitsrichtlinien teilen. Eine Domäne wird durch einen Domain Controller verwaltet und identifiziert.

- Der **FQDN** (**Fully Qualified Domain Name**), z. B. `firma.local`, ist der eindeutige Name der Domäne.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Domänenbaum (Domain Tree)
Ein **Domänenbaum** ist eine Sammlung von Domänen, die eine zusammenhängende, hierarchische Namensstruktur teilen. Beispielsweise gehören die Domänen `hamburg.firma.local` und `berlin.firma.local` zum selben Domänenbaum, da sie die gemeinsame Stamm-Domäne (`firma.local`) haben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Gesamtstruktur (Forest)
Die **Gesamtstruktur** (**Forest**) ist die oberste Hierarchieebene. Sie ist eine Sammlung von einem oder mehreren Domänenbäumen, die keine gemeinsame Namensstruktur haben müssen, aber über eine gemeinsame Konfiguration und ein Schema verfügen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Visualisierung der AD-Struktur

```text
(Forest)
+------------------------------------------------------------------+
|                                                                  |
|   (Domain Tree 1)                       (Domain Tree 2)          |
|   +--------------------------+          +----------------------+ |
|   |   (Domain: firma.local)  |          | (Domain: partner.com)| |
|   |                          |          |                      | |
|   | +------------------+     |          +----------------------+ |
|   | |  (OU: Marketing) |     |                                   |
|   | |  +-------------+ |     |                                   |
|   | |  | Benutzer A  | |     |                                   |
|   | |  +-------------+ |     |                                   |
|   | |  | Gruppe XYZ  | |     |                                   |
|   | |  +-------------+ |     |                                   |
|   | +------------------+     |                                   |
|   | | (OU: IT)         |     |                                   |
|   | +------------------+     |                                   |
|   | | (DC)             |     |                                   |
|   +--------------------------+                                   |
|                                                                  |
+------------------------------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Zwei Schlüsselkonzepte der Sicherheit

### 1. Authentifizierung (Wer bist du?)
Der Prozess, bei dem die Identität eines Benutzers überprüft wird. Im Active Directory geschieht dies primär durch das **Kerberos-Protokoll**, das eine sichere Überprüfung mittels kryptografischer Tickets durchführt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Autorisierung (Was darfst du?)
Der Prozess, der festlegt, welche Aktionen ein authentifizierter Benutzer ausführen darf. Autorisierung basiert auf den Berechtigungen, die einem Benutzer oder einer Gruppe zugewiesen sind. Hierbei sind vor allem die **Security Identifiers** (**SIDs**) von Objekten relevant, die in den Zugriffssteuerungslisten (ACLs) von Ressourcen verwendet werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Gruppenrichtlinien (Group Policy Objects - GPOs)
**GPOs** sind die mächtigsten Werkzeuge zur zentralen Konfiguration und Absicherung von AD-Umgebungen. Sie ermöglichen es Administratoren, nahezu jeden Aspekt von Benutzern und Computern zu steuern – von der Passwortrichtlinie über die Sperrung von USB-Anschlüssen bis hin zur Installation von Software. Hier erfährst du [mehr zum Thema GPO](/04-host-security/grundlagen/03_windows_group_policy_object_gpo_grundlagen.md)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wichtige Abwehrmechanismen
### 1. Least Privilege (Prinzip der geringsten Rechte)
Gewähre Benutzern nur die absolut notwendigen Berechtigungen, um ihre Aufgaben zu erfüllen. Dies minimiert die Angriffsfläche erheblich.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Strenge Passwortrichtlinien
Setze GPOs ein, um die Komplexität, Länge und den Ablauf von Passwörtern durchzusetzen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Multifaktor-Authentifizierung (MFA)
Eine zusätzliche Sicherheitsebene, die die Gefahr von gestohlenen Anmeldeinformationen reduziert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Gefahren und Angriffsvektoren
Angreifer zielen auf das Active Directory, um die Kontrolle über das gesamte Netzwerk zu erlangen. Häufige Angriffsvektoren sind:

- **Pass-the-Hash/Ticket-Angriffe:** Angreifer nutzen gestohlene Hashes oder Kerberos-Tickets, um sich als legitime Benutzer auszugeben.

- **AD-Reconnaissance:** Angreifer durchsuchen das AD, um Administratoren, verwundbare Konten und wertvolle Ressourcen zu identifizieren.

- **Unzureichende Privilegienverwaltung:** Übermäßig viele Benutzer mit administrativen Rechten sind ein häufiges Problem.




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