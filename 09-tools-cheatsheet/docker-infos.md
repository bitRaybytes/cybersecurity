# 🐳 Docker auf Kali Linux - Einführung und Installationsanleitung

## 📘 Einleitung

Docker ist eine Plattform zur Containerisierung von Anwendungen. Sie ermöglicht es, Software samt ihrer Abhängigkeiten in sogenannten **Containern** auszuführen. Diese Container sind leichtgewichtig, portabel und bieten eine isolierte Umgebung für Anwendungen.

In der IT-Sicherheit 🛡️ und DevOps ⚙️ wird Docker oft genutzt, um Testumgebungen zu erstellen, Tools zu isolieren oder Software reproduzierbar auszuführen.

---

## 🚀 Vorteile von Docker

* ✅ **Portabilität:** Container laufen überall gleich (lokal, Server, Cloud)
* 🔐 **Isolation:** Anwendungen laufen getrennt voneinander
* ⚡ **Schnelle Bereitstellung:** Images lassen sich schnell laden und starten
* ♻️ **Reproduzierbarkeit:** Gleiche Umgebung für Dev, Test und Prod
* 🪶 **Ressourcenschonung:** Container sind leichtgewichtiger als VMs

---

## 🧠 Wichtige Begriffe

| 📌 Begriff     | 💬 Bedeutung                                             |
| -------------- | -------------------------------------------------------- |
| **Image**      | Vorlage für einen Container                              |
| **Container**  | Laufzeitinstanz eines Images                             |
| **Dockerfile** | Datei zur Beschreibung, wie ein Image aufgebaut ist      |
| **Registry**   | Ort, an dem Images gespeichert werden (z. B. Docker Hub) |

---

## 🛠️ Docker auf Kali Linux installieren (Debian basiert)

### 1️⃣ System aktualisieren

```bash
sudo apt update && sudo apt upgrade -y
```

### 2️⃣ Benötigte Abhängigkeiten installieren 
(optional)

```bash
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release -y
```

### 3️⃣ Docker GPG-Schlüssel hinzufügen 
(optional, wenn 2 installiert wurde)

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 4️⃣ Docker Repository einrichten 
(optional)

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/nulldocker
```

### 5️⃣ Docker installieren

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

**oder**
```bash
sudo apt-get install -y docker.io
```

🖼️ ![Docker installieren](/cybersercurity/10-practice-labs/dvwa-lab/dvwa-ressources/pictures/step4installDocker.png)

#### weitere nützliche Docker Add-Ons (optional)

- Docker Compose
    - Docker Compose ist ein Tool für das Definieren von mehreren Container-Umgebungen, die gleichzeitig laufen und untereinander verlinkt sein können.
    - Nützlich, wenn man einzelne Porgramme in Containern starten will. 

So installierst du Docker Compose:

```bash
sudo apt install docker-compose
```

### 6️⃣ Docker testen 🧪

```bash
sudo docker run hello-world
```

🖼️ ![Docker Installation testen](/cybersercurity/10-practice-labs/dvwa-lab/dvwa-ressources/pictures/step5dockerTest.png)

### 🔓 Optional: Docker ohne sudo verwenden

```bash
sudo usermod -aG docker $USER
newgrp docker
```

> ℹ️ Danach ab- und wieder anmelden.

---

## 🧰 Docker Befehle für den Einstieg

| 🧾 Befehl                        | 🛠️ Beschreibung                     |
| -------------------------------- | ------------------------------------ |
| `docker run image`               | Container starten                    |
| `docker ps`                      | Laufende Container anzeigen          |
| `docker ps -a`                   | Alle Container anzeigen              |
| `docker images`                  | Lokale Images anzeigen               |
| `docker pull image`              | Image herunterladen                  |
| `docker exec -it <id> /bin/bash` | Shell in laufenden Container starten |
| `docker stop <id>`               | Container stoppen                    |
| `docker rm <id>`                 | Container löschen                    |

---

## 🔐 Sicherheitsaspekte bei Docker

* ⚠️ **Root-Rechte:** Standardmäßig benötigt Docker Root-Rechte → Sicherheitsrisiko
* 🧱 **Container-Isolation:** Stark, aber nicht so sicher wie echte VMs
* 🌐 **Netzwerkzugriff:** Container können auf das Netzwerk zugreifen → Firewall wichtig
* 🛡️ **Image-Vertrauen:** Nur Images aus vertrauenswürdigen Quellen verwenden

---

## 🧩 Fazit

Docker ist ein extrem nützliches Werkzeug, um Anwendungen und Tools **schnell**, **sicher** und **portabel** bereitzustellen. Besonders für **Pentester** 🕵️‍♂️ und **Entwickler** 👨‍💻 auf Kali Linux bietet Docker eine Möglichkeit, komplexe Tools oder komplette Lab-Umgebungen in isolierten Containern zu betreiben – ohne das Basissystem zu verunreinigen.

---

💡 **Tipp:** Verwende Docker in Kombination mit `docker-compose`, um mehrere Container gleichzeitig (z. B. Web + DB) zu starten 📦 ➕ 🗃️.

## Nützliche Links
- [Docker Website](https://www.docker.com/)

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/cybersercurity/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---