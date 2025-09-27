# 💻 Web-Anwendungen Grundlagen

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Komponenten einer Web-Anwendung](#komponenten-einer-web-anwendung)
    - [Frontend](#frontend)
    - [Backend](#backend)
- [Die URL](#die-url)
- [HTTP Nachrichten](#http-nachrichten)
- [HTTP Request](#http-request)
- [HTTP Methoden](#http-methoden)
- [HTTP Versionen](#http-versionen)
- [Status Codes](#status-codes)
- [Fazit: Sicherheitsrelevanz der Grundlagen](#fazit-sicherheitsrelevanz-der-grundlagen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einführung
**Web-Anwendungen** sind heute die Basis für fast jede dynamische Webseite, die Nutzer erleben. Sie unterscheiden sich grundlegend von statischen Webseiten, indem sie interaktive Funktionen, Benutzerkonten und die Verarbeitung von Daten ermöglichen. Dabei wird jedem Webseiten-Besucher das **Frontend** dargestellt, während das **Backend** unsichtbar im Hintergrund arbeitet und die notwendige Logik ausführt.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Komponenten einer Web-Anwendung
Web-Anwendungen basieren auf einer **Client-Server-Architektur**. Der Client, also der Browser des Nutzers, sendet Anfragen an den Server, der diese verarbeitet und eine Antwort zurückschickt.  Dieses Modell ist für die Web-Sicherheit von zentraler Bedeutung, da die Kommunikation zwischen beiden Seiten die Hauptangriffsfläche darstellt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Frontend
Das **Frontend** ist die Benutzeroberfläche, die direkt im Webbrowser des Nutzers gerendert wird. Es ist das, was der Nutzer sieht und mit dem er interagiert. Typischerweise wird das Frontend mit einer Kombination aus drei Schlüsseltechnologien entwickelt:



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### HTML (Hypertext Markup Language)
**HTML** ist die Grundlage und das Gerüst jeder Webseite. Es ist eine Auszeichnungssprache, die dem Browser die Struktur der Inhalte vorgibt.

Mit sogenannten Tags wie `<header>`, `<body>`, `<div>` werden Elemente wie Text, Bilder oder Formulare semantisch markiert und angeordnet. Aus Sicherheitssicht ist HTML wichtig, da hier oft die ersten Schwachstellen wie fehlerhafte Formulardefinitionen oder unsichere Links (`<a>` mit `target="_blank"`) zu finden sind.

Mit Tags wie `<p>` können bspw. Text-Inhalte auf die Webseite hinzugefügt werden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### CSS (Cascading Style Sheets)
**CSS** ist für die Formatierung und das Layout der Webseite zuständig. Es bestimmt das Aussehen von HTML-Elementen, also Farben, Schriftarten, Abstände und die Positionierung. Während CSS selbst keine Logik enthält, können Angreifer auch hier Schadcode einschleusen, um zum Beispiel Benutzeroberflächen zu manipulieren oder Daten über das Auslesen von Stilen zu exfiltrieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>




#### JS (JavaScript)
**JavaScript** ist die Programmiersprache des Webs, die Webseiten dynamisch und interaktiv macht. Während HTML und CSS statische Anweisungen sind, ermöglicht JavaScript logische Abläufe, wie die Validierung von Formularen, die Anzeige von Pop-ups oder das asynchrone Laden von Inhalten. Angreifer nutzen JavaScript, um Angriffe wie **Cross-Site Scripting** ([**XSS**](/03-web-security/schwachstellen/xss.md)) auszuführen, bei denen schädlicher Code im Browser des Opfers ausgeführt wird.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Backend

Das **Backend** einer Web-Anwendung ist das, was der User nicht sieht, doch entscheidend ist, damit die Anwendung überhaupt laufen kann. Es ist die serverseitige Logik, die sich um die Verarbeitung von Anfragen, die Kommunikation mit Datenbanken und die Authentifizierung von Nutzern kümmert. Oftmals sind mehrere Backend-Systeme miteinander verbunden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### Backend-Sprachen und Frameworks
Das Backend kann mit einer Vielzahl von Sprachen und Frameworks umgesetzt werden, darunter:

- **Serverseitige Skriptsprachen:** Python (mit Django, Flask), PHP, Node.js (JavaScript), Ruby (mit Ruby on Rails)

- **Kompilierte Sprachen:** Java, C#, Go



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Datenbanken
Mit **Datenkbanken** wie `MySQL`, `NoSQL`, `PostgreSQL` oder `MongoDB` werden Informationen dauerhaft gespeichert, modifiziert und abgerufen. Datenbanken können somit sensible Informationen wie Benutzernamen, E-Mail-Adressen, Passwörter und private Daten enthalten. Aus sicherheitstechnischer Sicht sind sie ein primäres Angriffsziel, und Schwachstellen wie [**SQL-Injection**](/03-web-security/injektionen/sql-injection-cheatsheet.md) gehören zu den gefährlichsten.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Infrastruktur
Verschiedene Infrastruktur-Komponenten arbeiten zusammen, damit Web-Anwendungen reibungslos laufen können. Diese umfassen:

- **Web-Server (z.B. Apache, Nginx):** Liefern statische Inhalte und leiten dynamische Anfragen an den Anwendungsserver weiter.

- **Anwendungs-Server:** Führen die serverseitige Logik aus.

- **Netzwerk-Endgeräte** wie Router und Firewalls.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### WAF (Web Application Firewall)
Eine **WAF** ist eine optionale, doch bei Gebrauch sehr nützliche Komponente für Web-Anwendungen. Sie ist eine spezielle Art von Firewall, die den HTTP/S-Verkehr analysiert und schädliche Anfragen wie SQL-Injections, XSS oder CSRF-Angriffe blockiert, noch bevor sie den Web-Server erreichen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die URL

Eine **URL** (**Uniform Resource Locator**) ist die eindeutige Adresse, die im Browser eingegeben wird, um eine Web-Anwendung zu erreichen. Jeder Teil einer URL kann von Angreifern manipuliert werden.

**Beispielhafter Aufbau einer URL:**

```text
                            Host/Domain  Pfad       Fragment
                                |         |            |
                         |------v----|  |-v--|    |----v----|
 http://benutzer:password@beispiel.de:80/seite?id=1#aufgabe2
 |-^-| |--------^--------|            |^|     |-^-|
   |             |                      |        |
Schema       Benutzer                  Port    Query 
                                              String
```

- **Schema:** Definiert das Protokoll für die Kommunikation. HTTP ist unverschlüsselt, während HTTPS eine sichere, verschlüsselte Verbindung mittels TLS/SSL herstellt.

- **Benutzer:** Authentifizierungsdetails in der URL sind eine massive Sicherheitslücke und sollten niemals verwendet werden.

- **Host/Domain:** Der Name der Webseite, die auf dem Server gehostet wird.

- **Port:** Gibt den Port an, über den die Verbindung zum Server hergestellt wird. Der Standard-Port für HTTP ist `80`, für HTTPS ist es `443`.

- **Pfad:** Zeigt auf eine spezifische Ressource auf dem Server.

- **Query String:** Enthält zusätzliche Daten, die an den Server übermittelt werden, oft in Schlüssel-Wert-Paaren. Dieser Teil ist ein häufiges Ziel für Injection-Angriffe.

- **Fragement:** Zeigt auf einen bestimmten Anker innerhalb der Webseite und wird nur clientseitig verarbeitet.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## HTTP Nachrichten

**HTTP-Nachrichten** sind die fundamentalen Datenpakete, die zwischen dem Browser (Client) und dem Web-Server ausgetauscht werden. Sie bestehen aus einem **Request** (Anfrage) vom Client und einem **Response** (Antwort) vom Server.

Kennzeichnend für HTTP-Messages sind bspw. die **Methode** (`GET`/`POST`/...), Header und Status-Codes (z.B. `404`).

**Jede HTTP-Nachricht folgt einem festen Format:**

- **Start Line:**
    - Enthält die grundlegenden Informationen zur Nachricht (z. B. Methode, Pfad und Version für einen Request; Statuscode und Statusmeldung für einen Response).
- **Headers:**
    - Schlüssel-Wert-Paare, die Metadaten zur Nachricht enthalten (z. B. `Content-Type`, `Cookie`, `User-Agent`). Sicherheitsrelevante Header sind von zentraler Bedeutung für die Absicherung von Web-Anwendungen.
- **Emtpy Line:**
    - Eine leere Zeile, die den Header vom Body trennt.

- **Body:**
    - Der Hauptteil der Nachricht, der die Nutzlast enthält, wie z. B. Formulardaten im Request oder HTML-Inhalt im Response. 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## HTTP Request
Ein **HTTP Request** wird von einem Client an den Server übermittelt. 

**beispielhafter Aufbau einer HTTP-Anfrage:**

```text
GET /user/login.html HTTP/1.1

Host: beispiel.com
User-Agent: Mozilla/5.0
Accept: */*
Accept-Language: de-de
Connection: keep-alive
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## HTTP Methoden
HTTP-Methoden definieren, welche Aktion mit der angeforderten Ressource durchgeführt werden soll.

| **Methode** | **Beschreibung** | **Sicherheitsrelevanz** |
|-------------|------------------|-------------------------|
| **GET**     | Fordert Daten von einer Ressource an. Ist **idempotent**, d. h. mehrfache Ausführung hat keine weiteren Effekte. | Sollte nur für den Abruf von Daten genutzt werden. Keine sensiblen Daten im Query String übertragen! Anfällig für CSRF. |
| **POST**     | Übermittelt Daten zur Verarbeitung an eine Ressource, oft für die Erstellung von neuen Daten. | Sollte für alle Aktionen genutzt werden, die den Zustand ändern (z.B. Login, Daten senden). |
| **PUT**     | Ersetzt eine existierende Ressource vollständig mit den übermittelten Daten. Ist **idempotent**. | Dient zur Modifikation von Ressourcen. |
| **DELETE**     | Löscht eine bestimmte Ressource. Ist **idempotent**. | Dient zur Löschung von Ressourcen. |
| **PATCH**     | Führt eine teilweise Aktualisierung einer Ressource durch. | Dient zur Modifikation von Ressourcen. |
| **HEAD**     | Ist wie GET, aber es wird nur der Header der Antwort geliefert, nicht der Body. | Wird zur schnellen Überprüfung von Meta-Informationen genutzt, ohne große Datenmengen zu laden. |
| **OPTIONS**     | Fordert Informationen über die Kommunikationsoptionen der Zielressource an. | Kann Aufschluss über erlaubte Methoden geben. |

***Idempotenz*** beschreibt in der Informatik, dass selbst bei mehrfacher Ausführung eines API-Aufrufs derselbe Zustand erreicht wird, wie bei einmaliger Ausführung.

**Beispiel:** Ein Status auf wird "Fertig" gesetzt. Beim erneutem Aufruf der API bleibt der Status auf "Fertig", statt ihn mehrmals zu setzen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## HTTP Versionen
Die HTTP-Versionen beschreiben die Entwicklung des Protokolls und die damit verbundenen Leistungs- und Sicherheitsverbesserungen.

| **Version** | **Information** |
|-------------|------------------|
| **HTTP/0.9**     | Die erste Version, die nur `GET`-Anfragen unterstützte. Extrem einfach und unsicher |
| **HTTP/1.0**     | Einführung von Headern und Status-Codes. Ermöglichte die Nutzung weiterer Methoden. |
| **HTTP/1.1**     | Der bis heute am weitesten verbreitete Standard. Ermöglicht persistente Verbindungen, was die Leistung verbessert. |
| **HTTP/2**     | Verbessert die Leistung durch Multiplexing (mehrere Anfragen über eine einzige Verbindung) und Header-Kompression. Erfordert standardmäßig TLS/SSL. |
| **HTTP/3**     | Basiert auf dem QUIC-Protokoll, das HTTP/2-Funktionen nativ im Transportprotokoll umsetzt. Entwickelt, um die Leistung bei mobilen Geräten und instabilen Netzwerken zu verbessern. |

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Status Codes
Ein **Status-Code** ist eine dreistellige Nummer, die im HTTP-Response gesendet wird, um den Status einer Anfrage zu kommunizieren. Sie sind für die Fehleranalyse und die Sicherheit von Web-Anwendungen essenziell.

| **Status Code** | **Beschreibung** |
|-------------|------------------|
| **100-199** | Die Anfrage wurde empfangen und wird weiter verarbeitet. |
| **200-299** | Die Anfrage war erfolgreich. |
| **300-399** | Der Client muss weitere Schritte unternehmen, um die Anfrage abzuschließen. |
| **400-499** | Die Anfrage enthält einen Fehler und kann nicht verarbeitet werden. |
| **500-599** | Der Server konnte die Anfrage nicht erfüllen. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Fazit: Sicherheitsrelevanz der Grundlagen
Die Grundlagen von Web-Anwendungen sind nicht nur die Bausteine für funktionale Webseiten, sondern auch die Grundlage für fast alle Web-Sicherheitslücken. Ein tiefes Verständnis von:

- Der **Client-Server-Architektur**

- Dem Aufbau einer **URL**

- Den verschiedenen **HTTP-Methoden** und **Status-Codes**

- Und dem Zusammenspiel von **Frontend**- und **Backend**-Komponenten

ist der erste Schritt, um Angriffsflächen zu identifizieren. Ohne dieses Wissen sind Angriffe wie XSS, SQL-Injection, CSRF oder die Manipulation von HTTP-Headern kaum nachvollziehbar.

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