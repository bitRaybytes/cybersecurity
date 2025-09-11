# 💻 Prozesse: Von der Software zum Task

## Inhaltsverzeichnis
- [Die wichtigsten Begriffe](#die-wichtigsten-begriffe)
- [Was ist ein Prozess?](#was-ist-ein-prozess)
- [Prozessmerkmale im Überblick](#prozessmerkmale-im-überblick)
- [Das Prozessmodell](#das-prozessmodell)
- [Haftungsausschluss](#haftungsausschluss)

---

## Die wichtigsten Begriffe
Bevor wir uns mit Prozessen befassen, lass uns einige grundlegende Konzepte klären, die oft verwechselt werden:

- **Programm:** Ein Programm ist die statische Lösung einer Programmieraufgabe, also der Algorithmus und die Befehle, die auf der Festplatte gespeichert sind. Es ist ein passiver Satz von Anweisungen.
- **Prozedur:** Eine Prozedur ist ein Unterprogramm. Sie kehrt nach ihrer Ausführung zum aufrufenden, übergeordneten Programm zurück.
- **Thread:** Ein **Thread** ist ein Ausführungsstrang innerhalb eines Prozesses. Er ist der kleinste Teil eines Programms, der vom Betriebssystem unabhängig ausgeführt werden kann. Ein Prozess kann einen oder mehrere Threads enthalten.

---

## Was ist ein Prozess?

Ein **Prozess** ist ein **Programm in Ausführung**. Sobald du ein Programm startest, wandelt der Betriebssystemkern es in einen Prozess um. Manchmal wird ein Prozess auch als **Task** oder **Prozessinstanz** bezeichnet.

Jeder Prozess hat seinen eigenen Speicherbereich und seine eigenen Ressourcen, die vom Betriebssystem verwaltet werden. Wenn ein Programm mehrere Dinge gleichzeitig erledigen muss (z. B. eine Benutzeroberfläche anzeigen und gleichzeitig Daten verarbeiten), erstellt es mehrere Threads.

**Beispiel:** Du öffnest deinen Webbrowser. Das Programm (die Software auf der Festplatte) wird zu einem Prozess im Speicher. Innerhalb dieses Prozesses können mehrere Threads laufen, z. B. ein Thread für das Laden der Webseite, ein anderer für die Wiedergabe eines Videos und ein dritter für das Scrollen.

---

## Prozessmerkmale im Überblick
Um einen Prozess zu identifizieren und zu verwalten, weist das Betriebssystem ihm bestimmte Merkmale zu:

- **Handles:** Dies sind Referenzen auf Ressourcen, die der Prozess vom Betriebssystem erhalten hat, z. B. offene Dateien, Registry-Schlüssel oder Pipes.
- **CPUs:** Die kumulierte Prozessorzeit, die ein Prozess seit seinem Start in Anspruch genommen hat.
- **PM(K) & WS(K):**
    - **PM(K)** (**P**aged **M**emory): Der Speicher, der bei Engpässen auf die Festplatte ausgelagert werden kann.
    - **WS(K)** (**W**orking **S**et): Der Teil des physikalischen RAMs (Arbeitsspeicher), den der Prozess aktuell belegt.
- **ID:** Eine eindeutige Nummer (**Process ID** oder `PID`), die jeder Prozess erhält.
- **SI** (**S**ession **I**D): Gibt die Sitzung an, in der der Prozess läuft. `ID 0` steht für Systemprozesse, `ID 1` für den ersten angemeldeten Benutzer usw.

In der **PowerShell** kannst du diese Informationen selbst abrufen:
```powershell
get-process | sort cpu -descending | select -first 10
```

Dieser Befehl listet die 10 Prozesse mit dem höchsten CPU-Verbrauch auf.

## Das Prozessmodell
Ein Prozess ist die grundlegende Einheit der Ausführung. Es können sich zwar mehrere Prozesse gleichzeitig im Speicher befinden, aber auf einem System mit einer einzigen CPU ist immer nur ein Prozess aktiv. Das Betriebssystem verwaltet die Prozessorzeit und teilt sie den Prozessen zu.

- **Eigenschaften von Prozessen:**
    - Jeder Prozess hat eine eigene, vom Betriebssystem verwaltete Umgebung.
    - Prozesse können andere Prozesse erzeugen (Eltern- und Kindprozesse).
    - Prozesse können miteinander kommunizieren und voneinander abhängen (kooperierende Prozesse).
    - Prozessen kann eine **Priorität** zugewiesen werden, die bestimmt, wie schnell sie die CPU erhalten.
    - Das Betriebssystem speichert alle Informationen über die Prozesse in einer **Prozesstabelle**.

## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Prozess_(Informatik)](https://de.wikipedia.org/wiki/Prozess_(Informatik))

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

---