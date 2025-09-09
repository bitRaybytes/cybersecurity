# 🔐 Kryptografie: Eine Einführung
## Inhaltsverzeichnis
- [Einführung](#einführung)
- [1. Symmetrische Verschlüsselung](#1-symmetrische-verschlüsselung)
- [2. Asymmetrische Verschlüsselung (Public Key)](#2-asymmetrische-verschlüsselung-public-key)
- [3. Hashing-Algorithmen](#3-hashing-algorithmen)
- [Zusammenfassung: Die Rolle der Kryptografie](#zusammenfassung-die-rolle-der-kryptografie)
- [Haftungsausschluss](#haftungsausschluss)

## Einführung
Kryptografie ist die Wissenschaft der sicheren Kommunikation in Anwesenheit von Gegnern. Im Kern geht es darum, Informationen so zu verbergen, dass nur autorisierte Parteien sie verstehen können. Sie ist ein fundamentales Werkzeug, um die **Vertraulichkeit** und **Integrität** von Daten zu gewährleisten, zwei Schlüsselprinzipien der CIA-Triade.

Die modernen Methoden der Kryptografie lassen sich in zwei Hauptkategorien unterteilen: symmetrische und asymmetrische Verschlüsselung, ergänzt durch die wichtigen Hashing-Algorithmen.

## 1. Symmetrische Verschlüsselung
Bei der **symmetrischen Verschlüsselung** wird derselbe geheime Schlüssel sowohl für die Ver- als auch für die Entschlüsselung von Daten verwendet. Es ist, als ob zwei Personen ein Vorhängeschloss und einen einzigen, identischen Schlüssel teilen.

### Funktionsweise:

1. Sender und Empfänger einigen sich auf einen geheimen Schlüssel.
2. Der Sender nutzt den Schlüssel, um eine Nachricht zu verschlüsseln (Klartext wird zu Chiffretext).
3. Der Empfänger verwendet denselben Schlüssel, um den Chiffretext wieder in Klartext zu entschlüsseln.

- **Vorteile:** Sie ist sehr schnell und effizient, weshalb sie oft für die Verschlüsselung großer Datenmengen verwendet wird.

- **Nachteile:** Der sichere Austausch des Schlüssels ist eine große Herausforderung. Wenn der Schlüssel kompromittiert wird, können alle damit verschlüsselten Nachrichten entschlüsselt werden.

### Visualisierung: Symmetrische Verschlüsselung
```yaml
+-------------+         (Geheimer Schlüssel)           +-------------+
|    Alice    | <------------------------------------> |     Bob     |
| (Sender)    |                                        | (Empfänger) |
+-------------+                                        +-------------+
       |                                                    ^
       |---------[ Verschlüsseln mit Schlüssel ]----------->|
       |                                                    |
       |<--------[ Entschlüsseln mit Schlüssel ]------------|
       |                                                    |
+------v------+                                      +------+------+
| Klartext    |                                      | Chiffretext |
+-------------+                                      +-------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 2. Asymmetrische Verschlüsselung (Public Key)

Die **asymmetrische Verschlüsselung** verwendet ein Schlüsselpaar: einen **öffentlichen Schlüssel** (Public Key) und einen **privaten Schlüssel** (Private Key). Der öffentliche Schlüssel kann mit jedem geteilt werden, der private Schlüssel muss streng geheim gehalten werden.

### Funktionsweise

1. Jede Person hat ein Schlüsselpaar. Bob möchte, dass Alice ihm eine Nachricht schickt.
2. Bob gibt Alice seinen öffentlichen Schlüssel.
3. Alice verschlüsselt ihre Nachricht mit Bobs öffentlichem Schlüssel.
4. Die verschlüsselte Nachricht kann nur mit Bobs privatem Schlüssel entschlüsselt werden.

- **Vorteile:** Der Schlüsselaustausch ist sicher, da nur der öffentliche Schlüssel geteilt wird. Dies löst das Hauptproblem der symmetrischen Kryptografie.

- **Nachteile:** Es ist deutlich langsamer und rechenintensiver als die symmetrische Verschlüsselung.

### Visualisierung: Asymmetrische Verschlüsselung
```yaml
+-------------+                             +-------------+
|    Alice    |                             |     Bob     |
| (Sender)    |                             | (Empfänger) |
+-------------+                             +-------------+
       |                                             ^
       |            Bobs Public Key                  |
       |-------------------------------------------->|
       |                                             |
       |-------[ Verschlüsseln mit Bobs PK ]-------->|
       |           (Nachricht)                       |
       |<--(Nur Bob kann entschlüsseln mit Bobs PK)--|
       |                                             |
+------v------+                                +-----+------------+
| Klartext    |                                | Bobs Private Key |
+-------------+                                +------------------+
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 3. Hashing-Algorithmen
Hashing ist eine Einwegfunktion. Ein Hashing-Algorithmus nimmt Daten beliebiger Größe entgegen und erzeugt daraus einen eindeutigen, festen Wert, den Hash-Wert oder Prüfsumme. Aus dem Hash-Wert kann nicht auf die ursprünglichen Daten geschlossen werden.

- **Bedeutung für die Integrität:** Hashing wird verwendet, um die Integrität von Daten zu überprüfen. Wenn sich auch nur ein Bit in den Originaldaten ändert, ändert sich der Hash-Wert dramatisch.

- **Beispiel:**

    - Du lädst eine Datei herunter und der Anbieter stellt eine Prüfsumme bereit.
    - Du berechnest den Hash-Wert der heruntergeladenen Datei selbst.
    - Stimmen beide Werte überein, ist die Integrität der Datei gewährleistet – sie wurde nicht manipuliert.

Gängige Hashing-Algorithmen sind **SHA-256** und **SHA-3**.

## Zusammenfassung: Die Rolle der Kryptografie

- **Vertraulichkeit:** Wird durch symmetrische und asymmetrische Verschlüsselung erreicht. Die **TLS/SSL-Verbindung** (HTTPS) auf Webseiten ist eine Kombination aus beiden: Asymmetrie für den sicheren Schlüsselaustausch, Symmetrie für die schnelle Datenübertragung.

- **Integrität:** Wird primär durch Hashing und digitale Signaturen sichergestellt.

Diese Konzepte sind das Rückgrat der modernen Cybersicherheit und essenziell, um Daten vor unerlaubtem Zugriff und Manipulation zu schützen.

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