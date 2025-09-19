# 💥 Buffer-Overflow: Grundlagen und Angriffsvektoren


## Inhaltsverzeichnis
-  [Was ist ein Buffer-Overflow?](#was-ist-ein-buffer-overflow)
-  [Speicheranordnung eines Programms](#speicheranordnung-eines-programms)
-  [Wie funktioniert ein Buffer-Overflow?](#wie-funktioniert-ein-buffer-overflow)
    -  [Der Angriff auf den Stack](#der-angriff-auf-den-stack)
-  [Endianness: Die Byte-Reihenfolge](#endianness-die-byte-reihenfolge)
-  [Heap-Overflows](#heap-overflows)
-  [Angriffsarten](#angriffsarten)
    -  [1. Classic Stack Smashing](#1-classic-stack-smashing)
    -  [2. Return-to-libc](#2-return-to-libc)
    -  [3. ROP (Return-Oriented Programming)](#3-rop-return-oriented-programming)
    -  [4. Heap Spraying](#4-heap-spraying)
    -  [5. NOP Sled](#5-nop-sled)
-  [Auswirkungen eines Buffer-Overflow](#auswirkungen-eines-buffer-overflow)
-  [Schutzmaßnahmen](#schutzmaßnahmen)
-  [Nützliche Links](#nützliche-links)
-  [Haftungsausschluss](#haftungsausschluss)


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



## Speicheranordnung eines Programms

Um einen Buffer-Overflow zu verstehen, muss man die grundlegende Speicheranordnung eines laufenden Programms kennen. Diese ist in verschiedene Segmente unterteilt:

```text
       Hohe Adressen
+-----------------------+ <- Stack (wächst nach unten)
|  Stack (LIFO)         |
|                       |
|  - Lokale Variablen   |
|  - Rücksprungadressen |
+-----------------------+
|                       |
|         Frei          |
|                       |
+-----------------------+ <- Heap (wächst nach oben)
|  Heap (dynamische)    |
|                       |
|  - Speicherallokation |
|  (malloc, new)        |
+-----------------------+
|  .BSS                 | <- Globale, nicht-initialisierte Variablen
|-----------------------|
|  .DATA                | <- Globale, initialisierte Variablen
|-----------------------|
|  .TEXT                | <- Programmcode
+-----------------------+
    Niedrige Adressen
```

Der **Stack** ist der primäre Angriffsvektor für klassische Buffer-Overflows. Er wird für temporäre Daten wie lokale Variablen und Funktionsaufrufe genutzt. Der `Stack` wächst von hohen zu niedrigen Speicheradressen.



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
Der Stack-basierte Buffer-Overflow ist die häufigste und am besten verstandene Art des Überlaufs. Er zielt darauf ab, die Rücksprungadresse (`Return Address`) einer Funktion zu überschreiben.


**Normaler Stack-Aufbau:**  

Wenn eine Funktion aufgerufen wird, wird ein sogenannter Stack Frame erstellt. Dieser Frame enthält unter anderem die Rücksprungadresse, die dem Programm sagt, wohin es nach Beendigung der Funktion zurückkehren soll.

```text
       Hohe Adressen
+------------------------+
|   Funktions-Parameter  |
|------------------------|
|   Rücksprungadresse    | <- Die entscheidende Adresse!
|------------------------|
|   EBP (Base Pointer)   |
|------------------------|
|   Puffer               | <- Unsere anfällige Variable (z.B. char buffer[16])
|------------------------|
|   Weitere lokale Daten |
+------------------------+
    Niedrige Adressen

```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

**Der Overflow Angriff**

Ein Angreifer sendet eine Eingabe, die länger ist als der Puffer. Diese überschüssigen Daten fließen über und überschreiben die benachbarten Speicherbereiche, einschließlich der Rücksprungadresse.

```text
       Hohe Adressen
+-------------------------+
|  Funktions-Parameter    |
|-------------------------|
|  Überschriebene Adresse | -> (Adresse unseres Shellcodes)
|-------------------------|
|  Manipulierter EBP      |
|-------------------------|
|  sehr langer Input      | <- Unser bösartiger Input überschreibt den Puffer
|-------------------------|
|  ... weitere Daten ...  |
+-------------------------+
    Niedrige Adressen

```

**Ergebnis:** Wenn die Funktion beendet wird, springt das Programm nicht mehr zur ursprünglichen Rücksprungadresse, sondern zu der neuen, vom Angreifer definierten Adresse. Dies führt zur Ausführung des bösartigen Codes (**Shellcode**), was dem Angreifer oft volle Kontrolle über das System gibt.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Endianness: Die Byte-Reihenfolge

Die **Endianness** beschreibt die Reihenfolge, in der Bytes im Arbeitsspeicher gespeichert werden. Sie ist entscheidend, um Speicheradressen korrekt zu manipulieren.

- **Little-Endian:** Das am wenigsten signifikante Byte (**Least Significant Byte**, **LSB**) wird an der niedrigsten Speicheradresse gespeichert. Die meisten modernen CPUs (Intel, AMD) verwenden Little-Endian.

    - **Beispiel:** Die Adresse 0x01234567 wird im Speicher als 67 45 23 01 abgelegt.

- **Big-Endian:** Das am meisten signifikante Byte (**Most Significant Byte**, **MSB**) wird an der niedrigsten Speicheradresse gespeichert. Wird oft in Netzwerken und bei älteren Systemen verwendet (z. B. PowerPC).

    - **Beispiel:** Die Adresse 0x01234567 wird im Speicher als 01 23 45 67 abgelegt.

Beim Buffer-Overflow muss der Angreifer die Rücksprungadresse in der korrekten Endianness schreiben, um den Exploit erfolgreich zu machen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Heap-Overflows

Neben dem Stack existiert der **Heap** – ein Speicherbereich für dynamische Allokationen (`malloc`, `new`).

Ein **Heap-Overflow** überschreibt Metadaten von Speicherblöcken im Heap und kann so Speicherverwaltungsstrukturen kompromittieren. Dies ermöglicht es einem Angreifer, eine Kette von Aktionen auszulösen, die letztlich zur Codeausführung führen.

Schema:
```text
Heap-Speicher:
+-----------+-----------+-----------+
| Block A   | Block B   | Block C   |
+-----------+-----------+-----------+

Überlauf von Block A überschreibt Metadaten von Block B.
```
Heap-Overflows sind oft komplexer, bieten aber mächtige Angriffsmöglichkeiten.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Angriffsarten

### 1. Classic Stack Smashing

- **Ziel:** Die Rücksprungadresse wird vom Angreifer überschrieben.
- **Anfälligkeit:** Typisch in älteren Systemen ohne moderne Schutzmechanismen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2. Return-to-libc

- **Methode:** Statt eigenen Shellcode einzuschleusen, wird die Rücksprungadresse auf eine bereits existierende Bibliotheksfunktion (z. B. `system("/bin/sh")`) gesetzt.
- **Vorteil:** Funktioniert auch bei aktivem **Data Execution Prevention** (**DEP**).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3. ROP (Return-Oriented Programming)

- **Methode:** Baut Ketten aus kleinen, existierenden Code-Schnipseln („**Gadgets**“) zusammen, die im Speicher bereits vorhanden sind.

- **Vorteil:** Umgeht Schutzmechanismen wie **DEP** und **ASLR** teilweise oder vollständig.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4. Heap Spraying

- **Methode:** Angreifer füllt den Heap mit großen Mengen an Shellcode, um die Trefferwahrscheinlichkeit zu erhöhen, wenn er eine bestimmte Adresse anspringt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### 5. NOP Sled

- **Methode:** Eine Kette von **NOP-Anweisungen** (`\x90`, No-Operation), die dem Prozessor sagen, er soll "nichts tun" und zur nächsten Anweisung springen.

- **Nutzen:** Platziert man einen NOP Sled vor dem Shellcode, muss der Angreifer die Rücksprungadresse nicht exakt treffen. Jeder Sprung innerhalb des NOP Sleds führt den Programmfluss zum Shellcode. Dies erhöht die Zuverlässigkeit des Exploits bei aktiviertem ASLR.



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