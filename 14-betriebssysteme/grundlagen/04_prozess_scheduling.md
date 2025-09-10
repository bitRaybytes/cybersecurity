# ⚙️ Prozess-Scheduling: Die Aufgabenverteilung der CPU

## Inhaltsverzeichnis
- [Was ist Prozess-Scheduling?](#was-ist-prozess-scheduling)
- [Grundsysteme des Schedulings](#grundsysteme-des-schedulings)
- [Scheduling-Strategien in der Praxis](#scheduling-strategien-in-der-praxis)
- [Nützliche Links]()
- [Haftungsausschluss](#haftungsausschluss)

---

## Was ist Prozess-Scheduling?
In Multitasking-Betriebssystemen gibt es oft mehr Prozesse im Zustand "bereit", als CPUs verfügbar sind. Die Aufgabe des **Schedulers** ist es, aus den bereiten Prozessen den nächsten auszuwählen, dem die CPU zugeteilt wird.

Ein guter Scheduler verfolgt mehrere Ziele:

* **Gerechtigkeit:** Jeder Prozess erhält einen "gerechten" Anteil an der Rechenzeit.
* **Effizienz:** Die CPU soll möglichst keine Leerlaufzeiten haben, wenn Prozesse bereitstehen.
* **Antwortzeit:** Interaktive Prozesse sollen schnell reagieren, um eine gute Benutzererfahrung zu gewährleisten.
* **Durchsatz:** Das System soll so viele Aufträge wie möglich in einem bestimmten Zeitraum abarbeiten.

---

## Grundsysteme des Schedulings
Es gibt zwei grundlegende Ansätze, wie die CPU den Prozessen zugeteilt wird:

* **Kooperatives Multitasking (Non-Preemptive):** Der aktive Prozess gibt die CPU von sich aus frei. Es ist wenig Verwaltungsaufwand nötig, aber ein fehlerhafter oder "unkooperativer" Prozess kann das gesamte System blockieren, da er die CPU nicht freigibt.
* **Verdrängendes Multitasking (Preemptive):** Der Scheduler kann einem Prozess die CPU jederzeit entziehen, um einem anderen, wichtigeren Prozess die Ausführung zu ermöglichen. Dies ist bei modernen Betriebssystemen der Standard, da es die Systemstabilität und die Bearbeitung dringlicher Aufgaben sicherstellt.

---

## Scheduling-Strategien in der Praxis
Die Auswahl des nächsten Prozesses aus der Bereit-Warteliste basiert auf verschiedenen Strategien:

* **Wer zuerst kommt, wird zuerst bedient (FCFS - First Come, First Served):** Die Prozesse werden in der Reihenfolge ihrer Ankunft abgearbeitet.
* **Zeitscheibenverfahren (Round Robin):** Jeder Prozess erhält eine feste, kurze Zeitspanne (Zeitscheibe). Nach Ablauf dieser Zeitspanne wird er verdrängt und in die Warteschlange zurückgestellt, sodass der nächste Prozess an der Reihe ist. Dies sorgt für eine gute Antwortzeit.
* **Prioritätssteuerung:** Jeder bereite Prozess hat eine Priorität. Die CPU wird dem Prozess mit der höchsten Priorität zugeteilt. Ein neuer Prozess mit höherer Priorität kann einen aktiven Prozess mit niedrigerer Priorität sofort verdrängen.

**Dynamische Prioritätsvergabe:** Die Priorität eines Prozesses kann sich während seiner Laufzeit ändern. Zum Beispiel kann die Priorität eines Prozesses, der lange auf die CPU gewartet hat, erhöht werden, um ihn schneller zu bedienen.

## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Prozess_(Informatik)#Prozess-Scheduling](https://de.wikipedia.org/wiki/Prozess_(Informatik)#Prozess-Scheduling)
- [Wikipedia: https://de.wikipedia.org/wiki/Prozess-Scheduler](https://de.wikipedia.org/wiki/Prozess-Scheduler)

## Haftungsausschluss
Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z.B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung. Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>