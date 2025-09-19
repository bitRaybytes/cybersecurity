# Cyber-Angriffe: Ein umfassender Überblick über die Grundlagen

Cyber-Angriffe sind absichtliche Handlungen, die darauf abzielen, Netzwerke, Systeme und Daten zu schädigen oder unbefugten Zugriff zu erlangen. Sie nutzen Schwachstellen und eine Vielzahl von Techniken, um ihre Ziele zu erreichen.


## Inhaltsverzeichnis
- [Grundbegriffe des Cyber-Angriffs](#grundbegriffe-des-cyber-angriffs)
- [Schadsoftware (Malware)](#schadsoftware-malware)
- [Verbreitungswege von Malware](#verbreitungswege-von-malware)
- [Angriffsarten: DoS, DDoS und Social Engineering](#angriffsarten-dos-ddos-und-social-engineering)
    - 1. [DoS und DDoS-Angriffe](#1-dos-und-ddos-angriffe)
    - 2. [Social Engineering & Identitätsdiebstahl](#2-social-engineering--identitätsdiebstahl)
- [Prävention und Schutz](#prävention-und-schutz)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundbegriffe des Cyber-Angriffs
Cyber-Kriminelle verfolgen unterschiedliche Ziele, von finanziellen bis hin zu politischen Motiven. Der Angriffsvektor ist die Kombination aus **Angriffsweg** und **Angriffstechnik**, die beschreibt, wie ein Angreifer in ein System eindringt.

- **Angriffsziele:** 
    - Datendiebstahl, Erpressung, Sabotage, Spionage, Wettbewerbsschädigung oder das Erreichen politischer Ziele.
- **Angriffswege:** 
    - Netzwerke, Webseiten, E-Mail, Datenspeicher, Hardware (wie IoT-Geräte oder Schnittstellen), Protokolle, Betriebssysteme, Software oder der Mensch selbst (Social Engineering).
- **Angriffstechniken:** 
    - Das Ausnutzen von Schwachstellen, Code-Injection, Malware, Spam oder physische Manipulation.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schadsoftware (Malware)
Malware (Malicious Software) ist ein Überbegriff für alle schädlichen Programme. Sie befallen IT-Systeme unbemerkt und können Daten ausspionieren, Systeme lahmlegen oder eine unbefugte Hintertür öffnen.

- **Viren:** Selbst-replizierender Code, der sich an Dateien oder Boot-Sektoren hängt und sich so verbreitet.
- **Trojaner:** Tarnen sich als nützliche Programme, um eine Hintertür (Backdoor) im System zu öffnen und weitere Schadsoftware nachzuladen.
- **Ransomware:** Verschlüsselt Daten oder blockiert den Systemzugriff, um Lösegeld (Ransom) zu erpressen.
- **Adware:** Zeigt unerwünschte Werbung an.
- **Scareware:** Täuscht Warnmeldungen vor, um Nutzer zur Installation schädlicher Software zu verleiten.
- **PUA (Potentially Unwanted Application):** Programme, die oft unerwünschte Zusatzfunktionen mit sich bringen und heimlich mit anderer Software installiert werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Verbreitungswege von Malware
Die Infektion eines Systems kann auf verschiedene Weisen erfolgen:

- **Infizierte E-Mail-Anhänge:** 
    - Malware versteckt sich in scheinbar harmlosen Dateien wie Rechnungen im PDF-Format oder Office-Dokumenten. Beim Öffnen wird die Schadsoftware ausgeführt.
- **Drive-by-Infection:** 
    - Schadsoftware wird automatisch heruntergeladen, wenn ein Nutzer eine manipulierte oder unsichere Website besucht. Der Vorgang läuft ohne Interaktion des Nutzers ab.
- **Soziale Netzwerke:** 
    - Angreifer verbreiten Malware über infizierte Links in Chats, Direktnachrichten oder Beiträgen. Diese Links führen zu schädlichen Websites oder veranlassen einen automatischen Download.
- **Spear-Infection:** 
    - Dies ist eine gezielte Form der Infektion. Der Angreifer nutzt detaillierte Informationen über das Opfer, um eine hochpersonalisierte Nachricht zu erstellen. Zum Beispiel eine E-Mail, die vorgibt von einem Kollegen zu sein und einen Anhang mit einem Virus enthält.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Angriffsarten: DoS, DDoS und Social Engineering
### 1. DoS und DDoS-Angriffe
Denial-of-Service (DoS)-Angriffe zielen darauf ab, einen Dienst oder Server durch Überlastung unerreichbar zu machen. Ein DDoS (Distributed Denial-of-Service)-Angriff ist eine erweiterte Form, bei der der Angriff von vielen über das Internet verteilten Geräten („Bots“) gleichzeitig ausgeführt wird.

Für DDoS-Angriffe werden oft Botnets genutzt. Ein Botnet ist ein Netzwerk infizierter Computer, die von einem Angreifer über einen zentralen Command-and-Control-Server (C&C) ferngesteuert werden.
```text
       +------------+
       | Angreifer  |
       +------------+
             |
             | Befehle
             v
+------------------------+
|  Command-and-Control   |
|  (C&C) Server          |
+------------------------+
             |
             +----> Botnet <--+------------+
             |                |            | usw. 
             v                v            v
    +----------+     +----------+     +----------+
    | PC Bot A | ... | PC Bot B | ... | PC Bot C |
    +----------+     +----------+     +----------+
          |               |                 |
    +------v---------------v----------------v----+
    |   Massenhafte Anfragen zur Überlastung     |
    +--------------------------------------------+
          |                                 |
+---------v---------------------------------v----------+
|             Ziel-Webserver (wird überlastet)         |
+------------------------------------------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Social Engineering & Identitätsdiebstahl
Social Engineering bezeichnet die Manipulation von Menschen, um an vertrauliche Informationen zu gelangen.

- **Phishing:** 
    - Massenhaft versendete E-E-Mails, die vorgeben, von einer seriösen Quelle zu stammen (z. B. einer Bank), um Zugangsdaten oder andere sensible Informationen abzufischen.

- **Spear-Phishing:** 
    - Ein gezielter Phishing-Angriff auf eine bestimmte Person oder Organisation.

- **CEO-Fraud:** 
    - Eine Form des Spear-Phishings, bei der sich der Angreifer als hochrangige Führungskraft ausgibt, um Mitarbeiter zu Geldüberweisungen oder zur Datenherausgabe zu bewegen.

- **Credential Stuffing:** 
    - Angreifer verwenden gestohlene Zugangsdaten (Benutzername und Passwort), um sich automatisiert in andere Dienste einzuloggen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Weitere Angriffsarten und -Techniken
- **Advanced Persistent Threats (APT):** 
    - Hochkomplexe, gezielte und langfristige Angriffe auf Unternehmen oder Organisationen.

- **[Man-in-the-Middle (MitM) Angriffe](/02-network-security/mitm_angriff.md):** 
    - Ein Angreifer schaltet sich unbemerkt zwischen zwei kommunizierende Parteien, um den Datenverkehr abzufangen oder zu manipulieren.

- **Keylogging:** 
    - Ein Programm, das jeden Tastenanschlag aufzeichnet, um Passwörter, Bankdaten und andere Informationen zu stehlen.

- **Jailbreak und Rooting:** 
    - Das Entfernen von Sicherheitseinschränkungen auf mobilen Geräten, was diese anfälliger für Angriffe macht.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Prävention und Schutz
Der Schutz vor Cyber-Angriffen erfordert eine Kombination aus technischen Maßnahmen und bewusstem Verhalten.

- **Technische Maßnahmen:** 
    - Verwende Firewalls, Antivirus-Software, halte deine Software immer auf dem neuesten Stand und aktiviere Multi-Faktor-Authentifizierung (MFA).

- **Bewusstes Verhalten:** 
    - Sei immer misstrauisch gegenüber unbekannten E-Mails, Links und Anhängen. Überprüfe die Seriosität des Absenders. Lösche Spam-Mails sofort und gib niemals vertrauliche Informationen über unsichere Kanäle preis.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Man-in-the-Middle (MitM) Angriffe](/02-network-security/angriffe/mitm_angriff.md)
- [Protokoll Header Cheatsheet](/02-network-security/protokoll_header_cheatsheet.md)




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