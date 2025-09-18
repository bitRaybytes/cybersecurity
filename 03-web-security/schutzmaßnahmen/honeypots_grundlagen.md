# 🍯 Honeypots - Wo Angreifer in Fallen tappen

## Inhaltsverzeichnis
- [Was sind Honeypots?](#was-sind-honeypots)
- [Wie funktioniert ein Honeypot?](#wie-funktioniert-ein-honeypot)
- [Architektur und Isolation](#architektur-und-isolation)
- [Arten von Honeypots](#arten-von-honeypots)
- [Speichern / Lagerung von Honeypots](#speichern--lagerung-von-honeypots)
- [Honeypots im Detail: Interaktionsstufen](#honeypots-im-detail-interaktionsstufen)
- [Tools für Honeypots](#tools-für-honeypots)
- [Nutzen von Honeypots](#nutzen-von-honeypots)
- [Wichtige Überlegungen](#wichtige-überlegungen)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was sind Honeypots?
Ein **Honeypot** ist eine digitale Falle, die Angreifer anlockt und ihr Verhalten aufzeichnet. Honeypots simulieren verwundbare, aber streng isolierte Systeme wie einen gefälschten Server oder eine Datenbank, um Angreifer abzulenken und wertvolle Informationen über ihre Taktiken, Werkzeuge und Motive zu sammeln. Die gewonnenen Daten ermöglichen es Sicherheitsteams, Schwachstellen zu erkennen und ihre Abwehrmaßnahmen zu verbessern, bevor ein Angriff auf die produktiven Systeme erfolgt.

Dies gibt Sicherheitsteams genügend Zeit, auf potenzielle Risiken einzugehen, indem sie beispielsweise die IP-Adresse blockieren oder neue Signaturen für ihre Intrusion Detection Systeme (IDS) erstellen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wie funktioniert ein Honeypot?

- **Täuschen:** Ein Honeypot sieht wie ein echtes, wertvolles Ziel aus, um Angreifer zum Angriff zu verleiten. Er präsentiert sich z. B. als ungesicherter Webserver, eine offene Datenbank oder ein verwundbarer IoT-Sensor.

- **Anlocken:** Angreifer, die das Netzwerk scannen oder versuchen, sich einzuhacken, werden in diese künstliche Umgebung gelockt, anstatt auf das echte, produktive System zu stoßen.
- **Überwachen:** Sämtliche Aktivitäten des Angreifers im Honeypot werden detailliert protokolliert und analysiert. Dazu gehören Tastatureingaben, genutzte Befehle, hochgeladene Malware und die verwendeten Angriffsmethoden.
- **Schützen:** Die gesammelten Informationen helfen dabei, das produktive System vor den aufgedeckten Angriffsmethoden zu schützen und zu verstärken.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Architektur und Isolation
Die absolute Isolation ist das wichtigste Prinzip bei der Implementierung eines Honeypots. Ein Honeypot muss sich in einem eigenen, isolierten Netzwerksegment befinden, das von allen produktiven Systemen getrennt ist.

```text
Beispielhafte Darstellung eines Honeypots:

+-------------+     +------------+     +-----------------+       +-----------------+
|   Internet  |     |  Firewall  |     |  Honeypot-Netz  |       |   Produktions-  |
| (Angreifer) |---->|            |---->|     (DMZ)       |---X---|     Netzwerk    |
|             |     |            |     +-----------------+       +-----------------+
|             |     |            |                                       ^
+-------------+     |            |                                       |
                    |            |                                       |
                    |            |     +--------------------+     +-----------------+
                    |            |---->| Überwachungsserver |---->| Analysestation  |
                    |            |     +--------------------+     +-----------------+
                    |            |
                    +------------+

```

Ein **Honeynet** ist eine Ansammlung von zwei oder mehr Honeypots in einem Netzwerk. Dies macht das Setup für Angreifer realistischer und ermöglicht eine komplexere Datenerfassung.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Arten von Honeypots

- **Serverseitige Honeypots** Simulieren Netzwerkdienste oder Anwendungen, um Angreifer von kritischen Systemen fernzuhalten.

- **Clientseitige Honeypots:** Imitieren eine Client-Anwendung (z. B. einen Browser), die unsichere Webseiten besucht, um Angriffe auf Plugins oder den Browser selbst zu erkennen.

- **E-Mail-Traps:** Auch **Spam-Traps**, sind fiktive E-Mail-Adressen, die an verborgenen Stellen im System platziert werden. Sie können nur durch automatisierte E-Mail-Harvester gefunden werden. Jede E-Mail an diese Adressen wird sofort als Spam klassifiziert und die IP-Adresse kann zu einer `denylist` hinzugefügt werden.

- **Malware-Honeypot:** Imitieren Softwareanwendungen und APIs, um Malware-Angriffe zu provozieren und die Schadsoftware zu analysieren.

- **Spider-Honeypots:** Webseiten und Links, die nur für Webcrawler zugänglich sind. Sie werden verwendet, um bösartige Web-Crawler oder automatisierte Scan-Tools zu identifizieren. 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Speichern / Lagerung von Honeypots
Der Standort deines Honeypots entscheidet darüber, welche Art von Angreifer du anziehst. Es gibt zwei Hauptansätze, die unterschiedliche Ziele verfolgen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Interne Honeypots
Ein interner Honeypot befindet sich innerhalb deines geschützten Firmennetzwerks.
```text
      +-----------*
      | Angreifer |
      | (Intern)  |
      +-----+-----+
            |
        +---+---+
        |  VPN  |
        +---+---+
            |
+-----------+-----------+
|  Internes Netzwerk    |
|  +----------------+   |
|  |  Produktions-  |   |
|  |    Server      |   |
|  +----------------+   |
|  |    Honeypot    |<----(Die Falle!)
|  +----------------+   |
+-----------------------+
``` 

- **Wozu?** Du willst Angreifer fangen, die es schon in dein Netzwerk geschafft haben. Ein interner Honeypot ist perfekt, um zu erkennen, ob sich jemand von einem kompromittierten System aus in deinem Netzwerk weiterbewegen will.

- **Vorteile:** Du bekommst Einblicke in die Taktiken von Angreifern, die bereits hinter deiner Firewall sind – also die, die am gefährlichsten sind. Er dient als Frühwarnsystem, bevor wichtige Daten gestohlen werden.

- **Nachteile:** Das Risiko ist enorm. Wenn der Honeypot nicht 100%ig isoliert ist, könnte der Angreifer die Falle als Brücke in dein echtes Netzwerk nutzen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Externe Honeypots
Ein externer Honeypot befindet sich in der Regel in der DMZ (Demilitarized Zone) oder direkt im Internet.

```text
        +----------+
        | Internet |
        +----+-----+
             |
    +--------+----------+
    | Firewall / Router |
    +--------+----------+
             |
    +--------+----------+
    |    Externer       |  <---- Direkter Kontakt!
    |    Honeypot       |
    +--------+----------+
             |
     +-------+--------+
     | Firewall / DMZ |
     +-------+--------+
             |
    +--------+----------+
    |   Internes        |
    |   Netzwerk        |
    +-------------------+
```

- **Wozu?** Du willst die weltweiten Angriffe und Scanning-Aktivitäten überwachen, die von außen auf deine Netzwerke zielen. Er schützt deine echten Server, indem er die Angreifer ablenkt.

- **Vorteile:** Du erfährst, welche Schwachstellen weltweit ausgenutzt werden und kannst deine echten Systeme daraufhin absichern. Das Risiko für dein internes Netzwerk ist gering, da der Honeypot komplett isoliert ist.

- **Nachteile:** Die Daten können sehr "verrauscht" sein, da du es meistens mit automatisierten Botnet-Angriffen zu tun hast, die nicht so komplex sind.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Honeypots im Detail: Interaktionsstufen

Honeypots werden oft nach dem Grad der Interaktion klassifiziert, den sie einem Angreifer ermöglichen.

- **Low-Interaction Honeypots:** Bieten eine sehr eingeschränkte Interaktionsmöglichkeit. Sie imitieren nur wenige Dienste und reagieren nur auf wenige Befehle. Sie sind einfach zu implementieren und haben ein sehr geringes Risiko, da Angreifer kaum tiefer in das System eindringen können. Sie eignen sich gut, um Massen-Scans und automatisierte Angriffe zu erkennen.

- **Medium-Interaction Honeypots:** Simulieren eine größere Bandbreite an Diensten und erlauben eine begrenzte Interaktion. Sie sind komplexer als Low-Interaction Honeypots und liefern detailliertere Informationen über die Angreifer. Das Risiko ist moderat, da ein Angreifer das System länger testen kann.

- **High-Interaction Honeypots:** Bieten eine fast vollständige Interaktion mit dem Angreifer. Sie sind in der Regel echte, aber stark isolierte Betriebssysteme. Sie liefern die meisten Informationen über die Taktiken und Werkzeuge eines Angreifers, bergen aber das höchste Risiko, da eine Fehlkonfiguration dazu führen könnte, dass der Angreifer aus der Sandbox ausbricht und auf das Produktionsnetzwerk zugreift.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tools für Honeypots
Hier sind einige der bekanntesten und nützlichsten Open-Source-Tools, mit denen du selbst Honeypots aufbauen und Angreifer analysieren kannst.

| Tool | Beschreibung | Art | GitHub-Link |
|------|--------------|-----|-------------|
| **Dionaea** | Ein Honeypot, der sich auf das Abfangen von Malware konzentriert, indem er viele Netzwerkdienste wie SMB und HTTP emuliert. | **Medium-Interaction** | [https://github.com/Dionaea/dionaea](https://github.com/Dionaea/dionaea) |
| **Cowrie** | Ein SSH- und Telnet-Honeypot, der einen Shell-Zugriff simuliert. Er zeichnet alle Tastatureingaben und Befehle des Angreifers auf. | **Medium-Interaction** | [https://github.com/cowrie/cowrie](https://github.com/cowrie/cowrie) |
| **T-Pot** | Ein umfassendes Honeypot-System, das verschiedene Low- und Medium-Interaction-Honeypots in einem Docker-Container-Netzwerk bündelt. | **Low- & Medium-Interaction** | [https://github.com/telekom-security/t-pot-autoupdate](https://github.com/telekom-security/t-pot-autoupdate) |
| **KFSensor** | Ein Windows-basierter Honeypot, der verschiedene Dienste simuliert und als IDS- und IPS-Lösung dienen kann. | **Low-Interaction** | [https://www.keyfocus.net/kfsensor](https://www.keyfocus.net/kfsensor) |
| **Glastopf** | Ein Web-Honeypot, der Schwachstellen in Webanwendungen (z. B. LFI) emuliert und Angreiferdaten sammelt. | **Medium-Interaction** | [https://github.com/glastopf/glastopf](https://github.com/glastopf/glastopf) |
| **Honny** | Ein minimalistischer Honeypot für Webanwendungen, der vor allem für das Erkennen von SQL-Injection-Angriffen nützlich ist. | **Low-Interaction** | [https://github.com/robert/honny](https://github.com/robert/honny) |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nutzen von Honeypots

- **Früherkennung:** Honeypots helfen, Angriffsmuster und neue Bedrohungen frühzeitig zu identifizieren, noch bevor sie die produktiven Systeme erreichen.

- **Verständnis von Angriffsmethoden:** Sicherheitsteams können durch die Analyse der gesammelten Daten lernen, welche Tools und Taktiken Angreifer einsetzen. Das kann dabei helfen, die eigenen Abwehrmaßnahmen gezielter zu gestalten.

- **Ablenkung:** Honeypots lenken Angreifer vom eigentlichen, wertvollen Ziel ab und halten sie in einer kontrollierten Umgebung fest.

- **Erhöhung der Sicherheit:** Die gesammelten Informationen können zur Verbesserung der Abwehrmechanismen, zur Erstellung neuer Firewall-Regeln oder zur Entwicklung von IDS-Signaturen verwendet werden.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wichtige Überlegungen

Honeypots sind ein mächtiges Werkzeug, aber sie bergen auch Risiken, die unbedingt beachtet werden müssen.

- **Isolation:** Ein Honeypot muss strikt isoliert sein, um zu verhindern, dass ein Angreifer über ihn Zugang zum echten System erhält. Ein Ausbruch aus der Honeypot-Umgebung ist das größte Risiko.

- **Kein Ersatz:** Honeypots ersetzen keine umfassenden Sicherheitsmaßnahmen wie Firewalls, Antivirus-Programme oder Intrusion Detection Systeme. Sie sind eine sinnvolle Ergänzung, aber keine alleinige Lösung.

- **Risiko der Entlarvung:** Wenn ein Angreifer einen Honeypot als solchen erkennt, kann das seine Motivation erhöhen, das eigentliche produktive System zu finden und anzugreifen. Dies kann zu ernsthaften Schäden führen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Links

- [Tryhackme: Introduction to Honepots](https://tryhackme.com/room/introductiontohoneypots)
- [Kaspersky What is a Honeypot](https://www.kaspersky.de/resource-center/threats/what-is-a-honeypot)
- [Wikipedia: Honeypot](https://de.wikipedia.org/wiki/Honeypot)
- [https://www.msxfaq.de/windows/sicherheit/honeypot.htm](https://www.msxfaq.de/windows/sicherheit/honeypot.htm)
- [https://www.honeyd.org/](https://www.honeyd.org/)

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
