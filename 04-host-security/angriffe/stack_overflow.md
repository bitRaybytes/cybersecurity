# 📝 Stack Overflow

## Inhaltsverzeichnis
- [Stack oder Buffer Overflow?](#stack-oder-buffer-overflow)
- [Wie sieht das im Speicher aus?](#wie-sieht-das-im-speicher-aus)
- [Unterschiede & Zusammenhänge](#unterschiede--zusammenhänge)
- [Zusammenfassung](#zusammenfassung)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Stack oder Buffer Overflow?
In der Welt der Cybersicherheit werden die Begriffe **Buffer Overflow** und **Stack Overflow** oft synonym verwendet, was aber technisch nicht korrekt ist. Um es einfach zu sagen: Ein **Buffer Overflow** ist ein Angriff oder eine Schwachstelle, während ein **Stack Overflow** das Symptom oder die Folge einer solchen Schwachstelle sein kann.

Ein **Buffer Overflow** kann an verschiedenen Stellen im Speicher auftreten, zum Beispiel auf dem **Stack** oder dem **Heap**. Ein **Stack Overflow** hingegen ist immer eine spezifische Art von Speicherüberlauf, die ausschließlich auf dem Stack stattfindet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist ein Stack Overflow?
Der Stack ist ein spezieller Bereich im Speicher, der für lokale Variablen, Parameter und Rücksprungadressen von Funktionen genutzt wird. Er wächst und schrumpft dynamisch mit jedem Funktionsaufruf.

Ein **Stack Overflow** tritt ein, wenn mehr Speicher benötigt wird, als für den Stack vorgesehen ist. Das kann auf zwei Arten passieren:

1. **Durch einen Buffer Overflow:** Bei dem ein Angreifer absichtlich einen Puffer auf dem Stack mit zu vielen Daten füllt, um die Rücksprungadresse zu manipulieren bzw. zu überschreiben.

2. **Durch unendliche Rekursion oder viele Funktionsaufrufe:** Jeder Funktionsaufruf reserviert Speicherplatz auf dem Stack. Wenn eine Funktion sich selbst immer wieder aufruft, ohne zu stoppen, füllt sich der Stack und läuft irgendwann über.

In beiden Fällen wird der zugewiesene Speicher auf dem Stack überschritten, was einen Programmabsturz oder, bei einem gezielten Angriff, die Ausführung von bösartigem Code zur Folge hat.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Beispiel (Pseudo-C):
```c
void recurse() {
    recurse(); // Endlosschleife durch Rekursion, weil Funktion sich selbst aufruft.
}

int main() {
    recurse();
    return 0;
}
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wie sieht das im Speicher aus?

Ein Stack ist in Schichten organisiert – wie ein Stapel Teller. Jeder neue Funktionsaufruf legt einen neuen „Teller“ oben drauf.



### Normaler Stack-Aufbau:
```text
+-------------------+
| Rücksprungadresse |
+-------------------+
| Lokale Variablen  |
+-------------------+
| Parameter         |
+-------------------+
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Stack mit Überlauf (zu viele Aufrufe/Daten)
```text
+-------------------+
| Rücksprungadresse |   <- überschrieben
+-------------------+
| Lokale Variablen  |   <- überfüllt
+-------------------+
| Parameter         |
+-------------------+
|  ... Überlauf ... |   <- neuer Speicher wird verdrängt
+-------------------+
```
In so einem Fall kann das Programm entweder abstürzen (**Segmentation Fault**) oder – bei einem gezielten Angriff – fremden Code ausführen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Unterschiede & Zusammenhänge
Um die Beziehung zwischen den beiden Konzepten zu verdeutlichen, betrachte die folgende Tabelle:

| Aspekt | Buffer Overflow | Stack Overflow |
|--------|-----------------|----------------|
| **Definition** | Eine Schwachstelle, die das Überschreiben eines Puffers ermöglicht. | Ein Zustand, bei dem der Stack-Speicher überfüllt ist. |
| **Ursache** | Schreiben von zu vielen Daten in einen Puffer. | 1. Ein Buffer Overflow auf Stack; 2. Ein Programmierfehler (z.B. endlose Rekursion) |
| **Ort** | Kann auf dem **Stack**, **Heap** oder in anderen Speichern auftreten. | Tritt ausschließlich auf dem **Stack** auf. |
| **Klassifikation** | Ist ein **Angriffsvektor** oder einen Schwachstelle. | Ist ein **Fehlerzustand** oder das Sympton eines Angriffs. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Zusammenfassung

- Ein **Buffer Overflow** ist ein Angriffsvektor – ein gezieltes Überschreiben eines Puffers.
- Ein **Stack Overflow** ist ein Fehlerzustand, der fast immer zum Programmabsturz führt – entweder durch Angriffe oder durch fehlerhafte Programmierung.
- Während Buffer Overflows gezielt ausgenutzt werden können, sind Stack Overflows oft „Nebenwirkungen“ davon.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Stapel%C3%BCberlauf](https://de.wikipedia.org/wiki/Stapel%C3%BCberlauf)

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