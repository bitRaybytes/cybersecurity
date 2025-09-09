# Virtual Private Network (VPN)

Ein **VPN (Virtual Private Network)** ermöglicht eine sichere, verschlüsselte Verbindung über unsichere Netzwerke wie das Internet.  
Dazu wird ein **virtueller Tunnel** erstellt, durch den der gesamte Datenverkehr geleitet wird.  

- Die **IP-Adresse** bleibt verborgen.  
- Der **Standort des Nutzers** ist nicht öffentlich einsehbar.  
- Der Datenverkehr ist gegen **Abhören und Manipulation** geschützt.  


## Inhaltsverzeichnis
- [Visualisierung: VPN-Tunnel](#visualisierung-vpn-tunnel)
- [Wichtige Sicherheitsaspekte eines VPN](#wichtige-sicherheitsaspekte-eines-vpn)
- [VPN-Arten](#vpn-arten)
- [Unternehmens-VPNs vs. kommerzielle VPNs](#unternehmens-vpns-vs-kommerzielle-vpns)
- [Proxy vs. VPN](#proxy-vs-vpn)
- [Split Tunneling](#split-tunneling)
- [Wichtige VPN-Protokolle](#wichtige-vpn-protokolle)
- [Vorteile von VPNs](#vorteile-von-vpns)
- [Nachteile von VPNs](#nachteile-von-vpns)
- [Vergleich: Proxy vs. VPN](#vergleich-proxy-vs-vpn)
- [Weiterführende Links](#weiterführende-links)
- [Haftungsausschluss](#haftungsausschluss)



## Visualisierung: VPN-Tunnel

```yaml
[ Client / Laptop ] --(verschlüsselt)-- [ Internet ] --(verschlüsselt)-- [ VPN-Server / Firmennetzwerk ]
```


## Wichtige Sicherheitsaspekte eines VPN

Ein sicheres VPN muss folgende Punkte gewährleisten:

1. **Authentizität** → nur berechtigte Nutzer haben Zugriff  
2. **Vertraulichkeit** → Daten bleiben vor Dritten geschützt  
3. **Integrität** → übertragene Informationen können nicht unbemerkt verändert werden  

→ Dies sind auch die Kernpunkte von **IPsec**-basierter VPN-Sicherheit.  



## VPN-Arten

### 1. **Site-to-Site-VPN (LAN-to-LAN)**
- Verbindet **ganze Netzwerke** (z. B. Firmenstandorte).  
- Kommunikation erfolgt zwischen VPN-Routern über verschlüsselte Tunnel.  
- Kostengünstige Alternative zu Standleitungen.  
- Auch als **Extranet-VPN** nutzbar (Verknüpfung verschiedener Unternehmen).  

```yaml
[ Standort A LAN ] ⇆ [ VPN Gateway ] ⇆ [ Internet ] ⇆ [ VPN Gateway ] ⇆ [ Standort B LAN ]
```



### 2. **End-to-Site-VPN (Remote Access)**
- Einzelne Nutzer greifen auf ein internes Firmennetz zu.  
- Typisches Szenario für **Homeoffice** oder **Remote-Arbeit**.  

```yaml
[ Remote Client ] ⇆ [ Internet ] ⇆ [ VPN Gateway / Firmenserver ]
```



### 3. **End-to-End-VPN**
- Direkte verschlüsselte Verbindung zwischen **zwei Endgeräten**.  
- Sicherheit unabhängig von zwischengeschalteten Netzwerken.  
- Anwendungsfall: sichere Kommunikation zwischen zwei Benutzern / Servern.  

```yaml
[ Client A ] ⇆ (VPN Tunnel) ⇆ [ Client B ]
```



## Unternehmens-VPNs vs. kommerzielle VPNs

| Merkmal              | Unternehmens-VPN (End-to-Site) | Kommerzielles VPN (Provider) |
|----------------------|---------------------------------|-------------------------------|
| Zugriff auf interne Ressourcen | ✅ Ja (z. B. Datenbanken, Server) | ❌ Nein |
| IP-Verschleierung    | sekundär                       | primär |
| Zielgruppe           | Mitarbeiter, Unternehmen       | Privatnutzer |
| Hauptzweck           | Remote Work, Netzwerksicherheit | Anonymität, Geoblocking |



## Proxy vs. VPN

Auf den ersten Blick ähneln sich VPNs und Proxys – beide verschleiern die IP-Adresse.  
Der entscheidende Unterschied liegt in der **Verschlüsselung und im Schutzumfang**.

| Aspekt            | VPN | Proxy |
|-------------------|-----|-------|
| **Verschlüsselung** | ✅ gesamter Datenverkehr | ❌ meist keine |
| **Umfang**         | ✅ systemweit (alle Apps) | ❌ anwendungsbezogen |
| **Sicherheit**     | ✅ starke Verschlüsselung (OpenVPN, IPSec, WireGuard) | ❌ unsicher, abhängig vom Protokoll |
| **Features**       | Kill-Switch, DNS-Leak-Schutz | selten vorhanden |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Split Tunneling

Mit **Split Tunneling** wird nur ein Teil des Datenverkehrs durch das VPN geleitet.  
Der Rest läuft direkt über den lokalen Internetzugang.

✅ Vorteile:
- Entlastet die VPN-Leitung  
- Nur Unternehmensressourcen laufen durch das VPN  

❌ Risiken:
- Nicht-VPN-Verbindungen sind potentiell unsicher  



## Wichtige VPN-Protokolle

| Protokoll   | Eigenschaften |
|-------------|---------------|
| **IPsec**   | Industriestandard, sicher, auf Netzwerkebene |
| **OpenVPN** | Open Source, flexibel, über UDP/TCP |
| **WireGuard** | modern, sehr schnell, minimalistisch, Kernel-nah |
| **IKEv2/IPSec** | stabil, besonders für mobile Geräte |
| **L2TP/IPSec** | älter, aber oft noch im Einsatz |
| **SSL-VPN** | basiert auf TLS, häufig für Remote Access |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vorteile von VPNs

- Schutz vor Abhören und Manipulation  
- Vertraulichkeit von Kommunikation  
- Remote-Zugriff auf Firmennetze  
- Umgehung von Geoblocking  
- Sicherung öffentlicher WLANs  



## Nachteile von VPNs

- Performance-Einbußen durch Verschlüsselung  
- VPN-Server kann theoretisch selbst protokollieren  
- Abhängigkeit von der Stabilität des VPN-Providers  
- In einigen Ländern verboten oder eingeschränkt  



## Vergleich: Proxy vs. VPN

```yaml
Proxy:
[ Client ] -> [ Proxy Server ] -> [ Internet ]
(meist unverschlüsselt, nur App-abhängig)

VPN:
[ Client ] ==(verschlüsselt Tunnel)==> [ VPN Server ] -> [ Internet ]
(systemweit verschlüsselt)
```



## Weiterführende Links

- [OpenVPN Projektseite](https://openvpn.net)  
- [WireGuard VPN](https://www.wireguard.com)  
- [IPSec Grundlagen (RFC 4301)](https://datatracker.ietf.org/doc/html/rfc4301)  
- [YouTube: VPN Grundlagen erklärt](https://www.youtube.com/watch?v=NL7ySTGXt9w)  




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