# 📚 Ressourcen-Repository zum ethischen Hacken

Der Weg zum IT-Sicherheitsexperten ist ein fortlaufender Lernprozess, der eine Kombination aus theoretischem Wissen, praktischer Anwendung und dem Verständnis von Industriestandards erfordert. Dieses Dokument kategorisiert die besten kostenlosen und kostenpflichtigen Ressourcen, um deine Fähigkeiten im Ethical Hacking und Penetration Testing aufzubauen und zu vertiefen.

## Inhaltsverzeichnis
- [Interaktive Lernplattformen & CTFs](#interaktive-lernplattformen--ctfs)
- [Wargames & Binary Exploitation](#wargames--binary-exploitation)
- [Target-Anwendungen & VMs (Lokale Übungsumgebung)](#target-anwendungen--vms-lokale-übungsumgebung)
- [Videobasierte Kurse & Bildungsressourcen](#videobasierte-kurse--bildungsressourcen)
    - [Empfohlene YouTube-Kanäle](#empfohlene-youtube-kanäle)
- [Referenzwerke & Sicherheitsstandards](#referenzwerke--sicherheitsstandards)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Interaktive Lernplattformen & CTFs 

Diese Plattformen sind ideal für praktische Übungen, da sie dir eine sichere Umgebung bieten, in der du deine Hacking-Fähigkeiten testen und verbessern kannst.

| Plattform | Fokus | Link |
|-----------|-------|------|
| TryHackMe | Geführte Kurse, Roadmaps (Anfänger-freundlich) | [tryhackme.com](https://tryhackme.com/) |
| Hack The Box | Realistische Hacking-Szenarien (Machines/Challenges) | [hackthebox.com](https://www.hackthebox.com/) |
| PortSwigger Web Security Academy | Web-Schwachstellen (SQLi, XSS, SSRF) mit Burp Suite | [portswigger.net/web-security/all-labs](https://portswigger.net/web-security/all-labs) |
| picoCTF | CTF-Lernprogramm für Schüler & Studenten | [picoctf.org](https://picoctf.org/) |
| Root-Me | Große Challenge-Bibliothek (400+ Aufgaben) | [root-me.org](https://www.root-me.org/) |
| Hacksplaining | Animierte Erklärungen von Web-Schwachstellen | [hacksplaining.com](https://hacksplaining.com/) |
| Hacking-Lab | CTF-Wettbewerbe und Online-Labs | [hacking-lab.com](https://hacking-lab.com/) |
| CTFtime | Zentraler Kalender und Übersicht aller globalen CTFs | [ctftime.org](https://ctftime.org/) |
| CyberTalents | Kostenlose Kurse, CTFs und Talentförderung | [cybertalents.com](https://cybertalents.com/) |
| LabEx | Interaktive Labs (Linux, DevOps, Cybersecurity) | [LabEx](https://labex.io/de) |
| W3Challs | Vielfalt von Challenges in verschiedenen Kategorien | [w3challs.com](https://w3challs.com/) |
| Ringzer0ctf | CTF-Wettbewerbe mit verschiedenen Challenges | [ringzer0ctf.com](https://ringzer0ctf.com/) |
| Hack This Site! | Community-Plattform mit realistischen Hacking-Szenarien | [hackthissite.org/](https://www.hackthissite.org/) |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wargames & Binary Exploitation

Diese Kategorie fokussiert sich auf das Hacken über die Kommandozeile (SSH) und das Ausnutzen von Schwachstellen in Software-Binaries.

| Plattform | Fokus | Zielsystem | Link |
|-----------|-------|------------|------|
| OverTheWire | Unix/Linux Basics, Networking, Exploitation | SSH / Wargames | [overthewire.org](https://overthewire.org/wargames/) |
| SmashTheStack | Binary Exploitation, Reverse Engineering | SSH / Wargames | [smashthestack.org](https://www.smashthestack.org/) |
| Microcorruption | Embedded Systems, Hardware Hacking, Assembler | Interaktives Interface | [microcorruption.com](https://microcorruption.com/login) |
| Try2hack | Älteste Web- und Binary Challenges | Diverse | [try2hack.nl](https://www.try2hack.nl/) |
| Deusx64 - Einführungskurs | Wargame-Einführungskurs | Browser / Wargame | [https://wargames.ret2.systems/course](https://wargames.ret2.systems/course) |
| Deusx64 | Wargame | Browser / Wargame | [https://deusx64.ai/](https://deusx64.ai/) |
| Netgarage | Fokus auf Reverse Engineering | Wargame | [io.netgarage.org](http://io.netgarage.org/) |
| Crackmes.one | Reverse Engineering für Anfänger bis Fortgeschrittene | Wargame | [crackmes.one](/https://crackmes.one) |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Target-Anwendungen & VMs (Lokale Übungsumgebung)
Diese Anwendungen und VMs sind absichtlich verwundbar. Sie ermöglichen es dir, eine kontrollierte Testumgebung lokal aufzubauen, um gängige Schwachstellen ohne Gefahr für reale Systeme zu testen.

| Ziel | Typ | Primäre Schwachstellen | Link |
|------|-----|------------------------|------|
| VulnHub | Virtuelle Maschinen (VMs) | System-Exploits, Privilege Escalation | [vulnhub.com](https://www.vulnhub.com/) | 
| WebGoat | Webanwendung (von OWASP) | OWASP Top 10 (SQLi, XSS, XXE) | [github.com/WebGoat/WebGoat](https://github.com/WebGoat/WebGoat) | 
| Mutillidae | Webanwendung (PHP/MySQL) | Allgemeine Web-Schwachstellen, Data Exposure | [github.com/webpwnized/mutillidae](https://github.com/webpwnized/mutillidae) | 
| Google Gruyere | Webanwendung (von Google) | XSS, CSRF, Path Traversal | [google-gruyere.appspot.com](https://google-gruyere.appspot.com/) | 
| Damn Vulnerable iOS App (DVIA) | Mobile Anwendung (iOS) | Mobile Security (Insecure Storage, Jailbreak Detection) | [github.com/prateek147/DVIA-v2](https://github.com/prateek147/DVIA-v2) | 
| Websploit | Containerisierte Labs (Docker) | Container Security, diverse Exploits | Verschiedene Docker Images | 
| Hacker's Home | Verwundbare VMs und Hacking-Challenges | Diverse System- und Web-Schwachstellen | [hbh.sh/home](https://hbh.sh/home) | 
| ITSecGames | Verschiedene Lern-VMs | Allgemeine Sicherheitskonzepte | [itsecgames.com](http://www.itsecgames.com/) | 




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Videobasierte Kurse & Bildungsressourcen

Diese Ressourcen bieten strukturierte Kurse und Video-Tutorials von Branchenexperten und Universitäten.


| Ressource | Format | Fokus |Link |
|-----------|--------|-------|-----|
| The Cyber Mentor (TCM) Academy| Kurse (Video) | Praxisnahe Pentesterschulungen (sehr angesehen) | [academy.tcm-sec.com](https://academy.tcm-sec.com/) |
| Cybrary | Kurse (Video/Text) | Breites Spektrum (Netzwerk, Compliance, Hacking) | [cybrary.it](https://www.cybrary.it/) |
| freeCodeCamp | Kurse (Video/Text) | Strukturierter Lehrplan zur Informationssicherheit | [freecodecamp.org/learn/information-security/](https://www.freecodecamp.org/learn/information-security/) |
| HackerOne Hacker101 | Videos/Kurse | Einführung in Bug-Bounty-Programme | [hackerone.com/hacker101](https://www.hackerone.com/hackers/hacker101) |
| EC-Council CyberSecurity Exchange | Kurse | Kostenlose Schulungen für angehende ethische Hacker | [eccouncil.org/cybersecurity-exchange/ethical-hacking/free-ethical-hacking-courses/](https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/free-ethical-hacking-courses/) |
| Udemy / Coursera / edX | MOOCs (Video) | Akademische und zertifizierte Kurse, oft auditierbar | [udemy.com](https://www.udemy.com/) / [coursera.org](https://www.coursera.org/) / [edx.org](https://www.edx.org/) |
| Cisco Networking Academy | Kurse | Netzwerksicherheit und Grundlagen (CCNA Security) | [netacad.com](https://www.netacad.com/courses/ethical-hacker?courseLang=en-US) |
| Master of Project Academy | Überblick (Video/Text) | Kostenloser Überblick über ethisches Hacken | [masterofproject.com/p/ethical-hacking-overview](https://masterofproject.com/p/ethical-hacking-overview?srsltid=AfmBOopWVFVQQYFDX7CimX4ydb_gZqoYMYtKxeL2twYGFpUPGcTg4MbZ) |
| boot.dev | Interaktive Plattform | Programmieren lernen mit Fokus auf Entwicklungssicherheit | [https://boot.dev/](https://www.boot.dev) |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Empfohlene YouTube-Kanäle
Diese Kanäle bieten tiefe technische Einblicke, Walkthroughs und Erklärungen komplexer Schwachstellen:

| Kanal | Format |
|-------|--------|
| [LiveOverflow](https://www.youtube.com/@liveoverflow) | Fokus auf Binary Exploitation, CTF-Lösung und tiefgehende technische Erklärungen. |
| [IppSec](https://www.youtube.com/@ippsec) | Hochwertige Walkthroughs von Hack The Box Machines. |
| [John Hammond](https://www.youtube.com/@_JohnHammond) | CTFs, Malware-Analyse und Security-Tipps. |
| [David Bombal](https://www.youtube.com/@davidbombal) | Netzwerke, Cloud Security und Zertifizierungsvorbereitung. |
| [Professor Messer](https://www.youtube.com/@professormesser/) | Netzwerke und Co - Zertifizierungsvorbereitung. |
| [Network Chuck](https://www.youtube.com/@NetworkChuck/) | Professionelle Videos zu Netzwerken, Frameworks und Zertifizierungsvorbereitung. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Referenzwerke & Sicherheitsstandards

Diese Referenzwerke und Standards sind unerlässlich für die Analyse, Dokumentation und Verteidigung von Systemen.

| Ressource | Fokus | Relevanz | Link |
|-----------|-------|----------|------|
| OWASP Foundation | Web Application Security Standards| Die Grundlage für alle Web-Sicherheitstests. OWASP Top 10 ist Pflichtlektüre. | [owasp.org](https://owasp.org/) |
| MITRE ATT&CK | Taktiken, Techniken & Prozeduren (TTPs) | Das Standard-Framework zur Modellierung und Verteidigung gegen reale Angriffe (Red/Blue Team). | [attack.mitre.org.](https://attack.mitre.org/) |
| Google Hacking Database (GHDB) | Suchmaschinen-Hacking (Dorks) |Finden von exponierten Informationen und verwundbaren Servern über Google. | [exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database) |
| HackerOne Bug-Bounty-Liste | Real-World Vulnerability Reports | Lernen Sie aus dokumentierten Fehlern anderer Hacker. | [hackerone.com/bugs](https://hackerone.com/bugs) |
| Hackernoon | Fachblog & Tutorials | Aktuelle Artikel und Analysen zu neuen Exploits und Techniken. | [hackernoon.com](https://hackernoon.com/) |
| TutorialsPoint | Einfache, verständliche Anleitungen |Grundlagenwissen zu vielen IT- und Hacking-Themen | [tutorialspoint.com](https://www.tutorialspoint.com/ethical_hacking/index.htm) |


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

🗓️ **Letzte Aktualisierung:** Oktober 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
