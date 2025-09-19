# 💥 Buffer-Overflow: Grundlagen und Angriffsvektoren
## Inhaltsverzeichnis
- [Was ist ein Buffer-Overflow?](#was-ist-ein-buffer-overflow)
- [Wie funktioniert ein Buffer-Overflow?](#wie-funktioniert-ein-buffer-overflow)
- [Heap-Overflows](#heap-overflows)
- [Angriffsarten](#angriffsarten)
- [Auswirkungen eines Buffer-Overflow](#auswirkungen-eines-buffer-overflow)
- [Schutzmaßnahmen](#schutzmaßnahmen)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#nützliche-links)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Was ist ein Buffer-Overflow?

Ein **Buffer-Overflow** (deutsch: Pufferüberlauf) ist eine Schwachstelle in einem Programm, die auftritt, wenn ein Programm versucht, mehr Daten in einen reservierten Speicherbereich (den **Puffer** oder **Buffer**) zu schreiben, als dieser aufnehmen kann.  
Die überschüssigen Daten "laufen über" und überschreiben benachbarte Speicherbereiche, was zu unvorhersehbarem Verhalten, Programmabstürzen oder im schlimmsten Fall zur Ausführung von bösartigem Code führt.

Diese Schwachstelle tritt meistens in Programmiersprachen wie `C` und `C++` auf, da sie keine automatische Überprüfung der Puffergröße während des Schreibvorgangs vornehmen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Wie funktioniert ein Buffer-Overflow?
### Analogie
Stell dir einen Puffer wie einen kleinen Eimer vor, der nur eine bestimmte Menge Wasser fassen kann.

- **Normaler Ablauf:** Wenn du genau die richtige Menge Wasser in den Eimer gießt, bleibt alles ordentlich und innerhalb des Eimers.

- **Buffer-Overflow:** Wenn du zu viel Wasser einfüllst, läuft es über den Rand und benetzt den Boden oder benachbarte Gegenstände.

In der Informatik sind diese "benachbarten Gegenstände" wichtige Speicherbereiche wie der **Stack** oder der **Heap**, die das Verhalten eines Programms steuern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Der Angriff auf den Stack
Ein klassischer Buffer-Overflow-Angriff zielt auf den **Stack** ab. Der Stack ist ein Bereich im Speicher, in dem u. a.  lokale Variablen und Funktionsaufrufe gespeichert werden.

**Normaler Stack-Aufbau:**  
```text
+-----------------------+
|      ... Daten ...    |
|-----------------------|
|     Rücksprungadresse |   <- Wichtige Anweisung, wohin das Programm zurückkehren soll
|-----------------------|
|    Lokale Variable    |   <- Dein Puffer z. B. char buffer[16]
|-----------------------|
|       ... Daten ...   |
+-----------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Overflow / Angriff

Ein Angreifer sendet mehr Daten an den Puffer, als dieser fassen kann. Diese überschreiben die Rücksprungadresse auf dem Stack.

```text
+-----------------------------+
|   ... weitere Daten ...     |
|-----------------------------|
|  Überschriebene Adresse --> | <- Rücksprungadresse wird manipuliert
|-----------------------------|
|  Böser Code (Shellcode)     | <- Eingeschleust vom Angreifer
|-----------------------------|
|   sehr langer Input         | 
+-----------------------------+

```

**Ergebnis:** Das Programm springt nicht mehr zurück in sauberen Code, sondern direkt in den vom Angreifer eingeschleusten **Shellcode**.

Wenn die Funktion, die den Puffer verwendet, beendet wird, springt das Programm nicht zur ursprünglichen Rücksprungadresse, sondern zu der neuen, vom Angreifer definierten Adresse. Dies führt zur Ausführung des bösartigen Codes (**Shellcode**), was dem Angreifer oft volle Kontrolle über das System gibt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Heap-Overflows

Neben dem Stack existiert der **Heap** – ein Speicherbereich für dynamische Allokationen (`malloc`, `new`).

Ein **Heap-Overflow** überschreibt Metadaten von Speicherblöcken im Heap und kann so Speicherverwaltungsstrukturen kompromittieren.

Schema:
```text
Heap-Speicher:
+-----------+-----------+-----------+
| Block A   | Block B   | Block C   |
+-----------+-----------+-----------+

Heap-Overflow überschreibt Block B -> Manipulation möglich
```
Heap-Overflows sind oft komplexer, bieten aber mächtige Angriffsmöglichkeiten.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Angriffsarten

### 1. Classic Stack Smashing

- **Ziel:** Rücksprungadresse wird vom Angreifen überschrieben.
- Anfälligkeit typisch in älteren Systemen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Return-to-libc

- Statt Shellcode einzuschleusen, wird die Rücksprungadresse auf vorhandene Bibliotheksfunktionen (z. B. `system("/bin/sh")`) gesetzt.
- **Vorteil:** Funktioniert auch bei aktivem **DEP**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. ROP (Return-Oriented Programming)

- Baut Ketten aus kleinen Code-Schnipseln („Gadgets“), die bereits im Speicher existieren.

- Umgeht Schutzmechanismen wie **DEP** und **ASLR** teilweise.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Heap Spraying

- Angreifer füllt den Heap mit bekannten Mustern und Shellcode, um die Trefferwahrscheinlichkeit bei Sprüngen zu erhöhen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Auswirkungen eines Buffer-Overflow
- **Programmabsturz:** Der einfachste und offensichtlichste Effekt. Die überschriebenen Daten führen zu einem Fehler in der Programmlogik, was den Dienst zum Absturz bringt (Denial of Service).

- **Remote Code Execution (RCE):** Der gefährlichste Fall. Ein Angreifer kann beliebigen Code ausführen, der ihm die Kontrolle über den Computer, das Netzwerk oder sensible Daten ermöglicht.

- **Datenkorruption:** Wichtige Datenstrukturen werden durch die überlaufenden Daten beschädigt, was zu inkonsistenten oder falschen Programmergebnissen führt.

- **Privilege Escalation:** Angreifen nutzen bestimmte Mechanismen aus, um die Rechte zu erhöhen und damit zum `root`-Benutzer werden. Damit fällt es Angreifern einfacher, an Daten zu kommen, die für normale Anwender nicht bestimmt sind.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Schutzmaßnahmen

Entwickler und Systemadministratoren können verschiedene Techniken einsetzen, um Buffer-Overflows zu verhindern oder deren Auswirkungen zu minimieren.

- **Sichere Programmierung:**
    - **Verwende sichere Funktionen:** Ersetze unsichere Funktionen wie `strcpy()` oder `gets()` durch ihre sicheren Pendants wie `strncpy()` oder `fgets()`, die die Größe des Zielpuffers überprüfen.
    - **Führe Validierungen durch:** Überprüfe immer die Länge der Eingabedaten, bevor du sie in einen Puffer schreibst.

- **Compiler-Schutz:**
    - **Stack Canaries:** Der Compiler fügt einen zufälligen Wert (den Canary) zwischen dem Puffer und der Rücksprungadresse ein. Wenn dieser Wert bei der Funktionsbeendigung verändert wurde, weiß das System, dass ein Überlauf stattgefunden hat und bricht das Programm ab.
    
- **Betriebssystemschutz:**
    - **Adress Space Layout Randomization (ASLR):** Dieses Sicherheitsmerkmal randomisiert die Speicheradressen von Schlüsselbereichen eines Prozesses. Dadurch wird es für einen Angreifer viel schwieriger, die genaue Adresse zu erraten, zu der er den Programmfluss umleiten muss.
    - **Data Execution Prevention (DEP):** Markiert bestimmte Speicherbereiche als nicht ausführbar. Das bedeutet, dass der Stack-Speicher, in den der Angreifer seinen bösartigen Code schreibt, nicht ausgeführt werden kann.
    - **NX-Bit:** Hardwareunterstützung, um Speicherbereiche als non-executable zu markieren.

- **Zusätzliche Schutzmechanismen:**
    - **WAF / IDS:** Für Webanwendungen, die unsicheren Code abfangen.
    - **Code Reviews & Fuzzing:** Frühes Erkennen von Überläufen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Puffer%C3%BCberlauf](https://de.wikipedia.org/wiki/Puffer%C3%BCberlauf)
- [OWASP: https://owasp.org/www-community/vulnerabilities/Buffer_Overflow](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
- [YouTube: Computerphile - Running  Buffer Ovrflow Attack (https://www.youtube.com/watch?v=1S0aBV-Waeo)](https://www.youtube.com/watch?v=1S0aBV-Waeo)

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