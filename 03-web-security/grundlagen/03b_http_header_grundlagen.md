# 🌐 HTTP-Header – Grundlagen und Sicherheit

## Inhaltsverzeichnis
- [Was sind HTTP Headers?](#was-sind-http-headers)
- [Der HTTP-Request Header](#der-http-request-header)
- [Der HTTP-Response Header](#der-http-response-header)
- [Sicherheitsrelevante HTTP-Response Header](#sicherheitsrelevante-http-response-header)
- [Angriffe durch Header-Manipulation](#angriffe-durch-header-manipulation)
- [Fazit und Verteidigungsstrategien](#fazit-und-verteidigungsstrategien)
- [Haftungsausschluss](#haftungsausschluss)



## Was sind HTTP Headers?

**HTTP Header** sind ein grundlegender Bestandteil der Hypertext Transfer Protocol (HTTP)-Nachrichten, die zwischen einem Client (z. B. deinem Webbrowser) und einem Server ausgetauscht werden. Sie dienen als Metadaten, die zusätzliche Informationen über die HTTP-Anfrage (Request) oder die HTTP-Antwort (Response) übermitteln.

Jeder Header besteht aus einem Namen und einem Wert, getrennt durch einen Doppelpunkt. Die Kombination dieser Header bestimmt das Verhalten und die Funktionen der Webanwendung.

Der Kommunikationsprozess sieht vereinfacht so aus:

```text
+-------------------+                         +-------------------+
|      CLIENT       |  HTTP-Request Headers   |      SERVER       |
|  (dein Browser)   |------------------------>|    (Webseite)     |
|                   |                         |                   |
|                   |<------------------------|                   |
|                   |  HTTP-Response Headers  |                   |
+-------------------+                         +-------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der HTTP-Request Header
Dieser Header-Typ wird vom Client an den Server gesendet, um Informationen über die Anfrage zu übermitteln. Ein Request Header enthält Details über den anfragenden Client, die gewünschten Daten und die akzeptierten Formate.

**Wichtige Request Header:**

- `Host`: Gibt den Hostnamen der angefragten Ressource an.

- `User-Agent`: Beschreibt den Browser, das Betriebssystem und andere Client-Informationen.

- `Accept-Language`: Informiert den Server über die bevorzugte Sprache des Clients.

- `Cookie`: Überträgt gespeicherte Sitzungsinformationen (Session-ID, etc.) an den Server.

**Beispiel für einen HTTP-Request:**

```text
{
  "GET /index.html HTTP/1.1",
  "Host": "[www.beispiel-website.com](https://www.beispiel-website.com)",
  "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
  "Accept-Language": "de-DE,de;q=0.9,en-US;q=0.8,en;q=0.7",
  "Cookie": "sessionID=a1b2c3d4e5f6g7h8"
}
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Der HTTP-Response Header
Dieser Header-Typ wird vom Server als Antwort auf eine Anfrage zurückgeschickt. Ein Response Header enthält Statusinformationen, Details über den Server und wichtige Anweisungen für den Client.

**Wichtige Response Header:**
- `Content-Type`: Zeigt den Medientyp der gesendeten Ressource an (z. B. `text/html`, `application/json`).

- `Set-Cookie`: Setzt ein Cookie im Browser des Clients.

- `Location`: Wird bei Weiterleitungen (`3xx`-Statuscodes) verwendet und gibt die neue URL an.

- `Server`: Beschreibt die verwendete Server-Software.

**Beispiel für eine HTTP-Response:**
```text
{
  "HTTP/1.1 200 OK",
  "Content-Type": "text/html; charset=UTF-8",
  "Content-Length": "1234",
  "Date": "Wed, 17 Sep 2025 14:30:00 GMT",
  "Server": "Apache/2.4.41"
}
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitsrelevante HTTP-Response Header
Als IT-Sicherheitsexperte ist es entscheidend, die Header zu kennen, die das Verhalten des Browsers aktiv zu Ihrem Vorteil steuern und gängige Angriffe mitigieren.

| **Header** | **Zweck und Sicherheit** |
|------------|--------------------------|
| `Strict-Transport-Security` | **HSTS** (**HTTP Strict Transport Security**) erzwingt, dass der Browser für eine bestimmte Zeit nur über HTTPS mit dem Server kommuniziert. Dies schützt vor **Downgrade-Angriffen**, bei denen ein Angreifer einen Benutzer zwingt, eine unsichere HTTP-Verbindung zu nutzen. |
| `Content-Security-Policy` | **CSP** definiert, welche externen Ressourcen (Skripte, Stylesheets, Bilder) geladen werden dürfen. Eine gut konfigurierte CSP ist der beste Schutz gegen **Cross-Site Scripting** (**XSS**)**-Angriffe**, da sie das Ausführen von nicht vertrauenswürdigem Skript-Code blockiert. |
| `X-Content-Type-Options` | Verhindert, dass der Browser den `Content-Type` von einer Ressource errät. Schützt vor **MIME-Sniffing-Angriffen**, die dazu führen könnten, dass eine eigentlich harmlose Datei als ausführbares Skript interpretiert wird. |
| `X-Frame-Options` | Verhindert das Einbetten einer Website in einen `<iframe>`. Dies ist der beste Schutz gegen **Clickjacking-Angriffe**, bei denen ein Angreifer eine unsichtbare Website über eine bösartige Seite legt. |
| `Referrer-Policy` | Steuert, welche Informationen (der `Referer`-Header) an die Zielseite gesendet werden, wenn ein Benutzer einem Link folgt. Hilft, die Offenlegung sensibler Informationen zu minimieren. |
| `Permissions-Policy` | Eine moderne Alternative zu veralteten Headern. Sie ermöglicht die selektive Deaktivierung von Browserfunktionen wie der Kamera, dem Mikrofon oder der Vollbildansicht. |

**Beispiel: Implementierung von Security Headern in einer JSON-Response**
```text
{
  "HTTP/1.1 200 OK",
  "Content-Type": "application/json",
  "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
  "Content-Security-Policy": "default-src 'self'; script-src 'self'",
  "X-Frame-Options": "DENY",
  "Referrer-Policy": "no-referrer",
  "Permissions-Policy": mikrophone 'none'; camera 'none';
}
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Angriffe durch Header-Manipulation
Angreifer können HTTP-Header manipulieren, um Schwachstellen auszunutzen. Da viele Header vom Client stammen, kann ein Angreifer diese fälschen oder ergänzen.

- **Host Header Injection:** Falsche `Host`-Header können bei bestimmten Konfigurationen zu Weiterleitungen oder dem Zurücksetzen von Passwörtern auf die Domain des Angreifers führen.

- **Session Hijacking:** Ein Angreifer stiehlt den `Cookie`-Header eines Benutzers, um dessen Sitzung zu übernehmen und als dieser Benutzer zu agieren.

- **XSS via Referer:** Ältere, ungeschützte Webseiten können Daten aus dem `Referer`-Header direkt in die Seite einfügen, was zu XSS-Schwachstellen führt.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Fazit und Verteidigungsstrategien
HTTP-Header sind nicht nur ein technisches Detail, sondern eine kritische Verteidigungslinie. Eine Fehlkonfiguration kann das Einfallstor für Angriffe sein.

**Wichtige Empfehlungen:**

- **Konfiguriere immer Sicherheits-Header:** Stelle sicher, dass deine Webanwendungen Header wie `Strict-Transport-Security` und `Content-Security-Policy` korrekt setzt.

- **Validiere alle Client-Eingaben:** Verlasse dich niemals auf Header-Werte wie den Referer oder den User-Agent, da diese leicht gefälscht werden können.

- **Verwende einen Reverse-Proxy/WAF:** Ein vorgeschalteter Proxy oder eine Web Application Firewall kann helfen, Header-Manipulationen abzufangen und zu bereinigen, bevor sie den Server erreichen.


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