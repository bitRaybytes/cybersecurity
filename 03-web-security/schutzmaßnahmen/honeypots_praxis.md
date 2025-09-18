# 🍯 Honeypot-Praxis – Aufbau und Absicherung

## Inhaltsverzeichnis
-  [Die Grundlage: Das Design und die Vorbereitung](#die-grundlage-das-design-und-die-vorbereitung)
-  [Tools und ihre Rolle im Aufbau](#tools-und-ihre-rolle-im-aufbau)
-  [Anleitung: Dein beispielhafter Cowrie-Honeypot](#anleitung-dein-beispielhafter-cowrie-honeypot)
-  [Der Sprung zum Honeynet](#der-sprung-zum-honeynet)
-  [Absicherung – Das A und O](#absicherung--das-a-und-o)
-  [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die Grundlage: Das Design und die Vorbereitung
Bevor du mit der Installation startest, überleg dir genau, was du erreichen willst. Ein professioneller Honeypot ist kein Zufallsprodukt, sondern eine wohlüberlegte Falle. Frag dich:

- **Was willst du fangen?** Willst du nur einfache Port-Scans sehen oder echte Angriffe auf deine Web-Anwendungen analysieren?

- **Welche Daten sind für dich wertvoll?** Reichen dir IP-Adressen oder brauchst du die genauen Befehle und Malware-Samples der Angreifer?

- **Wie viele Ressourcen hast du?** Ein kleiner Honeypot auf einer VM ist super für den Anfang, aber ein ganzes Honeynet braucht mehr Power.

Die Entscheidung zwischen einem **Low-Interaction-** und **High-Interaction-Honeypot** ist dein erster großer Schritt. Denk dran, das Ziel ist es, den Angreifer so lange wie möglich zu beschäftigen, ohne ihm zu viel preiszugeben.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tools und ihre Rolle im Aufbau
Das richtige Werkzeug macht den Unterschied. Die meisten Profi-Setups nutzen eine Kombination aus mehreren Tools, die in einem Honeynet zusammenarbeiten.

| Tool | Was es kann |
|------|-------------|
| **Cowrie** | Dein SSH/Telnet-Honeypot. Ideal, um Angriffe auf Linux-Server zu simulieren. Sammelt alle Shell-Befehle und hochgeladene Dateien. |
| **Dionaea** | Dein Malware-Honeypot. Fängt Exploits und Malware über Dienste wie SMB, FTP oder HTTP ab. Ein echter Magnet für automatisierte Angriffe. |
| **Glastopf** | Dein Web-Honeypot. Emuliert typische Web-Schwachstellen (LFI, SQL-Injection), um Web-Scanner in die Irre zu führen. |
| **T-Pot** | Das Schweizer Taschenmesser unter den Honeynets. Vereint Dutzende Honeypots (darunter Cowrie, Dionaea, etc.) in einem einfach zu installierenden Docker-Container. Perfekt für einen schnellen, professionellen Start. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Anleitung: Dein beispielhafter Cowrie-Honeypot
Diese Anleitung zeigt dir, wie du einen Low- bis Medium-Interaction-Honeypot mit Cowrie aufbaust. Es ist eine perfekte Blaupause, um zu verstehen, wie alles funktioniert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. System-Vorbereitung
Starte mit einer frischen, sauberen virtuellen Maschine (VM) oder einem Cloud-Server. Ganz wichtig: Dieses System muss komplett vom Rest deines Netzwerks isoliert sein!

- **OS:** Ubuntu Server 22.04 LTS oder Debian

- **Hardware:** 1 GB RAM, 1 CPU Core, 20 GB Speicher


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Isolation gewährleisten
- Stell sicher, dass die VM in einem **isolierten Netzwerksegment** (**VLAN**) oder einer DMZ hängt. Sie darf keinen Zugriff auf dein internes Netzwerk haben.

- Konfiguriere deine Firewall so, dass der Honeypot-Server nur ausgehenden Traffic zu einem zentralen Log-Server herstellen kann.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. Installation von Cowrie
Cowrie ist in Python geschrieben. Am einfachsten ist es, wenn du es direkt vom Git-Repository klonst.

```bash
# System aktualisieren
sudo apt update && sudo apt upgrade -y

# Abhängigkeiten installieren
sudo apt install git python3-pip virtualenv -y

# Cowrie klonen
git clone [http://github.com/cowrie/cowrie.git](http://github.com/cowrie/cowrie.git)
cd cowrie

# Virtuelle Umgebung erstellen und aktivieren
virtualenv cowrie-env
source cowrie-env/bin/activate

# Cowrie installieren
pip3 install -r requirements.txt
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 4. Konfiguration
Die wichtigste Datei ist cowrie.cfg. Hier kannst du das Verhalten deines Honeypots anpassen.

- **Simulierte IP:** Ändere den Standard-SSH-Port (`22`) auf einen ungewöhnlichen Port (`2222`), um automatisierte Angreifer abzufangen.

- **Fake-Hostnamen:** Gib deinem Honeypot einen coolen oder passenden Namen, wie `ubuntu-server`, `web-server` oder `db-prod`.

- **Benutzerkonten:** Füge gängige, aber schwache Benutzernamen (`root`, `admin`) mit einfachen Passwörtern (`123456`, `password`) hinzu.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 5. Starten und Überwachen
Jetzt startest du Cowrie und lenkst den Traffic um.

- **Cowrie starten:** `./bin/cowrie start`

- **Port-Weiterleitung:** Leite den öffentlichen Port 22 auf Port 2222 deiner VM um.

```bash
# Beispiel für Port-Weiterleitung mit iptables auf deinem Router/Firewall
# Dies leitet den Traffic von öffentlicher IP:22 zu Honeypot-IP:2222 weiter
# sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to-destination <Honeypot-IP>:2222
```
Alle Aktivitäten der Angreifer werden in den Cowrie-Logs gespeichert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Der Sprung zum Honeynet
Ein einzelner Honeypot ist gut, aber ein **Honeynet** liefert eine Fülle von Informationen. Denk es dir wie ein ganzes Spionagenetzwerk.

```text
                    +----------+
                    | Internet |
                    +----+-----+                
                         |
                    +----+-----+
                    | Firewall |
                    +----+-----+
                         |    
                         |      
        +----------------+----------------+
        |                |                |
+-------+------+  +------+-------+  +-----+--------+
| Web-Honeypot |  | SSH-Honeypot |  | IoT-Honeypot |
+-------+------+  +------+-------+  +-----+--------+
        |                |                |
        v                v                v
        +----------------+----------------+
                         |
                         v
        +----------------+----------------+
        |       Zentraler Log-Server      |
        +---------------------------------+
```

**Ein professionelles Honeynet-Setup besteht aus:**

- **Mehreren Honeypots:** Eine Mischung aus Web-, SSH- und IoT-Honeypots, alle in einem isolierten Netzwerk.

- **Einem zentralen Log-Server:** Alle Honeypots leiten ihre Logs an einen zentralen Server weiter. So hast du alle Daten an einem Ort.

- **Sensorik:** Tools wie **Snort** oder **Suricata** können den Netzwerkverkehr im Honeynet überwachen.

- **Automatisierung:** Skripte können die erbeutete Malware automatisch an eine Sandbox senden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Absicherung – Das A und O
Ein unsicherer Honeypot ist eine größere Bedrohung als gar keiner. Das Wichtigste ist, einen Ausbruch der Angreifer in dein echtes Netzwerk zu verhindern.

- **Netzwerk-Isolation:** Die allerwichtigste Regel. Der Honeypot darf niemals Zugriff auf deine echten Systeme haben. Nutze Firewalls und VLANs, um das sicherzustellen.

- **Strikte Firewall-Regeln:** Erstelle Regeln, die nur die absolut notwendige Kommunikation zulassen.

- **Logging:** Überwache nicht nur den Honeypot selbst, sondern auch seinen Host-Server. Jede verdächtige Aktivität sollte Alarm schlagen.

- **Regelmäßige Updates:** Halte das Betriebssystem und die Honeypot-Software immer auf dem neuesten Stand.

- **Reine Überwachung:** Installiere keine zusätzlichen, unnötigen Dienste auf dem Honeypot-Host.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Honeypots Grundlagen](/03-web-security/schutzmaßnahmen/honeypots_grundlagen.md)



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
