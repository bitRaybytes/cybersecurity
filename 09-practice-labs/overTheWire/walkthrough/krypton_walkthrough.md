# Over The Wire: Krypton

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Zugang](#zugang)
- [Allgemeine Infos](#allgemeine-infos)
- [Krypton 0 -> 1](#krypton-0---1)
- [Krypton 1 -> 2](#krypton-1---2)
- [Krypton 2 -> 3](#krypton-2---3)
- [Krypton 3 -> 4](#krypton-3---4)
- [Krypton 4 -> 5](#krypton-4---5)
- [Krypton 5 -> 6](#krypton-5---6)
- [Krypton 6 -> 7](#krypton-6---7)
- [Erkenntnisse](#erkenntnisse)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## Einführung
Im Gegensatz zu anderen Wargames (wie [Leviathan](/09-practice-labs/overTheWire/walkthrough/leviathan_walktrough.md) oder [Bandit](/09-practice-labs/overTheWire/walkthrough/bandit_walkthrough.md)) liegt der Fokus hier nicht auf System- oder Web-Exploits, sondern auf der **Entschlüsselung** von Informationen. Krypton ist ideal für Anfänger, die ihre ersten Schritte in der Kryptanalyse machen wollen. 

🔐 Dein Ziel ist es, dich durch mehrere Level zu arbeiten, indem du verschiedene **Verschlüsselungstechniken** knackst und Geheimnisse entschlüsselst.

Mehr zum Thema Verschlüsselung findest du [hier in der Repo](/06-crypto-stego/encoding_vs_encryption.md).



## Zugang
Der Einstieg erfolgt über eine **SSH-Verbindung



## Allgemeine Infos
- **Host:** krypton.labs.overthewire.org  
- **Port:** 2231  
- **Start-User:** krypton0
- **Start-Passwort:** KRYPTONISGREAT (kann veraltet sein!)
- **Ziel:** Knacken von Verschlüsselungstechniken
- **Betriebssystem:** Kali-Linux
- **Offizielle Seite:** [OverTheWire - Krypton](https://overthewire.org/wargames/krypton/)
- **Hinweis:** Passwörter können aktualisiert worden sein!



## Krypton 0 -> 1

<details>
    <summary>Lösung</summary>

### Einleitung

Auf der Webseite erhältst du schon die ersten Hinweise zur Verschlüsselungstechnik. Das Passwort ist der String aus der verschlüsselten Nachricht `S1JZUFRPTklTR1JFQVQ=`. Um die Nachricht zu entschlüsselt, benötigst du den richtigen Schlüssel.

### Lösung

Kopiere den String, der dir auf der Webseite [overthewire.org/wargames/krypton/krypton0.html](https://overthewire.org/wargames/krypton/krypton.html) angezeigt wird.

![Krypton1 base64 Passwort Verschlüsselung](/09-practice-labs/ressources/pictures/krypton1.png)

Öffne anschließend ein Terminalfenster und gib folgenden Befehl ein, um die base64 verschlüsselte Nachricht zu entschlüsseln:

```bash
echo S1JZUFRPTklTR1JFQVQ= | base64 -d
# Ersetze die verschlüsselte Nachricht mit der aktuellen auf der Webseite, falls nötig.
```

**Erklärung des Befehls:**

Der Befehl `base64` ist ein Werkzeug, das Daten im Base64-Format kodiert und dekodiert.

- `base64`: Der Befehl selbst.

- `-d`: Die Option `-d` steht für `--decode`. Sie weist das base64-Programm an, die eingegebenen Daten nicht zu kodieren, sondern zu dekodieren.

Der Befehl liest also den Base64-String `S1JZUFRPTklTR1JFQVQ=` von der Standardeingabe und wandelt ihn in seine ursprüngliche, lesbare Form zurück.

**Was ist Base64?**

Base64 ist eine Kodierungsmethode, die binäre Daten in eine Zeichenfolge umwandelt, die nur aus druckbaren ASCII-Zeichen besteht. Dies ist nützlich, um Daten sicher über Systeme zu übertragen, die nur Text verarbeiten können, wie z.B. in E-Mails oder URLs. Ein Base64-String besteht aus Groß- und Kleinbuchstaben (`A-Z`, `a-z`), Ziffern (`0-9`), dem Pluszeichen (`+`) und dem Schrägstrich (`/`). Das Gleichheitszeichen (`=`) wird am Ende als Padding-Zeichen verwendet, um sicherzustellen, dass die ursprünglichen Daten ein Vielfaches von 3 Bytes sind.

Im Beispiel `S1JZUFRPTklTR1JFQVQ=` wandelt der Befehl `base64 -d` diese ASCII-Zeichen zurück in ihre ursprüngliche binäre Repräsentation, die dann als normaler Text interpretiert wird.

Das Ergebnis der Dekodierung lautet: `KRYPTONISGREAT`.

![Krypton1 base64 Entschlüsselung](/09-practice-labs/ressources/pictures/krypton1b.png)

Wenn du möchtest, kannst du das Passwort auch speichern. 

Mit dem Passwort kannst du nun eine `SSH`-Verbindung mit dem User `krypton1` einzuloggen.
Gib im Terminal folgenden Befehl ein, um dich über `SSH` zu verbinden  

```bash
ssh -l krypton1 krypton.labs.overthewire.org -p 2231
```


</details>

## Krypton 1 -> 2

<details>
    <summary>Lösung</summary>

### Einleitung 

Auf der Seite zum zweiten Krypton-Level erhalten wir einen langen Hinweis-Text. Es steht drin, dass die Datei "`krypton2`" heißt und der Inhalt mit einer vereinfachten Rotation verschlüsselt wurde. Außerdem liegt der Inhalt in einem nicht-standardmäßigen Chiffriertextformat (Geheimtext) und wird normalerweise unabhängig von Wortgrenzen in 5er-Gruppen zusammengestellt. Das hilt, die Musterung des Schlüssels zu verschleiern.

Zuletzt steht, dass die Datei die Wortgrenzen des Klartextes beibehalten hat.
Das heißt im Klartext, dass die verschlüsselte Nachricht, die gleiche Anzahl an Buchstaben hat, wie die entschlüsselte Nachricht.

Klingt ganz nach der [`ROT13`-Verschlüsselung](https://de.wikipedia.org/wiki/ROT13), wo der Buchstabe `A` um 13 Stellen nach vorne verschoben wird und an der Position vom Buchstaben `N` liegt. Genauso mit `B -> O`, `C -> P`, `D -> Q` usw. Und dann das Ergebnis in Cluster gepackt.

Bei dieser Art der Verschlüsselung handelt es sich um eine symmetrische Verschlüsselung, die es ermöglicht, den selben Schlüssel für Ver- und Entschlüsselung zu benutzen.

Das Ziel ist nun, die Datei in unserem Verzeichnis zu finden, sie auszulesen, und den Inhalt mit einer rotierenden Verschlüsselung zu entschlüsseln und zu hoffen, dass wir richtig liegen.

### Lösung

Gib im Terminal folgende Befehle ein, um an das Passwort zu kommen:

```bash
ll      # listet Dateien und Verzeichnisse auf
find / -type f -name "krypton2" 2>/dev/null     # Datei in den Verzeichnissen finden und Fehlermeldung unterdrücken
cat /krypton/krypton1/krypton2                  # Datei-Inhalt ausgeben

echo "Dein ROT13 Code" | tr 'N-ZA-M' '[A-Z]'    # Datei entschlüsseln mit ROT13
```

**Erklärung der Befehle:**

1. `find / -type f -name "krypton2" 2>/dev/null`
    - `find / -type f -name "krypton"`: Programm, um Dateien und Verzeichnisse zu finden.
        - `/`: sucht in den gesamten Verzeichnissen nach der gesuchten Datei.
        - `-type f`: initiiert den Switch-Befehl des Typs `f` (File).
        - `-name "FileName"`: Datei, die gesucht werden soll.
        - `2>/dev/null`: **Standardbefehl**, der genutzt wird, um die Fehlerausgabe umzuleiten und zu "löschen".
            - `2>`: Umleitungsoperator mit Dateideskriptor `2`. Es gibt 3 Arten von Datenströmen:
                - `0`: `stdin` (Standardeingabe) - von hier liest ein Befehl Daten.
                - `1`: `stdout` (Standardausgabe) - hierin schreibt ein Befehl seine normalen Ausgaben.
                - `2`: `stderr` (Standard-Fehlerausgabe) - hier schreib ein Befehl Fehlermeldungen oder Diagnoseinformationen.
            - `/dev/null`: spezielle Gerätedatei in Unix-ähnlichen Systemen. Alles, was in diese Datei geschrieben wird, wird sofort verworfen (akzeptiert Daten, keine Speicherung).

2. `cat /krypton/krypton1/krypton2`
    - `cat <Pfad/evtl/Unterpfad/Dateiname>`: Programm, welches wie ein Pager den Inhalt der Dateien ausgibt. Funktioniert wie ein Pager.

3. `echo "Dein ROT13 Code" | tr 'N-ZA-M' '[A-Z]'`
    - `echo "..."`: Einfache Standardausgabe des Strings
    - `|`: (Pipe) Unix-Befehl, das Standardausgaben eines BEfehls mit der Standardeingabe eines anderen BEfehles verbindet. `echo` gibt den Text aus und wird als Eingabe für den nächsten Befehl, hier `tr` genutzt.
    - `tr`: (translate/transliterate) Ersetzt bestimmte Zeichen, indem es zwei Zeichensätze (Quell- und Zielzeichensatz) ersetzt. Dabei wird jede Positon des Quellzeichensatz (bsp: "ABCD") durch das entsprechende Zeichen an derselben Positon im Zielzeichensatz ersetzt.
        - `'N-ZA-M'`: ist der **Quellzeichensatz**
            - `N-Z`: umfasst alle Großbuchstaben von N bis Z.
            - `A-M`: umfasst alles Großbuchstaben von A bis M.
                - zusammen bilden sie das lateinische Alphabet, jedoch um 13. Stellen verschoben.
        - `[A-Z]`: ist der **Zielzeichensatz** und umfasst alle Großbuchstaben von A bis Z in normaler Reihenfolge.

`tr` such also nach jedem Zeichen, das in der verschlüsselten Nachricht vorkommt. Wenn es dann bspw. ein `N` findet, erstezt es das Zeichen mit einem `A` und so weiter.

> **weitere Möglichkeit:** Du könntest auch mit cyberchef.io das Passwort knacken.

![krypton2 Password](/09-practice-labs/ressources/pictures/krypton2c.png)

</details>

## Krypton 2 -> 3

<details>
    <summary>Lösung</summary>

### Einleitung

In diesem Level musst du die Datei `krypton3` finden und sie entschlüsseln. Neben der Datei gibt es im selben Verzeichnis das Programm `ecrypt`, eine `README`, die lohnenswert ist, sie zu lesen und eine `keyfile.dat`.

Letztere Datei (`.dat`, steht für Data/Daten) ist ein allgemeiner Container für Daten, der von verschiedenen Programmen erstellt wird und Text, Bilder, Audio oder Videos enthalten kann.

Da DAT-Dateien nicht standardisiert sind und von vielen verschiedenen Anwendungen erstellt werden, ist das Öffnen und Interpretieren schwierig. Um eine DAT-Datei zu öffnen, musst du wissen, welches Programm sie ursprünglich erstellt hat, und dieses Programm verwenden.

In der `README` wirst du im letzten Abschnitt noch einmal darauf hingewiesen, wie du einen Ordner im `/tmp/`-Verzeichnis erstellst und einen Link erstellst.

Das wird dir dabei helfen, an das Passwort für das nächste Level zu kommen.

### Lösung

Gib im Terminal folgenden Befehl ein, um die Datei zu finden, das Verzeichnis aufzulisten und einen Ordner im `/tmp`-Verzeichnis zu erstellen:

```bash
find / -type f -name "krypton3" 2>/dev/null     # findet Datei anhand des Typs f (file) und dem Namen
                                                # 2>/dev/null leitet Fehlermeldungen um
ll /krypton/krypton2/
mkdir /tmp/DeinOrdnerName        
```

![Krypton3 Datei finden](/09-practice-labs/ressources/pictures/krypton3.png)

Mit dem Befehl `find / -type f -name "krypton3"` wird nach der Datei in allen Verzeichnissen des System gesucht. `/` steht hier für das System-Verzeichnis, quasi der aller erste Ordner.
Der Switch `-type f` definiert dabei den Typ als `f`, also einer File/Datei. `-name` nutzt du, wenn dir der Name der Datei bekannt ist. Du kannst selbstverständlich auch nach bestimmten Dateigrößen suchen. Dazu musst du dann die Switches anpassen.

Jetzt müssen wir die Datei keyfile.dat verlinken, da wir sie höchstwahrscheinlich nicht im Verzeichnis, wo sie liegt, mit dem Programm `encrypt` entschlüsseln können.

Wechsle in den eben erstellten Ordner im Verzeichnis `/tmp/` und verlinke die Datei `keyfile.dat`

Gib im Terminal folgende Befehle ein:

```bash
cd /tmp/DeinOrdnerName                # wechselt im /tmp/-Verzeichnis in den angegeben Pfad.
ln -s /krypton/krypton2/keyfile.dat   # verlinkt die keyfile.dat in /tmp/DeinOrdnerName (kein Zielpfad nötig, da du bereits im Ordner bist).
ll                                    # ll oder ls optional, wenn du sehen willst, ob die Datei verlinkt wurde, da keine Rückmeldung kommt.
/krypton/krypton2/encrypt keyfile.dat # nutzt das Program encrypt um die keyfile.dat zu "entschlüsseln"
ls                                    # listet Inhalt des Verzeichnisses auf
cat ciphertext                        # liest den Ciphertext für das Entschlüsseln aus
```

![Krypton3 krypton3 entschlüsseln](/09-practice-labs/ressources/pictures/krypton3b.png)

Du bist nun in deinem Ordner im `/tmp/`-Verzeichnis. Mit `ln -s /krypton/krypton2/keyfile.dat` hast du einen Symbol-Link darin erstellst. Normalerweise kannst du das auch machen, wenn du in anderen Verzeichnissen bist. Dazu musst du dann noch das Zielverzeichnis angeben, in das die Datei verlinkt werden soll.

Nach dem die Datei `keyfile.dat` mit dem Programm `encrypt` entschlüsselt wurde, erhältst du eine weitere Datei ins `/tmp/`-Verzeichnis, die dir die Entschlüsselung für die Datei `krypton3` gibt.

Die Ausgabe über `cat ciphertext` zeigt den eigentlichen Entschlüsselungscode `YZABCD...`. Das heißt, dass du mit dem Programm `tr` den Inhalt aus `krypton3` genau nach dem Format im `ciphertext` entschüsseln musst.

![Krypton3 krypton3 entschlüsseln](/09-practice-labs/ressources/pictures/krypton3c.png)

Gib im Terminal folgende Befehle ein, um die Datei `krypton3` auszugeben und die Entschlüsselung nach dem Format durchzuführen:

```bash
cat /krypton/krypton2/krypton3          # OMQEMDUEQMEK ist aktuell der Inhalt von krypton3
echo OMQEMDUEQMEK | tr 'Y-ZA-X' '[A-Z]' # entschlüsselt den String aus echo.
```

![Krypton3 krypton3 entschlüsseln](/09-practice-labs/ressources/pictures/krypton3d.png)


Der Befehl `echo "..." | tr  'Y-ZA-X' '[A-Z]'` gibt den Befehl zunächst aus und entschlüsselt ihn mit `tr 'Y-ZA-X' '[A-Z]'` nach dem Pattern der Datei aus `ciphertext`.

Du erhältst einen weiteren verschlüsselten Text. Der hat es allerdings in sich. Bisher haben wir die Inhalte nur mit Großbuchstaben entschlüsselt. Um jedoch diesen neuen verschlüsselten Inhalt `PNRFNEVFRNFL` zu entschlüsseln, benötigst du die Kleinbuchstaben `a-z` dazu.

Gib im Terminal zum Abschluss folgenden Befehl ein, um den neuen Inhalt zu entschlüsseln und das Passwort für `krypton3` zu erhalten:

```bash
echo PNRFNEVFRNFL | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

![Krypton3 krypton3 Password](/09-practice-labs/ressources/pictures/krypton3e.png)

Herzlichen Glückwunsch! Du hast das Passwort erfolgreich entschlüsselt und kannst mit `exit` die `SSH`-Verbindung beenden und mit dem nächsten Level fortfahren.

</details>

## Krypton 3 -> 4

<details>
    <summary>Lösung</summary>

### Einleitung

Du hast nun die größte Schwäche von **Substitutionschiffren** kennengelernt: die wiederholte Nutzung desselben Schlüssels. 
In diesem Level hast du keine Möglichkeit, das Verschlüsselungssystem zu manipulieren.

Stattdessen musst du mehrere abgefangene Nachrichten (`found1`, `found2`, `found3`) entschlüsseln, die alle mit demselben Schlüssel verschlüsselt wurden.

Die entschlüsselten Nachrichten sind in amerikanischem Englisch verfasst – ein wichtiger Hinweis, der dir bei der Entschlüsselung helfen kann. Die Lösung für das nächste Level findest du in der Datei `krypton4`.

Im englischen Alfabet ist `E` der am häufigsten Vorkommende Buchstabe.

Ich werde dir diesmal eine sehr einfache und schnelle Variante zeigen, wie du mit Tools aus dem Internet, die verschlüsselte Nachricht entschlüsselt.

Die Vorgehensweise ist wie im letzten Level. Du suchst mit `find` nach einer bestimmten Datei und gibst den Inhalt mit `cat` aus.  

### Lösung

Gib im Terminal folgende Befehle ein:
```bash
find / -type f -name "krypton4" 2>/dev/null # Finde die Datei und unterdrücke Fehlermeldung
ls /krypton/krypton3/                       # listet das angegebene Verzeichnis auf
```

![Krypton4 Datei suchen](/09-practice-labs/ressources/pictures/krypton4.png)

Du weißt bereits, dass die Dateien alle den gleichen Schlüssel haben, mit dem sie verschlüsselt wurden. Das heißt, dass du den gleichen Schlüssel nehmen musst, um die Dateien zu entschlüsseln.
Das ist wichtig zu wissen. Denn ohne den richtigen Schlüssel, erhältst du nicht die korrekte entschlüsselte Nachricht.

Viel wichtiger ist zu wissen, dass du keinerlei wissen darüber hast, wie der Schlüssel aufgebaut ist. Das heißt, dass du den Schlüssel `bruteforcen` musst. 
Du kannst entweder ein `python`-Script schreiben, dass dir hilft, den Schlüssel zu `bruteforcen` oder du bedienst die den mannigfaltigen Online-Tools.

Ich nutze `QuipQuip`, das ein automatisierter Kryptogrammlöser ist und einfache `Substitution-Cipher` lösen kann.

Beachte, dass es hier Sinn ergibt, den `cat`-Befehl auch auf die Datei `krypton4` anzuwenden. Damit kannst du ihren Inhalt auch auf der Webseite von `QuipQuip` einfügen. Das machst du am Besten, wenn du zuerst einen der verschlüsselten Texte in das Inputfeld einfügst und am Ende mit einem Leerzeichen getrennt, den Inhalt aus der Datei `krypton4`.

Hier gehts zum Online-Tool `QuipQuip` von Edwin Olson: [https://quipqiup.com/](https://quipqiup.com/).

Gib folgende Befehle ein:

```bash
cat /krypton/krypton3/found1    # Ausgabe der Datei found1
## Kopiere den Inhalt aus der Nachricht und füge ihn auf der Internetseite QuipQuip ein.

cat /krypton/krypton3/krypton4  # Ausgabe der Datei krypton4
## Kopiere den Inhalt aus der Nachricht und füge ihn auf der Internetseite QuipQuip ein.
```
Öffne jetzt die Internetseite [https://quipqiup.com/](https://quipqiup.com/) und füge nacheinander die kopierten Inhalte aus den beiden Dateien ein.

Kopiere den ausgegebenen Inhalt aus der Datei `found1` und füge ihn im Online-Tool ein:

![Krypton4 found1 ausgeben und kopieren](/09-practice-labs/ressources/pictures/krypton4b.png)

Kopiere den ausgegebenen Inhalt der Datei `krypton4` und füge ihn im Online-Tool ein:

![Krypton4 krypton4 ausgeben und kopieren](/09-practice-labs/ressources/pictures/krypton4b2.png)

Nach dem Einfügen der beiden Inhalte solltest du auf QuipQuip folgenden Ansicht erhalten:

![Krypton4 Online-Tool QuipQuip](/09-practice-labs/ressources/pictures/krypton4c.png)

Drücke jetzt auf den Button `Solve` und lass das Tool die Nachrichten entschlüsseln. Sobald es fertig mit dem `bruteforcen` ist, wirst du die Nachricht entschlüsselt vorfinden und kannst am Ende des Textes das Passwort zum nächsten Level herauslesen.

![Krypton4 Passwort](/09-practice-labs/ressources/pictures/krypton4d.png)

Das aktuelle Passwort zum nächsten Level ist: `brute`.

Nutze es, um dich mit `krypton4` in das nächste Level einzuloggen.

</details>

## Krypton 4 -> 5

<details>
    <summary>Lösung</summary>



</details>

## Krypton 5 -> 6

<details>
    <summary>Lösung</summary>

</details>

## Krypton 6 -> 7

<details>
    <summary>Lösung</summary>

</details>



## Erkenntnisse
- `echo:` Gibt Text direkt im Terminal aus.
- `| (Pipe):` Leitet die Ausgabe eines Befehls an die Eingabe eines anderen weiter.
- `base64 -d:` Dekodiert Daten, die im Base64-Format vorliegen.
- `tr:` Übersetzt oder ersetzt Zeichen und ist damit ideal für Substitutionschiffren.
- `find:` Sucht nach Dateien im Dateisystem. Oft wird es mit `2>/dev/null` kombiniert, um Fehlermeldungen zu unterdrücken.
- `cat:` Zeigt den Inhalt von Dateien an.
- `ln -s:` Erstellt symbolische Links (Symlinks), also Verknüpfungen zu anderen Dateien.

## Nützliche Links

- [Wikipedia: ROT13-Verschlüsselung](https://de.wikipedia.org/wiki/ROT13)
- [Linux Manual: `tr`](https://man7.org/linux/man-pages/man1/tr.1.html) 
- [https://quipqiup.com/](https://quipqiup.com/)


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

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

