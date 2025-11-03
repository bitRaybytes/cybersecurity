# Betriebsarten von AES (Modes of Operations)

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Grundlagen: IV, Nonce und AEAD](#grundlagen-iv-nonce-und-aead)
- [Die Betriebsarten von AES](#die-betriebsarten-von-aes)
    - [1. ECB Electronic Codebook (veraltet/unsicher)](#1-ecb-electronic-codebook-veraltetunsicher)
    - [2. CBC (Cipher Block Chaining)](#2-cbc-cipher-block-chaining)
    - [3. CFB (Cipher Feedback)](#3-cfb-cipher-feedback)
    - [4. OFB (Output Feedback)](#4-ofb-output-feedback)
    - [5. CTR (Counter Mode)](#5-ctr-counter-mode)
    - [6. GCM (Galois/Counter Mode)](#6-gcm-galoiscounter-mode)
- [Spezialmodus: XTS](#spezialmodus-xts)
- [Kritische Sicherheitswarnung](#kritische-sicherheitswarnung)
- [Nützliche Links & Quellen](#nützliche-links--quellen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung
Die **Betriebsarten** (**Modes of Operations**) einer Blockchiffre wie AES definieren, wie der Algorithmus zur Verschlüsselung von Daten eingesetzt wird, die länger sind als die zugrunde liegende Blockgröße (AES hat eine Blockgröße von 128 Bit).

Die Wahl des Modus ist daher entscheidend für drei wesentliche kryptografische Ziele:

1. **Vertraulichkeit (Confidentiality):** Schutz der Daten vor unbefugter Einsicht (reine Verschlüsselung).

2. **Integrität (Integrity):** Schutz der Daten vor unbefugter Veränderung (Manipulation).

3. **Authentizität (Authenticity):** Sicherstellung, dass die Daten vom erwarteten Absender stammen.

**Wichtig:** Nicht alle Modi bieten alle drei Ziele. Ältere Modi (ECB, CBC, CTR) gewährleisten nur Vertraulichkeit und müssen durch separate MACs oder Hashes ergänzt werden, um Integrität und Authentizität zu erreichen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Grundlagen: IV, Nonce und AEAD

| Begriff | Bedeutung | Anforderung | Betroffene Modi |
|---------|-----------|-------------|-----------------|
| **Initialisierungsvektor (IV)** | Ein zufällliger Wert, der dem ersten Klartextblock hinzugefügt wird, um die Verschlüsselung zu randomisieren. | **Zufällig & Öffentlich**. Muss bei jedem Verschlüsselungsvorgang neu und zufällig sein. Wiederverwendung ist katastrophal. | CBC, CFB, OFB |
| **Nonce (Number used Once)** | Ein nicht-zufälliger, aber **einmaliger** Wert, der sicherstellt, dass die Key-Stream-Generierung nie mit demselben Startwert erfolgen. | **Einmalig & Öffentlich**. Darf niemals mit demselben Schlüssel wiederverwendet werden. | CTR, GCM |
| **AEAD** | **Authenticated Encryption with Associated Data**. Kombiniert Vertraulichkeit und Integrität in einem einzigen kryptografischen Modus. | **De-facto-Standard**. Sollte der Standard für moderne Anwendungen sein. | GCM |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die Betriebsarten von AES

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 1. ECB Electronic Codebook (veraltet/unsicher)
- **Funktion:** Jeder Klartextblock wird **unabhängig** von den anderen Blöcken verschlüsselt, wobei **immer der gleiche Schlüssel** verwendet wird.

- **Verschlüsselung:** $C$<sub>i</sub>$​=E$<sub>K</sub>​$(P$<sub>i</sub>$​)$

- **Eigenschaften:**
    - **Kein Initialisierungsvektor (IV)** erforderlich.
    - Ermöglicht **parallele** Ver- und Entschlüsselung.
    - **Kritisches Sicherheitsrisiko:** Identische Klartextblöcke ​$(P$<sub>i</sub>$​)$ führen zu identischen Geheimtextblöcken ​$(C$<sub>i</sub>$​)$. Dies **lässt das Muster im Geheimtext erkennen** (was bei Bildern sehr deutlich wird).

- **Anwendung:** Sollte in neuen Anwendungen **vermieden** werden. Nur für die Verschlüsselung von Single-Value-Keys in Datenbanken akzeptabel.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2. CBC (Cipher Block Chaining)
- **Funktion:** Jeder Klartextblock wird **vor der Verschlüsselung** mit den Geheimtext des **vorherigen Blocks** verknüpft (XOR-Operation).

- **Verschlüsselung:** $C$<sub>i</sub>$​=E$<sub>K</sub>​$(P$<sub>i</sub> ​$⊕$ $C$ <sub>i−1</sub> $​)$ (wobei $C$<sub>0</sub> der **Initialisierungsvektor (IV)** ist).

- **Eigenschaft:** 
    - Erfordert einen **zufälligen** und **nicht-geheimen IV** für den ersten Block (muss zufällig sein!).
    - **Padding erforderlich:** Da CBC mit vollen Blöcken arbeitet, muss der letzte Block mit einem Padding-Schema (z. B. PKCS#7) aufgefüllt werden.
    - **Fehlerfortpflanzung:** Ein Bitfehler im Geheimtext beeinflusst die Entschlüsselung des aktuellen Blocks und desselben Bit-Offsets im nächsten Block.
    - Die Verschlüsselugn ist **sequenziell** (nicht parallelisierbar), die Entschlüsselung jedoch **parallelisierbar**.

- **Kritisches Sicherheitsrisiko:** **Padding Oracle Attack**. Wenn ein System dem Angreifer verrät, ob das Padding eines entschlüsselten Blocks korrekt ist (indirekt über eine Fehlermeldung), kann dies zur Entschlüsselung von Daten oder zur Fälschung des Geheimtextes führen.

- **Anwendung:** Weit verbreitet für die **Verschlüsselung von Festplatten und Daten**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 3. CFB (Cipher Feedback)

- **Funktion:** Verwandelt einen Blockchiffrier-Algorithmus in einen **Stromchiffrier-Algorithmus** (Stream Cipher Mode). Der vorherige Geheimtextblock wird verschlüsselt und das Ergebnis mit dem aktuellen Klartextblock verknüpft (XOR).

- **Verschlüsselung:** $C$<sub>i</sub>$​=P$<sub>i</sub> ​$ ⊕$ $E$<sub>K</sub>$(C$<sub>i-1</sub>$​)$ (wobei $C$<sub>0</sub> der **Initialisierungsvektor (IV)** ist).

- **Eigenschaften:**
    - Erfordert einen **IV**.
    - **Kein Padding erforderlich**, da es in Stream-Einheiten arbeitet.
        - Kann die Daten in kleineren Einheiten als der Blockgröße verarbeiten (z. B. 8 Bit).
    - Ver- und Entschlüsselung sind **sequenziell**.
    - **Fehlerfortpflanzung:** Ein Bitfehler im Geheimtext pflanzt sich über die Länge eines Blocks fort (ähnlich wie CBC).

- **Anwendung:** Eignet sich für Anwendungen, bei denen die Daten in **kleinen, gestreamten Einheiten** verarbeiten werden müssen (z.B. sichere Terminal-Sitzungen).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 4. OFB (Output Feedback)
- **Funktion:** Ähnlich wie CFB, arbeitet aber unabhängig vom Klar- und Geheimtext. Es wird ein **Keystream** (Schlüsselstrom) generiert, indem der Output des Chiffrierers (nicht der Geheimtext) immer wieder in den Chiffrierer zurückgeführt und verschlüsselt wird. 

- **Verschlüsselung:** $C$<sub>i</sub>$​=P$<sub>i</sub> ​$ ⊕$ $O$<sub>i</sub> (wobei $O$<sub>i</sub> $=E$<sub>K</sub> $($ $O$ <sub>i-1</sub> $)$ der **Initialisierungsvektor (IV)** ist).

- **Eigenschaften:**
    - Erfordert einen **IV**.
    - **Kein Padding erforderlich.**
    - Der Keystream kann **vorab berechnet** werden.
    - **Fehler-Toleranz:** Ein Bitfehler im Geheimtext beeinflusst bei der Entschlüsselung nur das entsprechende Klartextbit (Keine Fehlerfortpflanzung).

- **Anwendung:** Eignet sich für **sprachanfällige Übertragungskanäle** (z. B. Satellitenkommunikation), da Fehler nicht stark propagieren.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 5. CTR (Counter Mode)
- **Funktion:** Erzeugt einen **Keystream** durch das Verschlüsseln eines **einfachen Zählers** ($T$<sub>i</sub>), der für jeden Block hochgezählt wird. Der Klartext wird dann mit diesem Keystream verknüpft (XOR).

- **Verschlüsselung:** $C$<sub>i</sub>$​=P$<sub>i</sub> ​$ ⊕$ $E$<sub>K</sub>$(T$<sub>i</sub> $)$

- **Eigenschaften:**
    - Erfordert einen **zufälligen und nicht-geheimen Nonce** (ein Startwert), aus dem der Zähler generiert wird.
    - **Kein Padding erforderlich.**
    - **Extrem schnell:** Sowohl Ver- als Entschlüsselung sind **vollständig parallelisierbar**.
    - **Fehler-Toleranz:** Keine Fehlerfortpflanzung (wie OFB).

- **Kritische Sicherheitswarnung:** **Nonce/Counter-Wiederverwendung ist absolut tödlich!** Wie bei OFB führt die Wiederverwendung des Keystreams zur sofortigen Kompromittierung der Vertraulichkeit (wie oben beschrieben).
- **Anwendung:** Sehr beliebt in modernen Protokollen wie **TLS/IPsec** aufgrund seiner hohen Performance und der Möglichkeit zur Parallelisierung.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 6. GCM (Galois/Counter Mode)

- **Funktion:** Eine **authentifizierte Verschlüsselungs-Betriebsart** (**Authenticated Encryption with Associated Data, AEAD**). Sie kombiniert die **Verschlüsselungsgeschwindigkeit von CTR** mit einer **gleichzeitigen Authentifizerung** (mittels Galois Field Multiplication).

- **Eigenschaften:**
    - Bietet **Vertraulichkeit** (Verschlüsselung) und **Authentizität/Integrität** (Schutz vor unbefugter Änderung).
    - Erfordert einen **Nonce**, typischerweise 96 Bit (12 Byte) lang.
    - Bietet ein **Authentication Tag** (Integritätprüfungswert), der beweist, dass der Geheimtext nicht manipuliert wurde. 
    - **Sehr schnell** und voll parallelisierbar.
    - **Associated Data (AD):** Ermöglicht die Einbeziehung nicht-verschlüsselter (aber authentisierter) Header-Informationen in die Integritätsprüfung.

- **Kritische Sicherheitswarnung:** **Nonce-Wiederverwendung** ist hier noch katastrophaler als bei CTR. Sie verletzt nicht nur die Vertraulichkeit, sondern ermöglicht auch die **vollständige Fälschung (Forgery)** zukünftiger Nachrichten ohne Kenntnis des Schlüssels.

- **Anwendung:** **Aktueller De-Facto-Standard** in TLS 1.3, SSH und modernen Protokollen. **Wird für die meisten neuen Anwendungen empfohlen**.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Spezialmodus: XTS

- **XTS (Xor-Encrypt-Xor with Tweakable Ciphertext Stealing):**

    - **Funktion:** Ein Modus, der speziell für die Festplattenverschlüsselung (**Disk Encryption**) entwickelt wurde (z. B. `BitLocker`, `VeraCrypt`).

    - **Eigenschaften:** Bietet keine **Authentifizierung/Integrität**, da dies bei Festplatten-Sektoren nicht primär erforderlich ist. Er wurde so konzipiert, dass er die Auswirkungen lokaler Blockfehler minimiert und eine zufällige Lese- und Schreibzugriffszeit auf die Sektoren ermöglicht.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Kritische Sicherheitswarnung

**NIEMALS Nonce/IV mit demselben Schlüssel wiederverwenden**

Die **Wiederverwendung** eines Initialisierungsvektors (IV) in CBC oder eines Nonce/Counters in CTR/OFB/GCM mit demselben Schlüssel führt zur sofortigen Kompromittierung der Vertraulichkeit.

- **Bei CTR/OFB/GCM:** Der Keystream wird doppelt verwendet, was Kryptoanalyse (Two-Time Pad Angriff) ermöglicht.

- **Bei GCM:** Die Wiederverwendung führt zur vollständigen Wiederherstellung des Authentifizierungsschlüssels, was beliebige Nachrichten-Fälschungen ermöglicht.

Daher muss der IV/Nonce für jede Nachricht, die mit demselben Schlüssel verschlüsselt wird, eindeutig sein.

**Immer Authentifizierte Verschlüsselung (AEAD) nutzen**

Verwende, wann immer möglich, AEAD-Modi wie GCM oder ChaCha20-Poly1305 (ein weiterer schneller Stream-Cipher-basierter AEAD-Modus).

Warum? Reine Verschlüsselungsmodi (CBC, CTR, OFB) schützen nicht vor Manipulation. Ein Angreifer könnte den Geheimtext verändern und die Nachricht würde im Backend entschlüsselt, ohne dass das System die Manipulation erkennt (z. B. einen Kontostand von 100 auf 999 ändern). AEAD verhindert dies durch den obligatorischen Authentication Tag.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links & Quellen

- [Wikipedia: Advanced Encryption Standard](https://de.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [Wikipedia: Betriebsmodus](https://de.wikipedia.org/wiki/Betriebsmodus_(Kryptographie))
- [CyberChef GitHub Repo](https://github.com/gchq/CyberChef)




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
