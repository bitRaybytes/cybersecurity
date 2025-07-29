# pfSense in VirtualBox installieren und konfigurieren

## ✨ Einleitung

**pfSense** ist eine kostenlose, auf FreeBSD basierende Open-Source-Firewall- und Router-Distribution. In diesem Guide zeigen wir dir Schritt für Schritt, wie du pfSense in Oracle VirtualBox installierst und als Firewall für dein internes Lab-Netzwerk konfigurierst.

---

## ✈ Voraussetzungen

* Oracle VirtualBox installiert
* pfSense ISO-Image ([Download hier](https://www.pfsense.org/download/))
* Mindestens zwei virtuelle Netzwerkkarten (WAN & LAN)
* Host-System: Windows, Linux oder macOS

---

## ✅ Schritt 1: Virtuelle Maschine anlegen

1. Starte VirtualBox und klicke auf **Neu**.
2. Name: `pfSense`, Typ: `BSD`, Version: `FreeBSD (64-bit)`
3. RAM: mind. **1024 MB** (besser: 2048 MB)
4. Festplatte: **VHD** oder **VDI**, dynamisch, **mind. 10 GB**

---

## 🔧 Schritt 2: Netzwerkkarten konfigurieren

1. Klicke auf **Netzwerk** > Adapter 1:

   * **Aktiviert:** Ja
   * **Angeschlossen an:** `Netzwerkbrücke` (WAN)
2. Adapter 2:

   * **Aktiviert:** Ja
   * **Angeschlossen an:** `Internes Netzwerk` (Name z. B. `Labnet`)

> Tipp: Wenn du mehrere VMs im selben Labnet verbinden willst, stelle alle auf das gleiche interne Netzwerk ein.

---

## 🌄 Schritt 3: ISO mounten & pfSense installieren

1. In den VM-Einstellungen: Unter **Massenspeicher** das pfSense ISO als CD einbinden
2. Starte die VM
3. Wähle im Boot-Menü **Install pfSense** aus
4. Folge dem Installationsassistenten:

   * Standard-Partitionierung verwenden
   * ZFS oder UFS auswählen (UFS einfacher für Einsteiger)
   * Installation starten
5. Nach der Installation:

   * Neustarten
   * ISO **auswerfen**, sonst bootet die Installation erneut

---

## 🛡️ Schritt 4: Netzwerkschnittstellen zuweisen (WAN/LAN)

1. Beim ersten Booten fragt pfSense, welche Schnittstellen zugewiesen werden sollen:

   * Beispiel:

     * `em0` = WAN
     * `em1` = LAN
2. LAN wird automatisch eine IP zugewiesen (z. B. `192.168.1.1`)

---

## 🌐 Schritt 5: Zugriff über Webinterface

1. Starte eine weitere VM im gleichen **Internen Netzwerk (Labnet)**
2. Gib im Browser der Lab-VM folgende Adresse ein:

   ```http
   http://192.168.1.1
   ```
3. Anmelden:

   * Benutzer: `admin`
   * Passwort: `pfsense`

4. Starte den Web-Wizard zur Erstkonfiguration

   * Hostname, DNS, WAN/LAN konfigurieren
   * Neues Admin-Passwort setzen

---

## 🔒 Schritt 6: Wichtige Grundeinstellungen im WebGUI

| Funktion                | Ort im WebGUI                    | Empfehlung                         |
| ----------------------- | -------------------------------- | ---------------------------------- |
| Passwort ändern         | System > User Manager            | Starkes Passwort wählen            |
| SSH aktivieren          | System > Advanced > Admin Access | Optional, mit Key empfehlenswert   |
| DHCP für LAN aktivieren | Services > DHCP Server           | Ja, für Labnetzwerk sinnvoll       |
| Updates durchführen     | System > Update                  | Direkt nach Installation ausführen |

---

## 🚀 Schritt 7: pfSense als Gateway/Firewall nutzen

* VMs im **Internen Netzwerk** erhalten über pfSense Internetzugang, wenn:

  * WAN korrekt mit deinem Netzwerk verbunden ist
  * NAT aktiv ist (Standard)
  * Regeln im LAN-Zweig ausgehenden Verkehr zulassen

---

## 🧱 Tipps für dein Cybersecurity Lab

* ⚖️ Regeln definieren: Blockiere ausgehenden Verkehr gezielt
* 🕵️ IDS/IPS aktivieren: Installiere z. B. Snort oder Suricata
* ⚖️ VLANs konfigurieren: Netztrennung für Pentesting & Zielsysteme
* ⚡ Traffic mitschneiden: Diagnostics > Packet Capture

---

## ✉ Weitere Infos & Doku

* [Offizielle pfSense-Dokumentation](https://docs.netgate.com/pfsense/en/latest/)
* [Netgate Forum](https://forum.netgate.com/)

---

## 🔹 Fazit

Mit pfSense hast du eine leistungsstarke und flexible Firewall-Lösung in deinem virtuellen Lab. Sie bietet dir alle Möglichkeiten, ein realistisches und sicheres Netzwerk für Cybersecurity-Tests, Pentesting oder Netzwerktraining aufzubauen.

> Viel Spaß beim Tüfteln im eigenen Labnet! ✨

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