# 🌐 So funktioniert eine Website

## Inhaltsverzeichnis
- [Grundlagen des Webs](#grundlagen-des-webs)
    - [Client-Server-Modell](#client-server-modell)
    - [DNS – das Telefonbuch des Internets](#dns---das-telefonbuch-des-internets)
    - [HTTP/HTTPS](#httphttps)
- [Webserver-Grundlagen](#webserver-grundlagen)
    - [Was ist ein Webserver?](#was-ist-ein-webserver)
    - [Virtuelle Hosts](#virtuelle-hosts)
- [Weitere Web-Komponenten](#weitere-web-komponenten)
    - [1. Load Balancer (Lastverteiler)](#1-load-balancer-lastverteiler)
    - [2. CDN (Content Delivery Networks)](#2-cdn-content-delivery-networks)
    - [3. Datenbanken](#3-datenbanken)
    - [4. WAF (Web-application-firewall)](#4-waf-web-application-firewall)
- [Statische vs. Dynamische Inhalte](#statische-vs-dynamische-inhalte)
    - [Frontend und Backend](#frontend-und-backend)
- [Zusammenspiel – Request → Response](#zusammenspiel--request--response)



## Grundlagen des Webs

### Client-Server-Modell

Das Web basiert auf dem **Client-Server-Modell**:
- **Client:** Dein Browser (Chrome, Firefox, Edge) → stellt Anfragen.
- **Server:** Hostet die Website → sendet Antworten zurück.

```yaml
+------------+                  +-----------+
|   Client   | --- Anfrage ---> |  Server   |
|  (Browser) |                  | (Website) |
+------------+ <--- Antwort --- +-----------+
```


Der Client **fordert** Inhalte an, der Server **liefert** Inhalte aus.



### DNS - das Telefonbuch des Internets

Der Browser versteht keine Domainnamen wie `example.com`, sondern nur **IP-Adressen**.  
Hier kommt das **Domain Name System (DNS)** ins Spiel:

```yaml
+----------+      "example.com?"         +----------+
|  Browser | --------------------------> | DNS-Server |
+----------+                             +----------+
     ^                                        |
     |          "93.184.216.34"               v
     +----------------------------------------+
     |
     v
(Verbindung zu 93.184.216.34 wird aufgebaut)
```

- **Domain:** Menschenlesbarer Name (`example.com`)
- **DNS-Server:** Übersetzer von Domain → IP
- **IP-Adresse:** Ziel des Clients (z. B. des Webservers)



### HTTP/HTTPS

- **HTTP (HyperText Transfer Protocol):** Basisprotokoll für Webseiten.
- **HTTPS (HTTP Secure):** HTTP + **TLS/SSL-Verschlüsselung** → schützt vor Mitlesen & Manipulation.


```yaml
Browser <--- HTTP (unverschlüsselt) ---> Server
Browser <--- HTTPS (verschlüsselt) ----> Server
```

## Webserver-Grundlagen

### Was ist ein Webserver?

Ein **Webserver** ist eine Software, die eingehende Anfragen empfängt und Webseiten-Inhalte über das HTTP-Protokoll an den Browser sendet. Die am häufigsten verwendeten Webserver-Programme sind `Apache`, `Nginx`, `IIS` und `NodeJS`.

Ein Webserver liefert Dateien aus seinem **Stammverzeichnis** (**root directory**). Wenn du beispielsweise `http://www.beispiel.com/bild.jpg` anfragst, sendet der Server die lokale Datei `/var/www/html/bild.jpg` von seiner Festplatte an dich zurück.

### Virtuelle Hosts
Webserver können über **virtuelle Hosts** mehrere Websites mit unterschiedlichen Domainnamen hosten. Der Server prüft den Hostnamen in der Anfrage und gleicht ihn mit den virtuellen Hosts ab, die in Konfigurationsdateien definiert sind.

Findet er eine Übereinstimmung, liefert er die entsprechende Website aus; andernfalls wird die Standard-Website angezeigt. Virtuelle Hosts ermöglichen es, jede Website in einem eigenen Verzeichnis auf der Festplatte abzulegen.

```yaml
+------------+ Host Anfrage: example.com
|  Browser   | ----------------------> +------------------------+
+------------+                         | Webserver mit mehreren |
                                       |      vHosts            |
                                       +------------------------+
                                                 |
                                         /var/www/example.com
                                                 |
                                         /var/www/blog.example.com
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Weitere Web-Komponenten

### 1. Load Balancer (Lastverteiler)
Wenn eine Website zu viel Traffic hat, um von einem einzigen Server bewältigt zu werden, kommen Load Balancer zum Einsatz.

- **Funktion:** Ein Load Balancer empfängt alle Anfragen und leitet sie an einen von mehreren Webservern im Hintergrund weiter.

- **Algorithmen:** Er nutzt Algorithmen wie **Round-Robin** (der die Anfragen der Reihe nach verteilt) oder **Weighted** (der die Anfrage an den Server mit der geringsten Auslastung sendet).

- **Ausfallsicherheit:** Load Balancer führen regelmäßige "**Health Checks**" durch, um sicherzustellen, dass die Server ordnungsgemäß funktionieren. Wenn ein Server nicht antwortet, wird er vorübergehend aus der Verteilung genommen.
- **Zusammenfassend:**
    - verteilt Anfragen auf mehrere Server  
    - erhöht Geschwindigkeit & Ausfallsicherheit

```yaml
            +-----------------+
            |  Load Balancer  |
+---------+ |                 |
| Client  |-------------------+----> Webserver 1
+---------+ |                 |
            +-----------------+----> Webserver 2
            |                 |
            |                 |
            |                 |----> Webserver 3
```


### 2. CDN (Content Delivery Networks)
Ein **CDN** speichert statische Dateien wie Bilder, Videos, CSS und JavaScript auf Tausenden von Servern weltweit.

- **Funktion:** Wenn ein Nutzer eine dieser Dateien anfordert, wird die Anfrage zum geografisch nächsten CDN-Server geleitet, um die Ladezeiten zu verkürzen und den Traffic zum Hauptserver zu reduzieren.
- **Zusammenfassend:** schnellerer Zugriff, weniger Last auf Hauptserver  

```yaml
User in Berlin    ------> CDN-Server in Frankfurt
User in New York  ------> CDN-Server in New Jersey
```

### 3. Datenbanken
Datenbanken sind unerlässlich für Websites, die Nutzerinformationen speichern müssen. Webserver kommunizieren mit ihnen, um Daten zu speichern und abzurufen.

- **Beispiele:** Gängige Datenbanken sind `MySQL`, `MSSQL`, `MongoDB` und `Postgres`.
- Speichern von dynamischen Inhalten (z. B. Userdaten, Posts, Produkte).
- Kommunikation über SQL oder API-Abfragen.

```yaml
+-----------+      +------------+      +------------+
|  Client   | ---> | Webserver  | ---> | Datenbank  |
+-----------+      +------------+      +------------+
                          ^                 |
                          | <--- SQL-Queries / Antworten
                          v
```

### 4. WAF (Web Application Firewall)
Eine WAF agiert als Schutzschild zwischen einer Webanfrage und dem Webserver.

- **Funktion:** Sie analysiert Anfragen auf bekannte Angriffsversuche oder Bots und nutzt Rate Limiting (Begrenzung der Anfragen pro Sekunde), um DDoS-Angriffe zu verhindern.

- **Schutz:** Verdächtige Anfragen werden blockiert, bevor sie den Webserver erreichen können.

```yaml
+---------+       +-----+       +-----------+
| Client  | ----> | WAF | ----> | Webserver |
+---------+       +-----+       +-----------+
                     |
                     `-- Filtert schädliche Anfragen
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Statische vs. Dynamische Inhalte
- **Statischer Inhalt:** Dies sind Dateien, die sich nie ändern, wie Bilder (`.jpg`) oder Stylesheets (`.css`). Sie werden direkt vom Webserver ausgeliefert.

- **Dynamischer Inhalt:** Dies sind Inhalte, die sich je nach Nutzeranfrage oder Daten ändern. Beispiele sind Suchergebnisse oder eine Homepage, die die neuesten Blogeinträge anzeigt.

### Frontend und Backend
Die Website, die du in deinem Browser siehst (HTML, CSS, JavaScript), ist das Frontend.

Die Logik und Verarbeitung, die im Hintergrund ablaufen, sind das Backend. Hier werden Programmier- und Skriptsprachen wie PHP, Python oder Node.js verwendet, um dynamische Inhalte zu erstellen und mit Datenbanken zu interagieren.

```yaml
+-----------------+
|  User (Browser) |
+-----------------+
        |
        v
+------------------------+
|  Frontend (HTML, CSS)  |
+------------------------+
        |
        v
+---------------------------+       +-------------+
| Backend (PHP, Python, ...) | ---> | Datenbank   |
+---------------------------+       +-------------+
```


**Wichtig:** Du siehst im Quellcode einer Webseite niemals den Backend-Code. Das, was du siehst, ist das Ergebnis der Backend-Verarbeitung. Diese Interaktion eröffnet, wenn nicht korrekt implementiert, viele Sicherheitslücken.

## Zusammenspiel – Request → Response

1. Ablauf einer Webseiten-Anfrage:
2. User gibt Domain ein (`www.example.com`)
3. DNS löst Domain auf (z. B. `93.184.216.34`)
4. Browser baut HTTPS-Verbindung zum Webserver auf
5. Webserver prüft Anfrage:
    - Statische Datei? --> direkt senden
    - Dynamischer Inhalt? --> Backend + Datenbank
6. Antwort zurück an den Browser

```yaml
      +-----------+
      |  Browser  |
      +-----------+
            |
            v  (1) DNS-Anfrage
      +-----------+
      | DNS-Server|
      +-----------+
            |
            v  (2) IP-Adresse
      +----------------+
      | Load Balancer  |
      +----------------+
            |
            v  (3) Anfrage
      +----------------+
      |  Webserver     |
      +----------------+
            |
            v  (4) Backend-Verarbeitung (Datenbank / PHP-Script)
      +-----------------+
      |  Datenbank /... |
      +-----------------+
            ^
            |  (5) Antwort
      +----------------+
      |  Webserver     |
      +----------------+
            |
            v  (6) Antwort
      +----------------+
      | Load Balancer  |
      +----------------+
            |
            v  (7) Antwort an Browser
      +-----------+
      |  Browser  |
      +-----------+
```

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

📅 **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
