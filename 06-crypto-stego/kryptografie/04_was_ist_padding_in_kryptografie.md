# Was ist Padding in der Kryptografie?


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Padding bei Symmetrischen Blockchiffren](#padding-bei-symmetrischen-blockchiffren)
- [Padding bei Asymmetrischen Algorithmen (RSA)](#padding-bei-asymmetrischen-algorithmen-rsa)
- [Zusammenfassung](#zusammenfassung)
- [Nützliche Links & Quellen](#nützliche-links--quellen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

**Padding** (deutsch: **Füllung** oder **Auffüllung**) ist ein essentieller Prozess in der Kryptografie, der hauptsächlich bei der Verwendung von Blockchiffren (wie AES, DES, 3DES, Blowfish) sowie bei asymmetrischen Algorithmen (wie RSA) eingesetzt wird.

Der Hauptzweck des Padding ist es, die kryptografische Sicherheit und die technische Korrektheit der Verschlüsselung zu gewährleisten.

**Visuelle Darstellung des Prozesses (Blockchiffren)**
Der Padding-Prozess stellt sicher, dass der Klartext immer in exakt definierte Blöcke aufgeteilt wird, bevor die Blockchiffre angewendet wird.

```text
+-----------+       +--------------------+          +---------------+
|  Klartext | ----> | Padding hinzufügen | -------> |  Blockchiffre | --> Geheimtext
+-----------+       +--------------------+          +---------------+
(Variable Länge)    (Auf volle Blockgröße füllen)   (Feste Blockgröße)
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Padding bei Symmetrischen Blockchiffren

Symmetrische Blockchiffren verarbeiten Daten immer in Blöcken fester Größe (z. B. 128 Bit bei AES oder 64 Bit bei DES).

- **Technische Notwendigkeit:** Wenn die **Klartextdaten** kein exaktes Vielfaches dieser Blockgröße sind, muss der letzte Block bis zur vollen Blockgröße **aufgefüllt** werden.

- **Funktionsweise & Gängige Schemata:** Beim Verschlüsseln wird Padding hinzugefügt. Beim Entschlüsseln muss der Algorithmus genau wissen, welches Schema verwendet wurde, um das Padding **korrekt zu entfernen** und den originalen Klartext zu rekonstruieren.

- **Beispiel-Schema:** `PKCS#5` / `PKCS#7 Padding`. Dies ist der heute gängige Standard. Hierbei wird der letzte Block mit Bytes gefüllt, deren Wert die Anzahl der hinzugefügten Bytes angibt. Wenn beispielsweise $N$ Bytes zur Füllung benötigt werden ($N$ ist zwischen 1 und der Blockgröße), werden $N$ Bytes mit dem Wert $N$ angehängt (z. B. $0x05, 0x05, 0x05, 0x05, 0x05$).

- **Zero Padding:** Der Block wird mit Null-Bytes ($0x00$) gefüllt. Dieses Schema ist unsicher, da es nicht immer eindeutig ist, wie viele Padding-Bytes entfernt werden müssen (wenn die Nachricht selbst mit Null-Bytes endet).


### Gefahr: Der Padding Oracle Attack

Dies ist die gravierendste Sicherheitslücke, die direkt aus einer fehlerhaften Implementierung des Entschlüsselungs- und Padding-Entfernungs-Prozesses entsteht.

**Was ist ein Padding Oracle?**

Ein **Padding Oracle** ist ein System (z. B. ein Webserver), das einem Angreifer unbeabsichtigt mitteilt, ob der entschlüsselte Chiffretext **gültiges oder ungültiges Padding** enthält.

- **Das Leck:** Die Information wird oft durch unterschiedliche Fehlermeldungen (z. B. "Invalid Padding" vs. "Internal Error"), unterschiedliche Antwortzeiten (**Timing-Attacke**) oder gar keine Antwort nach einem Fehler preisgegeben.

- **Folge:** Ein Angreifer kann diese binäre Ja/Nein-Information (gültiges Padding?) nutzen, um **die gesamte Nachricht ohne Kenntnis des geheimen Schlüssels zu entschlüsseln** (**Chosen-Ciphertext Attack**).


**Funktionsweise**
1. Der Angreifer manipuliert den vorletzten Chiffretext-Block.

2. Er sendet die manipulierte Nachricht an den Server (das Oracle).

3. Durch systematisches Brute-Forcing des letzten Bytes im vorletzten Block kann der Angreifer genau jene Byte-Werte erraten, die beim Entschlüsseln zu einem gültigen Padding führen.

4. Diese Information erlaubt es ihm, über die XOR-Operation und die Blockchiffre-Formel ($P_i = D_K(C_i) \oplus C_{i-1}$) das tatsächliche Klartext-Byte zu berechnen.

5. Der Prozess wird für alle Bytes im Block wiederholt.


**Wichtigste Abwehrmaßnahme: Authenticated Encryption**

Die beste Verteidigung ist die Verwendung von **Authenticated Encryption with Associated Data (AEAD)-Modi** wie **AES-GCM** (Galois/Counter Mode) oder **ChaCha20-Poly1305**.

Diese Modi stellen sicher, dass die Integritätsprüfung (Authentifizierung) des Chiffretexts vor der Entschlüsselung und Padding-Prüfung erfolgt. Wenn ein Angreifer den Geheimtext manipuliert, wird dies sofort vom **Message Authentication Code** (**MAC**) erkannt und die Verarbeitung gestoppt, bevor das Padding-Resultat preisgegeben werden kann.

> **Regel:** Prüfe den MAC immer vor der Entschlüsselung (Encrypt-then-MAC oder besser: AEAD-Modi verwenden).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Padding bei Asymmetrischen Algorithmen (RSA)

Bei asymmetrischen Algorithmen wie RSA dient Padding nicht nur der Längenanpassung, sondern hat eine **entscheidende Sicherheitsfunktion**.

- **Sicherheitsprobeme ohne Padding (Deterministic RSA):** Reine RSA-Operationen sind deterministisch. Das bedeutet, derselbe Klartext führt immer zum selben Geheimtext. Dies kann Angreifern (z. B. durch Chosen-Ciphertext-Angriffe) Schwachstellen zur Ausnutzung bieten.

- **Sicherheitsverbesserung:** Padding-Schemata führen eine **zufällige Komponente** in den Verschlüsselungsprozess ein.

- **Beispiel-Schema:** **OAEP** (**Optimal Asymmetric Encryption Padding**). 
    - **Zweck:** OAEP macht RSA **probabilistisch** (zufällig) und bietet einen **Proof of Security** im Random Oracle Model, was bedeutet, dass es resistent gegen alle bekannten adaptiven Chosen-Ciphertext-Angriffe ($CCA2$) ist.
    - **Funktionsweise:** Es nutzt einen **Zufalls-Seed** (Keim) und zwei kryptografische Hash-Funktionen (sogenannte Mask Generating Functions, MGFs), um die Nachricht zu maskieren, bevor die RSA-Operation angewendet wird. Dies stellt sicher, dass die gleiche Nachricht jedes Mal anders verschlüsselt wird.

- **Veraltet:** **PKCS#1 v1.5 Padding** für RSA (oft in alten TLS-Implementierungen zu finden) ist veraltet und anfällig für den **Bleichenbacher's Attack** (ein Oracle Attack gegen RSA). Es sollte durch OAEP ersetzt werden.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Zusammenfassung

| Funktion | Blockchiffre (AES) | Asymmetrisch (RSA) |
|----------|--------------------|--------------------|
| **Hauptzweck** | **Längenanpassung** (letzter Block füllen) | **Sicherheitserhöhung** (Zufälligkeit hinzufügen) |
| **Gefahr ohne** | Fehlerhafte Entschlüsselung | Anfälligkeit für kryptografische Angriffe |
| **Gängiger Standard** | PKCS#5 / PKCS#7 | OAEP |
| **Experten-Tipp** | **AEAD** verwenden (z. B. AES-GCM) | OAEP verwenden |


> **Kurz gesagt:** Ohne Padding wäre die Verschlüsselung entweder **technisch unmöglich** oder **kryptografisch unsicher**. Es ist ein **obligatorischer Bestandteil** jedes sicheren Verschlüsselungsprozesses.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links & Quellen

- [Wikipedia: Padding](https://de.wikipedia.org/wiki/Padding_(Informatik))
- [Wikipedia: Padding Oracle Attack](https://en.wikipedia.org/wiki/Padding_oracle_attack)
- [RFC 8446 (TLS1.3) - Entfernung von unsicheren Modi (inkl. CBC mit Padding-Risiko)](https://datatracker.ietf.org/doc/html/rfc8446)


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

🗓️ **Letzte Aktualisierung:** Oktober 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
