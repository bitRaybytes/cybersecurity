# Over The Wire: Leviathan

> **ACHTUNG: SPOILER GEFAHR**

## Inhaltsverzeichnis

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

Nach dem Befehl wirst du gefragt, ob du den Fingerprint bestätigen willst. Tippe `yes` ein, um fortzufahren.

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

**Erklärung:** 
- `ll`: Verzeichnis auflisten, auch versteckte Dateien/Verzeichnisse, sowie Berechtigungen
- `cd [PFAD]`: wechselt in den angegebenen Pfad
- `cat`: "Pager", um Inhalte der Dateien auszugeben (cat = concatenate)

</details>

---

## Leviathan 1 -> 2

**Aufgabe:** Finde das Passwort zu `leviathan2`

<details>
    <summary>Lösung</summary>

</details>
