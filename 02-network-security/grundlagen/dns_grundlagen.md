# 🌐 DNS (Domain Name System)
## Inhaltsverzeichnis
- [Was ist DNS (Domain Name System)](#was-ist-dns-domain-name-system)
    - [Funktionsweise und Hierarchie des DNS](#funktionsweise-und-hierarchie-des-dns)
    - [Record-Typen](#record-typen)
    - [Dynamisches DNS (DDNS)](#dynamisches-dns-ddns)
    - [Nützliche Befehle](#nützliche-befehle)
- [Sicherheitsaspekte](#sicherheitsaspekte)
- [Nützliche Links](#nützliche-befehle)
- [Haftungsausschluss]()



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist DNS (Domain Name System)
DNS ist ein essenzieller Dienst, der Domain-Namen in IP-Adressen umwandelt. Es funktioniert wie ein "Telefonbuch" des Internets, da es für Menschen einfacher ist, sich Namen wie google.com zu merken, als deren zugehörige IP-Adresse (`142.250.187.142`).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Funktionsweise und Hierarchie des DNS
Die Namensauflösung im DNS ist ein hierarchischer und verteilter Prozess. Wenn du eine Webseite besuchst, erfolgt die Auflösung in mehreren Schritten:



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Der DNS-Auflösungsprozess
```text
+-----------+                                   +----------------------+
|   Client  | 1. Frage: "Wer ist google.com?"   |  Lokaler DNS-Server  |
+-----------+                                   +----------------------+
            |                                   |
            | 2. Frage: "Wer ist .com?"         |
            +---------------------------------->|   Root-Server  |
            |    (Verweis auf TLD-Server)       |
            |-----------------------------------+
            |                                   |
            | 3. Frage: "Wer ist google.com?"   |
            +---------------------------------->|   TLD-Server (.com)  |
            |  (Verweis auf autoritativen NS)   |
            |-----------------------------------+
            |                                   |
            | 4. Frage: "Wer ist google.com?"   |
            +---------------------------------->|  Autoritativer Nameserver |
            |   (Antwort: "142.250.187.142")    |
            |-----------------------------------+
            |                                   |
            | 5. Caching & Antwort              |
            |<----------------------------------+
```

1. **Lokaler DNS-Server (Resolver):** Dein Gerät fragt den konfigurierten DNS-Server nach der IP-Adresse.

2. **Root-Server:** Findet der lokale DNS-Server die Information nicht, leitet er die Anfrage an einen Root-Server weiter. Dieser antwortet mit der Adresse des zuständigen TLD-Servers.

3. **TLD-Server (Top-Level-Domain):** Der TLD-Server (`.com`, `.de`, `.org`) verweist auf den autoritativen Nameserver, der für die Domain google.com zuständig ist.

4. **Autoritativer Nameserver:** Dieser Server ist die höchste Autorität für die Domain und liefert die gesuchte IP-Adresse zurück.

5. **Caching:** Der lokale DNS-Server speichert die erhaltene Information, um zukünftige Anfragen zu dieser Domain schneller beantworten zu können.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Record-Typen
DNS-Einträge (**Records**) enthalten die Informationen, die vom Nameserver bereitgestellt werden. Hier sind die wichtigsten Typen:

| Record Typ | Beschreibung | Beispiel |
|------------|--------------|----------|
| **A-Record (Address Record):** | Ordnet einen Domain-Namen einer IPv4-Adresse zu. | **Beispiel:** example.com -> 93.184.216.34 |
| **AAAA-Record (Quad-A Record):** | Ordnet einen Domain-Namen einer IPv6-Adresse zu. | **Beispiel:** `example.com` -> `2606:2800:220:1:248:1893:25c8:1946` |
| **CNAME-Record (Canonical Name):** | Ein Alias-Eintrag. Er verweist eine Domain auf eine andere. | **Beispiel:** `www.example.com` -> `example.com` |
| **MX-Record (Mail Exchanger):** | Legt den Mailserver für eine Domain fest. | **Beispiel:** `example.com` -> `mail.example.com` |
| **NS-Record (Nameserver):** | Verweist auf die autoritativen Nameserver der Subdomains. | **Beispiel:** `example.com` -> `ns1.example.com` |
| **TXT-Record (Text Record):** | Speichert beliebigen Text, oft für Verifizierungszwecke oder zur Absenderauthentifizierung von E-Mails | **Beispiel:** `SPF`, `DKIM` |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dynamisches DNS (DDNS)
Dynamisches DNS ist ein Dienst, der den DNS-Eintrag einer Domain automatisch aktualisiert, wenn sich die öffentliche IP-Adresse ändert. Dies ist besonders nützlich für Heimnetzwerke, bei denen der Internetanbieter die öffentliche IP regelmäßig ändert. So bleibt deine Domain (`meinserver.dyndns.org`) immer erreichbar.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Befehle
Um DNS-Einträge zu überprüfen, kannst du folgende Befehle im Terminal verwenden:

- **Linux/macOS:** `dig` [Domain-Name] oder `nslookup` [Domain-Name]
- **Windows:** `nslookup` [Domain-Name]

Beispiel mit `dig`:
```bash
dig example.com
```

Beispiel mit `nslookup`:
```bash
nslookup google.com
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsaspekte
Die häufigste Bedrohung für DNS ist **DNS-Poisoning** (auch **Cache-Poisoning** genannt). Hierbei manipuliert ein Angreifer den Cache eines DNS-Servers, indem er falsche IP-Adressen für legitime Domain-Namen einträgt. Dadurch werden Nutzer auf eine vom Angreifer kontrollierte Website umgeleitet, was für Phishing-Angriffe genutzt werden kann.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Maßnahmen:

- **DNSSEC (DNS Security Extensions):** Eine Erweiterung, die die DNS-Daten kryptografisch signiert, um ihre Authentizität zu gewährleisten.

- **Validierung:** DNS-Resolver validieren die Signaturen, um manipulierte Einträge zu erkennen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [protokoll_header_cheatsheet.md](/02-network-security/protokoll_header_cheatsheet.md)
- [Wikipedia: DNS](https://de.wikipedia.org/wiki/Domain_Name_System)



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

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---