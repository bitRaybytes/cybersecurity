# pfSense in VirtualBox installieren und konfigurieren

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Voraussetzungen](#voraussetzungen)
- [Grundlagen der Lab-Architektur](#grundlagen-der-lab-architektur)
- [Schritt 1: Virtuelle Maschine anlegen](#schritt-1-virtuelle-maschine-anlegen)
- [Schritt 2: Netzwerkkarten konfigurieren](#schritt-2-netzwerkkarten-konfigurieren)
- [Schritt 3: ISO mounten & pfSense installieren](#schritt-3-iso-mounten--pfsense-installieren)
- [Schritt 4: Netzwerkschnittstellen zuweisen (WAN/LAN)](#schritt-4-netzwerkschnittstellen-zuweisen-wanlan)
- [Schritt 5: Zugriff über Webinterface](#schritt-5-zugriff-über-webinterface)
- [Schritt 6: Wichtige Grundeinstellungen im WebGUI](#schritt-6-wichtige-grundeinstellungen-im-webgui)
- [Schritt 7: pfSense als Gateway/Firewall nutzen](#schritt-7-pfsense-als-gatewayfirewall-nutzen)
- [Tipps für dein Cybersecurity Lab](#tipps-für-dein-cybersecurity-lab)
- [Weitere Infos & Doku](#weitere-infos--doku)
- [Fazit](#fazit)
- [Haftungsausschluss](#haftungsausschluss)




## Einleitung

**pfSense** ist eine kostenlose, auf FreeBSD basierende Open-Source-Firewall- und Router-Distribution. Sie ist eine der meistgenutzten und leistungsstärksten Lösungen für Heimanwender und Unternehmen. pfSense bitete dir die Möglichkeit, eine realistische Netzwerkumgebung in deiner Virtualisierung zu erstellen und zu steuern. Dieser Guide zeigt dir Schritt für Schritt, wie du pfSense in Oracle VirtualBox installierst und als zentrales Gateway für dein internes Lab-Netzwerk konfigurierst.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Voraussetzungen

- Oracle VirtualBox installiert
- pfSense ISO-Image ([Download hier](https://www.pfsense.org/download/))
- Mindestens zwei virtuelle Netzwerkkarten (WAN & LAN)
- Host-System mit ausreichend Ressourcen (mindestens 2 GB RAM für pfSense)
- Eine weitere VM, z. B. Kali Linux oder Windows, um das Lab-Netzwerk zu testen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundlagen der Lab-Architektur

Bevor du mit der Installation beginnst, solltest du die grundlegende Netzwerkarchitektur deines Labs verstehen. Dein Ziel ist es, eine isolierte Umgebung zu schaffen, die deine produktive Host-Umgebung nicht gefährdet.

Hier ist ein einfaches Schema für eine sichere Lab-Umgebung:
```text
                           +-----------------------+
                           |      Host-System      |
                           |    (Windows/Linux)    |
                           +-----------+-----------+
                                       |
                                       |  Virtuelle Netzwerkbrücke
                                       |
                            +----------+-----------+
                            |      pfSense VM      |
                            +----------+-----------+
                                  | WAN (Adapter 1)
                                  |
                            +-----+-----------------+
                            |    Internes Netzwerk  |
                            |     (z.B. Labnet)     |
                            +--------+--------------+
                                     |
                            +--------+-----+-------------+------------+
                            |  Lab-VM 1    |  Lab-VM 2   |  Lab-VM 3  |
                            +--------------+-------------+------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schritt 1: Virtuelle Maschine anlegen

1. Starte VirtualBox und klicke auf **Neu**.

2. Gib der VM einen passenden Namen, wie `pfSense-Firewall`. Wähle als Typ `BSD` und als Version **FreeBSD (64-bit)**.
3. Weise der VM mindestens **2048** MB RAM zu. **1024 MB** sind das absolute Minimum, aber für ein performantes Lab-Setup sind 2 GB oder mehr empfehlenswert.
4. Erstelle eine virtuelle Festplatte. Wähle **VDI** (VirtualBox Disk Image) und lege eine dynamisch allokierte Größe von **mindestens 10 GB** fest.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Schritt 2: Netzwerkkarten konfigurieren
Dieser Schritt ist entscheidend für die Funktionalität deines Labs. Die erste Netzwerkkarte (WAN) stellt die Verbindung zur Außenwelt her, die zweite (LAN) zum internen Lab-Netzwerk.

1. Wähle die VM aus und gehe zu Einstellungen > Netzwerk.

2. **Adapter 1 (WAN):**
   - **Aktiviert:** Ja
   - **Angeschlossen an:** `Netzwerkbrücke` (WAN)
   - Wähle deine physikalische Netzwerkkarte aus, die eine Internetverbindung hat (z. B. dein WLAN- oder Ethernet-Adapter). Dies ermöglicht es pfSense, eine IP-Adresse von deinem Router zu erhalten und als Gateway zu agieren.

3. Adapter 2:
   - **Aktiviert:** Ja
   - **Angeschlossen an:** `Internes Netzwerk` (Name z. B. `Labnet`)
   - Gib dem internen Netzwerk einen eindeutigen Namen, z. B. `Labnet.` Alle anderen virtuellen Maschinen, die du in diesem Lab testen oder angreifen möchtest, müssen ebenfalls diesen Namen als internes Netzwerk verwenden.

> Tipp: Wenn du mehrere VMs im selben Labnet verbinden willst, stelle alle auf das gleiche interne Netzwerk ein.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schritt 3: ISO mounten & pfSense installieren

1. **In den VM-Einstellungen:** Unter **Massenspeicher** wähle das CD-Symbol aus und navigiere zum heruntergeladenen pfSense ISO-Image.
2. Starte die VM.
3. Wähle im Boot-Menü **Install pfSense** aus und drücke Enter.
4. Folge dem Installationsassistenten:

   - Akzeptiere die Standard-Einstellungen (Keymap, etc.).
   - Wähle die empfohlene **Auto** (**UFS**) **Guided Installation** für Einsteiger. ZFS bietet zwar fortgeschrittene Features wie Snapshots, ist aber für ein einfaches Lab nicht zwingend notwendig.
5. Nach der Installation wirst du gefragt, ob du in eine Shell wechseln möchtest. Wähle **Nein**.
6. Bevor du die VM neu startest, gehe in die **VM-Einstellungen** und **wirf das ISO aus**. Andernfalls bootet die Installation erneut.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schritt 4: Netzwerkschnittstellen zuweisen (WAN/LAN)
Nach dem Neustart bootet pfSense in die Konsole und fragt nach der Zuweisung der Schnittstellen.

1. Drücke `n`, um die VLAN-Konfiguration zu überspringen.

2. Wechsle nun die Reihenfolge der Schnittstellen, da sie nicht mit deinen vorherigen Einstellungen übereinstimmen. Gib zuerst die LAN-Schnittstelle an und dann die WAN-Schnittstelle:
   - `WAN interface name`: Wähle die Schnittstelle, die mit deinem externen Netzwerk verbunden ist (z. B. `vtnet0` oder `em0`).
   - `LAN interface name`: Wähle die Schnittstelle, die mit deinem internen Lab-Netzwerk verbunden ist (z. B. `vtnet1` oder `em1`).
3. Bestätige mit y. pfSense weist der LAN-Schnittstelle automatisch die IP-Adresse `192.168.1.1` zu.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schritt 5: Zugriff über Webinterface
Um pfSense zu konfigurieren, benötigst du eine weitere VM (z. B. Kali Linux), die mit dem gleichen internen Netzwerk (`Labnet`) verbunden ist. 

1. Starte die Lab-VM und sorge dafür, dass sie eine IP-Adresse von pfSense erhält (i.d.R. automatisch über DHCP).
2. Öffne den Browser in der Lab-VM und navigiere zur LAN-IP von pfSense:

   ```http
   http://192.168.1.1
   ```
3. Melde dich mit den Standard-Zugangsdaten an:
   - **Benutzer:** `admin`
   - **Passwort:** `pfsense`

4. Folge dem Web-Wizard zur Erstkonfiguration, um Hostname, DNS-Server und das Administrator-Passwort zu ändern. **Das Ändern des Standardpassworts ist ein kritischer erster Schritt zur Absicherung!**



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schritt 6: Wichtige Grundeinstellungen im WebGUI

| Funktion                | Ort im WebGUI                    | Empfehlung                         |
| ----------------------- | -------------------------------- | ---------------------------------- |
| **Admin-Passwort ändern**   | System > User Manager       | **Wichtiger Sicherheitsschritt:** Sofort ein starkes, einzigartiges Passwort wählen.            |
| **DHCP für LAN aktivieren** | Services > DHCP Server  | Standardmäßig aktiviert, aber überprüfen. Unverzichtbar für die einfache Konfiguration deiner Lab-VMs. |
| **Firewall-Regeln prüfen** | Firewall > Rules | Die Standardregeln lassen jeglichen ausgehenden Verkehr vom LAN ins WAN zu. Für ein sicheres Lab solltest du diese Regeln später anpassen. |
| **SSH aktivieren**      | System > Advanced > Admin Access | Wenn benötigt, nur für vertrauenswürdige Hosts und am besten mit Public-Key-Authentifizierung. **Deaktiviere es, wenn du es nicht brauchst.** |
| **Updates durchführen**     | System > Update                  | Unmittelbar nach der Installation alle verfügbaren Updates einspielen. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Schritt 7: pfSense als Gateway/Firewall nutzen
Deine pfSense-Instanz agiert nun als Gateway und Firewall für dein internes Netzwerk.

- Alle VMs im `Labnet` können über pfSense auf das Internet zugreifen.

- Der gesamte ausgehende Traffic aus deinem Lab wird durch pfSense geleitet und über **NAT** (**Network Address Translation**) maskiert.
- Der ausgehende Traffic aus deinem Lab wird durch pfSense geleitet und über NAT maskiert, wodurch deine internen IP-Adressen vor dem Internet verborgen bleiben.
- Du kannst nun gezielte Firewall-Regeln erstellen, um den Traffic zwischen deinen Lab-VMs zu steuern oder bestimmten Verkehr zu blockieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Tipps für dein Cybersecurity Lab

- **Regeln definieren:** `Firewall > Rules` kannst du Traffic zwischen verschiedenen internen Subnetzen blockieren. Blockiere beispielsweise den Traffic zwischen deinem Pentesting-System und deinem Zielsystem.

- **IDS/IPS aktivieren**: Installiere Pakete wie **Snort** oder **Suricata** unter `System > Package Manager`. Diese Intrusion Detection/Prevention Systeme helfen dir, Angriffe in Echtzeit zu erkennen und zu blockieren
- **VLANs konfigurieren:** Erstelle in pfSense getrennte virtuelle LANs, um deine Netzwerke zu segmentieren (z. B. ein Segment für Angreifer, ein anderes für Zielsysteme und ein drittes für administrative Systeme).
- **Traffic mitschneiden:** Die Funktion `Diagnostics > Packet Capture` ist ein mächtiges Werkzeug, um den Netzwerkverkehr zu analysieren.
- **VPN-Server einrichten:** Erstelle einen VPN-Server in pfSense, um dich von außerhalb sicher mit deinem Lab-Netzwerk zu verbinden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Weitere Infos & Doku

- [Offizielle pfSense-Dokumentation](https://docs.netgate.com/pfsense/en/latest/)
- [Netgate Forum](https://forum.netgate.com/)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Fazit

Mit pfSense hast du eine leistungsstarke und flexible Firewall-Lösung in deinem virtuellen Lab. Sie bietet dir alle Möglichkeiten, ein realistisches und sicheres Netzwerk für Cybersecurity-Tests, Pentesting oder Netzwerktraining aufzubauen.

> Viel Spaß beim Tüfteln im eigenen Labnet!


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
