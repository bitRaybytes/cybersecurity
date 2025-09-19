# 💻 Virtuelle Maschinen – Grundlagen und Sicherheit
## Inhaltsverzeichnis

- [Was ist Virtualisierung?](#was-ist-virtualisierung)
- [Der Hypervisor: Das Herzstück](#der-hypervisor-das-herzstück)
- [Typen von Hypervisoren](#typen-von-hypervisoren)
- [Architektur und Visualisierung](#architektur-und-visualisierung)
- [Sicherheitsrisiken und Abwehrmaßnahmen](#sicherheitsrisiken-und-abwehrmaßnahmen)
- [Haftungsausschluss](#haftungsausschluss)

## Was ist Virtualisierung?
**Virtualisierung** ist eine Technologie, die es ermöglicht, eine einzige physische Hardware-Ressource (z. B. einen physischen Server) in mehrere isolierte, logische Einheiten aufzuteilen. Jede dieser logischen Einheiten, eine **Virtuelle Maschine** (**VM**), verhält sich wie ein eigenständiger Computer, der ein eigenes Betriebssystem (Guest OS) und eigene Anwendungen ausführen kann.

Das Hauptziel der Virtualisierung ist die **Effizienz**: Sie ermöglicht es, die Auslastung der Hardware zu maximieren, Kosten zu senken und die Verwaltung zu vereinfachen. Für die IT-Sicherheit bietet Virtualisierung zudem einzigartige Möglichkeiten zur Isolation und zum **Sandboxing**.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der Hypervisor: Das Herzstück
Der **Hypervisor**, auch als **Virtual Machine Monitor** (**VMM**) bekannt, ist die zentrale Software, die die Virtualisierung ermöglicht. Er erstellt und verwaltet die virtuellen Maschinen und stellt sicher, dass sie sich die physischen Ressourcen der darunterliegenden Hardware (CPU, RAM, Speicher) teilen können, ohne sich gegenseitig zu stören.

Der Hypervisor fungiert als Vermittler zwischen den VMs und der physischen Hardware. Er ist die wichtigste Sicherheitsgrenze und schützt die VMs voreinander und vor der zugrunde liegenden Hardware.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Typen von Hypervisoren
Man unterscheidet zwischen zwei Haupttypen von Hypervisoren, die jeweils unterschiedliche Architekturen und Sicherheitsmodelle aufweisen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Typ 1: Bare-Metal-Hypervisor
Ein **Bare-Metal-Hypervisor** wird direkt auf der physischen Hardware (dem **Bare Metal**) installiert, ohne dass ein separates Host-Betriebssystem erforderlich ist. Er hat direkten Zugriff auf die Hardware-Ressourcen.

- **Sicherheitsvorteil:** Da der Hypervisor direkt auf der Hardware läuft und keine weitere Schicht (Host OS) dazwischen liegt, ist die Angriffsfläche im Vergleich zu Typ-2-Hypervisoren deutlich geringer. Dies macht ihn zur bevorzugten Wahl für Rechenzentren und Cloud-Computing-Umgebungen. Beispiele hierfür sind **VMware ESXi**, **Microsoft Hyper-V** und **KVM**.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Typ 2: Hosted-Hypervisor
Ein **Hosted-Hypervisor** läuft als Anwendung auf einem bereits installierten Host-Betriebssystem (z. B. Windows, macOS oder Linux). Die VMs laufen somit auf einer weiteren Abstraktionsebene.

- **Sicherheitsnachteil:** Der Hypervisor und die VMs sind von der Sicherheit des Host-Betriebssystems abhängig. Wenn das Host OS kompromittiert wird, können alle darauf laufenden VMs gefährdet sein. Beispiele sind **VirtualBox** und **VMware Workstation**. Dieser Typ eignet sich eher für Entwicklungs- oder Testumgebungen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Architektur und Visualisierung
### Typ 1: Bare-Metal-Hypervisor Architektur
```text
+-------------------------------------------------+
|              Virtuelle Maschinen (VMs)          |
+-------------------------------------------------+
|               VM 1 | VM 2 | VM 3 | ...          |
+-------------------------------------------------+
|                 Hypervisor Typ 1                |
+-------------------------------------------------+
|           Physische Hardware (Bare Metal)       |
+-------------------------------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Typ 2: Hosted-Hypervisor Architektur
```text
+-------------------------------------------------+
|              Virtuelle Maschinen (VMs)          |
+-------------------------------------------------+
|               VM 1 | VM 2 | VM 3 | ...          |
+-------------------------------------------------+
|                 Hypervisor Typ 2                |
+-------------------------------------------------+
|                Host-Betriebssystem              |
+-------------------------------------------------+
|                 Physische Hardware              |
+-------------------------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Sicherheitsrisiken und Abwehrmaßnahmen
### 1. VM-Sprawl (Verwilderung)
Ein unkontrolliertes Wachstum der VMs im Netzwerk. Dies führt zu mangelndem Überblick, vergessenen, ungepatchten Systemen und überflüssigen Diensten, die Angriffsvektoren darstellen können.

Abwehrmaßnahme: Implementiere eine strenge VM-Governance-Strategie. Führe regelmäßige Audits durch und verwende automatisierte Tools zur Verwaltung und Entfernung nicht benötigter VMs.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. VM-Escaping
Dies ist der Super-GAU in der Virtualisierung. Ein Angreifer, der die Kontrolle über eine VM erlangt hat, bricht aus der virtuellen Umgebung aus und greift den Hypervisor oder andere VMs an.

- **Abwehrmaßnahme:** Halte den Hypervisor, das Host OS und die VMs stets mit den neuesten Patches auf dem aktuellen Stand. Implementiere eine strikte Netzwerktrennung und verwende Sicherheitslösungen, die speziell für virtuelle Umgebungen entwickelt wurden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. Kompromittierung des Hypervisors
Wenn der Hypervisor selbst Schwachstellen aufweist, kann ein Angreifer die vollständige Kontrolle über alle darauf laufenden VMs erlangen.

- **Abwehrmaßnahme:** Setze auf eine "**Hardening**"-Strategie für den Hypervisor. Deaktiviere alle unnötigen Dienste und Schnittstellen und sorge für eine minimale Konfiguration.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Netzwerk-Isolation
Der Datenverkehr zwischen VMs auf demselben Host wird oft nicht von herkömmlichen Firewalls überwacht, da er intern abgewickelt wird.

- **Abwehrmaßnahme:** Verwende **virtuelle Firewalls** und **Netzwerk-Mikrosegmentierung**, um den Datenverkehr zwischen VMs zu kontrollieren und zu protokollieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 5. Sichere Konfiguration
Eine unsachgemäße Konfiguration, z. B. zu viele Administrator-Rechte oder Standard-Passwörter, kann die gesamte Umgebung gefährden.

- **Abwehrmaßnahme:** Verwende das **Prinzip der geringsten Rechte** (**Least Privilege**) und sorge für die korrekte Konfiguration aller virtuellen Umgebungen.


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