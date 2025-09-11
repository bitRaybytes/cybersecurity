# 🚦 Prozesszustände: Der Lebenszyklus eines Tasks

## Inhaltsverzeichnis
- [Die drei Hauptzustände](#die-drei-hauptzustände)
- [Zustandswechsel im Detail](#zustandswechsel-im-detail)
    - [Dispatch](#dispatch-bereit---aktiv)
    - [Timer-Runout](#timer-runout-aktiv---bereit)
    - [Block](#block-aktiv---blockiert)
    - [Wakeup](#wakeup-blockiert---bereit)
- [Prozesshierarchie](#prozesshierarchie)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



## Die drei Hauptzustände
Während seiner Abarbeitung durchläuft ein Prozess verschiedene Zustände. Für ein System mit einer einzigen CPU sind diese Zustände:

* **Aktiv (Running):** Der Prozess wird gerade von der CPU ausgeführt.
* **Bereit (Ready):** Der Prozess ist bereit, die CPU zu nutzen, aber ein anderer Prozess ist gerade aktiv. Er wartet in einer Warteliste.
* **Blockiert (Blocked):** Der Prozess kann nicht ausgeführt werden, weil er auf das Eintreten eines bestimmten Ereignisses wartet. Zum Beispiel wartet er auf eine Benutzereingabe, das Fertigstellen eines Druckauftrags oder das Laden einer Datei.  

```text
                   +-----------+                       +------------+
                   |   Neu     |                       |   Aktiv    |
                   +-----------+                       +------------+
                         |                                    |  ^
                         |                                    |  |
                         v                                    v  |
                   +-----------+                         +------------+
                   |   Bereit  |------------------------>|  Blockiert |
                   +-----------+                         +------------+
                       ^                                          |
                       |                                          |
                       +------------------------------------------+
```

**Merke:** Ein Prozess kann nur dann „aktiv“ sein, wenn er die CPU gerade besitzt.

## Zustandswechsel im Detail
Der Wechsel zwischen den Zuständen wird von bestimmten Ereignissen ausgelöst.

### Dispatch (bereit -> aktiv):
Der Scheduler wählt einen Prozess aus der Bereit-Warteliste aus und weist ihm die CPU zu. Dieser Prozesswechsel wird auch als Kontextwechsel bezeichnet, da der Zustand des vorherigen Prozesses gespeichert und der neue Zustand geladen werden muss.

### Timer-Runout (aktiv -> bereit):
Die zugewiesene Rechenzeit (Zeitscheibe) eines Prozesses ist abgelaufen. Das Betriebssystem entzieht dem Prozess die CPU und setzt ihn wieder auf die Bereit-Liste. Dies ist eine Form des verdrängenden Multitaskings.

Dieses Prinzip heißt **präemptives Multitasking**.

### Block (aktiv -> blockiert):
Ein Prozess benötigt eine Ressource, die momentan nicht verfügbar ist (z. B. ein I/O-Gerät). Er fordert die Ressource an und wechselt in den blockierten Zustand, um die CPU für andere Prozesse freizugeben. Dies ist der einzige Zustandswechsel, den ein Prozess selbst auslösen kann.

### Wakeup (blockiert -> bereit):
Das Ereignis, auf das der blockierte Prozess gewartet hat, ist eingetreten (z. B. die Datei ist geladen, die Benutzereingabe ist erfolgt). Der Prozess wird auf die Bereit-Liste verschoben und kann wieder auf die CPU warten.

**Vereinfachte Darstellung eines Prozess-Lebenszyklus:**  
```text
                +-----------+
                |   Neu     |
                +-----------+
                      |
                      v
                +-----------+      CPU-Zeit        +-----------+
                |  Bereit   | ------------------>  |   Aktiv   |
                +-----------+                      +-----------+
                      ^                                 |
                      |                                 |
             Wakeup   |                                 |   Block
   (Ereignis fertig)  |                                 | (I/O warten)
                      |                                 v
                +-----------+ <------------------- +-----------+
                | Blockiert |                      |  Aktiv    |
                +-----------+                      +-----------+

```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Prozesshierarchie
Wenn du ein Programm startest, erzeugt das Betriebssystem einen Prozess. Dieser Prozess kann wiederum neue Prozesse erstellen, die als Kindprozesse (Child Processes) bezeichnet werden. Der ursprüngliche Prozess ist der Elternprozess (Parent Process).

- **Elternprozess (Parent Process):** Der ursprüngliche Prozess.

- **Kindprozesse (Child Processes):** Prozesse, die vom Elternprozess erzeugt wurden.


- Wenn ein Elternprozess beendet wird, werden in der Regel auch alle seine Kindprozesse beendet.

```text
Systemprozess (PID 1)
    ├── sshd (PID 102)
    │      ├── bash (PID 245)
    │      │     └── vim (PID 301)
    │      └── bash (PID 260)
    └── apache2 (PID 120)
           ├── apache2 worker (PID 200)
           └── apache2 worker (PID 201)

```

Dieses Konzept der Hierarchie ist wichtig für die Organisation und Verwaltung von Prozessen im Betriebssystem.

**Wichtig:**

- Jeder Kindprozess hat genau einen Elternprozess.

- Ein Elternprozess kann mehrere Kinder haben.

- Beendet sich ein Elternprozess, übernimmt oft **init**/**systemd** (PID 1) als „Adoptivelternteil“ die verwaisten Prozesse.


## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Prozess_(Informatik)#Prozesszust%C3%A4nde](https://de.wikipedia.org/wiki/Prozess_(Informatik)#Prozesszust%C3%A4nde)
- [Microsoft Windows Commands - Tasklist: https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)

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

🗓️ **Letzte Aktualisierung:** Septmeber 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---