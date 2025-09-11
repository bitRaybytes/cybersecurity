# 💬 Interprozesskommunikation (IPC) und Synchronisation

## Inhaltsverzeichnis
- [Warum ist Kommunikation wichtig?](#warum-ist-kommunikation-wichtig)
- [Methoden der Interprozesskommunikation](#methoden-der-interprozesskommunikation)
- [Das Problem der Prozess-Synchronisation](#das-problem-der-prozess-synchronisation)
- [Haftungsausschluss](#haftungsausschluss)



## Warum ist Kommunikation wichtig?
In Multitasking-Betriebssystemen müssen Prozesse, die quasi parallel ablaufen, häufig miteinander kommunizieren und sich synchronisieren.  
Die Gründe dafür sind vielfältig:

* **Zusammenarbeit:** Komplexe Softwaresysteme bestehen oft aus mehreren kooperierenden Prozessen, die aufeinander abgestimmt werden müssen.
* **Datenaustausch:** Häufig müssen Daten von einem Prozess zum anderen übertragen werden.
* **Ressourcen-Management:** Prozesse greifen auf begrenzte Ressourcen wie Drucker oder Dateien zu, was eine Synchronisation erfordert, um Konflikte zu vermeiden.



## Methoden der Interprozesskommunikation (IPC)
Das Betriebssystem stellt verschiedene Methoden bereit, damit Prozesse miteinander kommunizieren können:

- **Gemeinsame Speicherbereiche:** Prozesse können auf dieselben Variablen oder Datenbereiche im Speicher zugreifen. Diese Methode ist sehr schnell, erfordert aber eine sorgfältige Synchronisation, um Datenverluste zu vermeiden.
- **Pipes:** Unidirektionale Datenkanäle zwischen zwei Prozessen. Ein Prozess schreibt Daten am Ende des Kanals hinein, der andere liest sie am Anfang wieder aus. Pipes werden oft im Speicher oder als Dateien realisiert.
- **Dateien:** Prozesse können Daten in Dateien schreiben, die von anderen Prozessen gelesen werden können. Dies ist eine einfache, aber oft langsame Form der Kommunikation.
- **Signale:** Signale sind asynchrone Ereignisse, die einen Prozess unterbrechen (wie ein Software-Interrupt). Sie werden vom Betriebssystem, von einem anderen Prozess oder durch einen Programmfehler ausgelöst.
- **Nachrichten (Message Queues):** Prozesse senden und empfangen Nachrichten über eine vom Betriebssystem verwaltete "Mailbox". Die Kommunikation erfolgt über spezielle Systemaufrufe.
- **Streams & Sockets:** Logisch gesehen haben Streams eine ähnliche Funktion wie Pipes, ermöglichen aber die Kommunikation über Rechnernetze hinweg.
- **Prozedurfernaufrufe (RPC):** Ein Prozess kann eine Prozedur aufrufen, die sich in einem anderen Prozess befindet, auch über Netzwerkadressgrenzen hinweg. Dies ist besonders nützlich für **Client-Server-Beziehungen** (Microservices).



## Das Problem der Prozess-Synchronisation
Datenverluste oder inkonsistente Zustände können auftreten, wenn mehrere Prozesse gleichzeitig auf eine gemeinsame Ressource zugreifen. Dies wird als **Race Condition** bezeichnet.

**Beispiel:**
Stell dir vor, Prozess A und Prozess B sollen beide eine Datei in eine Spooler-Warteschlange eintragen. Beide lesen gleichzeitig die Adresse des nächsten freien Platzes (z. B. Position 7). Prozess B schreibt seine Datei an Position 7 und erhöht den Zähler auf 8. Bevor Prozess B fertig ist, erhält Prozess A wieder die CPU, schreibt ebenfalls seine Datei an Position 7 und erhöht den Zähler auf 8. Die Datei von Prozess B wurde dadurch unwissentlich überschrieben und geht "verloren".

### Beispiel: Druckspooler

```text
Prozess A: schreibt Datei an Position 7
Prozess B: schreibt ebenfalls Datei an Position 7
=> Datei von B wird überschrieben

   +-----------+          +-----------+
   | Prozess A | ----+--> |           |
   +-----------+     |    |           |
                     v    |   Spool   | --> Fehler
   +-----------+     |    |           |
   | Prozess B | ----+--> |           |
   +-----------+          +-----------+
```

### Lösung: Kritische Abschnitte

- **Semaphore** / **Mutex** steuern den Zugriff.
- Nur ein Prozess darf gleichzeitig in den kritischen Abschnitt.

```text
[ Prozess A ] --lock--> [ kritischer Abschnitt ] --unlock-->
[ Prozess B ] --wait--> [ kritischer Abschnitt ]
```

Um solche Probleme zu vermeiden, müssen Prozesse ihre Zugriffe auf **kritische Abschnitte** – also Codebereiche, die auf gemeinsame Ressourcen zugreifen – synchronisieren.

## Nützliche Links
- [Wikipedia: https://de.wikipedia.org/wiki/Prozess_(Informatik)#Interprozesskommunikation](https://de.wikipedia.org/wiki/Prozess_(Informatik)#Interprozesskommunikation)
- [The GNU C Library (glibc) manual: https://sourceware.org/glibc/manual/](https://sourceware.org/glibc/manual/)


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