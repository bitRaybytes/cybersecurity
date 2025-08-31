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

---

## Leviathan 0 -> 1

**Aufgabe:** Finde das Passwort zu `leviathan1`

<details>
    <summary>Lösung</summary>

Als erstes müssen wir uns über `SSH` mit dem Benutzernamen und Passwort einloggen.
Gib dazu in deinem Terminal folgenden Befehl ein:

```bash
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
```

Nach dem Befehl wirst du gefragt, ob du die Verbindung bestätigen willst. Tippe `yes` ein, um fortzufahren.

![leviathan1 Zugang](/10-practice-labs/ressources/pictures/leviathan1.png)


Erfolgreich eingeloggt, sehen wir die Shell des Users `leviathan0` auf dem Host `@leviathan`.
Damit haben wir nun Zugriff auf die Daten des Users und können uns auf die Suche des Passworts machen.

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

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 1 -> 2

**Aufgabe:** Finde das Passwort zu `leviathan2`

<details>
    <summary>Lösung</summary>

Logge dich nun mit dem User `leviathan1` und dem dazugehörigen Passwort ein.

```bash
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
```

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

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 2 -> 3

**Aufgabe:** Finde das Passwort zu `leviathan3`

<details>
    <summary>Lösung</summary>

</details>

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 3 -> 4

**Aufgabe:** Finde das Passwort zu `leviathan4`

<details>
    <summary>Lösung</summary>

</details>

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 4 -> 5

**Aufgabe:** Finde das Passwort zu `leviathan5`

<details>
    <summary>Lösung</summary>

</details>

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 5 -> 6

**Aufgabe:** Finde das Passwort zu `leviathan6`

<details>
    <summary>Lösung</summary>

</details>

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Leviathan 6 -> 7

**Aufgabe:** Finde das Passwort zu `leviathan7`

<details>
    <summary>Lösung</summary>

</details>

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Erkenntnisse

- Mit `file` kann man herausfinden, welche Art/Architektur eine Datei hat.
- `ll` oder `ls -l` zeigt die Berechtigungen einer Datei an.
- `ltrace` zeigt, welche Bibliotheksfunktionen aufgerufen werden (z.B. strcmp, fopen).
- `string` zeigt die Systemaufrufe (Dateizugriffe, execve, etc.). Hilfreich, wenn Datei etwas außerhalb lädt.
- `SETUID`-Bit für die Vererbung der Berechtigung auf Group/Others

---

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

## Nützliche Links
- [WikiPedia: Executable and Linking Format](https://de.wikipedia.org/wiki/Executable_and_Linking_Format)
- [LowLevel.eu](https://www.lowlevel.eu/wiki/ELF-Tutorial)
- [Struktur eine ELF Datei von Ange Albertini](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format#/media/File:ELF_Executable_and_Linkable_Format_diagram_by_Ange_Albertini.png)
