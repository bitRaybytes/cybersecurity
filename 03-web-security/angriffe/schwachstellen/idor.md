# IDOR - Grundlagen


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Wie funktionieren IDOR-Angriffe?](#wie-funktionieren-idor-angriffe)
    - [1. URL-Parameter-Manipulation (Gängigster Fall)](#1-url-parameter-manipulation-gängigster-fall)
    - [2. Parameter im Body (POST/PUT-Request)](#2-parameter-im-body-postput-request)
    - [3. Nicht-refernzierte IDs](#3-nicht-refernzierte-ids)
- [Klassifizierung von IDOR-Schwachstellen](#klassifizierung-von-idor-schwachstellen)
- [Was ist IDOR-Middleware](#was-ist-idor-middleware)
- [Präventive Abwehrmaßnahmen](#präventive-abwehrmaßnahmen)
    - [1. Autorisierungsprüfung auf Serverseite (Priorität 1)](#1-autorisierungsprüfung-auf-serverseite-priorität-1)
    - [2. Indirekte Objektreferenzen (OID - Obscured Identifier)](#2-indirekte-objektreferenzen-oid---obscured-identifier)
    - [3. Nutzung von Session- oder Request-Scope-Objekten](#3-nutzung-von-session--oder-request-scope-objekten)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung
**Insecure Direct Object Reference** (**IDOR**), zu deutsch: Unsichere direkte Objekt-Referenz, ist eine Angriffsmethode, bei der ein Angreifer direkte Referenzen auf interne Implementierungsobjekte (wie z. B. Dateinamen, Datenbankschlüssel oder Verzeichnisnamen) in URL-Parametern, Formularfeldern oder HTTP-Headern modifiziert.

Wenn eine Anwendung diese vom Benutzer bereitgestellten Referenzen nicht ausreichend validiert, kann der Angreifer auf Daten oder Funktionen zugreifen, die eigentlich anderen Benutzern oder Berechtigungsstufen vorbehalten sind. IDOR ist eine Form eines **Broken Access Control**-Problems und zählt zu den kritischsten Schwachstellen laut OWASP.

Beispiel: Eine Anwendung nutzt die Benutzer-ID in der URL:
```html
https://app.example.com/profil?user_id=1234
```
Ein Angreifer ändert die ID von `1234` auf `1235`, um das Profil eines anderen Benutzers einzusehen.

Mit IDOR-Angriffen können unter Anderem Nutzerkonten ausgenutzt werden, um Bezahlungen auf Namen eines anderen zu unternehmen, Daten zu löschen oder administrative Funktionen auszuführen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wie funktionieren IDOR-Angriffe?
IDOR tritt auf, wenn die Anwendung die Benutzer-ID zur Identifizierung eines Objekts (z. B. ein Dokument, eine Bestellung, ein Profil) verwendet, ohne zu prüfen, ob der aktuell eingeloggte Benutzer auch tatsächlich berechtigt ist, auf dieses Objekt zuzugreifen.

Hier sind die gängigsten Szenarien:



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. URL-Parameter-Manipulation (Gängigster Fall)
Der Angreifer manipuliert den Wert eines Parameters, der direkt auf eine Datenbankressource verweist.

| Szenario | Original-URL (Angreifer A) | Angriffs-URL (Zugriff auf Daten von B) |
|----------|----------------------------|----------------------------------------|
| **Profilaufruf** | `.../profil/edit?id=A_Profil_001` | `...profil/edit?id=B_Profil_002` |
| **Rechnungsabruf** | `.../rechnung?nummer=2025_042` | `.../rechnung?nummer=2025-043` |
| **Datei-Download** | `...download?file=kundenbericht.pdf` | `...download?file=passwort_liste.txt` |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Parameter im Body (POST/PUT-Request)
Die Objekt-REferenz wird nicht in der URL, sondern im Body einer POST- oder PUT-Anfrage gesendet.

**Beispiel:** Ein Benutzer möchte seine E-Mail-Adresse ändern. Der Request-Body sieht so aus:
```text
{
    "user_id": 1234,
    "email": "neue_email_angreifer.com"
}
```

Der Angreifer ändert jetzt die `user_id` von `1234` auf die ID eines Zielbenutzers (`5678`), um dessen E-Mail zu ändern, ohne sich erneut authentifizieren zu müssen. 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. Nicht-refernzierte IDs
Manchmal greifen Angreifer auf Endpunkte zu, die nicht direkt referenziert, aber leicht erraten werden könne, z. B. eine API, die auf numerische IDs basiert.

```html
https://api.example.com/v1/user/1
https://api.example.com/v1/user/2
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Klassifizierung von IDOR-Schwachstellen

IDOR-Schwachstellen werden oft nach der Art des Objekts oder der Operation klassifiziert:

| Klassifizierung | Beschreibung | Gefährdung |
|-----------------|--------------|------------|
| **Vertikale IDOR** | Zugriff auf Objekte, die einer höheren Berechtigungsstufe (z. B. Admin) vorbehalten sind. | Übernahme von Administrationsfunktionen, Löschung kritischer Daten. |
| **Horizontale IDOR** | Zugriff auf Objekte, die gleichberechtigten Benutzern (z. B. Benutzer B statt Benutzer A) gehören. | Diebstahl von Nutzerdaten, Finanzbetrug. |
| **Globale IDOR** | Zugriff auf Objekte, die nicht authentifizierten oder nicht autorisierten Benutzern vorbehalten sind. | Leckage von Unternehmensdaten, wenn z. B. ungeschützte interne API-Endpunkte direkt aufgerufen werden können. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was ist IDOR-Middleware
Eine Middleware ist ein Teil eines Programms, das bei jedem Request läuft, bevor dieser die eigentliche Business-Logik erreicht (und oft auch auf dem Weg zurück zur Response). Sie sitzt zwischen Request und Response.


```text
+---------+                      +----------+
| Anfrage |-------+              | Response |
+---------+       |              +----+-----+
                  V                   ^
            +-----+------+            |
            | Middleware |            |
            +-----+------+            |
                  |                   |
                  V                   |
            +-----+------+            |
            | Middleware |------------+
            +------------+
```



Eine **IDOR-Middleware** (präziser: eine **Autorisierungs-Middleware**) ist die entscheidende Verteidigungsebene. Ihre Aufgabe ist es, zu prüfen:

1. Ist der Benutzer authentifiziert (eingeloggt)?

2. Ist der Benutzer autorisiert, auf das spezifische Objekt zuzugreifen, das in der Anfrage referenziert wird (z. B. `user_id=1234`)?

Dein Beispiel-Code zeigt genau diesen Zweck (hier in JavaScript/Node-ähnlicher Darstellung für eine Route, die nur Admins erlaubt):

```js
// Beispiel-Code einer Login/Admin-Prüfung als Middleware (konzeptuell)
 public function handle ($request, Closure $next)
 {
    // 1. Prüfung der Authentifizierung und der Rolle
    if ($request->user() == null || ! $request->user()->role->name=="Admin")
    {
        // Wenn nicht Admin, wird die Anfrage umgeleitet
        return redirect()->route('home');
    }
    // Wenn Admin, wird die Anfrage zur Ziel-Funktion weitergeleitet
    return $next($request);
 }
```
**Nutzen:** Durch eine gut implementierte Autorisierungs-Middleware werden IDOR-Angriffe verhindert, indem sichergestellt wird, dass die Referenz auf ein Objekt (z. B. die ID) immer nur im Kontext des berechtigten Benutzers ausgeführt wird.



**Nutzen:** Man kann durch diese Variante Schutz vor `XSS`-Angriffen bewerkstelligen. 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Präventive Abwehrmaßnahmen

Die wirksamste Abwehr gegen IDOR ist die konsequente Implementierung von Zugriffskontrollen (Access Control) auf der Serverseite.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. Autorisierungsprüfung auf Serverseite (Priorität 1)

Jeder Endpunkt, der auf eine Ressource zugreift, die durch eine ID referenziert wird, muss prüfen, ob der eingeloggte Benutzer die Berechtigung für dieses spezifische Objekt besitzt.

```js
// Schlechtes Beispiel (anfällig für IDOR)
function getOrder(orderId, currentUserId) {
    // Holt Bestellung NUR anhand der ID, prüft NICHT den Besitzer
    return db.query(`SELECT * FROM orders WHERE id = ${orderId}`);
}

// Gutes Beispiel (IDOR-sicher)
function getOrder(orderId, currentUserId) {
    // Holt Bestellung NUR, wenn ID und der aktuelle Benutzer übereinstimmen
    return db.query(`SELECT * FROM orders WHERE id = ${orderId} AND user_id = ${currentUserId}`);
}

```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. Indirekte Objektreferenzen (OID - Obscured Identifier)

Verwenden Sie anstelle der direkten Datenbank-IDs (z. B. user_id=1234) indirekte, nicht sequenzielle Referenzen.

- **UUIDs (Universally Unique Identifiers):** Diese langen, zufälligen Zeichenketten (z. B. `a1b2c3d4-e5f6-...`) sind für Angreifer nicht erratbar.

- **Hash-Werte oder verschlüsselte IDs:** Die Datenbank-ID wird verschlüsselt, bevor sie an den Client gesendet wird. Nur der Server kann sie entschlüsseln.

**Beispiel mit OID:**
| IDOR-anfällig (Direkt) | IDOR-sicher (OID/UUID) |
| :--- | :--- |
| `.../profil?id=123` | `.../profil?id=a1b2c3d4-e5f6-7890-...` |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. Nutzung von Session- oder Request-Scope-Objekten
Nutze, wenn möglich, anstelle von Parametern, die der Benutzer manipulieren kann, Objekte, die bereits sicher im Kontext der Benutzersitzung oder des aktuellen Requests gespeichert sind (z. B. die `session_id` oder die `user_id` aus dem Authentifizierungs-Token).


## Nützliche Links
- [YouTube: OWASP DevSlop: Hunting for IDORs with Katie Paxton-Fear (Video)](https://www.youtube.com/watch?v=lNcbSILRugM&t)
- [PortSwigger: IDOR](https://portswigger.net/web-security/access-control/idor)
- [TryHackMe: IDOR (kostenpflichtiger Raum)](https://tryhackme.com/room/idor)


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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
