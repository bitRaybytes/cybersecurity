# Grundlagen der Web-Sicherheit

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Das Internet (Web)](#das-internet-web)
    - [Sicherheitsaspekt](#sicherheitsaspekt)
    - [Echte Szenarien](#echte-szenarien)
- [Infrastruktur des Internet](#infrastruktur-des-internet)
    - [Komponenten von Web-Services](#komponenten-von-web-services)
- [Sicherheitsmaßnahmen](#sicherheitsmaßnahmen)
    - [Sicherheit der Anwendung](#sicherheit-der-anwendung)
    - [Sicherheit des Web Servers](#sicherheit-des-web-servers)
    - [Schutz des Host Systems](#schutz-des-host-systems)
    - [Logging](#logging)
- [Schutzsysteme für das Web](#schutzsysteme-für-das-web)
    - [Content Delivery Network (CDN)](#content-delivery-network-cdn)
    - [Web Application Firewalls (WAF)](#web-application-firewalls-waf)
        - [WAF: Erkennungsmethoden](#waf-erkennungsmethoden)
    - [Antivirus](#antivirus)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

Das Internet und die darauf basierenden Web-Applikationen sind heute allgegenwärtig und bilden das Fundament für zentrale Aktivitäten wie Online-Banking, -Shopping und soziale Medien. Unabhängig davon, ob es um den aktiven Schutz einer Unternehmenswebseite oder die forensische Analyse eines laufenden Angriffs geht, ist das fundierte Verständnis der Architektur und Funktionsweise von Web-Applikationen unerlässlich.   
Sie stellen die am häufigsten exponierte und somit kritischste Angriffsfläche dar. Der fundamentale Wandel von isolierten Desktop-Anwendungen hin zu permanent vernetzten Web-Applikationen erfordert eine angepasste Denkweise und bildet den ersten Schritt hin zu einer robusten, risikobasierten Sicherheit.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das Internet (Web)

Dynamische Web-Applikationen wie Online-Banking oder Online-Shopping sind heute nicht mehr wegzudenken.

Früher, als Software über CDs oder Boot-Disks installiert wurde, fungieren heute Browser als Übersetzer und Vermittler von Informationen zwischen Client (Benutzer) und Server.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Sicherheitsaspekt

Moderne Webanwendungen bieten Vorteile wie schnellere Updates und globale Verfügbarkeit. Mittlerweile gibt es zahlreiche komplexe Anwendungen wie Discord oder Microsoft Teams, die vollständig oder teilweise im Browser laufen.
Der Wandel von Desktop-Applikationen zu Web-Anwendungen bringt jedoch erhebliche und nicht zu unterschätzende Risiken mit sich.

Webanwendungen sind mittlerweile übliche Angriffsvektoren für Angreifer, die das Ziel haben, Daten auszuspähen, Kontrolle zu erlangen oder Geschäftsprozesse zu unterbrechen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Echte Szenarien
Die folgenden Szenarien bieten einen kurzen Einblick in die häufigsten Angriffsvektoren auf Webanwendungen.

1. **Szenario: SQL Injection (SQLi):**

Ein Angreifer entdeckt eine unzureichend validierte Benutzereingabe (z. B. in einem Login-Feld). Anstatt des Benutzernamens fügt er einen bösartigen SQL-Code-Ausschnitt (z. B. `' OR 1=1 --`) ein. Der Webserver leitet diesen Code direkt an die Datenbank weiter, wodurch der Angreifer die Abfrage manipulieren kann. Das Ergebnis kann die Umgehung der Authentifizierung oder das Auslesen sensibler Datenbanktabellen sein.


2. **Szenario: Cross-Site Scripting (XSS):**

Ein Angreifer findet eine Webseite, die Benutzereingaben ungesäubert (nicht sanitized) anzeigt (z. B. in einem Kommentarfeld). Er injiziert ein bösartiges JavaScript-Snippet. Wenn ein unschuldiger Benutzer diese Seite besucht, wird das JavaScript im Browser des Opfers ausgeführt. Dies kann dazu führen, dass die Session Cookies des Opfers an den Angreifer gesendet oder schädliche Aktionen im Namen des Opfers durchgeführt werden (Session Hijacking).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Infrastruktur des Internet

Beim Besuch einer Webseite sendet dein Browser einen Request an einen Web-Server. Der Server verarbeitet deine Anfrage, führt ggf. Authentifizierungsprozesse durch und liefert dann das Ergebnis in einem Response zurück.

Dieser Prozess von Anfrage (Request) und Antwort (Response) ist der Grundpfeiler der Web-Infrastruktur. Durch Manipulation von HTTP-Headern oder des Request-Bodies können Angreifer Zugriff auf Bereiche erhalten, für die sie auf normalem Wege keinen Zugriff erhalten würden (z. B. durch **Insecure Direct Object Reference** - **IDOR**).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Komponenten von Web-Services

Jede Internetseite benötigt drei grundlegende Komponenten, damit sie funktioniert:

- **Anwendung (Application):** Beinhaltet den gesamten Code, Assets (Bilder, Logos) und die Logik, die diktiert, wie die Seite sich zu verhalten hat (Frontend- und Backend-Code).
- **Web Server:** Eine Software (z. B. Apache, Nginx, IIS), die die Anwendung hostet, auf eingehende Anfragen lauscht und die Antworten an den Client zurückgibt.
- **Host System (Host Machine):** Das Betriebssystem (OS) wie Linux oder Windows, das den Web Server und die Anwendung betriebsbereit macht.


```text
+--------------+  [Öffnet Webseite]   +----------+        +-------------+  [Verarbeitung]    +--------------------+
| Dein Browser |--------------------> | Internet | -----> |   Apache    |------------------> | Web App            |
+--------------+                      +----------+        |  Web Server |  [z.B "/login"]    | geschrieben in PHP |
                                                          +-------------+                    +--------------------+
``` 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsmaßnahmen
Ein umfassender Sicherheitsprozess muss die drei Komponenten **Application**, **Web Server** und **Host Machine** gleichwertig behandeln.

### Sicherheit der Anwendung

- **Sicheres Programmieren:** Unsafe Funktionen (z. B. `eval()` in JavaScript, `exec()` in PHP) vermeiden und sensible Informationen (**Secrets**) niemals im Quellcode speichern (z. B. API-Schlüssel).
- **Eingabe-Validierung und Sanitisierung:** Muss immer auf dem Server durchgeführt werden. Alle Benutzereingaben müssen validiert (entsprechen sie dem erwarteten Format?) und sanitisiert (werden potenziell schädliche Zeichen entfernt oder entschärft?) werden, um SQL Injections, XSS und Command Injections zu verhindern.
- **Zugriffskontrollen:** Benutzerrollen und Berechtigungen müssen strikt implementiert werden, um **Insecure Direct Object Reference** (**IDOR**) und **Privilege Escalation** zu verhindern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Sicherheit des Web Servers

- **Loggen:** Detaillierte Log-Dateien (Access Logs) von allen Web-Anfragen erstellen und zentralisieren.
- **Web Application Firewall (WAF):** Blockiert und filtert schadhaften Datenverkehr, bevor dieser die Anwendung erreicht.
- **Content Delivery Network (CDN):** Reduziert die direkte Offenlegung des Origin-Servers und nutzt integrierte WAFs (siehe unten).
- **HTTP Security Headers:** Korrekte Konfiguration von Headern wie `Content-Security-Policy` (CSP), `X-Content-Type-Options` oder `Strict-Transport-Security` (HSTS).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Schutz des Host Systems

- **Least Privilege:** Web-Services und Datenbanken müssen unter dedizierten, gering privilegierten Benutzern laufen, um im Falle einer Kompromittierung des Web-Servers eine laterale Bewegung im Betriebssystem zu verhindern.
- **System Hardening:** Unnötige Dienste, Protokolle und Ports auf dem Host-System deaktivieren/blockieren. Das Schließen unnötiger Angriffsvektoren ist eine grundlegende Sicherheitspraxis.
- **Antivirus:** Antivirus Programme liefern einen guten Schutz für bekannten Malware. Integriere sie in deine Schutzmaßnahmen um schadhafte Programme zu erkennen.
- **Patch Management:** Betriebssystems, des Web Servers und aller Anwendungs-Frameworks (z. B. Node.js, PHP, Python) ist kritisch, um bekannte Schwachstellen zu eliminieren.

### Sicherheitstipps
- **Starke Authentifizierung:** Lass nicht zu, dass jeder sensible Informationen einsehen kann. Schütze deine `admin`-Page oder sonstige sensible Informationen, indem du starke Benutzerauthentifizierung durchführst.
- **Session Management:** Sichere Handhabung von Sitzungen, z. B. durch Setzen des `Secure` und `HttpOnly` Flags für Cookies.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Logging

Server können Statusevents erstellen und in sogenannten Log-Files hinterlegen. Diese dienen Administratoren zur Überprüfung, ob etwas Ungewöhnliches passiert ist. Die Log-Files enthalten Informationen zu Anfragen von Clients auf den Server und umfassen üblicherweise: die Client IP-Adresse, den Zeitstempel, die angefragte URL, den HTTP-Status-Code und den User Agent.

Diese Informationen sind essenziell für die forensische Analyse nach einem Sicherheitsvorfall und zur Echtzeit-Erkennung von Angriffen.

**Vereinfachte Darstellung eines Access Log-Eintrags:**

```text
1.  10.10.10.10 [09/Okt/2025:12:00:00] "GET /index.html HTTP/1.1" 200 1024 "-" "Mozilla/5.0"
2.  10.10.10.10 [09/Okt/2025:12:00:00] "GET /login.html HTTP/1.1" 200 2302 "/login.html" "Mozilla/5.0"
3.  10.10.10.10 [09/Okt/2025:12:00:00] "GET /login.html HTTP/1.1" 200 4127 "/login.html" "Mozilla/5.0"
    \_________/ \____________________/  \______________________/  \_/ \__/ \___________/ \___________/
         |                |                         /              /    \         \            \
     Client IP       Zeitstempel           Angefragte Seite     Status  Antwort  Referrer    User Agent
                                                                Code    Größe    (Quelle)

```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schutzsysteme für das Web

### Content Delivery Network (CDN)

Ein **Content Delivery Netzwork**  speichert und liefert Inhalte aus seinem Cache von einem Server, der geographisch näher am Endbenutzer liegt, als der Ursprungsserver (Origin). Dies reduziert die Latenz und verbessert die Performance. Ein CDN fungiert als Edge-Computer und bietet dabei eine wesentliche Sicherheitsebene.

Die Vorteile von CDNs sind:

- **IP-Verschleierung:** CDNs verstecken die Origin-IP-Adresse des Host-Servers, was es Angreifern erschwert, ein bestimmtes System direkt anzugreifen (z. B. mit DDoS oder Vulnerability Scans).
- **DDoS Schutz:**  CDNs absorbieren Traffic-Spitzen, indem sie den Datenverkehr weltweit verteilen und filtern.
- **HTTPS erzwingen:** Verschlüsselte Kommunikation über TLS ist bei den meisten CDNs im Standard konfiguriert und oft kostenlos verfügbar.
- **Integrierte WAFs:** Anbieter wie **Cloudflare CDN**, **Amazon CloudFront** oder **Azure Front Door** bieten integrierte WAFs.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Web Application Firewalls (WAF)

Eine **Web Application Firewall** (**WAF**) ist eine spezialisierte Firewall, die den HTTP/HTTPS-Datenverkehr auf der Application Layer (Layer 7) überwacht, filtert und blockiert. Im Gegensatz zu herkömmlichen Netzwerk-Firewalls, die Ports und IPs filtern, analysiert eine WAF den tatsächlichen Inhalt des Requests (Header, Body, URL-Parameter). Ihr Ziel ist es, Angriffe wie SQL Injection, XSS, Path Traversal und viele andere der **OWASP Top 10** zu verhindern, bevor sie die Anwendung erreichen.

Die Funktionalität basiert auf einem Regelwerk, das typische Angriffsmuster (Signaturen) erkennt und blockiert.

| WAF Typ | Bereitstellung | Vorteile | Nachteile |
|---------|----------------|----------|-----------|
| **Cloud-based** | Reverse Proxy (CDN-Funktion) | Einfache Implementierung, Skalierbarkeit, DDoS-Schutz, versteckte Origin-IP. | Weniger Kontrolle, Datenverkehr muss durch Drittanbieter geleitet werden. |
| **Host-based** | Modul in Webserver-Software (z. B. `mod_security` für Apache) | Hohe Anpassbarkeit, Zugriff auf interne Anwendungsdaten. | Ressourcenintensiv für den Host, kein Schutz vor DDoS auf Netzwerkebene. |
| **Network-based** | Hardware- oder virtuelle Appliance vor dem Webserver | Hohe Leistung, niedrige Latenz, zentralisierte Verwaltung. | Hohe Kosten, Wartungsaufwand, Single Pint of Failure (SPOF) möglich. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### WAF: Erkennungsmethoden

| Methode | Beschreibung | Beispiel für Erkennung |
|---------|--------------|------------------------|
| **Signatur-basiert** | Sucht nach bekannten Mustern oder Strings, die typisch für Angriffe sind. | Der String `UNION SELECT` oder `<script>` im Request-Body. |
| **Regelbasiert (Positiv/Negativ)** | **Negativ:** Blockiert alles außer bekannten, sicheren Anfragen (Whitelist). **Positiv:** Blockiert alle bekannten böswilligen Muster (Blacklist). | Blockiert alle Anfragen, deren Query-Parameter nicht ausschließlich aus alphanumerischen Zeichen bestehen (Positiv-Modell). |
| **Heuristisch/Behavioral** | Basiert auf maschinellem Lernen und identifiziert Anomalien oder ungewöhnliches Benutzerverhalten. | Ein Benutzer führ in 5 Sekdungen 50 fehlgeschlagene Login-Versuche durch (Brute-Force).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Antivirus

Antivirus (AV)-Software oder moderne Endpoint Protection Platforms (EPP) und Endpoint Detection and Response (EDR)-Lösungen sind für die Web-Sicherheit relevant, da sie die Host Machine schützen.

- **Rolle:** Sie verhindern, dass nach einer erfolgreichen Kompromittierung der Web-Anwendung (z. B. durch einen Webshell-Upload oder RCE) die bösartige Nutzlast (Malware) auf dem Host-Betriebssystem ausgeführt werden kann.
- **Funktionalität:** Erkennung und Quarantäne von:

    - Bekannter Malware (Signatur-basiert).

    - Dateien, die schädliches Verhalten zeigen (Behavioral-Analyse, z. B. Versuche, die Registry zu ändern oder Systemdateien zu verschlüsseln).

Sie dienen als letzte Verteidigungslinie auf dem Betriebssystem, falls alle vorgeschalteten Kontrollen (WAF, sichere Codierung) versagt haben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links

- [TryHackMe: Web Security Essentials](https://tryhackme.com/room/websecurityessentials)
- [TryHackMe: Web Applications Basics](https://tryhackme.com/room/webapplicationbasics)
- [TryHackMe: HTTP in Detail](https://tryhackme.com/room/httpindetail)
- [OWASP TOP 10 (Aktuelle Liste)](https://owasp.org/www-project-top-ten/)
- [PortSwigger Web Security Academe](https://portswigger.net/web-security)


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

📅 **Letzte Aktualisierung:** Oktober 2025   
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
