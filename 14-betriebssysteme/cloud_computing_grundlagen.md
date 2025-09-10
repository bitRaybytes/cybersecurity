# ☁️ Cloud Computing & Edge Computing: Grundlagen

## Inhaltsverzeichnis
- [Was ist Cloud Computing?](#was-ist-cloud-computing)
- [Wichtige Merkmale](#wichtige-merkmale)
- [Cloud-Dienstmodelle (SaaS, PaaS, IaaS)](#cloud-dienstmodelle-saas-paas-iaas)
- [Cloud-Bereitstellungsmodelle](#cloud-bereitstellungsmodelle)
- [Vorteile und Nachteile](#vorteile-und-nachteile)
- [Edge & Fog Computing: Die Ergänzung der Cloud](#edge--fog-computing-die-ergänzung-der-cloud)
- [Vergleich Cloud vs. Edge vs. Fog](#vergleich-cloud-vs-edge-vs-fog)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## Was ist Cloud Computing?

**Cloud Computing** ermöglicht den Zugriff auf IT-Ressourcen (z. B. Server, Speicher, Datenbanken, Software) über das Internet, anstatt sie lokal zu betreiben. Es handelt sich um eine Verlagerung der Rechenleistung von einem lokalen System in ein entferntes, zentralisiertes Rechenzentrum, die "**Cloud**".

```text
+-------------+                          +-------------------+
|  Dein Gerät |------------------------->| Die Cloud         |
+-------------+        Internet          |  (Rechenzentrum)  |
                                         +-------------------+
                                                    |
                                                    v
                                        Speicher, Server, Datenbanken
```

## Wichtige Merkmale

- **On-Demand-Zugriff:** Ressourcen können nach Bedarf in Echtzeit bereitgestellt und skaliert werden.
- **Pay-as-you-go:** Die Abrechnung erfolgt nutzungsbasiert, d. h., du zahlst nur für das, was du wirklich nutzt (z. B. Datenvolumen oder Rechenzeit).
- **Ortsunabhängigkeit:** Der Zugriff auf die Ressourcen ist von überall mit einer Internetverbindung möglich.
- **Skalierbarkeit:** Leistungsressourcen lassen sich flexibel und oft automatisiert anpassen, um Lastspitzen abzufangen.
- **Hohe Verfügbarkeit:** Cloud-Anbieter garantieren in der Regel eine hohe Ausfallsicherheit und bieten integrierte Daten-Backups.

## Cloud-Dienstmodelle (SaaS, PaaS, IaaS)

Cloud-Dienste werden in verschiedene Modelle unterteilt, je nachdem, wie viel Kontrolle der Nutzer hat.

1. **IaaS (Infrastructure as a Service):**

    - Stellt die grundlegende IT-Infrastruktur bereit: virtuelle Server, Speicher und Netzwerke.

    - Der Nutzer verwaltet Betriebssysteme und Anwendungen selbst.

    - **Beispiele:** AWS EC2, Microsoft Azure VMs, Google Compute Engine.

2. **PaaS (Platform as a Service):**

    - Bietet eine Entwicklungsumgebung und Frameworks zur Erstellung von Anwendungen.

    - Die zugrundeliegende Infrastruktur wird vom Anbieter verwaltet.

    - **Beispiele:** Google App Engine, Heroku, AWS Elastic Beanstalk.

3. **SaaS (Software as a Service):**

    - Stellt fertige Anwendungen über das Internet zur Verfügung.

    - Der Nutzer greift nur auf die Software zu, die gesamte Verwaltung liegt beim Anbieter.

    - **Beispiele:** Google Workspace, Microsoft 365, Salesforce.

4. **DaaS (Desktop as a Service):**

    - Stellt komplette Betriebssysteme (virtuelle Desktops) als Cloud-Dienst bereit.

    - **Beispiel:** Citrix Virtual Apps and Desktops.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Cloud-Bereitstellungsmodelle

Je nachdem, wer die Cloud betreibt und wie sie zugänglich ist, unterscheidet man verschiedene Bereitstellungsmodelle:

- **Public Cloud:** Dienste werden von einem Drittanbieter öffentlich und über das Internet bereitgestellt. (z. B. Amazon AWS, Microsoft Azure, Google Cloud).

- **Private Cloud**: Exklusive Cloud-Lösung für eine einzige Organisation. Sie kann im eigenen Rechenzentrum oder bei einem Anbieter gehostet werden.

- **Hybrid Cloud:** Eine Kombination aus Public und Private Cloud, um von der Flexibilität der Public Cloud und der Sicherheit der Private Cloud zu profitieren.

- **Community Cloud:** Eine Cloud-Infrastruktur, die von mehreren Organisationen mit gemeinsamen Interessen (z. B. Forschung, Regierung) gemeinsam genutzt wird.


## Vorteile und Nachteile

| **Vorteile** | **Nachteile** |
|--------------|---------------|
| Geringe Anfangskosten (keine Hardware-Käufe) | Laufende monatliche Kosten |
| Hohe Skalierbarkeit und Flexibilität | Abhängigkeit von einer stabilen Internetverbindung |
| Reduzierter Wartungsaufwand | Datenschutz- und Sicherheitsrisiken |
| Ortsunabhängiger Zugriff | Anbieter-Abhängigkeit (Vendor Lock-in) |
| Hohe Verfügbarkeit und Ausfallsicherheit | Fehlende Kontrolle über die physische Hardware |

## Edge & Fog Computing: Die Ergänzung der Cloud

**Cloud Computing** stößt bei Anwendungen, die eine extrem niedrige Latenz oder eine schnelle, lokale Datenverarbeitung erfordern (z. B. IoT, Echtzeit-Analyse), an seine Grenzen. Hier kommen **Edge** und **Fog Computing** ins Spiel. Sie verlagern die Datenverarbeitung näher an den Ort der Entstehung.

- **Edge Computing:** Daten werden direkt am "Rand" des Netzwerks verarbeitet – also am Endgerät selbst oder in dessen unmittelbarer Nähe. Dies minimiert die Latenz und reduziert den Bandbreitenbedarf. Typisch für IoT-Geräte.

- **Fog Computing:** Eine Zwischenschicht zwischen den Endgeräten (Edge) und der Cloud. Fog-Nodes verarbeiten Daten teilweise lokal, leiten aber komplexere oder aggregierte Daten zur weiteren Verarbeitung an die zentrale Cloud weiter.

## Vergleich Cloud vs. Edge vs. Fog


| Merkmal | Cloud Computing | Edge  Computing | Fog Computing | 
|---------|------------------|------|-----------|
| **Datenverarbeitung** | Zentralisiert im Rechenzentrum | Dezentral am Endgerät | Zwischenschicht |
| **Latenz** | Abhängig von der Entfernung | **Sehr gering** (lokal) | **Niedrig** (lokale Knoten) |
| **Bandbreitenbedarf** | Hoch | Gering | Mittel - nur wichtige Daten |
| **Echtzeitfähigkeit** | Eingeschränkt | **Optimal** | Gut für zeitkritische Anwendungen |
| **Komplexität** | Einfach | Hoch (Hardware nötig) | Mittel (Management von Nodes) |
| **Szenario** | Speicherung, Big Data | Echtzeit-Analyse, IoT | Smart Grids, Telemedizin |

## Nützliche Links
- [https://de.wikipedia.org/wiki/Cloud_Computing](https://de.wikipedia.org/wiki/Cloud_Computing)
- [https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Empfehlungen-nach-Angriffszielen/Cloud-Computing/Grundlagen/grundlagen_node.html](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Empfehlungen-nach-Angriffszielen/Cloud-Computing/Grundlagen/grundlagen_node.html)

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