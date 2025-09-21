# Over The Wire: Bandit

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Tipps zu Beginn](#tipps-zu-beginn)
- [Bandit 0 ➝ Zugang zur Challenge](#bandit-0-zugang-zur-challenge)
- [Bandit 0 ➝ 1](#bandit-0---1)
- [Bandit 1 ➝ 2](#bandit-1---2)
- [Bandit 2 ➝ 3](#bandit-2---3)
- [Bandit 3 ➝ 4](#bandit-3---4)
- [Bandit 4 ➝ 5](#bandit-4---5)
- [Bandit 5 ➝ 6](#bandit-5---6)
- [Bandit 6 ➝ 7](#bandit-6---7)
- [Bandit 7 ➝ 8](#bandit-7---8)
- [Bandit 8 ➝ 9](#bandit-8---9)
- [Bandit 9 ➝ 10](#bandit-9---10)
- [Bandit 10 ➝ 11](#bandit-10---11)
- [Bandit 11 ➝ 12](#bandit-11---12)
- [Bandit 12 ➝ 13](#bandit-12---13)
- [Bandit 13 ➝ 14](#bandit-13---14)
- [Bandit 14 ➝ 15](#bandit-14---15)
- [Bandit 15 ➝ 16](#bandit-15---16)
- [Bandit 16 ➝ 17](#bandit-16---17)
- [Bandit 17 ➝ 18](#bandit-17---18)
- [Bandit 18 ➝ 19](#bandit-18---19)
- [Bandit 19 ➝ 20](#bandit-19---20)
- [Bandit 20 ➝ 21](#bandit-20---21)
- [Bandit 21 ➝ 22](#bandit-21---22)
- [Bandit 22 ➝ 23](#bandit-22---23)
- [Bandit 23 ➝ 24](#bandit-23---24)
- [Bandit 24 ➝ 25](#bandit-24---25)
- [Bandit 25 ➝ 26](#bandit-25---26)
- [Bandit 26 ➝ 27](#bandit-26---27)
- [Bandit 27 ➝ 28](#bandit-27---28)
- [Bandit 28 ➝ 29](#bandit-28---29)
- [Bandit 29 ➝ 30](#bandit-29---30)
- [Bandit 30 ➝ 31](#bandit-30---31)
- [Bandit 31 ➝ 32](#bandit-31---32)
- [Bandit 32 ➝ 33](#bandit-32---33)
- [Bandit 33 ➝ 34](#bandit-33---34)
- [Haftungsausschluss](#haftungsausschluss)

## Einleitung

Das OverTheWire: Bandit Wargame ist ein großartiger Einstieg in die Welt der Linux-Sicherheit und Command Line Tools. Die Level bauen aufeinander auf und bringen dir grundlegende Techniken zur Informationsbeschaffung, Dateiverwaltung, Textmanipulation und Zugriffskontrolle bei.

Es müssen schrittweise und in Reihenfolge Herausforderungen absolviert werden. 
Die Levels sind hierbei über eine `SSH`-Verbindung erreichbar.

Um in das nächste Level zu kommen, müssen bestimmte CTFs (Capture the flags) erreicht werden. In Bandit sind das meistens Passwörter, die wir aus Dateien extrahieren sollen.

Mehr zu [Over the Wire und dem Level Bandit](https://www.overthewire.org/wargames/bandit) erhältst du hier.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Tipps zu Beginn:

### Ordner anlegen und Passwörter organisieren
Optional kannst du deine Bandit-Passwörter organisieren, indem du sie in einem Ordner speicherst, der für jedes einzelne Level eine Datei hat, oder du erstellst eine einzige Datei dafür. Es ist dir überlassen.

Falls du bspw. einen Ordner erstellen willst:

Öffne dein Terminal und erstelle einen Odner in einem Verzeichnis, Beispiel: `Documents`. Gib dazu in deinem Terminal folgenden Befehl ein:

1. Ordner `bandits` erstellen:
```bash
mkdir ~/Documents/bandits
```

2. Datei mit Passwort in den Ordner `bandits` hinzufügen
```bash
echo "lvl0 passwort: {Passwort einfügen}" > ~/Documents/bandits/0
```
Diesen Befehl musst du dann nach einem Exit eingeben, und das Password aus dem Level zuvor mit dem Platzhalter `{Passwort einfügen}` ersetzen.

- `{Passwort einfügen}` Hier fügst du das Passwort aus dem Level ein.
- `>` legt die Ausgabe des `echo` in einer Datei ab.
- `0` (Null) ist die Zahl, die dem Level zugehört.
    - Für `bandit0` ist es `> 0`,
    - für `bandit1` ist es `> 1` usw.


Du kannst zum Speichern von Text-Dateien auch einen Editor wie `nano` nutzen. Ich habe mich für den obigen Weg entschieden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Handbücher der Befehle und Programme in Shell

Für mehr Informationen zu einem Befehl oder einem Programm kannst du den Befehl `man` verwenden.

Beispiel:
Du willst wissen, was das Programm `cat` machen kann? Gib dazu folgeden Befehl in das Terminal:
```bash
man cat
```

Für jedes weitere Programm ersetzt du das Wort `cat` mit der entsprechenden Bezeichnung des Programms.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 0 Zugang zur Challenge:
**Aufgabe:** Logge dich mit `bandit0` ein und beginne die Challenges
<details>
  <summary>Bandit 0 Zugang zur Challenge</summary>
  

Gib in deinem Terminal (ich nutze Kali Linux VM) folgenden Befehl ein:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

**oder**

```bash
ssh -l bandit0 bandit.labs.overthewire.org -p 2220
```

- `-l` steht für User, indem Fall `bandit0`.
- `bandit.labs.overthewire.org` ist der Host, zu dem wir Kontakt aufnehmen wollen.
- `-p` steht für den Port `-p 2220` ist also der Port `2220`.

> Passwort für bandit0: **bandit0**

Das Passwort wird in der Shell beim Eingeben nicht angezeigt. Solltest du jedoch ein falsches Passwort eingeben, erhältst du eine dementsprechende Rückmeldung.

Wir verbinden uns grundsätzlich aus dem Kali Terminal stets über eine sichere `ssh`-Verbindung. Es gibt Ausnahmen ;-).

**Hinweis:**
In den meisten Bandit-Level ist es nötig, mit dem Befehl `exit` die SSH-Verbindung zu trennen, damit du dich mit dem nächsten "User" anmelden kannst. Die User sind dabei die Level, die du als Userkennung nutzt, also `bandit0` für den Beginn und das erste Passwort zu `bandit1`. `bandit1` für das Level, um das Password für `bandit2` zu erhalten und so weiter.


![Bandit Challenge starten](/09-practice-labs/ressources/pictures/otw-0.png)

  
</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 0 -> 1

**Aufgabe:** Finde das Passwort in der Datei `readme`.
<details>
    <summary>Lösung</summary>

Mit dem Zugang des `bandit0`-Useraccounts haben wir nun Zugriff auf die Challenges.
Nun ist es an der Zeit, unsere Linux-Skills auf das nächste Level zu befördern.


In deinem Terminal gibst du nachfolgend diese Befehle ein:

```bash
ls
cat readme
```

Speicher das Passwort in einer Datei außerhalb dieser SSH-Verbindung. Dies machst du mit folgendem Befehl:

```bash
echo "lvl1 passwort: {Passwort hier rein}" > ~/Documents/bandits/1
```


**Erklärung:**

- `ls` zeigt die Dateien im aktuellen Verzeichnis (`home/bandit1`) aufgelistet an
    -  Dadurch wird die `readme`-Datei sichtbar. 
- `cat` steht für concatenate (zusammenführen), wird als Pager zum Auslesen von Dateien genutzt.

![Login bandit1](/09-practice-labs/ressources/pictures/otw-1.png)

> Verlasse die SSH-Verbindung mit dem Befehl: `exit` und speichere das Passwort.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 1 -> 2

**Aufgabe:** Das Password findest du in der Datei mit der Bezeichnung: `-` im Home-Verzeichnnis.
<details>
    <summary>Lösung</summary>

```bash
ls
cat ./-
```

**Erklärung:** 

`./` verweist auf das aktuelle Verzeichnis. Ohne dies würde `cat -` den Standard-Input erwarten.

![bandit2 Passwort](/09-practice-labs/ressources/pictures/otw-2.png)

Kopiere das Passwort und speichere es wieder ab. Ändere die Bezeichnung des Lvls und die Zahl der zu speichernden Datei, wenn du nicht willst, dass die Datei sich überschreibt.

Logge dich anschließend wieder aus (`exit`).

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 2 -> 3

**Aufgabe:** Datei heißt `--spaces in this filename--`
<details>
    <summary>Lösung</summary>


```bash
ls -la
cat "--spaces in this filename--"
```

Oder:

```bash
cat spaces\ in\ this\ filename
```

Oder:

```bash
cat ./--spaces\ in\ this\ filename
```
**Erklärung:** 
Dateinamen mit Leerzeichen müssen in Anführungszeichen oder mit `\` escaped werden.

![bandit3 Passwort](/09-practice-labs/ressources/pictures/otw-3.png)

Speichere das Passwort und beende die Shell.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 3 -> 4

**Aufgabe:** Versteckte Datei im Verzeichnis `inhere` finden.
<details>
    <summary>Lösung</summary>


Logge dich ein mit dem Benutzer `bandit2` und gib das Passwort ein, welches du aus `bandit1` erhalten hast.

Wenn du eingeloggt bist, gib folgende Befehle ein, um das Passwort für `bandit3` zu finden:

```bash
ls -la
cd inhere/
ll
cat ./... Hiding-From-You
```

**Erklärung:** 
- `ls -la` und `ll` zeigen auch versteckte Dateien und Ordner (beginnen mit `.`).
- `./` gibt an, dass es sich um das aktuelle Verzeichnis handelt.

![bandit4 Passwort](/09-practice-labs/ressources/pictures/otw-4.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 4 -> 5

**Aufgabe:** Finde die Datei mit `human-readable` Inhalt im Verzeichnis `inhere`.
<details>
    <summary>Lösung</summary>


Nach erfolgreichem Login solltest du nun mit `bandit4` verbunden sein. Gib nun im Terminal folgende Befehle ein:
1. Zum Verzeichnis navigieren:
```bash
ll
cd inhere/
ls
```

2. For-Schleife verwenden und `file` einbinden:
Du erhältst eine Liste an Dateien. Um nicht jede Datei einzeln mit dem Befehl `file {Datei}` zu inspizieren, kannst du in deiner Shell eine For-Schleife nutzen.

Gib in deinem Terminal nun folgenden Befehl ein:

```bash
for i in $(ls) ; do file "./$i" ; done ;
``` 

**Erklärung:** 
- `file` erkennt den Typ der Datei.
- `for i in $(ls) ; ...` For-Loop iteriert über den Befehl `ls` und übergibt die Ausgabe (im Beispiel `--file0X`) in die Variable `i`.
    - `$(ls)` ist eine sogenannten `Command Substitution`
    - bei einer `CS` wird der Befehl in Klammern ausgeführt und seine Ausgabe wird als Wert verwendet und in einer For-Loop in die Schleife zugeführt.
- `... ; do file "./$i" ; done ;` der erste Part ist dafür zuständig, alle Objekte aus `i` über `file` "auszulesen". `./` beschreibt wieder, wo die Datei zu finden ist, die die Variable `i` ansprechen soll. `... done ;` beendet die For-Schleife.

3. Lies die Datei aus:
```bash
cat ./-file07
```

Deine Ein- und Ausgabe sollte dann so aussehen:

![bandit5 Passwort](/09-practice-labs/ressources/pictures/otw-5.png)

Speicher das Passwort und beende die Shell, um fortzufahren.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 5 -> 6

**Aufgabe:** Finde die Datei mit diesen Bedingungen:
<details>
    <summary>Lösung</summary>


* ist 1033 Bytes groß
* nicht ausführbar
* ASCII-Text


```bash
ll
cd inhere/
ll
for dir in . ; do find -readable -size 1033c -not -executable ; done ; 
cat ./maybehere07/.file2
```

Oder: 
```bash
find . -type f -size 1033c ! -executable -exec file {} \; | grep ASCII
cat ./inhere/maybehere07/.file2
```

**Erklärung:**

- `find`: sucht nach Dateien
- `-size 1033c`: genau 1033 Bytes, das `c` steht für den Datentyp `bytes`
- `! -executable`: nicht ausführbar
- `-exec file {}`: prüft Dateityp

![bandit6 Passwort](/09-practice-labs/ressources/pictures/otw-6.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 6 -> 7

**Aufgabe:** Datei gehört Benutzer `bandit7` und Gruppe `bandit6`, Größe 33 Bytes
<details>
    <summary>Lösung</summary>


```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```
**Erklärung:**

- `2>/dev/null` unterdrückt Fehlermeldungen
- `/` durchsucht das gesamte Dateisystem (langsam!)

![bandit7 Passwort](/09-practice-labs/ressources/pictures/otw-7.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 7 -> 8

**Aufgabe:** Datei mit Passwort enthält das Wort "millionth"
<details>
    <summary>Lösung</summary>


```bash
ls
cat data.txt | grep millionth
```

**Erklärung:** 
- `grep` durchsucht Zeilen nach Muster.

![bandit8 Passwort](/09-practice-labs/ressources/pictures/otw-8.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 8 -> 9

**Aufgabe:** Passwort ist die einzige Zeile, die **einmal** vorkommt
<details>
    <summary>Lösung</summary>


```bash
ll 
cat data.txt | sort data.txt | uniq -u
```

**Erklärung:**

* `sort`: sortiert Zeilen (wobei das durch `uniq -u` optional ist).
* `uniq -u`: zeigt nur einmalig vorkommende Zeilen.

![bandit9 Passwort](/09-practice-labs/ressources/pictures/otw-9.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 9 -> 10

**Aufgabe:** Passwort ist in einer Datei als Hexdump gespeichert.
<details>
    <summary>Lösung</summary>


```bash
ls
stings data.txt | grep ====
```

Oder:

```bash
xxd -r data.txt | strings
```

**Erklärung:**

* `xxd -r`: wandelt Hexdump zurück in binär
* `strings`: extrahiert druckbare Zeichen

![bandit10 Passwort](/09-practice-labs/ressources/pictures/otw-10.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 10 -> 11

**Aufgabe:** Datei wurde base64-kodiert
<details>
    <summary>Lösung</summary>


Base64 ist ein Algorithmus, der zur Verschlüsselung von Nachrichten verwendet werden kann. 

```bash
ls
cat data.txt | base64 -d
```

**Erklärung:** `base64 -d` dekodiert Base64-Text.

![bandit11 Passwort](/09-practice-labs/ressources/pictures/otw-11.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 11 -> 12

**Aufgabe:** Der Inhalt der Datei wurde verschlüsselt. 
<details>
    <summary>Lösung</summary>

Es wurde ein Algorithmus verwendet, der alle Klein- und Großbuchstaben um 13 Stellen verschiebt.

> Diese Verschlüsselung hört sich doch ganz nach `ROT13` an. Lass uns mal weitersehen...

Gib im Terminal folgende Befehle ein: 
```bash
ls
cat data.txt
```
Siehst du, das haben sie gemeint, als sie sagten, dass die Stellen um 13 Zeichen verschoben wurden:

![bandit12 Passwort](/09-practice-labs/ressources/pictures/otw-12.png)

Du kannst nun über [google.de](https://www.google.de) nach `ROT13` oder `ROT13 decrypt` suchen oder direkt Cyberchef.io nutzen.

Ich habe über Google nach `ROT13` gesucht und wurde direkt fündig.
Nach dem Entschlüsseln erhalten wir folgendes Passwort:
![Passwort entschlüsseln](/09-practice-labs/ressources/pictures/otw-12b.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Bandit 12 -> 13

**Aufgabe:** Passwort wurde mehrfach mit verschiedenen Kompressionen komprimiert
<details>
    <summary>Lösung</summary>


1. Datei ausfindig machen:
```bash
ls
file data.txt
```

![bandit13 Datei ausfindig machen](/09-practice-labs/ressources/pictures/otw-13.png)

2. Ordner im `/tmp/`-Verzeichnis erstellen. 
```bash
mkdir /tmp/tmp.DeinOrdnerName
cd /tmp/tmp.DeinOrdnerName
```

3. Hexdump zurück in binär umwandeln:
```bash
xxd -r ~/data.txt > data
```
![bandit13 Ordner erstellen und Datei umwandeln](/09-practice-labs/ressources/pictures/otw-13b.png)

4. Datei inspizieren und modifizieren:
```bash
file data
mv data data.gz
ls
```

Nach dem inspizieren der Datei mit `file` siehst du, dass die Datei eine `gzip`- Datei ist. Also muss diese umbenannt werden, wofür du den Befehl `mv` verwenden kannst.

![bandit13 inspizieren](/09-practice-labs/ressources/pictures/otw-13c.png)


**Erklärung:** 
- `file` erkennt Dateityp → danach das passende Tool (gzip, bzip2, tar, etc.)
- `mv` verschiebt Dateien. Aber richtig angwendet ändert es den Dateinamen.

5. Datei mit `gzip` entpacken:

Weiter gehts im Terminal mit folgenden Befehlen:
```bash
ll
gzip -d data.gz
ls
file data
```

![bandit13 Datei umwandeln](/09-practice-labs/ressources/pictures/otw-13d.png)

**Erklärung:**
- `gzip` ist ein ausführbares und bereits vorinstalliertes Programm, um Dateien zu entpacken oder komprimieren. Es ähnelt `7zip` oder `winRar`
    - der Switch `-d` steht für Dekomprimieren.

Jetzt wurde aus einer `gzip`-Datei eine `bzip2`-Datei.
- `bzip2` ist ein genau wie `gzip` ein Tool, um Dateien zu komprimieren und dekomprimieren.

6. Mach so weiter, bis zum Passwort:

```bash
mv data data.bz2
bzip2 -d data.bz2
ls
file data
mv data data.gz
gzip -d data.dz
ls
file data
mv data data.tar
tar -xvf data.tar
ls
file data5.bin
mv data5.bin data5.bin.tar
...
usw.
```

Dies machst du so lange, bist du zum Passwort für das nächste Level kommst.

![bandit13 Dateien inspizieren, umbennen, dekompirimieren, umwandeln](/09-practice-labs/ressources/pictures/otw-13e.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 13 -> 14

**Aufgabe:** Das Passwort ist in einer Datei, welche nur vom User `bandit14` betrachtet werden kann. Es gibt jedoch einen privaten SSH-Key.
<details>
    <summary>Lösung</summary>


Schon einmal eine SSH-Verbindung über eine SSH-Verbindung aufgebaut? 
Los gehts:
Gib im Terminal folgenden Befehle ein:
```bash
ls      # um die Dateien aufzulisten
ssh -l bandit14 bandit.labs.overthewire.org -p 2220 -i sshkey.private   # SSH-Verbindung 
```

Sobald du die `SSH`-Verbindung initiiert hast, wirst du gefragt, ob du sicher bist, fortzufahren. Du bestätigst hier mit `yes`.

**Erklärung:**
- `ssh -l bandit14 bandit.labs.overthewire.org -p 2220 -i sshkey.private` 
    - `-l` steht für User und bandit14 ist der Username.
    - `bandit.labs.overthewire.org -p 2220` auf DNS mit Port 2220 verbinden.
    - `-i` steht für Datei. 

![bandit14 Zugang mit SSH-Key](/09-practice-labs/ressources/pictures/otw-14.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 14 -> 15

**Aufgabe:** Das Passwort gibt es über einen initiierten `netcat`-Service auf Port `30000`.
<details>
    <summary>Lösung</summary>


Dazu müssen wir das aktuelle Passwort dieses Levels in den `netcat`-Service einfügen.


1. Aktuelles Passwort herausfinden:
Das Passwort für das aktuelle `bandit14` Level liegt in `/etc/bandit_pass/bandit14` und kann mit dem Befehl `cat` ausgelesen werden.

```bash
cat /etc/bandit_pass/bandit14
```

Kopiere anschließend das Passwort in die Zwischenablage.

2. Mit dem `netcat`-Service verbinden:

```bash
nc localhost 30000
```

Dass du mit dem `netcat`-Service verbunden bist, siehst du daran, dass deine Shell nun einen blinkenden Zeiger hat.
Hier kannst du nicht viel machen und jeder Befehl wird dich aus der Session werfen, nachdem es dich noch einmal nach dem Passwort auffordert.

Füge hier einfach das kopierte Passwort aus den Befehlen aus Punkt 1. hier ein und bestätige anschließend mit `Enter`.

**Erklärung:** 

- `nc` (netcat) verbindet sich mit einem TCP-Port und zeigt den Output.

![Bandit15 netcat-Service](/09-practice-labs/ressources/pictures/otw-15.png)

**Tipp:**
Um die `netcat`-Session zu beenden, drücke auf Windows `Strg` + `c` und auf Linux/Mac `Control` + `c`.

**Hinweis:**
In diesem Level musst du einmal die `bandit14` SSH-Verbindung mit `exit` beenden und anschließend die SSH-Verbindung zu `bandit13`, da wir über die SSH-Verbindung aus bandit13 eine SSH-Verbindung zu bandit14 geöffnet haben.

Und solange die SSH-Verbindung des Users `bandit14` existiert, exisitiert die SSH-Verbindung zu `bandit13`. 

</details>

 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 15 -> 16

**Aufgabe:** Das Passwort für das nächste Level kann wieder über den `netcat`-Service erhalten werden.
<details>
    <summary>Lösung</summary>


* Port 30001
* localhost mit SSL/TLS Verschlüsselung

Melde dich mit dem Benutzernamen `bandit15` über eine `SSH`-Verbindung an und gelange in das nächste Level.

Gib im Terminal anschließend folgenden Befehl ein:

```bash
cat /etc/bandit_pass/bandit15   # kopiere das Passwort, falls nicht schon gesehen
ncat --ssl localhost 30001      # Mit ncat Service verbinden und SSL Verschlüsselung nutzen.
```
</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 16 -> 17

**Aufgabe:** Die Zugangsdaten des nächsten Levels erhältst du, wenn du den richtigen Server anwählst.
<details>
    <summary>Lösung</summary>


* Port Range `31000-32000`
* `localhost`


1. Nmap Scan durchführen, um Ports zu überprüfen:

```bash
nmap -sV -p 31000-320000 localhost
```

Dann sollte deine Shell folgendes ausgeben:
![Bandit16 nmap Scan](/09-practice-labs/ressources/pictures/otw-16.png)

**Erklärung zu der List im roten Rechteck:**
- `PORT`: Zeigt den Port an, der in der Range 31000-32000 genutzt wird.
- `STATE`: Zeigt, ob der Port offen, geschlossen, gefiltert usw. ist.
- `SERVICE`: Zeigt, was für ein Service darauf läuft.

Dein Nmap Scan sollte auch ungefähr so aussehen.
Du siest 5 Ports, mit offenem State und unterschiedlichen Services, die darauf laufen.
- `echo`: kein Standard State wie `open, closed, filtered` usw. Ganz normaler `echo` Befehl (RFC 862). Gibt erhaltene Eingaben wieder zurück.
- `ssl/echo`: referenziert eine spezielle `nmap-Scripting Enginge (NSE)` und ist kein Standard für einen Port State.
- `ssl-enum-ciphers`-Scripts, falls du mehr erfahren willst.

In der Aufgabe wird erläutert, dass einige Server die deine Ausgabe zurückgeben und nur ein Server dir genau das Passwort gibt. 

Wo glaubst du, wirst du erfolgreich sein, wenn du eine Verbindung aufbaust?

Versuchen wir doch mal den Port, wo der `STATE` auf `ssh/unknown` steht. Wir haben eine sichere Verbindng, aber wissen noch nicht, was sich hinter dieser verbirgt.

Lass es uns testen, zumal wir alle Server im schlimmsten Fall durchtesten müssen.

2. Verbindung zum Server aufbauen und Passwort erhalten

Baue eine Verbindung zum laufenden Server auf Port `31790` auf.
Gib in deiner Shell folgenden Befehl ein und füge anschließend das Passwort aus dem aktuellen Level ein:
```bash
ncat --ssl localhost 31790
```

![Bandit17 Verbindung zum Server](/09-practice-labs/ressources/pictures/otw-17.png)

3. Kopiere den RSA Private Key:

Markiere und kopiere den privaten Key, den du erhalten hast. Erstelle eine neue Datei und füge den kopierten private Key in diese Datei.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 17 -> 18

**Aufgabe:** Es gibt zwei Dateien im `/home`-Verzeichnis. 1. `password.old` und `password.new`. Das Passwort für das nächste Level ist in `password.new`. Differenz zwischen beiden Dateien?
<details>
    <summary>Lösung</summary>


1. SSH-Verbindung aufbauen
Gib im Terminal des `bandit16`-Users folgende Befehle ein, um dich mit SSH zu verbinden:

```bash
ssh -l bandit17 bandit.labs.overthewire.org -p 2220 -i /dein/dateiPfadZur/Datei
```

Wenn du erfolgreich eingeloggt bist, dann kannst du das neue Passwort zum `bandit17`-Level erfahren, indem du folgenden Befehl eingibst:

```bash
cat /etc/bandit_pass/bandit17
```
![Bandit17 Passwort herausfinden](/09-practice-labs/ressources/pictures/otw-18d.png)

Solltest du wie nachfolgend eine Fehlermeldung erhalten, dann liegt das daran, dass die Datei, die den private Key hält, von anderen Usern auch zugänglich ist. Das beudetet für dich, dass du die Berechtigungen für die Datei entziehen musst, sodass nur du Zugriff auf diese hast.

![Bandit18 Verbindung zum Server](/09-practice-labs/ressources/pictures/otw-18.png)

Um dies zu tun, gib folgende Befehl ein:
```bash
ll      # optional um noch einmal eine Liste der Dateien zu haben
chmod 400 rsa.key
```
![Bandit18 Verbindungsfehler beheben](/09-practice-labs/ressources/pictures/otw-18b.png)

2. Verbindung nach Fehlerbehebung aufbauen.

Gib im Terminal den folgenden Befehl ein und bestätige mit yes:

```bash
ssh -l bandit17 bandit,labs.overthewire.org -p 2220 -i /dein/dateiPfadZur/Datei
```

![Bandit18 Verbindung zum Server](/09-practice-labs/ressources/pictures/otw-18c.png)

Nun solltest du erfolgreich mit der Shell des Benutzers `bandi17` verbunden sein.

3. Passwort für Bandit18 herausfinden:

Jetz haben wir noch das Password für `bandit18`, welches sich in der Datei `password.new` befindet. 
In dieser Datei gibt es 100 Wörter - das kannst du herausfinden, wenn du den Befehl `wc -w Datei` eingibst. 

Diese Dateien unterscheiden sich nur um eine Zeile.
Du kannst folgenden Befehl nutzen, um herauszufinden, wo der Unterschied dieser Dateien im Inhalt ist:

```bash
diff -d passwords.new passwords.old
```

![Bandit18 Passwort herausfinden](/09-practice-labs/ressources/pictures/otw-18e.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 18 -> 19

**Aufgabe:** Das Passwort ist in einer Datei im `home`-Verzeichnis. 
<details>
    <summary>Lösung</summary>


In dieser Challenge musst du einen anderen Weg finden, wie du dich als User `bandit18` einloggen kannst.
Wenn du nämlich eine `ssh`-Verbindung aufbaust, wirst du nach dem Passwort gefragt. Gibst du das Passwort ein, erhältst du die Nachricht `Byebye` und wirst wieder ausgeloggt.

Das liegt daran, dass die `.bashrc` des Userse `bandit18` modifiziert wurde und jedes mal, wenn dieser User sich über eine `ssh`-Verbindung einloggen möchte, wird er wieder ausgeloggt.

![Bandit19 Login-Versuch mit SSH und Passwort](/09-practice-labs/ressources/pictures/otw-19.png)

![Bandit19 byebye](/09-practice-labs/ressources/pictures/otw-19b.png)

**Wie kannst du das umgehen?**

Dazu nutzt du eine `ssh`-Verbindung und gibst die Shell an, die du nutzen willst.
Dein Befehl sollte dann so aussehen:

- Entweder mit einer Anzeige der Shell-Oberfläche:
```bash
ssh -t -l bandit18 bandit.labs.overthewire.org -p 2220 bash --norc --noprofile
```
![Bandit19 Login-Versuch mit anderer Bash über SSH](/09-practice-labs/ressources/pictures/otw-19c.png)

**Erklärung:**
- `-t`: Dieser Switch sorgt dafür, dass ein Pseudo-Terminal beim Host erzwungen wird (auch, wenn nicht unbedingt nötig).
- `bash --norc`: Nach erfolgreichem Login wird eine neue Bash-Shell gestartet, jedoch wird die userspezifische Datei `~/.bashrc` nicht geladen. 
- `--noprofile`: Die Dateien `~/.profile`, `~/.bash_profile` und `~/.bash_login` werden ebenfalls ignoriert.

Die letzten beiden Switche sorgen dafür, dass die Shell keine Konfigurationen lädt, was nützlich sit, um saubere und kontrollierte Umgebungen zu starten.

- Oder ohne `-t`:
```bash
ssh -l bandit18 bandit.labs.overthewire.org -p 2220 bash --norc --noprofile
```

![Bandit19 Login-Versuch ohne Anzeige über SSH](/09-practice-labs/ressources/pictures/otw-19d.png)

**Tipp:**

Du kannst in diesem Befehl `bash` mit `dash` oder einer anderen Shell ersetzen und versuchen, ob du dich einloggen kannst.

Kopiere und speicher das Passwort für die nächste Challenge.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 19 -> 20

**Aufgabe:** Um das Passwort herauszufinden, muss du die `setuid binary` im `home`-Verzeichnis nutzen.
<details>
    <summary>Lösung</summary>


Wenn du mit `bandit19` eingeloggt bist, gibst du folgendene Befehle ein:
```bash
ls           # listet alle Dateien im aktuellen Verzeichnis auf
./bandit20-do cat /etc/bandit_pass/bandit20
```

`bandit20.do*` ist ein ausführbares Programm. Es lässt zu, dass du Dateien des Users `bandit20` nutzen kannst. 
Das kannst du verwenden, um beispielsweise das Passsword des Users auszuspähen (`cat /etc/bandit_pass/bandit20`).


![Bandit20 Password ausspähen](/09-practice-labs/ressources/pictures/otw-20.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 20 -> 21

**Aufgabe:** Du findest eine `setuid binary` in deinem `home`-Verzeichnis. Diese Datei stellt eine Verbindung zu deinem `localhost` an einem bestimmten `<Port>` her.
<details>
    <summary>Lösung</summary>


Um das Passwort für dieses Level zu erhalten, übermittelst du das Passwort des aktuellen Levels über das Programm `suconnect` an den Server und erhältst dann eine Nachricht.

1. Mit `ls` oder `ll` Dateien auflisten:

Gib im Terminal folgende Befehle als User `bandti20` ein:
```bash
ll
```

Dann solltest du folgende Ausgabe erhalten:

![Bandit21 Dateien auflisten mit `ll`](/09-practice-labs/ressources/pictures/otw-21.png)

2. `namp`-Scan deines Localhost:

Im nächsten Step erkundschaftest du die Netzwerklandschaft deines `localhost`.
Gib im Terminal folgenden Befehl ein:

```bash
nmap -sV localhost
```

Das sollte dir die Ports auflisten und die dazugehörigen `STATES` und den `SERVICE`.
Wir haben **8 Ports** verfügbar, von dem ein Port ein Standard-Port ist (`22/tcp` für `ssh`).

![Bandit21 Nmap Scan](/09-practice-labs/ressources/pictures/otw-21b.png)

3. Zweites Terminal starten um Password zu erhalten:

Starte ein zweites Terminal. Wir benötigen es, um mit `nc` eine Verbindung zu "belauschen".
In diesem Terminal ist es nötig, dich wieder mit einer `ssh`-Verbindung anzumelden. Logge dich einfach mit dem Benutzernamen `bandit20` ein und nutze das Passwort.

Sobald du dich in diesem zweiten Terminal eingeloggt hast, gibst du in diesem Terminal **Nr 2** folgenden Befehl ein:

```bash
bash --norc --noprofile
```

Damit erzeugst du eine neue Shell (`bash`) und kannst über den gleichen Benutzeraccount quasi parallel Befehle steuern.

Das nutzen wir, um unser Passwort zu erhalten.

Im nächsten Schritt gibst du im Terminal **Nr 1** folgende Befehle ein:
```bash
cat /etc/bandit_pass/bandit20      # Ausgabe des aktuellen Passworts
nc -lnvp 3000       # startet den Netcat Service (belauschen des Ports 3000)
```

![Bandit21 Passwort erhalten](/09-practice-labs/ressources/pictures/otw-21c.png)

Jetzt läuft ein `netcat`-Service, der den Port `3000` belauscht.
Im Terminal **Nr 2** gibst du nun folgenden Befehl ein:
```bash
./succonect 3000
```

> Es kann sein, dass du den Befehl erneut eingeben musst, so wie auf dem Bild zu sehen. Erst danach war es dem `nc`-Service möglich, eine Verbindung zu erhalten.

![Bandit21 Passwort erhalten](/09-practice-labs/ressources/pictures/otw-21d.png)

Anschließend kannst du beide Terminals mit `exit` beenden und die nächste Challenge antreten.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 21 -> 22

**Aufgabe:** Ein Programm läuft automatisch zu regulären Intervallen aus `cron`. Schau in `/etc/cron.d/` vorbei.
<details>
    <summary>Lösung</summary>


Logg dich mit `bandit21` über `ssh` ein und gib das Passwort des letztes Levels hier ein.

Anschließend solltest du mit `bandit21` verbunden sein.
Nun gibst du folgende Befehle in das Terminal:

```bash
cd /etc/crond.d         # wechselt in das relevante Verzeichnis
ls                      # listet alle Dateien auf
cat cronjob_bandit22    # relevante Datei für diese Challenge
cat /usr/bin/cronjob_bandit22   # liest die Datei aus
cat /tmp/GibDeineZeichenHierEin # liest die Datei im /tmp-Verzeichnis aus
```

Kopiere und speichere das Passwort für die nächste Challenge.

![Bandit21 Passwort ermitteln](/09-practice-labs/ressources/pictures/otw-22.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 22 -> 23

**Aufgabe:** Ein Programm führt sich zu regulären Intervallen aus. Schau in `/etc/cron.d/` vorbei.
<details>
    <summary>Lösung</summary>


1. `cron`-Dateien ausfindig machen:

Wechsle dazu in das `/etc/cron.d/`-Verzeichnis und schau dir die dort befindlichen Dateien in Ruhe an.

```bash
cd /etc/cron.d/         # navigiert in das Verzeichnis
ls                      # listet die Dateien auf
cat cronjob_bandit23    # gibt die Datei aus
```

Deine Ausgabe sollte dann folgendermaßen aussehen:
```bash
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

2. Verzeichnis in `/tmp/ erstellen

Erstelle für diese Challenge einen vorrübergehenden Ordner im `/tmp`-Verzeichnis, kopiere die `cronjob_bandit23.sh`-Datei, bennene sie um und wechsle in deinen neuen Ordner:

```bash
mkdir /tmp/DeinOrdnerName       # erstelle einen Ordner im /tmp-Verzeichnis
cp /usr/bin/cronjob_bandit23.sh /tmp/DeinOrdnerName/neueDatei.sh     #kopiere und umbenennen der Datei
cd /tmp/DeinOrdnerName      # in dein neues Verzeichnis wechseln
ls      # optional
```

![Bandit23 cronjob Datei analysieren](/09-practice-labs/ressources/pictures/otw-23.png)

3. `nano`-Editor oder `cat` eine Zeile herauskopieren

Wir brauchen nur einen einzige Zeile aus der kopierten Datei. Lies sie entweder mit dem `nano`-Editr aus oder nutze den `cat`-Befehl dafür.

Als Beispiel nutze ich meinen Dateinamen. Deinen solltest du anpassen.

```bash
nano newcron.sh     # öffnet im Nano Editor die newcron.sh Datei
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

![Bandit23 Code-Zeile kopieren](/09-practice-labs/ressources/pictures/otw-23c.png)

Du erhältst anschließend eine sehr lange Buchstaben- und Zahlenreihenfolge. Kopiere es und gib folgenden Befehl ein, um die Challenge abzuschließen:

```bash
cat /tmp/foo # foo mit deinem "Code" ersetzen
```

Nach dem Auslesen der Datei erhältst du das Passwort für `bandit23`.

![Bandit21 Passwort](/09-practice-labs/ressources/pictures/otw-23b.png)


Kopiere und speichere das Password für die nächste Challenge.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 23 -> 24

**Aufgabe:**

<details>
    <summary>Lösung</summary>

1. `cronjobs`-Dateien anzeigen und öffnen

```bash
ls /etc/cron.d
cat /etc/cron.d/cronjob_bandit24
```

2. `cronjob`-Skrip öffnen:
```bash
cat /usr/bin/cronjob_bandit24.sh
```

Anschließend solltest du das Skript sehen, welches alle Dateien nach Ausführung in dem Verzeichnis `/var/spool/bandit24/foo/` löscht.

3. Datei erstellen, Rechte vergeben, kopieren:

Das Programm löscht alle Dateien und somit auch das Passwort. Schreibe ein Skript, welches genau dieses Passwort in das `/tmp/`-Verzeichnis kopiert. 

**Hinweis:** Du musst zusätzlich die Rechte ändern, damit der User `bandit24` die Datei ausführen darf, auch, wenn du sie als `bandit23` erstellt hast. Genauso ist das der Grund, warum wir eine Datei im `/tmp/`-Verzeichnis erstellen, um die Root-Rechte etwas umgehen zu können. Kein harmloser Exploit also!

Gib im Terminal folgende Befehle ein, um eine Datei im `/tmp/`-Verzeichnis zu erstellen, ihr Rechte zu vergeben und sie anschließend in das Verzeichnis für den `cronjob` zu kopieren:

Beachte: Zum erstellen dieser Datei nutze ich den `nano`-Editor. Du kannst aber jeden anderen Editor dafür verwenden.

```bash
nano /tmp/deinDateiName.sh  # eine ausführbare Datei mit .sh erstellen
```

Nach diesem Befehl öffnet sich der Editor. Schreibe nun folgendes Skript:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/passwort
```

**Was passiert hier?**
Wenn du ein Skript erstellen willst, welches ausührbar ist, dann musst du einen Compiler angeben.
In diesem Fall leitest du das mit einem `Sheband`, also der Zeile `#!/bin/bash`, ein. 
Das sagt dem Compiler, dass es sich um ein `Bash`-Skript handelt und es wird dementsprechend kompiliert.

Die Zeile `cat /etc/bandit_pass/bandit24 > /tmp/passwort` liest das Passwort aus dem Verzeichnis `/etc/bandit...` und fügt es mit `/tmp/passwort` im Verzeichnis `/tmp/` in eine neue Datei names `passwort`.

Nach dem Skrippten speicherst du die Datei mit `Strg` + `x`. Du wirst gefragt, ob du die Datei speichern willst: Bestätige mit `y`. Und noch einmal bestätigen, weil dir die Möglichkeit gegeben wird, den Namen der Datei zu ändern. Den Dateinamen kannst du so beibehalten.

Bleiben noch die Rechtevergabe und das Kopieren in das Verzeichnis `/var/spool/bandit/foo/`

```bash
chmod -R 777 /tmp/deinDateiName.sh
cp /tmp/deinDateiName.sh /var/spool/bandit24/foo
```

4. Warten, bis der Cronjob ausgeführt wird

Das Programm `cronjob_bandit24.sh` führt sich zu regulären Intervallen selbst aus. In diesem Skript sind es alle 60 Sekunden. Nach einer gewissen Zeit kannst du mit folgenden Befehl die Datei auslesen:

```bash
cat /tmp/password # die Datei aus dem erstellten Skript ausgeben
```

Solltest du das Passwort aus dem aktuellen Level erhalten, probieren den letzten Befehl noch einmal aus.

Du solltest jetzt das Passwort erhalten und kannst mit der nächsten Challenge fortfahren.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 24 -> 25

**Aufgabe:** Ein `daemon` belauscht auf Port `30002`und gibt dir das richtige Passwort, nach der Eingabe des aktuellen Passwort und der richtigen Kombination einer vierstelligen Zahl.

<details>
    <summary>Lösung</summary>

1. Ein Skript im `/tmp/`-Verzeichnis erstellen:

Um das Passwort knacken zu vereifachen, erstellst du ein Skript, welches die Bedingung erfüllt (Passwort und 4-Stelliger Pin).
Gib im Terminal folgenden Befehle ein:

```bash
nano /tmp/deineDatei.sh
```

Der Nano Editor sollte sich nun öffnen. Jetzt schreibst du folgendes in das Skript:

```bash
#!/bin/bash
passwort="HierKommtDasPasswortDesAktuellenLevelsRein"

for pin in $(seq -w 0000 9999) ; do
    echo "Versuch PIN: $pin"
    ergebnis=$(echo "$passwort $pin" | timeout 0.05 nc localhost 30002)
    if [[ $ergebnis != *"Wrong!"* && $ergebnis != "" ]]; then
        echo "Korrekte PIN: $pin"
        echo "$ergebnis"
        break
    fi
done
```

**Was passiert hier?**

- `passwort=....`: Variable Passwort mit dem Passwort des aktuellen Levels `bandit24`.
- `for pin in $(seq -w 0000 9999) ; do`: Start einer Schleife.
    - `seq -w`: gibt führende Nullen mit aus.
    - `0000 - 9999`: alle vier Stellen werden getestet.
-  `echo "Versuch PIN: $pin"`: gibt den aktuellen Versuch aus.
- `ergebnis=$(echo "$passwort $pin" | timeout 0.05 nc localhost 30002)`: Sendet den String `"passwort pin"` per nc an Port 30002 auf localhost.
    - `timeout 0.05`: Trennt Verbindung nach 0.05 S, falls Dienst nicht antwortet (Schutz vor Hänger).
    - `Wrong!` oder `""` werden in der Variable `ergebnis` gespeichert.
- `if [[ $ergebnis != *"Wrong!"* && $ergebnis != "" ]]; then`: prüft, ob Antwort nicht "Wrong!" enthält und nicht leer ist.
    - Wenn beides zutrifft, geht das Skript davon aus, dass die richtige PIN gefunden wurde.
    - `echo "Korrekte PIN: $pin"`: Gibt die gefunden korrekte PIN aus.
    - `echo "$ergebnis"`: Gibt die Server Antwort aus.
    - `break`: Beendet die den Prozess
- `fi`: Beendet If-Schleife.
- `done`: Beendet die For-Schleife.

Nun solltest du das Passwort für `bandit25` erhalten haben und kannst mit der nächsten Challenge fortfahren.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 25 -> 26

**Aufgabe:** Logge dich mit `bandit26` über die Shell von `bandit25` an. Beachte, dass die Shell von `bandit26` nicht `/bin/bash` ist.

<details>
    <summary>Lösung</summary>

Melde dich mit `bandit25` an und schau mit `ls` in dein `home`-Verzeichnis. Dort findest du einen `bandit26.sshkey`. Den solltest du nutzen, um dich mit `bandit26` über SSH authentifizieren zu können.

1. Herausfinden, welche Bash genutzt wird:
Gib folgende Befehle ein:
```bash
ls      # listest alle Dateien im aktuellen Verzeichnis auf
cat /etc/passwd | grep bandit26     # um herauszufinden, welche Shell Bandit26 hat
```

Die Ausgabe von `/etc/passwd/` sollte dann so aussehen:
```bash
bandit26:x:11026:11026:bandit Level 26:/home/bandit26:/usr/bin/showtext
```

2. `cat` von `/usr/bin/showtext`:

Gib die Datei mit `cat` aus, um mehr darüber zu erfahren.
```bash
cat /usr/bin/showtext
```
`showtext` startet also das `more`-Programm, welches wie ein Pager die Inhalte einer Datei ausgibt.
Nutzen wir dieses wissen.

3. Starte eine `ssh`-Verbindung

Nutze die Datei in deinem Home-Verzeichnis und gib folgenden Befehl ein:

```bash
ssh -l bandit26 -p 2220 -i bandit26.sshkey bandit.labs.overthewire.org
```
Du wirst jedoch leider rausgeschmissen ohne, dass du etwas machen kannst.

Nützlich zu wissen, wie das Programm `more` funktioniert.
Kurz gesagt:
`more` Filter einen Text zum bildschirmweise Blättern. "Primitiver" als Less. Wenn du mehr über `more` erfahren willst, dann lies dazu die [manpage](https://linux.die.net/man/1/more) dazu.

4. Verkleinere dein Terminalfenster:

Das Programm `more` blättert also bildschirmweise duch den Inhalt einer Datei. 

> Verkleinere dein Terminalfenster und versuche eine erneute SSH-Verbindung aufzubauen. 

Gib nach dem Verkleiner deines Terminals folgenden Befehl ein:
```bash
ssh -l bandit26 -p 2220 -i bandit26.sshkey bandit.labs.overthewire.org
```

Das sollte nun funktionieren und du solltest im Terminal so etwas sehen wie `More xy%`. Das deutet daraufhin, dass dein Terminal nun klein genug ist.

Das ist "Out-of-the-box" par excellance und ich muss gestehen, dass ich hier googlen musste. 

**Hinweis:** Solltest du wieder keine Verbindung haben, dann verkleiner dein Fenster ein wenig mehr. Dabei ist es nicht wichtig ob du die Breite veränderst, sondern vielmehr die Höhe des Fensters.

Ob du erfolgreich bist, siehst du daran, dass unten links in deinem Terminalfenster das Wort `More XY%` steht. Dabei ist XY% der Inhalt, die more bereits gescrollt ist.

5. Mit `more` auf die das Passwort zugreifen:

Mit `more` zu arbeiten, kann anfangs schwierig sein. 
Sobald du jedoch Zugang mit `more` aufgebaut hast, gibst du folgende Befehle ein:

```bash
v       # startet den Vi Editor in more

# 1. Befehl:
# im Vi Editor gibst du dann folgendene Befehle ein:
ESC     # Drücke ESC, wenn du im Vi-Editor etwas schreiben willst

# einmal ESC drücken, dann kannst du schreiben.
# wenn du fertig bist, drücke die ENTER-Taste.
:e /etc/bandit_pass/bandit26   # wie der cat-Befehl.
#ENTER // wenn du erfolgreich bist, sollte das Passwort ausgegeben werden.
```
Hier hast du bereits das Passwort erhalten. Du könntest du einen anderen Weg wählen und eine Shell erzeugen.

Angenommen, du hast den ersten Schritt nicht gemacht und das Passwort nicht erhalten.
Wir können über den `Vi`-Editor versuchen, eine Shell zu erhalten.

Dazu legen wir mit `:set shell=/bin/bash` die Shell fest die wir dann mit `:shell` aufrufen.
Gib im `Vi`-Editor dazu folgenden Befehle ein:

```bash
# 1. Befehl:
ESC     # Um etwas zu schreiben.
:set shell=/bin/bash    # setzt die Shell "Bash" ein.
# ENTER

# 2. Befehl:
:shell      # ruft die Shell auf
# ENTER
```

Nun solltest du Zugriff auf die normale Shell haben und mit folgendem Befehl das Passwort erhalten:

```bash
cat /etc/bandit_pass/bandit26
```

![Passwort](/09-practice-labs/ressources/pictures/otw-26.png)

Bleib gleich eingeloggt und mach in dem Terminal von `bandit26` weiter, aber speichere das Passwort, falls du erneut darauf zugreifen möchtest (optional).

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 26 -> 27

**Aufgabe:** Finde das Passwort von `bandit27`

<details>
    <summary>Lösung</summary>

Nach dem du erfolgreich mit `bandit26` eingeloggt bist, gibst du im Terminal folgenden Befehl ein:

```bash
ls      # listet die Dateien auf. Es sollte `bandit27-do` vorhanden sein
./bandit27-do   # führe die Datei aus und schau was passiert.
```

Wenn du die Datei ausführst, dann wirst du eine Fehlermeldung erhalten. Gut für dich. Als "Hacker" musst du lernen, Fehler bzw. Fehlermeldungen für dich zu nutzen.

Folgende Fehlermeldung erscheint, nach Start des Programms `bandit27-do`
```text
Run a command as another User.
   Example: ./bandit27-do id
```

Um an das Passwort zu kommen, ersetze die `id` mit einem Befehl. Gib folgendes in das Terminal ein:
```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Anschließend wirst du das Passwort für `bandit27` erhalten. Speichere es und nutze es für die nächste Challenge.

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 27 -> 28

**Aufgabe:** Git Repository klonen und das Passwort finden.

<details>
    <summary>Lösung</summary>

Sobald du als `bandit27` eingelogt bist, erstellst du ein Verzeichnis im `/tmp`-Verzeichnis, klonst die Repository aus `GitHub` und liest die darin befindliche Datei aus.

Gib im Terminal folgenden Befehle ein:
```bash
mkdir /tmp/DeinOrdnerName   # Erstelle einen Ordner
cd /tmp/DeinOrdnerName      # in den Ordner navigieren
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo  # klont die Repo aus GitHub

# Sobald das abgeschlossen ist:
ls      # Verschafft dir einen Überblick
cd repo # in den Ordner wechseln
cat README  # Datei ausgeben
```

Du solltest nun dein Passwort für `bandit28` erhalten haben. Speichere es und nutze es in der nächsten Challenge.

![Bandit28 Passwort](/09-practice-labs/ressources/pictures/otw-28.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 28 -> 29

**Aufgabe:** Git Repository klonen und das Passwort finden.

<details>
    <summary>Lösung</summary>

Im ersten Schritt machst du in `bandit28` das selbe wie du es in `bandit27` gemacht hast.
Du erstellst einen Ordner, navigierst dorthin und klonst die GitHub-Repository des `bandit28`-Users.

Dazu gibst du folgende Befehle ein:
```bash
mkdir /tmp/DeinOrderName    # Erstellt deinen Ornder im /tmp-Verzeichnis
cd /tmp/DeinOrdnerName      # navigiert in deinen erstellen Ordner
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo

# Wenn du nach einem Passwort gefragt wirst, dann gib das des aktuellen Levels ein
```

Anschließend sollte die Repository in deinen Ordner geklont werden.

Gib nun folgenden Befehl ein, um das Passwort zu erhalten:
```bash
git show
```

![Bandit29 Passwort](/09-practice-labs/ressources/pictures/otw-29.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 29 -> 30

**Aufgabe:** Git Repository klonen und das Passwort finden.

<details>
    <summary>Lösung</summary>

Im ersten Schritt machst du in `bandit29` das selbe wie du es in `bandit28` gemacht hast.
Du erstellst einen Ordner, navigierst dorthin und klonst die GitHub-Repository des `bandit29`-Users.

Dazu gibst du folgende Befehle ein:
```bash
mkdir /tmp/DeinOrderName    # Erstellt deinen Ornder im /tmp-Verzeichnis
cd /tmp/DeinOrdnerName      # navigiert in deinen erstellen Ordner
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
# Wenn du nach einem Passwort gefragt wirst, dann gib das des aktuellen Levels ein
cd repo     # wechselt in die GitHub-Repo
```

Anschließend führen dich diese Befehle direkt an dein Ziel:

```bash
git branch          # zeigt alle Branches an
git checkout dev    # wechselt in den dev Branch
cat README.md       
```

Du solltest nun das Passwort erhalten und kannst mit der nächsten Challange fortfahren.

![Bandit30 Passwort](/09-practice-labs/ressources/pictures/otw-30.png)


</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 30 -> 31

**Aufgabe:** Git Repository klonen und das Passwort finden.

<details>
    <summary>Lösung</summary>

Um an das Passwort von `bandit31` zu kommen, gehst du folgendermaßen vor:

1. Klone die Git-Repo

Genau wie die letzten 3 Level, erstellst du einen Ordner, navigierst hinein und klonst die Git-Repo.
Gib dazu folgende Befehle ein:

```bash
mkdir /tmp/DeinOrderName    # Erstellt deinen Ornder im /tmp-Verzeichnis
cd /tmp/DeinOrdnerName      # navigiert in deinen erstellen Ordner
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
# Wenn du nach einem Passwort gefragt wirst, dann gib das des aktuellen Levels ein
cd repo     # wechselt in die GitHub-Repo
```
![Bandit31 GitHub-Repo klonen](/09-practice-labs/ressources/pictures/otw-31.png)

2. An das Passwort kommen

Jetzt, nach dem die Git-Repo geklont wurde, gibst du folgende Befehle ein, um herauszufinden, was es für Branches gibt. 

**Tipp:** Nutze bei `git checkout` die `TAB`-Taste mehrmals hintereinander, um herauszufinden, wohin du navigieren kannst.

```bash
git branch -a   # listet alle lokalen und remote Branches auf
# mit dem nächsten Befehl kommt eine Besonder
git checkout    # drücke die Tab-Taste zwei Mal
```

Wenn alles richtig ist, dann solltest du nun branches sehen, in die du wechseln kannst. 
Mir ist direkt einer aufgefallen: `secret`.

![Bandit31 git submodul](/09-practice-labs/ressources/pictures/otw-31b.png)

Allerdings gibt es ein Problem. `secret` ist kein Branch in den du wechseln kannst, sondern ein Submodul.
Es wird in der Repo als `blob` - ein `binary large object` - gehandelt . Was du hier tun kannst, ist, das Submodul `secret` zu "entpacken".

Gib dazu folgende Befehl ein, um das Passwort zu erhalten:
```bash
git unpack-file secret      # "entpackt" den blob aus dem Submodul `secret`
cat .merge_file_6nv679      # gibt den Inhalt der erhaltenen File aus
```

Jetzt solltest du das Passwort für die nächste Challenge ausgegeben bekommen.

![Bandit31 Passwort](/09-practice-labs/ressources/pictures/otw-31c.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 31 -> 32

**Aufgabe:** Git Repository klonen und das Passwort finden.

<details>
    <summary>Lösung</summary>

Um an das Passwort zu kommen, müssen wir die GitHub-Repo für User `bandit31` klonen und eine Datei mit `git push` auf die Repo hochladen.

1. Ordner erstellen und GitHub-Repo klonen:
```bash
mkdir /tmp/DeinOrdnerName   # erstelle einen Ordner im `/tmp`-Verzeichnis
cd /tmp/DeinOrdnerName      # in deinen Ordner navigieren
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo  # Passwort des Users eingeben und Repo klonen
cd repo     # alternativ hiervor ls oder ll um aufzulisten.
```
![Bandit32 GitHub Repo klonen](/09-practice-labs/ressources/pictures/otw-32.png)

2. Readme lesen, .gitignore modifizieren und Datei hochladen

Liste auf, was in der Repo vorhanden ist und lies die README.md durch.

```bash
ll      # Lange Liste, um versteckte Dateien anzuzeigen
cat README.md   # README.md ausgeben
git branch      # zeigt aktuellen Branch an
cat .gitignore  # gibt die .gitignor aus
nano .gitignore # öffnet den nano-Editor, um Datei zu bearbeiten
```
![Bandit32 Dateien modifizieren](/09-practice-labs/ressources/pictures/otw-32b.png)

Die Hälfte ist geschafft.

**Wichtig:** `.gitignore` ist eine Datei, die als "Konfiguration" verwendet wird, die es Entwickler ermöglicht, Dateien und/oder Informationen auszuschließen, die dann beim Hochladen übersprungen werden.

Beim Auslesen der Datei ist dir vermutlich aufgefallen, dass der Inhalt der `.gitignore`-Datei alle `.txt`-Dateien überspringen würde. Jedoch müssen wir diese Dateien laut der `README.md` hochladen, um das Passwort zu erhalten.

Die Zeile in `.gitignore` kannst du einfach auskommentieren, was dieses Problem damit löst.

Kommentieren die Zeile im `nano`-Editor aus und speichere die Datei, ohne den Namen zu überschreiben.


3. Datei erstellen und hochladen:

Erstelle eine Datei, gib den Inhalt `May I come in?` ein, speichere die Datei und lade sie über ein `commit` auf die Repository.

Gib folgende Befehle ein:

```bash
echo "May I come in?" > key.txt # Echo Ausgabe in Datei
git add .       # Veränderungen auf git hinzufügen.
git commit -m "May I come in?"  # Commits in den Stash laden.
git push        # alles in die Repo hochladen
```

Anschließend solltest du dein Passwort erhalten und kannst mit dem nächsten Level fortfahren.

![Bandit32 Passwort](/09-practice-labs/ressources/pictures/otw-32d.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 32 -> 33

**Aufgabe:** Finde das Passwort in der UPPERCASE-Shell.

<details>
    <summary>Lösung</summary>

Für dieses Level ist er vorteilhaft, etwas über Variablen in der Programmierung zu wissen und ganz besonders, wie du sie in einer Shell verwenden kannst. Variablen in einer Shell zu definieren und aufzurufen ist etwas anders, als in einer der bekannten Programmiersprachen.

In diesem Level kannst du das Terminal nicht mit gewohnten Befehlsabfolgen steuern. Verwende ein `$`-Zeichen, um mit der Shell interagieren zu können. Ein `$`-Zeichen zeigt der Shell auch gleichzeitig, dass es sich dabei um eine Variable handelt.

Außerdem wirst du beim Herumprobieren feststellen, dass du nicht viele Befehle hast, die du aufrufen kannst, da deine Rechte sehr eingeschränkt sind.

Befehle wie `whoami` oder `clear` funktionieren in dieser Shell nicht.

Um aber an das Passwort zu kommen, gibst du folgende Befehle ein:

```sh
$0      # diese Variable referenziert eine Shell und lässt uns aus der jetzigen ausbrechen.
whoami  # zeigt, als welcher User du angemeldet bist.
cat /etc/bandit_pass/bandit33   # Passwort ausgeben.
```

Du solltest nun das Passwort für `bandit33` erhalten und kannst mit der letzten Challenge fortfahren.

![Bandit33 Passwort](/09-practice-labs/ressources/pictures/otw-33.png)

</details>




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Bandit 33 -> 34

**Aufgabe:** Es gibt aktuell kein Level 34.

<details>
    <summary>Lösung</summary>

Aktuell gibt es kein weiteres Level mehr. Die Entwickler sind jedoch bemüht, weitere Challenges hinzuzufügen.

![Bandit34 Nachricht](/09-practice-labs/ressources/pictures/otw-34.png)


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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
