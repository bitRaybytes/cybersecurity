# Over The Wire: Leviathan

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Zugang](#zugang)
- [Allgemeine Infos](#allgemeine-infos)
- [Leviathan 0 -> 1](#leviathan-0---1)
- [Leviathan 1 -> 2](#leviathan-1---2)
- [Leviathan 2 -> 3](#leviathan-2---3)
- [Leviathan 3 -> 4](#leviathan-3---4)
- [Leviathan 4 -> 5](#leviathan-4---5)
- [Leviathan 5 -> 6](#leviathan-5---6)
- [Leviathan 6 -> 7](#leviathan-6---7)
- [Erkenntnisse](#erkenntnisse)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)

----

## Einführung
Leviathan ist ein Wargame auf [OverTheWire](https://overthewire.org/wargames/), das sich speziell an Anfänger richtet.  
Es vermittelt die Grundlagen von **Privilege Escalation** (Rechteausweitung) und einfachen **Security Misconfigurations** in Linux-Systemen.  

Im Gegensatz zu anderen Wargames (wie Bandit oder Natas) liegt der Fokus hier weniger auf Web-Exploits, sondern auf klassischen **Binary Exploits** und **Dateiberechtigungen** in einer Unix-Umgebung.  

---

## Zugang
Der Einstieg erfolgt über eine **SSH-Verbindung**

---

## Allgemeine Infos
- **Host:** leviathan.labs.overthewire.org  
- **Port:** 2223  
- **Start-User:** leviathan0
- **Start-Passwort:** leviathan0
- **Ziel:** Schrittweise Privilege Escalation bis zum höchsten User
- **Betriebssystem:** Kali-Linux
- **Offizielle Seite:** [OverTheWire - Leviathan](https://overthewire.org/wargames/leviathan/)
- **Hinweis:** Passwörter können aktualisiert worden sein!

---

## Leviathan 0 -> 1

**Aufgabe:** Finde das Passwort zu `leviathan1`

<details>
    <summary>Lösung</summary>

### Login

Als erstes müssen wir uns über `SSH` mit dem Benutzernamen und Passwort einloggen.
Gib dazu in deinem Terminal folgenden Befehl ein:

```bash
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
```

Nach dem Befehl wirst du gefragt, ob du die Verbindung bestätigen willst. Tippe `yes` ein, um fortzufahren.

![leviathan1 Zugang](/10-practice-labs/ressources/pictures/leviathan1.png)


Erfolgreich eingeloggt, sehen wir die Shell des Users `leviathan0` auf dem Host `@leviathan`.
Damit haben wir nun Zugriff auf die Daten des Users und können uns auf die Suche des Passworts machen.

### Lösung

Gib folgende Befehle ein, um das Passwort für das nächste Level zu erhalten:

```bash
ll           # listet alle Dateien und Verzeichnisse im aktuellen Verzeichnis, in dem sich der User befindet
cd .backup/  # cd wechselt in das Verzeichnis
ll           # listet die Dateien und Verzeichnisse im .backup/-Verzeichnis
cat bookmarks.html | grep passw
```

![leviathan1 Passwort](/10-practice-labs/ressources/pictures/leviathan1b.png)

**Erklärung der Befehle:** 
- `ll`: Verzeichnis auflisten, auch versteckte Dateien/Verzeichnisse, sowie Berechtigungen.
- `cd [PFAD]`: wechselt in den angegebenen Pfad.
- `cat`: "Pager", um Inhalte der Dateien auszugeben (cat = concatenate).

### Passwort
> Das Passwort zu `leviathan1` lautet `3QJ3TgzHDq`.

Speichere das Passwort und beende die `SSH`-Sitzung mit `exit`.


</details>

---

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Leviathan 1 -> 2

**Aufgabe:** Finde das Passwort zu `leviathan2`

<details>
    <summary>Lösung</summary>

### Login

Logge dich nun mit dem User `leviathan1` und dem dazugehörigen Passwort ein.

```bash
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
```


### Lösung

Im Level eingeloggt kannst du dich wieder auf die Suche nach dem Passwort machen.

In deinem `HOME`-Verzeichnis findest du eine `ELF`-Datei.

---

#### Mehr über ELF-Dateien erhältst du hier in diesem kurzen Exkurs:
<details><summary>Exkurs: ELF-Dateien</summary>

### Was ist eine ELF-Datei?
ELF steht für **Executable and Linkable Format**.
Es ist das Standard-Dateiformat für ausführbare Programme, Objektdateien, Shared Libraries und Core Dumps auf Unix-ähnlichen Systemen (Linux, BSD, etc.).

### Aufbau einer ELF-Datei
Eine ELF-Datei besteht aus mehreren Abschnitten:
- **Header**: Enthält grundlegende Infos (Typ, Architektur, Einstiegspunkt).
- **Program Header Table**: Beschreibt, wie Segmente in den Speicher geladen werden.
- **Sections**: Enthalten Code, Daten, Symbole, Strings usw.
- **Section Header Table**: Enthält Metadaten zu den Sections.

### Typische ELF-Typen
- **Executable (ET_EXEC)**: eigenständige Programme.
- **Relocatable (ET_REL)**: Objektdateien zum Linken.
- **Shared Object (ET_DYN)**: Bibliotheken (*.so).
- **Core Dump (ET_CORE)**: Speicherabbild nach einem Absturz.

### Wichtige Tools
- `file <binary>` → zeigt Art (z. B. *ELF 32-bit LSB executable*).  
- `readelf -h <binary>` → listet Header-Infos.  
- `strings <binary>` → zeigt lesbare Zeichenketten.  
- `objdump -d <binary>` → Disassembly des Codes.  

### Warum ist das wichtig?
In CTFs und Wargames wie **Leviathan** sind ELF-Binaries häufig das Ziel.  
Durch Analyse kannst du:
- Hardcodierte Passwörter finden (`strings`, `ltrace`).  
- Das Verhalten des Programms verstehen.  
- SUID-Programme untersuchen, die Privilegien weitergeben.  


***EXKURS ENDE***
</details>
 
 ---

Mit dem Befehl `ll` siehst du nun die Datei `check` und eine ausführliche Information über die Berechtigungen.

```text
-r-sr-x---  1   leviathan2 leviathan1 15084 Aug 13 13:17 check*
```

![Leviathan2 Elf Datei](/10-practice-labs/ressources/pictures/leviathan2.png)


Folgendes ist speziell an dieser Datei:
Das `s` ist der `setuid`-Bit, anstelle eines `x`-Bit. Das bedeutet: Wenn jemand die Datei ausführt, dann läuft sie mit den Rechten von `leviathan2` (Owner). Das kann für eine Privilege Escalation ausgenutzt werden.

**Aufschlüsselung der Berechtigungen:**
- `-r-sr-x---`
    - Das erste Zeichen `-` -> normale Datei (kein Verzeichnis (d), kein Symlink).
    - Danach 9 Berechtigungsstufen: `r-sr-x---`
        - `r-s`
            - `r-s` -> Owner (leviathan2) darf lesen (`r` für read).
            - `s` -> setuid-Bit => vererbt Berechtigung.
        - `r-x` -> Group (leviathan1)
            - Mitglieder der Gruppe durfen lesen und ausführen, aber nicht schreiben.
        - `---` Others (hier keiner)
            - Alle anderen User haben keinerlei Zugriff.
- `leviathan2` -> Owner der Datei
- `leviathan1` -> Gruppe

Du kannst einfach mal die Datei mit `./check` starten und schauen, wie sie sich verhält und was sie dir anzeigt. Probiere es aus, wenn du willst.

Gib im Terminal dazu folgende Befehle ein:

```bash
file check      # Infos über die Datei; bestätigt dir Art/Architektur (z.B. Elf 32-bit LSB executable)
ls -l check     # Berechtigungen der Datei anschauen
ltrace ./check  # zeigt, welche Bibliotheksfunktionen aufgerufen werden (z.B. strcmp, fopen)
```

Vereinfacht gesagt, kannst du mit `ltrace` beobachten, was mit deiner Eingabe passiert.

Gib einfach irgendein Passwort ein.

![Leviathan2 Privilege Escalation](/10-practice-labs/ressources/pictures/leviathan2b.png)

### Privilege Escalation
- `Schritt 1`: Passwort erraten => Eingabe eines x-beliebigen Wortes.
- `Schritt 2`: Die Funktion `strcmp` vergleicht die Eingabe aus `Schritt 1` mit der Eingabe aus dem hardcordierten Vergleichs-String im Binary.
    - wird das Passwort korrekt eingegeben, wird die Datei als `leviathan2` ausgeführt.


### So kommst du ans Passwort

Gib im Terminal nun folgende Befehle ein, nach dem du die Datei `check*` ausgeführt hast.

```bash
# Schritt 1
ltrace ./check  # führt die Datei check aus.
# Sobald ausgeführt, gib folgendes Passwort ein:
sex

whoami          # zeigt dir deinen aktuellen User an.
# Du solltest leviathan1 angezeigt bekommen.

# Schritt 2
# starte nun ./check erneut
./check
# gib das Passwort ein:
sex

# Schritt 3
whoami          # nun solltest du aufgrund der Privilege Escalation leviathan2 sein.

# Passwort auslesen:
cat /etc/leviathan_pass/leviathan2
```

![Leviathan2 Password](/10-practice-labs/ressources/pictures/leviathan2c.png)



### Passwort

Das Passwort "sex" ist nur ein hardcodierter Vergleichs-String im Binary.
Wird er korrekt eingegeben, startet das Programm mit den Rechten von leviathan2.

> Das Passwort zu `leviathan2` lautet `NsN1HwFoyN`. 

Logge dich mit `exit` mehrmals aus, bis du in dem Terminal deiner VM bist.


</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Leviathan 2 -> 3

**Aufgabe:** Finde das Passwort zu `leviathan3`

<details>
    <summary>Lösung</summary>

### Login

```bash
ssh -l leviathan2 leviathan.labs.overthewire.org -p 2223
# Anschließend gibst du das Passwort aus der Lösung des letzten Levels ein
```

### Einleitung

In diesem Level liegt der Fokus auf der Eingabeverarbeitung von Programmen.
Das bereitgestellte Binary (`printfile`) wirkt auf den ersten Blick wie ein einfacher Pager, der nur eine Datei anzeigt. Doch beim genaueren Hinsehen (z. B. mit `ltrace` oder `strings`) wird klar, dass das Programm Eingaben nicht neutral behandelt, sondern sie direkt an externe Befehle weiterreicht.

Die Aufgabe besteht also darin, die **Art der Eingabeprüfung** zu verstehen und auszunutzen. Oft ergeben sich dadurch Möglichkeiten wie Command Injection oder die Umgehung von Prüfungen durch Symlinks oder alternative Pfadangaben.

**Kurz gesagt:**
👉 Hier lernst du, wie wichtig es ist, Programme kritisch zu hinterfragen, die Benutzereingaben an Systembefehle weiterreichen – ein häufiger Sicherheitsfehler in der Praxis.

### Lösung

Verschaffe dir mit `ls` oder `ll` einen Überblick über dein `HOME`-Verzeichnis.
Eine Datei namens `printfile` vom User `leviathan3` ist für uns ausführbar. 

![Leviathan3 Home-Verzeichnis](/10-practice-labs/ressources/pictures/leviathan3.png)

Ich habe `file printfile` nicht eingeben, doch kann sagen, dass es auch eine `ELF 32 Bit Executable" Datei ist, wie im Level zuvor.

Führe die Datei aus und finde heraus, wofür sie geeignet ist:

Die Datei nutzt also einen `filename`. Wofür das wohl gut ist.

Versuchen wir direkt die Datei mit dem Passwort auszulesen. Gib dazu folgende Befehle ein:

```bash
./printfile /etc/leviathan_pass/leviathan3
```

Schade! Wir haben keine Vererbung der Berechtigung des Users erhalten, um an das Passwort zu kommen.

![Leviathan3 Datei ausführen](/10-practice-labs/ressources/pictures/leviathan3b.png)

Schauen wir uns das Ganze mit dem `ltrace`-Befehl an und finden heraus, wie das Programm die Benutzereingabe verarbeitet. 
Teste es mit einer Datei deiner Wahl (wir nutzen die `.bashrc`-Datei)

Gib dazu im Terminal folgenden Befehl ein
```bash
ltrace ./printfile .bashrc
```

![Leviathan3 ltrace Befehl mit einer Datei](/10-practice-labs/ressources/pictures/leviathan3c.png)

Das Programm erhält als erstes die Funktions `access()`. Diese Funktion überprüft, ob der Benutzer (hier leviathan2) berechtigt ist, diese Datei auszuführen.
Die UserID wird über `geteuid()` erhalten und später mit `setreuid` neu gesetzt. Außerdem kannst du erkennen, dass das Programm `/bin/cat` gecallt wird, welche die Datei ausgeben soll.

Was passiert, wenn mehr als eine Datei hinzugefügt wird?

Gib im Terminal folgende Befehl ein, damit du mehr als eine Datei ausgibst:

```bash
ltrace ./printfile .bashrc .bash_logout
```

![Leviathan3 zwei Dateien testen](/10-practice-labs/ressources/pictures/leviathan3d.png)

Die Antwort auf die Frage, ob mehr als eine Datei ausgeführt werden kann, lautet also: Nein!

Versuchen wir mal eine Datei auszuführen, die ein Leerzeichen im Dateinamen enthält.
Dazu musst du zuerst einen Ordner im `/tmp/`-Verzeichnis erstellen und anschließend mit `touch` eine Textdatei mit einem Leerzeichen im Namen, also bspw. "test datei.txt" hinzufügen.

***Ich musste das Verzeichnis und die Datei nachträglich erneut erstellen, da ich ein Problem hatte. Lass dich vom nächsten Bild nicht verirren.***

Gib im Terminal folgende Befehle ein, um ein Verzeichnis und eine Datei darin zu erstellen und es mit dem Programm auszugeben:

```bash
mktemp -d   # erstellt ein Verzeichnis im /tmp/, merke dir den Namen des Verzeichnisses
touch /tmp/tmp.VerzeichnisName/"test datei.txt" # erstellt die Datei
ltrace ./printfile /tmp/tmp.VerzeichnisName/"test datei.txt"
```

![Leviathan3 Datei mit Leerzeichen ausführen](/10-practice-labs/ressources/pictures/leviathan3e.png)

Leerzeichen scheint das Programm nicht zu lesen. Wie können wir das nun für uns nutzen, ist die Frage?

Wenn also die `test datei.txt` mit Leerzeichen nach test nicht mehr liest, könnten wir dann nicht eine Datei (z.B. `/etc/leviathan_pass/leviathan3`) mit einer Datei im `/tmp/tmp.VerzeichnisName` verlinken? 

Probierne wir es aus und erstellen einen sogenannten `Softlink` (symbolische Links), die wir eine Verknüpfung auf dem Desktop agieren. Ein Softlink enthält einen Pfad zu einer anderen Datei oder einem Verzeichniss, statt direkt auf die Daten zuzugreifen. Die Funktionsweise ist einfach: Doppelklick auf die Verknüpfung leitet dich systemseitig direkt zur Zieldatei weiter.

Verlinke die `/etc/leviathan_pass/leviathan3`-Datei im `/tmp/tmp.VerzeichnisName`, verändere die Berechtigungen des Verzeichnisses und führe die Datei `test datei.txt` im Anschluss aus.

**Hinweis:** Das Bild zeigt dir den `touch`-Befehl. Den brauchst du nicht erneut eingeben, da du die Datei bereits erstellt hast.

Gib dazu im Terminal folgende Befehle ein:

```bash
ln -s /etc/leviathan_pass/leviathan3 /tmp/tmp.VerzeichnisName/test # erstellt einen Softlink (-s);
ll /tmp/tmp.VerzeichnisName # zeigt eine detaillierte Liste des Verzeichnisses.
chmod 777 /tmp/tmp.VerzeichnisName # ändert die Berechtigungen des gesamten Verzeichnisses in 3x rwx-Bits
./printfile /tmp/tmp.VerzeichnisName/"test datei.txt"   # Aufgrund der Verarbeitung wird nicht "test datei.txt" ausgegeben, sondern die verlinkte Datei "test"
```

![Leviathan3 Datei Link erstellen](/10-practice-labs/ressources/pictures/leviathan3f.png)


</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Leviathan 3 -> 4

**Aufgabe:** Finde das Passwort zu `leviathan4`

<details>
    <summary>Lösung</summary>

### Login
```bash
ssh -l leviathan3 leviathan.labs.overthewire.org -p 2223
```

### Einleitung

Auch in diesem Level liegt der Fokus auf der Eingabeverarbeitung von Programmen.
Das bereitgestellte Binary (`level3`) wirkt auf den ersten Blick wie eine einfache Passwort abfrage. Doch beim genaueren Hinsehen (z. B. mit `ltrace` oder `strings`) wird klar, dass das Programm Eingaben nicht neutral behandelt, sondern sie mit einem String vergleicht und bei richtiger Eingabe den Zugang zu einer Shell gewährt.

Die Aufgabe besteht darin, die **Art der Eingabeprüfung** zu verstehen und auszunutzen. Oft ergeben sich dadurch Möglichkeiten wie Command Injection oder die Umgehung von Prüfungen durch Symlinks oder alternative Pfadangaben.

### Lösung

Sobald du dich über die `SSH`-Verbindung eingeloggt hast, kannst du dir mit dem Befehl `ls` oder `ll` einen Überblick über dein `HOME`-Verzeichnis verschaffen.

Darin siehst du eine ausführbare ELF-Datei. Führe sie zunächst normal aus, damit du herausfinden kannst, was das für ein Programm ist.
Im Anschluss führst du das Programm mit dem vorangestellten Befehl `ltrace` aus.

Gib im Terminal folgende Befehle ein:
```bash
ll          # Auflistung des aktuellen Verzeichnisses
./level3    # Ausführbare ELF-Datei; Passwortabfrage
# gib irgendein Passwort ein: unser Beispiel "test"

# Nun mit ltrace:
ltrace ./level3 
# gib auch hier irgendein Passwort ein
```

![Leviathan4 ELF Datei testen](/10-practice-labs/ressources/pictures/leviathan4.png)

Im ersten, normalen Durchlauf des Programms konnten wir nicht viel feststellen. Wir wissen, dass das Programm ein Passwort abfragt. Weil das Passwort falsch war, gibt das Programm die Fehlermeldung `bzzzzzzzzap. WRONG` aus.

Mit `ltrace` hingegen erhalten wir mehr Informationen. 

Das Programm startet mit `ltrace` wie gewohnt und du wirst wieder aufgefordert, ein Passwort einzugeben.
Als erstes startet die Funktion `strcmp("h0no33", "kakaka")`, gefolgt von der CL Ausgabe `fgets(Enter the passwort>)`. Sobald ein Passwort eingegeben wird, wird die erste `strcmp("h0no33", "kakaka")` überschrieben und eine neue `strcmp("test","snlprintf\n")` wird aufgebaut.

Diese erste Funktion hat hier keinerlei Auswirkungen und ist nicht von großer Bedeutung, da die zweite Funktion die Eingabe des Passworts testet.

Das heißt, unsere Eingabe mit dem Beispiel "test" war nicht erfolgreich, weil die Funktion `strcmp` den Wert "test" mit dem Wert "snlprintf" vergleicht und feststellt, dass sie nicht gleich sind.

**Wie kannst du das für dich nutzen?**

Da die Eingabe mit der zweiten Funktion strcmp und dem String "snlprint" verglichen wird, geben wir mal das Wort "snlprint" ein (ohne Anführungszeichen) und schauen, was passiert.

Gib im Terminal folgenden Befehl ein:
```bash
ltrace ./level3
# anschließend: snlprintf
```

Wir haben eine Shell! Das heißt, dass unsere Eingabe mit der hart gecodeten `strcmp`-Wert "snlprint" vergleichen wird. Und da wir dem Programm sagen, dass unser Passwort auch `snlprint` ist, erhalten wir dadurch eine Verbindung zur Shell.

Das heißt, wir sind aus der `SSH`-Verbindung "ausgebrochen" und haben einen neuen Zugang außerhalb dieser erhalten.

Führe das Programm in der Shell nun erneut aus. Dies bringt das System durcheinander und du erhältst eine `Privilege Escalation`.

Gib im Terminal folgende Befehle ein, um an das Passwort zu kommen:
```bash
# weiter in der Shell von der ersten Privilege Escalation
./level3
# Passwort: snlprint

whoami  # optional: zeigt, dass du beim zweiten Lauf die PE als User leviathan4 erreicht hast.
cat /etc/leviathan_pass/leviathan4
```

Du solltest das Passwort über die Privilege Escalation nun erhalten und kannst anschließend die Shell-Sitzungen beenden.
Dazu musst du mehrmals den Befehl `exit` eingeben.

![Leviathan4 ELF Datei testen](/10-practice-labs/ressources/pictures/leviathan4b.png)

### Infografik

```text

    erste SSH-Sitzung durch Login des User leviathan3
                        |
                        V
    1. Ausführen des Programms "level3"
    erste Privilege Escalation durch richtiges Passwort
    => Ausbruch aus SSH-Situng in Shell von levithan3
                        |
                        V
    2. Ausführen des Programms "level3" in der Shell
    von Leviathan 3 mit dem gleichen Passwort.
    => Ausbruch aus Shell von leviathan3 in die Shell
    von leviathan4
``` 



</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Leviathan 4 -> 5

**Aufgabe:** Finde das Passwort zu `leviathan5`

<details>
    <summary>Lösung</summary>

### Einleitung

Auch in diesem Level liegt der Fokus auf der Eingabeverarbeitung von Programmen. In einem Verzeichnis findest du das Programm `bin*`. Das `*` zeigt an, dass du es eine ausführbare Datei ist. Auch wieder eine ELF.

### Lösung

Liste die Dateien und Verzeichnisse in deinem `HOME`-Verzeichnis mit dem Befehl `ll` auf, da es eine verstecktes Verzeichnis gibt.
Wechsle mit `cd .trash/` in das Verzeichnis und liste hier erneut auf, was in diese Verzeichnis ist.

Du sist eine ausführbare Datei namens `bin`.
Sobald du das Programm ausführt, erhältst du eine lange binäre Zahlenreihe.

Diese Zahlenreihe hat sicher eine versteckte Botschaft. Nun gilt es, dieses Nachricht in eine für uns menschen lesbare Sprache zu bringen. 

Dafür gibt es mehrere Möglichkeiten. Du könntest ein `Python`-Script schreiben, dass die binären Zahlenfolge in das `ASCII`-Format bringt.
Das Programm `perl` nutzen, auf einer Webseite das binären Zahlenformat in ASCII umwandeln oder die Umrechnung selbst ausrechnen.

Ich zeige dir wie du an Passwort kommst mit einer Internetseite und einmal mit perl.

### Einfachste Methode: Internetseite

Gib im Terminal folgenden Befehl ein, um das Programm zu starten. Gehe dann anschließend auf die Suchmaschine deiner Wahl und suche nach Möglichkeiten, wie du `Binärzahlen` in `ASCII` umwandeln kannst.

Gesamte Befehl, als wärst du frisch eingeloggt:
```bash
ll          # listet dein Home-Verzeichnis auf
cd .trash/  # wechselt in das Verzeichnis
ll          # listet das Verzeichnis auf, in das du gewechselt bist
./bin       # führt das Programm aus

file bin     #optional
ltrace ./bin #optional
```

![Leviathan5 bin-Datei ausführen](/10-practice-labs/ressources/pictures/leviathan5.png)

Kopiere die binäre Zahlenfolge und suche im Internet nach `binary to ascii`. Du solltest schnell fündig werden. Wähle eine Webseite aus und kopiere den Inhalt deiner Zwischenablage in das Eingabefeld und konvertiere sie in das ASCII-Format.

![Leviathan5 bin-Datei ausführen](/10-practice-labs/ressources/pictures/leviathan5b.png)


### Methode mit dem Befehl perl

Die zweite Möglichkeit beinhaltet das Programm `perl`.
Kopiere die binäre Zahlenfolge.

Gib anschließend im Terminal folgenden Befehl ein und achte darauf, dass du die Leerzeichen zwischen den binären Blöcken entfernst:


```bash
echo 01100100HierWeiterDeineZahlenfolge | perl -lpe '$_=pack"B*",$_'
```

**Erklärung des Befehls:**
- `echo 010101`: echo des binären Zahlenformats.
- `|`: Pipen eines anderen Befehls, der an echo angestellt wird.
- `perl`: startet den Perl-Interpreter.
    - `-l`: aktiviert ***line-end-processing***:
        - Entfernt automatisch `\n` am Ende der Eingabezeilen.
        - Hängt nach de rVerarbeitung wieder ein `\n` ab die Ausgabe dran.
    - `-p`: bedeutet ***read-process-print loop***
        - Perl liest jede Eingabzeile in die spezielle Variable `$_`,
        - führt den angegeben Code darauf aus und
        - gibt das Ergebnis automatisch wieder aus.
    - `-e`: führt den angegebenen Perl-Code direkt aus (kein Skript nötig).
    - `$_=pack"B*",$_`:
        - `$_`: Standardvariable in Perl, in der jede Eingabzeile steckt.
        - `pack`: wandlet Daten nach einem bestimmten Template in Binär-/Textform um.
        - `"B*"`: Template für pack. Bedeutet:
            - interpretiere die Eingabe als **Bit-String**, bei dem das **höchstwertige Bit zuerst** gelesen wird (im Gegensatz zu `"b*"`).
            - Beispiel: `"01000001"` wird als **ASCII-Code 65** interpretiert.
        - `$_=`: überscreibt die Eingabezeile mit der konvertierten Zeichenfolge.


![Leviathan5 bin-Datei ausführen](/10-practice-labs/ressources/pictures/leviathan5c.png)



</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Leviathan 5 -> 6

**Aufgabe:** Finde das Passwort zu `leviathan6`

<details>
    <summary>Lösung</summary>

### Einleitung

Auch in diesem Level liegt der Fokus auf der Eingabeverarbeitung von Programmen. In einem Verzeichnis findest du das Programm `leviathan5*`. Das `*` zeigt an, dass du es eine ausführbare Datei ist.

Das Programm sucht nach der Datei `/tmp/file.log` im `/tmp/`-Verzeichnis. Wenn du die Datei ausführst, dann erhältst du die Fehlermeldung, dass die Datei im angegeben Pfad nicht gefunden werden konnte.

Es genügt wohl nicht, nur eine Datei anzulegen und diese mit dem Programm auszuführen. Der Befehl `ltrace` gibt uns Aufschluss darüber, welche Befehle und Funktionen das Programm verarbeitet. Vielleicht kannst du dir daraus etwas herleiten, um eine Privilege Escalation hervorzurufen.

Mit dem `ltrace`-Befehl habe ich bspw. erfahren, dass das Programm folgendermaßen funktioniert:
- `__libc_start_main()` -> Hauptfunkton, um das Programm ausführbar zu machen.
- `fopen("/tmp/file.log", "r")` -> Öffnet die Datei im Verzeichnis, im `read`-Modus.
- `fgetc()` -> Das Zeichenweise (vorzeichenlose Zeichen) lesen von Bytes aus einem Eingabestream, der zuvor mit `fopen()` geöffnet wurde. Return dieses Zeichens als `int`
- `feof()` -> prüft, ob das Ende einer Datei erreicht ist und gibt einen Wert ungleich Null (`true`), wenn Dateiende erreicht (EOF, End-of-File) oder Null (`false`), wenn nicht.
- `fclose()` -> wird verwendet, um einen zuvor geöffneten Datei-Stream zu schließen (leert gleichzeitig den Puffer).
- `getuid()` -> POSIX-Aufruf, der reale Benuter-ID des aufrufenden Prozesses zurückgibt. Ermittelt, ob Benutzer Programm ausführen darf.
- `setuid()` -> Systemaufruf, der Prozessen ermöglicht, die Benutzer-ID (EUID) auf eine andere ID zu setzen (was temporär erhöhte Berechitungen verleiht).
- `unlink()` -> Funktion, um Dateinamen oder einen Link zu einer Datei zu entfernen.


> **Tipp:** Mit `touch` erstellst du eine Datei, ohne sie sofort zu öffnen wie bspw. dem `nano`-Editor.

Die Funktion `unlink()` klingt interessant. 

Was ist, wenn du die Datei `/etc/leviathan_pass/leviathan6` mit dem gesuchten Dateinamen verlinkst? 

### Lösung

Gib im Terminal folgende Befehle ein, um an das Passwort zu kommen:

```bash

ll                  # listet das Verzeichnis aus, in dem du bist (wir starten im Home-Verzeichnis)
./leviathan5        # startet das Programm normal, ohne die Datei /tmp/file.log => Fehler!
ltrace ./leviathan5 # Startet das Programm mit dem ltrace Befehl
touch /tmp/file.log # erstellt die Datei file.log im /tmp/-Verzeichnis

ltrace ./leviathan6 /tmp/file.log   # startet das Programm mit ltrace und der Datei
# Die Datei wird im Anschluss gelöscht!
```

![Leviathan6 Programm erkunden](/10-practice-labs/ressources/pictures/leviathan6.png)

Die Datei ist nun gelöscht, doch wir haben eine Menge an Informationen erhalten, die du bereits vorab in der Einleitung erfahren konntest.

Verlinke die Datei `/etc/leviathan_pass/leviathan6` mit der gesuchten File aus dem Programm. Schau im Anschluss nach, ob die Datei verlinkt wurde:

```bash
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
ll /tmp/file.log
```

![Leviathan6 Passwort-Datei verlinken](/10-practice-labs/ressources/pictures/leviathan6b.png)

![Leviathan6 File auflisten](/10-practice-labs/ressources/pictures/leviathan6c.png)

Die Datei ist erfoglreich verlinkt worden. Probiere die Datei mit dem Programm auszuführen und schau, was passiert.

Gib dazu folgenden Befehl im Termianl ein:

```bash
./leviathan5 /tmp/file.log
```

![Leviathan6 Passwort](/10-practice-labs/ressources/pictures/leviathan6d.png)

Herzlichen Glückwunsch! Der String, der dir ausgegeben wurde, ist das Passwort für `leviathan6`. Du kannst mit `exit` die Verbindung trennen und mit dem nächsten Level weitermachen. 

</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Leviathan 6 -> 7

**Aufgabe:** Finde das Passwort zu `leviathan7`

<details>
    <summary>Lösung</summary>

</details>

---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Erkenntnisse

### Allgemeines
- Mit `file` kann man herausfinden, welche Art/Architektur eine Datei hat.
- `ll` oder `ls -l` zeigt die Berechtigungen einer Datei an.
- `ltrace` zeigt, welche Bibliotheksfunktionen aufgerufen werden (z.B. strcmp, fopen).
- `strings` zeigt die Systemaufrufe (Dateizugriffe, execve, etc.). Hilfreich, wenn Datei etwas außerhalb lädt.
- `SETUID`-Bit für die Vererbung der Berechtigung auf Group/Others.
- `ln` Verlinkungen von Dateien, um Rechte zu umgehen (`-h` Hardlink; `-s` Softlink).

### Funktionen aus Sprachen wie PHP, C, C++
- `__libc_start_main()` -> Hauptfunkton, um das Programm ausführbar zu machen.
- `access()` -> Prüft, ob Prozess Zugriff auf eine Datei hat und welche Berechtigungen dafür nötig sind.
- `strcmp()` -> Vergleicht Strings miteinander. Kann auch Eingabewerte erhalten.
- `fopen()` -> Öffnet eine Datei, "r" für Read, "w" Write, usw., oder bereitet sie vor.
- `fgetc()` -> Das Zeichenweise (vorzeichenlose Zeichen) lesen von Bytes aus einem Eingabestream, der zuvor mit `fopen()` geöffnet wurde. Return dieses Zeichens als `int`
- `feof()` -> Prüft, ob das Ende einer Datei erreicht ist und gibt einen Wert ungleich Null (`true`), wenn Dateiende erreicht (EOF, End-of-File) oder Null (`false`), wenn nicht.
- `fclose()` -> Wird verwendet, um einen zuvor geöffneten Datei-Stream zu schließen (leert gleichzeitig den Puffer).
- `puts()` -> Ausgabe in die CL
- `getuid()` -> POSIX-Aufruf, der reale Benuter-ID des aufrufenden Prozesses zurückgibt. Ermittelt, ob Benutzer Programm ausführen darf.
- `setuid()` -> Systemaufruf, der Prozessen ermöglicht, die Benutzer-ID (EUID) auf eine andere ID zu setzen (was temporär erhöhte Berechitungen verleiht).
- `unlink()` -> Funktion, um Dateinamen oder einen Link zu einer Datei zu entfernen.
- `getchar()` -> Liest ein einzelnes Zeichen von der Standardeingabe (typischerweise Tastatur).
- `snprintf()` -> Gibt die Anzahl der Byte zurück, die in das Array geschrieben werden, ohne das abschließende Nullzeichen (`\0`) zu zählen.
- `setreuid()` -> Ermöglicht es einem Prozess, seine reale Benutzer-ID (`Real UID`) und seine effektive Benutzer-ID (`Effective UID`) gleichzeitig zu ändern.
- `system()` -> Ermöglicht, einen Befehl direkt aus einem Programm heraus an das Betriebssystem zu übergeben und dort ausführen zu lassen
- `atoi()` -> Funktion, die eine Zeichenkette in einen Ganzzahlwert (Integer) umwandelt.
---


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [WikiPedia: Executable and Linking Format](https://de.wikipedia.org/wiki/Executable_and_Linking_Format)
- [LowLevel.eu](https://www.lowlevel.eu/wiki/ELF-Tutorial)
- [Struktur eine ELF Datei von Ange Albertini](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format#/media/File:ELF_Executable_and_Linkable_Format_diagram_by_Ange_Albertini.png)

---

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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
