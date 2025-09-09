# Man-in-the-Middle (MitM) Angriffe: Das unsichtbare Risiko

## Inhaltsverzeichnis
- [Was ist ein Man-in-the-Middle-Angriff?](#was-ist-ein-man-in-the-middle-angriff)
- [So funktioniert ein MitM-Angriff](#so-funktioniert-ein-mitm-angriff)
- [Gängige Angriffstechniken](#gängige-angriffstechniken)
    - 1. [ARP-Spoofing (Address Resolution Protocol)](#1-arp-spoofing-address-resolution-protocol)
    - 2. [DNS-Spoofing](#2-dns-spoofing)
    - 3. [SSL/TLS-Hijacking und Downgrade-Angriffe](#3-ssltls-hijacking-und-downgrade-angriffe)
- [So schützt du dich vor MitM-Angriffen](#so-schützt-du-dich-vor-mitm-angriffen)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

## Was ist ein Man-in-the-Middle-Angriff?
Ein **Man-in-the-Middle-Angriff** (**MitM**) ist eine Cyber-Attacke, bei der sich ein Angreifer heimlich zwischen zwei kommunizierende Parteien schaltet. Ziel ist es, den Datenverkehr abzufangen, abzuhören oder zu manipulieren, ohne dass die Opfer dies bemerken. Der Angreifer agiert dabei als unsichtbarer Vermittler.

**Analogie**

Stell dir vor, du schickst einen Brief an einen Freund. Ein MitM-Angriff wäre, wenn jemand den Brief abfängt, ihn liest, vielleicht sogar umschreibt und ihn dann an den Empfänger weiterleitet. Weder du noch dein Freund würden etwas davon mitbekommen, außer dass der Brief vielleicht leicht verändert ist oder verspätet ankommt.

## So funktioniert ein MitM-Angriff
Ein Angreifer nutzt verschiedene Techniken, um sich in die Kommunikation einzuschleichen. Hier sind die gängigsten Phasen eines Angriffs:

- **Abfangen:** 
    - Der Angreifer leitet den Datenverkehr zwischen den beiden Opfern auf sich um.
- **Entschlüsselung:** 
    - Wenn der Datenverkehr verschlüsselt ist (z. B. HTTPS), muss der Angreifer ihn entschlüsseln, um an die Daten zu gelangen.

- **Manipulation & Weiterleitung:** 
    - Der Angreifer liest die Daten, ändert sie bei Bedarf und leitet sie an den ursprünglichen Empfänger weiter, um den Angriff zu verbergen.

**Schematische Darstellung**

```yaml
         +-----------------+             +-----------------+
         |    Opfer A      |             |     Opfer B     |
         +-----------------+             +-----------------+
                  |                                |
                  |                                |
        (Verschlüsselte Daten)        (Verschlüsselte Daten)
                  |                                |
      +------------------------------------------------------+
      |            Man-in-the-Middle-Angreifer               |
      +------------------------------------------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Gängige Angriffstechniken
### 1. ARP-Spoofing (Address Resolution Protocol)
[ARP-Spoofing](/02-network-security/arp_spoofing.md) ist eine der häufigsten MitM-Techniken in lokalen Netzwerken. Der Angreifer sendet gefälschte ARP-Nachrichten, um die MAC-Adresse mit der eigenen zu verknüpfen.

- **Wie es funktioniert:**
    - **Angreifer zu Opfer A:** Der **Angreifer** teilt **Opfer A** mit, dass die MAC-Adresse des Routers (Gateway) jetzt seine eigene ist.
    - **Angreifer zu Opfer B (Router):** Der **Angreifer** teilt dem **Router** mit, dass die MAC-Adresse von Opfer A jetzt seine eigene ist.
    - Der gesamte Datenverkehr zwischen **A** und dem **Router** fließt nun über den **Angreifer**.

```yaml
 Opfer A                 Angreifer                   Router/Gateway
        |                           |                           |
        |--- "Ich bin der Router" ->|                           |
        |                           |<--- "Ich bin Opfer A" ----|
        |                           |                           |
        |---- Datenfluss ---------->|                           |
        |                           |---- Datenfluss ---------->|
```

### 2. DNS-Spoofing
Bei DNS-Spoofing manipuliert ein Angreifer die Namensauflösung. Der Angreifer sendet gefälschte DNS-Antworten, um eine Domain mit einer falschen IP-Adresse zu verknüpfen. Das Opfer wird dann auf eine gefälschte Website umgeleitet, die der echten täuschend ähnlich sieht.

### 3. SSL/TLS-Hijacking und Downgrade-Angriffe
Hierbei schaltet sich der Angreifer in eine HTTPS-Verbindung ein.

- **SSL-Hijacking:** Der Angreifer agiert als Proxy und kommuniziert mit dem Opfer über eine gefälschte SSL-Zertifikatsverbindung, während er gleichzeitig die tatsächliche, sichere Verbindung zum Zielserver herstellt.

- **Downgrade-Angriffe:** Der Angreifer zwingt die Kommunikation zwischen Client und Server dazu, von einer sicheren, modernen Verschlüsselung (z.B. TLS 1.3) auf eine unsichere oder unverschlüsselte Version (z.B. HTTP) zu wechseln.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## So schützt du dich vor MitM-Angriffen
- **Nutze VPNs:** 
    - Ein Virtual Private Network (VPN) verschlüsselt deinen gesamten Datenverkehr von deinem Gerät aus. Selbst wenn ein Angreifer sich dazwischenschaltet, kann er die Daten nicht entschlüsseln.

- **Achte auf HTTPS:** 
    - Achte darauf, dass du immer HTTPS-Websites besuchst. Überprüfe das Schloss-Symbol in der Adressleiste und die Details des Zertifikats.

- **Zertifikatswarnungen ernst nehmen:** 
    - Wenn dein Browser eine Warnung vor einem ungültigen oder verdächtigen Sicherheitszertifikat anzeigt, ignoriere diese Warnung nicht. Sie ist ein starker Hinweis auf einen möglichen MitM-Angriff.

- **Vermeide öffentliches WLAN:** 
    - In öffentlichen WLAN-Netzwerken, wie in Cafés oder Flughäfen, ist das Risiko eines MitM-Angriffs am höchsten. Verzichte auf den Zugriff auf sensible Daten (Online-Banking, E-Mails) in solchen Netzwerken.

- **Nutze Tools zur Überwachung:** 
    - Für Netzwerkadministratoren gibt es Tools wie Wireshark und ARP-Guard, die verdächtige Aktivitäten wie ARP-Spoofing erkennen können.

## Nützliche Links
- [Wikipedia: Man-in-the-Middle-Angriff](https://de.wikipedia.org/wiki/Man-in-the-Middle-Angriff)
- [Wikipedia: ARP Spoofing](https://de.wikipedia.org/wiki/ARP-Spoofing)
- [arp_spoofing.md](/02-network-security/arp_spoofing.md) 

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