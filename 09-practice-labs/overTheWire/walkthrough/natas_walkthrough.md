# Over The Wire: Natas

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Natas 0 Zugang](#natas-0-zugang)
- [Natas 0 -> 1](#natas-1---2)
- [Natas 1 -> 2](#natas-1---2)
- [Natas 2 -> 3](#natas-2---3)
- [natas 3 -> 4](#natas-3---4)
- [natas 4 -> 5](#natas-4---5)
- [natas 5 -> 6](#natas-5---6)
- [natas 6 -> 7](#natas-6---7)
- [natas 7 -> 8](#natas-7---8)
- [natas 8 -> 9](#natas-8---9)
- [natas 9 -> 10](#natas-9---10)
- [natas 10 -> 11](#natas-10---11)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

Einleitung zum Wargame Natas (OverTheWire)

`Natas` ist ein webbasiertes Wargame, das vom Projekt `OverTheWire.org` angeboten wird. Es richtet sich an alle, die sich mit Web-Sicherheit und Web-Hacking beschäftigen möchten.

Das Spiel besteht aus mehreren Leveln, die jeweils eine Webseite mit einer spezifischen Schwachstelle darstellen. Ziel ist es, den Zugang zum nächsten Level zu erhalten, indem du das Passwort findest, das irgendwo in der Anwendung versteckt ist – oft durch Ausnutzung klassischer Web-Sicherheitslücken.

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

> Der Name Natas ist "Satan" rückwärts geschrieben – ein Hinweis auf die tiefergehenden Sicherheitsaspekte, die in den Levels behandelt werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Natas 0 Zugang

**Aufgabe:** Erhalte Zugang zur Challenge


Username:   natas0

Password:   natas0

URL:        http://natas0.natas.labs.overthewire.org

<details>
    <summary>Lösung</summary>

Gib die URL in deinem Browser ein. Melde dich mit dem Username `natas0` und dem Passwort `natas0` auf der Webapplikation an.
Sobald du eingeloggt bist, erscheint die Webseite und die erste Challenge beginnt. 

Finde das Passwort für das nächste Level `natas1`.

Öffne den Inspector und inspiziere die Webseite. Drücke entweder die `rechte Maustaste` und wähle `untersuchen` aus oder drücke auf der Tastatur `Strg` + `Umschalt` + `c` auf Windows und Linux/Mac `Cmd` + `Umschalt` + `c`.

![Natas1 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas1.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 1 -> 2

Username:   natas1

URL:        http://natas1.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

In diesem Level ist es nicht möglich, das Kontextmenü per rechter Maustaste zu öffnen. Allerdings kennst du bereits ein Shortcut. Verwende ihn und untersuche die Webseite.

![Natas2 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas2.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 2 -> 3

Username:   natas2

URL:        http://natas2.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Das erste, was du siehst, ist der Satz: `There is nothing on this page`.
Wenn also das Passwort nicht auf dieser Seite zu finden ist, gibt es vielleicht eine weitere Seite oder eine Datei. 

Inspiziere den HTML-Code. Es gibt eine interessante Sache.

![Natas3 Source Code inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3.png)

```html
<img src="files/pixel.png">
```

Interessant ist vor allem eine Sache: `files/". Das lässt daraufschließen, dass es einen Verzeichnis gibt, in dem dieses Bild zu finden ist. Vielleicht auch mehr?

Um das herauszufinden, gib folgendes in die URL-Zeile deines Browsers ein:

```http
http://natas2.natas.labs.overthewire.org/files
```

![Natas3 neue URL untersuchen](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3b.png)

Sieh an! Neben dem Bild, welches sich im Source Code befindet, gibt es eine Datei mit der Bezeichung `Users.txt`.
Klick auf die Datei, um das Passwort zu erhalten.

![Natas3 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas3c.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 3 -> 4

Username:   natas3

URL:        http://natas3.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Nach dem du dich eingeloggt hast, sieht du das gleiche wie das Level zuvor:
`There is nothing on this page`

Du kannst den Source-Code analyisieren, doch das ist nur Zeitverschwendung.

Es gibt eine Alternative, die es uns ermöglicht, die Konfigurationen einer Webseite bzw. eines Webservers anzusehen.

Bei dieser `robots.txt`-Datei handelt es sich um eine Textdatei, die Webseitenbetreiber konfigurieren, wenn sie nicht wollen, dass ihre Webseite(n) indexiert werden sollen. Webcrawler erhalten von dieser Datei ihre Anweisungen, damit sie wissen, wie sie mit den Inhalten umzugehen haben.

Es kann zum Beispiel dafür genutzt werden, dass sensible Bereiche einer Webseite wie es der `admin`-Bereich ist, nicht auf Google oder sonstigen Suchmaschinen anzeigen zu lassen.

Gib im Browser die folgende URL ein:
```http
http://natas3.natas.labs.overthewire.org/robots.txt
```

![Natas4 robots.txt Information](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4.png)

In der `robots.txt` ist also zu sehen, dass der Webseitenbetreiber nicht möchte, dass das Verzeichnis `/s3cr3t/` von einem Webcrawler indexiert wird.

Verändere nun deine URL so, dass du zu diesem Verzeichnis navigierst.

```http
http://natas3.natas.labs.overthewire.org/s3cr3t/
```
![Natas4 /file Zugang](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4b.png)

Schon wieder eine `user.txt`-Datei. Klick sie an und erhalte das Passwort für das nächste Level.

![Natas4 Password](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas4c.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 4 -> 5

Username:   natas4

URL:        http://natas4.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Gib im Browser deiner Wahl die URL für dieses Level ein und bestätige mit der `Enter`-Taste.
Melde dich mit `natas4` und dem Passwort aus dem zuvorherigen Level an.

Wenn du eingeloggt bist, siehst du eine Nachricht, die so viel sagt wie, dass du nicht berechtigt bist, über den Host auf die Seite zu kommen.

Das einfachste, was du versuchen kannst, ist es, den Request-Header so zu modifizieren, dass du einer `GET`-Methode einen weiteren Parameter hinzufügst und probierst, ob es funktioniert. 

1. Analysiere die Header der Webseite

Inspiziere die Webseite und klicke auch den Reiter `Network`. Es wird vermutlich so sein, dass du auf `Reload` klicken musst, damit die Aufzeichnung neu geladen wird.

Anschließend erhältst du die Webseiten, die mit dem Webserver kommunizieren. Klicke auf die Datei, die den Source-Code für die Webseite beinhaltet. Das sollte die erste Aufzeichnung sein.

Wenn du mehr über das Thema `headers` in HTML-Request erfahren willst, kannst du bei den [Mozilla Developer Dokumentation für Header](https://developer.mozilla.org/de/docs/Web/API/Headers) vorbeischauen.

![Natas5 Header inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas5.png)

Sobald du auf die Aufzeichnung klickst, tauchen rechts weitere Informationen dazu auf.
Es gibt zwei Möglichkeiten für dich nun vorzugehen.

Entweder du drückst im `Request Headers` auf den Button `Raw` und siehst die rohe Fassung. Das hat den Vorteil, dass du beispielsweise im Reiter `Console` die in `JavaScript` vorhandene WebAPI `fetch` verwenden kannst, die die Daten einer Webseite `fetchen` also anfordern kann.

Falls du den Header über die `fetch`-API nutzen willst, kopierst du die `Raw`-Fassung des Request-Headers, klickst du auf den Reiter `Console`, und gibst folgenden Befehl ein:

```JavaScript
fetch("http://natas4.natas.labs.overthewire.org", {
    method: "GET" // optional, da bei fetch() GET standard
    headers: 
    {
        'Host': 'natas4.natas.labs.overthewire.org',
        'Authorization': 'Basic deinKey',
        'Referer': 'http://natas5.natas.labs.overthewire.org'
    }
}).then(response => response.text()).then(console.log(response))
```

**Erklärung:**
- `fetch()`: moderner als `XMLHttpRequest`, sendet und verarbeitet HTTP Anfragen
- `method`: `GET` als Methode (bei `fetch()` im Standard) für HTTP-Anfrage
- `headers`:
- `.then(response => response.text())`: Dann die Ausgabe in eine Text-Datei speichern und
- `.then(console.log(response))`: die Datei in der Console ausgeben.

Mehr zur `WebAPI - fetch()` erhältst du hier: [Mozilla Developer - Verwendung der fetch API](https://developer.mozilla.org/de/docs/Web/API/Fetch_API/Using_Fetch).


### **Oder**

Wenn die Methode oben nicht funktioniert, dann liegt das vermutlich daran, dass der Browser beim Senden die Daten beim Senden überschreibt und sie somit wieder wahrheitsgemäß an den Server übermittelt.

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


</details>





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Natas 5 -> 6

Username:   natas5

URL:        http://natas5.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Beim Einloggen auf die Webseite erhältst du die Nachricht `Access disallowed. You are not logged in`.
Das kann darauf schließen, dass in unserem Header ein Cookie gesetzt sein könnte. Das kannst du folgednermaßen überprüfen.

Begib dich direkt in den `DevTools` deines Browser zum Reiter `Network`. Lade, sofern nötig die `Network`-Daten neu mit `reload` (Firefox).

Beim Analysieren des Headers der Challenge-Webseite sollte dir ein `Response`-Parameter im Header besonders ins Auge fallen: `Set-Cookie: loggedin=0`.

![Natas6 Header analysieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas6.png)

Mit dieser Information kannst du versuchen, den Header so zu manipulieren, dass du dem Server surgerierst, eingeloggt zu sein.

Öffne dein Terminal und gib folgenden Befehl ein:
```bash
curl -u natas5:deinNatasPassword -H "Cookie: loggedin=1" http://natas5.natas.labs.overthewire.org
```

Und siehe da, du hast das Passwort im Terminal erhatlten.

![Natas6 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas6b.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 6 -> 7

Username:   natas6

URL:        http://natas6.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

<details>
    <summary>Lösung</summary>

Gleich zu Beginn der Challenge wirst du aufgefordert, etwas einzugeben.
Du hast nur den Begriff `Input secret` stehen und ein Eingabefeld.

Klicke jedoch auf den Link `View sourcecode`.

Du kommst dann auf den Sourcecode der Seite, die die `POST`-Methode und die Daten aus dem Eingabefeld verarbeitet.

Schau dir den Code an. Fällt dir etwas auf?

![Natas7 Sourcecode analysieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7.png)

Im PHP-Code findest du die Zeile `include "includes/sercret.inc"` ganz zu Beginn des Codes.

Gib folgendes nun in die Browser URL ein:
```http
http://natas6.natas.labs.overthewire.org/includes/secret.inc
```

Falls du eine leere Seite angezeigt bekommst, bist du schon mal richtig. Inspiziere die Seite und du erhältst den richtigen Inhalt der `$secret`-Variable für das Eingabefeld auf der "Hauptseite".

Kopiere den Schlüssel ohne die Anführungsstriche. Du musst vermutlich einen Doppelklick auf den Wert der Variable machen. Kopiere den Inhalt und begib dich wieder auf die "Hauptseite", wo du diesen `Secret-Key` über `POST` an den Server übermittels.

![Natas7 Secret Key](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7b.png)

Nach dem du diesen versteckten Inhalt per `Submit Query` übermittelt hast, solltest du das Passwort für die nächste Challenge erhalten.

![Natas7 Secret Key](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas7c.png)

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

Diesmal gibt es ein kleines Navigationsmenü, wie du es bspw. von gewöhnlichen Webseiten kennst.
Wenn du beide Links klickst, dann wirst du nicht viel im Frontend zu sehen bekommen.

Inspiziere den Code auf der `Home`-Seite und schau dir an, was du finden kannst:

![Natas8 Code inspizieren](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas8.png)

Im Code findest du einen Hinweis darauf, wo das Passwort für `natas8` zu finden ist. Die Frage ist nur, was du mit dieser Information unternehmen kannst?

Wenn du auf einen der Links auf der Homepage (`home` / `about`) nicht geklickt hast, dann solltest du die Seite einmal über einen dieser Links laden und beobachten, wie die URL sich verändert.

Deine URL sollte ungefähr so aussehen `....org/index.php?page=home`. Da du nicht nur die Home, sondern auch die About-Seite habst, steht in der URL nun ein `?page=`. 

Es sagt also folgendes im URL Browser:

> "Ich habe eine Variable nach `index.php` gestellt, aber ich weiß nicht, welcher Wert als nächstes kommt. Dazu warte ich einfach ab, was mein User mir mitteilt und übergebe ihn die anschließend. Warte du bis dahin."

Um es einfach auszudrücken :).

Versuche nun mit dem Tipp der Homepage, ob du an das Passwort kommst.

Gib im Browser folgende URL ein und du solltest das Passwort erhalten:

```http
http://natas7.natas.labs.overthewire.org/index.php?etc/natas_pass/natas8
```

![Natas8 Passwort](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas8b.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 8 -> 9

Username:   natas8

URL:        http://natas8.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

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


Falls du keinen Pfad angegeben hast, schau in Linux in deinem `home`-Verzeichnis nach. Da bewegst du dich als User standardmäßig, wenn du das Terminal öffnest und eine Shell-Session startest.

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

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 9 -> 10

Username:   natas9

URL:        http://natas9.natas.labs.overthewire.org

**Aufgabe:** Finde das Passwort.

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

Somit bekommt `$_REQUEST['needle']` der Wert `test`.

Falls du dich fragst, was `needle` bedeutet: Das ist nur der Name des `input`-Feldes, der über `php` im Backend ausgelesen wird.

In der **zweiten** `if`-Kondition kannst du sehen, dass der Code nicht "sanitisiert" wird. 

```php
passthru("grep -i $key dictionary.txt");
```

Das bedeutet, dass der Code ohne Maskierung oder Prüfung an ein Shell-Kommando übergeben werden kann. Ein Angreifer kann also beliebige Shell-Befehle ausführen.

Wie kannst du das für dich nutzen?

Du musst diesen Code "escapen" indem du ein `;` eingibst und anschließend deine Terminal-Befehle.

Stell dir mit dem `passthru()`-Code so vor:
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

Nun weißt du, dass durch die Fehlverarbeitung des Codes (ohne Prüfung auf Command Injections), Befehle eingegeben werden können, die auf dem Server verarbeitet werden.

Drücke `Strg` + `F` (Windows) bzw `Cmd` + `F` (Mac/Linux) und suche nach `natas`. Das sollte dir die vorhandenen Verzeichnisse auflisten, die in deren Bezeichnung "natas" vorkommt.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10d.png)

Du kannst mit `; ls /etc/XY` dich durch die Verzeichnisse probieren, aber der richtige Befehl ist:

```bash
; ls /etc/natas_webpass/natas10
```

Damit solltest du das Passwort für die nächste Challenge erhalten.

![Natas10 Hauptseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas10e.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 10 -> 11

Username:   natas10

URL:        http://natas10.natas.labs.overthewire.org


**Aufgabe:** Finde das Passwort.

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

`grep` gibt Inhalte aus, die mit dem Suchbegriff übereinstimmen. `-i` ignoriert dabei  Groß- und Kleinschreibung in den Patterns. Anschließend wird der Suchbegriff übergeben und mit der Datei `dictionary.txt` verglichen.


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

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Natas 11 -> 12

Username:   natas11

URL:        http://natas11.natas.labs.overthewire.org


**Aufgabe:** Finde das Passwort.


<details>
    <summary>Lösung</summary>

Mit dem Zugang zu `natas11` bekommst du eine komplett neue Challenge. Du erhältst bereits im Vorfeld einen Hinweis darauf, wie die Cookies geschützt sind, nämlich mit eine `XOR`-Verschlüsselung.

![Natas12 Startseite](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12.png)

Ich gehe nicht all zu sehr darauf ein, wie die XOR-Verschlüsselung funktioniert. Es handelt sich hierbei jedoch um eine sehr einfache kryptografische Operation, die auf der binärer Exklusiv-Oder Logik basiert.

Schau auf den Sourcecode, um näheres darüber zu erfahren, was im Backend mit den Daten geschieht.



![Natas12 Sourcecode Analyse](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12b.png)



Wenn du dich durch den Sourcecode liest, dann erkennst du 3 Funktionen.

1. `xor_ecrypt($in)` - die für die XOR-Verschlüsselung zuständig ist.
2. `loadData($def)` - die für das laden des Default-Werts ist.
3. `saveData($d)` - die für das Speichern der Daten ist.

Die `xor_encrypt`-Funktion scheint interessant zu sein, da du darüber die Entschlüsselung vornehmen kannst.

Es ist jedoch wichtig, den exakten Schlüssel zu kennen, damit der Klartext korrekt ausgegeben werden kann. Leider wird er im Code zensiert (`<censored>`).

Der Sourcecode verrät jedoch auch, dass eine neue Variable `$data` mit dem Wert aus der Funktion `loadData($defaultdata)` initialisiert wird. Daraufhin wird geprüft, ob sich in diesem Array ein Parameter mit besonderem Wert `preg_match('/^#?:[a-f\d]{6})...`, was auf den Inhalt des Eingabfelds auf der Hauptseite von `natas11` hindeutet. 

Also verarbeitet die Funktion `loadData()` den gesamten Inhalt der Variable `$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");`. 

Die 3. Funktion `saveData()` übermittelt den eingebenen Wert aus dem Cookie und übergibt ihn an den Server. Also muust du versuchen den Cookie so zu modifizieren, dass der Server die Nachricht erhält, den Parameter auf `"showpassword":"yes"` zu setzen.

Wie der Inhalt formatiert und verschlüsselt wird, ist am einfachsten in der Funktion `safeData()` zu sehen, wo der gesamte String (hier `$defaultdata`) zuerst ins JSON gebracht wird (`json_encode()`), dann mit der Funktion `xor_encrypt()` XOR verschlüsselt und anschließend mit Base64 kodiert.

Geh in die Dev-Tools deines Browser in den Reiter Netzwerk und lade ggfs. die Seite über den Button `reload` neu.

![Natas12 DevTools Netzwerk Cookie Analyse](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12c.png)

Klicke auf die Datei mit dem `Initiator` `document` bzw. dem `Type` `html`. Das ist die Hauptseite.   
Schau dir den Cookie an. Neben den Google Analytics Parametern (`_ga_`), die für einen Payload unbedeutend sind, steht der Cookie Wert `data` mit einem, so wie es aussieht Base64 kodiertem Inhalt.

Mir kam dann die Idee, chatGPT anzuleiten ein Skript zu schreiben welches mir den potenziellen XOR-Schlüssel ausgibt. 

Du kennst zwar nicht den korrekten Schlüssel, doch in einer einfachen binären Logik kannst du den Klartext, den du über den Sourcecode erhältst, mit dem verschlüsselten Wert vergleichen und so vielleicht auf den richtigen Schlüssel kommen (du wirst in der realen Welt nicht so einfach an den Klartext kommen).

ChatGPT half mir daraufhin einen Code zu entwickeln, der den Klartext aus dem Sourcecode nimmt und mit dem Base64-Wert aus dem Cookie vergleicht.

**Wichtig:** Der Inhalt aus der Variable des Sourcecodes muss im standardmäßigen, kompakten JSON-Format sein. Achte also darauf, dass du keine unnötigen Leerzeichen verwendest, da diese die Byte-Anzahl verändern und die XOR-Entschlüsselung auf Serverseite fehlschlagen lassen.

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



Wenn du diese Skript ausführst, dann solltest du den XOR-Schlüssel erhalten haben.   

> Es kann sein, dass die Passwörter aktualisiert worden sind.

![Natas12 XOR-Schlüssel](/09-practice-labs/ressourcen/pictures/overthewire/natas/natas12d.png)

Nun hast du den Schlüssel und kannst damit versuchen den manipulierten JSON-String an den Server zu übermitteln.

Wichtig ist dabei, dass du die Reihenfolge zum Verschlüsseln richtig einhältst.   
Zuerst den String ins JSON-Format bringen, dann mit dem richtigen Schlüssel die XORen und zum Schluss den gesamten String Base64 verschlüsseln.

Dazu gehen du auf [cyberchef.io](https://cyberchef.io). Hier kannst du dein eigenes Verschlüsselungsrezept einstellen. Du benötigst nur 2 Methoden, da du den String so eingibst, dass er bereits im standardmäßigen JSON-Format ist, sodass du ihn direkt XOR verschlüsseln und Base64 kodieren kannst.

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

</details>

-----

## Natas 12 -> 13

Username:   natas12

URL:        http://natas12.natas.labs.overthewire.org

<details>
    <summary>Lösung</summary>

</details>

# Wird laufend fortgesetzt, bis das letzte Level geschafft ist.



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
