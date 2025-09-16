# 🐳 Docker auf Kali Linux - Einführung und Installationsanleitung


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Vorteile von Docker](#vorteile-von-docker)
- [Docker vs. VM](#docker-vs-vm)
- [Wichtige Begriffe](#wichtige-begriffe)
- [Docker auf Kali Linux installieren (Debian basiert)](#docker-auf-kali-linux-installieren-debian-basiert)
- [Praktische Anwendungsfälle](#praktische-anwendungsfälle)
- [Docker Befehle für den Einstieg](#docker-befehle-für-den-einstieg)
- [Sicherheitsaspekte bei Docker](#sicherheitsaspekte-bei-docker)
- [Fazit](#fazit)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

**Docker** ist **eine Plattform zur Containerisierung** von Anwendungen. Sie ermöglicht es, Software samt ihrer Abhängigkeiten in sogenannten Containern auszuführen. Diese Container sind leichtgewichtig, portabel und bieten eine isolierte Umgebung für Anwendungen.

In der IT-Sicherheit und im Anwendungsdevelopment ist Docker ein unverzichtbares Werkzeug. Für Pentester bietet es eine saubere und schnelle Möglichkeit, Testumgebungen aufzubauen. Für Entwickler stellt es sicher, dass die Anwendung überall, von der lokalen Maschine bis zur Cloud, in der exakt gleichen Umgebung läuft.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Vorteile von Docker

* **Portabilität:**  Ein Container läuft überall gleich, sei es auf einem lokalen PC, einem Server oder in der Cloud.  
* **Isolation:** Anwendungen laufen getrennt voneinander, was Abhängigkeitskonflikte verhindert und die Sicherheit erhöht.
* **Schnelle Bereitstellung:** Images lassen sich schnell laden und Container in Sekunden starten.
* **Reproduzierbarkeit:** Die gleiche Umgebung kann jederzeit wiederhergestellt werden, was Fehler reduziert und die Zusammenarbeit erleichtert.
* **Ressourcenschonung:** Container sind wesentlich leichtgewichtiger und effizienter als virtuelle Maschinen (VMs), da sie den Kernel des Host-Betriebssystems mitnutzen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Docker vs. VM

Der grundlegende Unterschied liegt in der Abstraktionsebene. Eine VM virtualisiert die gesamte Hardware, einschließlich des Betriebssystems. Ein Container virtualisiert nur die Laufzeitumgebung der Anwendung und teilt sich den Kernel des Host-Systems.

```text
    +-------------------------------------+
    |              HOST-OS                |
    | +----------------+ +----------------+
    | |  VM (Guest-OS) | |  VM (Guest-OS) |
    | | +------------+ | | +------------+ |
    | | | Applikation| | | | Applikation| |
    | | +------------+ | | +------------+ |
    | +----------------+ +----------------+
    +-------------------------------------+
                Virtuelle Maschinen



    +----------------------------------------+
    |              HOST-OS                   |
    | +----------+ +----------+ +----------+ |
    | | Container| | Container| | Container| |
    | | (App)    | | (App)    | | (App)    | |
    | +----------+ +----------+ +----------+ |
    +----------------------------------------+
              Containerisierung
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wichtige Begriffe

|   Begriff      |  Bedeutung                                               |
| -------------- | -------------------------------------------------------- |
| **Image**      | Die statische, unveränderliche Vorlage für einen Container. Ein Image enthält den Code, die Bibliotheken und die Konfigurationen einer Anwendung. |
| **Container**  | Eine laufende Instanz eines Images. Ein Container ist die dynamische, isolierte Umgebung, in der die Anwendung ausgeführt wird. |
| **Dockerfile** | Eine Textdatei, die alle Befehle enthält, die zum Erstellen eines Images benötigt werden. Sie dient als Bauanleitung für das Image. |
| **Registry**   | Ein Speicherort für Docker-Images, ähnlich wie GitHub für Code. Die bekannteste Registry ist Docker Hub. |
| **Volume**     | Ein persistenter Speicher, der außerhalb des Containers existiert. Er wird verwendet, um Daten zu speichern, die über die Lebensdauer eines Containers hinaus erhalten bleiben sollen. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Docker auf Kali Linux installieren (Debian basiert)

### 1. Vorbereitung
Aktualisiere dein System, um sicherzustellen, dass alle Pakete aktuell sind.  
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Benötigte Abhängigkeiten installieren 
Installiere die benötigten Abhängigkeiten (optional).
```bash
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release -y
```
![Docker Installation testen](/09-practice-labs/ressources/pictures/step3installDependencies.png)

### 3. Docker GPG-Schlüssel hinzufügen 
Lade den offiziellen GPG-Schlüssel von Docker herunter und füge ihn deinem System hinzu, um die Echtheit der Pakete zu verifizieren (optional, wenn 2 installiert wurde).

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 4. Docker Repository einrichten 
Füge das offizielle Docker-Repository zu deiner APT-Quellenliste hinzu (optional).

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. Docker installieren
Aktualisiere die Paketliste und installiere die Docker Engine, CLI und Containerd.  
  
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

**oder**
```bash
sudo apt-get install -y docker.io
```

![Docker installieren](/09-practice-labs/ressources/pictures/step4installDocker.png)



### 6. Docker testen

```bash
sudo docker run hello-world
```

![Docker Installation testen](/09-practice-labs/ressources/pictures/step5dockerTest.png)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Optional: Docker ohne sudo verwenden

```bash
sudo usermod -aG docker $USER
newgrp docker
```

> Danach ab- und wieder anmelden.

### Weitere nützliche Docker Add-Ons (optional)

- Docker Compose
    - Docker Compose ist ein Tool für das Definieren von mehreren Container-Umgebungen, die gleichzeitig laufen und untereinander verlinkt sein können.
    - Nützlich, wenn man einzelne Porgramme in Containern starten will. 

So installierst du Docker Compose:

```bash
sudo apt install docker-compose
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Praktische Anwendungsfälle

Als Pentester und Entwickler auf Kali Linux wirst du Docker lieben. Hier sind einige typische Szenarien:

- **Isolierte Lab-Umgebungen:** Starte verwundbare Webanwendungen wie [**DVWA** (**Damn Vulnerable Web Application**)](/09-practice-labs/dvwa-lab/dvwa-lab.md) oder **WebGoat** in einem isolierten Container, ohne dein Host-System zu gefährden.
- **Tool-Isolation:** Installiere und teste unsichere oder nicht offizielle Hacking-Tools in einem separaten Container, um dein Hauptsystem sauber zu halten.
- **Reproduzierbare Exploits:** Baue eine exakte Umgebung mit einem bestimmten Betriebssystem und einer bestimmten Softwareversion nach, um einen Exploit präzise zu testen.
- **Multi-Container-Setups:** Mit **Docker Compose** kannst du komplexe Umgebungen, wie eine Webanwendung mit einer Datenbank und einem Redis-Cache, mit einem einzigen Befehl starten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Docker Befehle für den Einstieg

|   Befehl                         |  Beschreibung                        |
| -------------------------------- | ------------------------------------ |
| `docker run image`               | Startet einen neuen Container aus einem Image. |
| `docker ps`                      | Zeigt alle **laufenden** Container an. |
| `docker ps -a`                   | Zeigt **alle** Container an (laufende und gestoppte). |
| `docker images`                  | Listet alle lokal gespeicherten Images auf. |
| `docker pull <image-name>`       | Lädt ein Image aus einer Registry herunter. |
| `docker exec -it <container_id> /bin/bash` | Startet eine interaktive Shell im laufenden Container. Sehr nützlich für die Fehlerbehebung. |
| `docker stop <container_id>`     | Stoppt einen laufenden Container. |
| `docker rm <container_id>`       | Löscht einen gestoppten Container. |
| `docker rmi <image_id>`          | Löscht ein lokales Image. |
| `docker login`                   | Meldet sich an einer Docker Registry an. |
| `docker build -t <tag> `         | Erstellt ein Image aus einem Dockerfile im aktuellen Verzeichnis. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsaspekte bei Docker

### 1. Root-Rechte
Docker benötigt standardmäßig Root-Rechte, um die Container-Engine zu verwalten. Dies ist ein großes Sicherheitsrisiko, da ein Fehler in Docker oder ein Missbrauch durch einen kompromittierten Benutzer weitreichende Folgen für das Host-System haben kann.
- **Empfehlung:** Verwende immer einen dedizierten, nicht-privilegierten Benutzer und füge ihn der `docker`-Gruppe hinzu, um `sudo` bei Docker-Befehlen zu vermeiden.

### 2. Container-Isolation: 
Stark, aber nicht so sicher wie echte VMs.  
Obwohl die Isolation von Containern robust ist, ist sie nicht so absolut wie bei einer VM. Wenn eine Schwachstelle im Linux-Kernel selbst existiert, könnte ein Angreifer aus dem Container ausbrechen (**Container Escape**) und auf das Host-System zugreifen.

- **Netzwerkzugriff:** Container können auf das Netzwerk zugreifen -> Firewall wichtig.
- **Image-Vertrauen:** Nur Images aus vertrauenswürdigen Quellen verwenden.

### 3 Images vertrauen
Lade niemals Images von unbekannten oder nicht vertrauenswürdigen Quellen herunter. Solche Images könnten bösartigen Code enthalten, der dein System kompromittiert, sobald du den Container startest.
- **Empfehlung:** Verwende Images von offiziellen Repositories auf Docker Hub oder erstelle deine eigenen Images.

### 4. Konfiguration und Zugriffsrechte

Achte darauf, wie du Container konfigurierst. Setze keine unnötigen Privilegien (`--privileged`) und mounte keine sensiblen Host-Verzeichnisse in den Container (`-v /etc:/etc`). Dies könnte es einem Angreifer ermöglichen, auf kritische Systemdateien zuzugreifen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fazit

Docker ist ein extrem nützliches Werkzeug, um Anwendungen und Tools **schnell**, **sicher** und **portabel** bereitzustellen. Besonders für **Pentester** und **Entwickler** auf Kali Linux bietet Docker eine Möglichkeit, komplexe Tools oder komplette Lab-Umgebungen in isolierten Containern zu betreiben – ohne das Basissystem zu verunreinigen.

**Tipp:** Verwende Docker in Kombination mit `docker-compose`, um mehrere Container gleichzeitig (z. B. **Web** + **DB**) zu starten und ihre Konfiguration in einer einzigen, reproduzierbaren Datei zu verwalten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Docker Website](https://www.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Dokumentation](https://docs.docker.com/)
- [Offizielle Docker-Installations-Anleitung](https://docs.docker.com/engine/install/debian/)


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
