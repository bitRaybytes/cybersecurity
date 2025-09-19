# 🛡️ VPN - Virtual Private Network

## Inhaltsverzeichnis
- [Einleitung: Was ist ein VPN?](#einleitung-was-ist-ein-vpn)
- [VPN-Arten](#vpn-arten)
- [Unterschied zwischen VPN und Proxy](#unterschied-zwischen-vpn-und-proxy)
- [Split Tunneling](#split-tunneling)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung: Was ist ein VPN?

Ein **VPN** (**Virtual Private Network**) ermöglicht eine sichere, verschlüsselte Verbindung über unsichere Netzwerke wie das Internet. Es erstellt einen virtuellen, abhörsicheren "**Tunnel**", durch den der gesamte Datenverkehr geleitet wird. Dadurch bleibt die ursprüngliche IP-Adresse des Nutzers verborgen und der physische Standort ist nicht öffentlich einsehbar.

Ein VPN ist ein **logisches**, **geschlossenes Netzwerk**, das unabhängig von spezieller Hardware funktioniert. Es wird häufig genutzt, um sichere Datenverbindungen über öffentliche Netzwerke aufzubauen. Nur autorisierte Teilnehmer innerhalb des VPNs können miteinander kommunizieren und Daten austauschen.


```text

+------------+                            +-----------+                   +------------+
| Dein Gerät |--------------------------->| VPN-Server|------------------>| Ziel-Server|
+------------+                            +-----------+                   +------------+
   |                                                                            |
   |-- Datenverkehr --|                                   |-- Entschlüsselter --|
      (verschlüsselt)                                          (Datenverkehr)

```

Ein VPN muss folgende Sicherheitsaspekte gewährleisten:
- **Authentizität:** Nur berechtigte Nutzer haben Zugriff auf den Tunnel.
- **Vertraulichkeit:** Die Daten im Tunnel bleiben vor Dritten geschützt.
- **Integrität:** Die übertragenen Informationen können nicht unbemerkt manipuliert werden.

Für den Aufbau eines VPN-Tunnels sind sichere Protokolle wie **IPsec**, **OpenVPN** oder **WireGuard** unerlässlich.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## VPN-Arten

Es gibt verschiedene Arten von VPNs, die für unterschiedliche Anwendungsfälle genutzt werden. Die drei wichtigsten sind:

### 1. Site-to-Site-VPN

Auch **LAN-to-LAN **genannt, verbindet es komplette Netzwerke, z. B. verschiedene Firmenstandorte oder Niederlassungen.

- Die Kommunikation erfolgt über eine verschlüsselte Verbindung zwischen zwei VPN-Routern.
- Spart Kosten, da statt teurer Standleitungen eine Internetverbindung genutzt wird.
- Ermöglicht auch Extranet-VPNs, die die Netzwerke verschiedener Unternehmen sicher verbinden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Remote-Access-VPN

Auch als **End-to-Site-VPN** bekannt, ermöglicht es einzelnen Nutzern eine sichere Verbindung zu einem Firmennetzwerk.

- Wird häufig für Remote-Arbeit genutzt, damit Mitarbeiter von unterwegs auf interne Systeme zugreifen können.
- Das Endgerät (Laptop, Smartphone) eines Nutzers wird dabei Teil des Unternehmensnetzwerks.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. End-to-End-VPN

Stellt eine direkte, verschlüsselte Verbindung zwischen zwei Endgeräten her.

- Erhöht die Sicherheit, da der gesamte Datenverkehr unabhängig von den genutzten Netzwerken verschlüsselt bleibt.
- Wird für sichere Kommunikation zwischen zwei Benutzern oder Geräten verwendet.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Unterscheidung: Kommerzielle vs. Unternehmens-VPNs

- **Unternehmens-VPNs** (oft Remote-Access) verbinden Nutzer mit einem privaten Firmennetzwerk, um auf interne Ressourcen zuzugreifen.

- **Kommerzielle VPN-Anbieter** verbinden Nutzer mit einem öffentlichen VPN-Server, der lediglich Anonymität und IP-Verschleierung bietet – aber keinen Zugriff auf ein internes Netzwerk.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Unterschied zwischen VPN und Proxy

Auf den ersten Blick scheinen VPNs und Proxys ähnlich zu funktionieren, da beide die IP-Adresse verschleiern und den Datenverkehr über einen externen Server leiten. Es gibt jedoch wesentliche Unterschiede in Bezug auf Sicherheit, Verschlüsselung und Funktionsumfang.


| **Merkmal** | **VPN** | **Proxy** |
|-------------|---------|-----------|
| Verschlüsselung | Verschlüsselt gesamten Datenverkehr (`End-to-End`) | Verschlüsselt in der Regel nichts, leitet nur Anfragen um. |
| **Umfang des Schutzes** | Schützt das **gesamte Gerät** (alle Programme, Browser, Apps). | Arbeitet meist nur auf **Anwendungsebene** (z.B. nur für einen Browser). | 
| **Sicherheits-Features** | Bietet zusätzliche Features wie **Kill-Switch** oder **DNS-Leak-Schutz** | Bietet kaum zusätzliche Sicherheit, hängt vom Protokoll (z. B. HTTPs) ab. |
| **Protokolle** | Nutzt sichere Protokolle wie **OpenVPN** oder **WireGuard** | Nutzt oft unsicher Protokolle wie HTTP, HTTPS, SOCKS. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Split Tunneling

Mit **Split Tunneling** wird nicht der gesamte Datenverkehr des Geräts durch den VPN-Tunnel gesendet. Stattdessen wird nur ein Teil des Datenverkehrs – beispielsweise nur der für das Firmennetzwerk bestimmte Traffic – verschlüsselt, während der restliche Internetverkehr unverschlüsselt über die normale Verbindung läuft.

### Vorteile von Split Tunneling

- **Effizienz:** Die VPN-Leitung wird entlastet, da nur der notwendige Datenverkehr verschlüsselt wird.

- **Performance:** Der Zugriff auf öffentliche Webseiten (z. B. News, Social Media) ist schneller, da er nicht über den VPN-Server umgeleitet wird.

- **Bandbreite:** Die Bandbreite des Firmennetzwerks wird nicht durch private Aktivitäten von Mitarbeitern (z. B. Video-Streaming) beansprucht.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links
- [https://de.wikipedia.org/wiki/IPsec](https://de.wikipedia.org/wiki/IPsec)
- [https://de.wikipedia.org/wiki/Virtual_Private_Network](https://de.wikipedia.org/wiki/Virtual_Private_Network)

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