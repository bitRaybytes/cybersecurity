# Hex-Dump - Analyse von Protokoll-Header

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Was ist Hex-Dump?](#was-ist-hex-dump)
- [Was ist bei der Berechnung zu beachten?](#was-ist-bei-der-berechnung-zu-beachten)
    - [Endianness: Die Byte-Reihenfolge](#endianness---die-byte-reihenfolge)
    - [Beispiel eines Hex-Dumps](#beispiel-eines-hex-dumps)
- [Beispiele zur Berechnung](#beispiele-zur-berechnung)
    - [Übung 1: Server-Antwrot (SYN/ACK)](#übung-1-server-antwrot-synack)
- [Übersicht der Header Analyse](#übersicht-der-header-analyse)
    - [I. IPv4-Header-Analyse (Zeilen 0000 - 0010)](#i-ipv4-header-analyse-zeilen-0000---0010)
    - [II. TCP-Header-analyse (ab Byte 20, Zeile 0010)](#ii-tcp-header-analyse-ab-byte-20-zeile-0010)
- [Weitere Übungen](#weitere-übungen)
    - [Übung 2: Datenübertragung (PSH/ACK)](#übung-2-datenübertragung-pshack)
    - [Übung 3: Ein anderes Protokoll (ICMP "Ping")](#übung-3-ein-anderes-protokoll-icmp-ping)
- [Haftungsausschluss](#haftungsausschluss)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

Egal, was du im Internet machst: die Grundlage jeder Kommunikation sind **Protokolle** und ihre **Header**.

Header sind die **Steuerzentralen** der Datenpakete. Sie übermitteln wichtige Informationen (wie Absender, Empfänger, Paketlänge und Protokolltyp), die die Gegenstelle benötigt, um die notwendigen Anweisungen für die Verarbeitung der Daten zu erhalten. Ohne diese Header wüsste kein Router, wohin das Paket geschickt werden muss.

Wie Protokolle und ihre Header aufgebaut sind, erfährst du hier: [Protokoll-Header-Cheatsheet](/02-network-security/protokoll_header_cheatsheet.md)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was ist Hex-Dump?

Ein **Hex-Dump** (auch **Speicherabzug** genannt) ist die rohste Ausgabe eines Datenpakets oder eines Speicherbereichs, dargestellt in **hexadezimaler** Form.

Das primäre Ziel des Hex-Dumps ist die einfache Berechnung und Allokation von Header-Informationen im jeweiligen Protokoll.

**Das goldene Prinzip: Bits, Bytes und Hex**

Die Berechnung der Header-Felder erfolgt immer durch die Abfolge von **Bits** und **Bytes**. In der IT und Elektrotechnik ist die kleinste Informationseinheit das **Bit**, das nur zwei Zustände kennt: **0** (Aus/False) oder **1** (An/True).

| Einheit | Abkürzung | Größe | Darsellung im Dump |
|---------|-----------|-------|--------------------|
| **Bit** | b | Der kleinste Zustand (0 oder 1) | Wird in der Analyse nur indirekt benötigt. |
| **Nibble** | | 4 Bits | **Eine** Hex-Ziffer (z.B `F` oder `2`). |
| **Byte** | B | 8 Bits | **Zwei** Hex-Ziffern (z.B. `45`). |

**Merke:** Jedes **Zweier-Pärchen** in deinem Hex-Dump (z.B. `16`, `00`, `35`) repräsentiert genau **ein Byte** oder **8 Bits** des Pakets. Die Header-Analyse besteht darin, diese Bytes gemäß dem Protokoll-Bauplan (dem Header-Diagramm) zu interpretieren.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Was ist bei der Berechnung zu beachten?
Header sind wie eine **Kette von Wegweisern** aufgebaut. Ein Header sagt dir immer, welcher Header als Nächstes kommt. Ohne zu wissen, welches Protokoll vor dir liegt, wirst du schwer herausfinden, welcher Header es ist.

Es ist daher **unbedingt notwendig** zu wissen, welcher Header bzw. welches Protokoll verwendet wird. Ohne den **Bauplan** eines Protokolls ist der Hex-Dump nämlich bedeutungslos. Du entschlüsselst das Paket, indem du diese Kette systematisch verfolgst:

1. **Start:** Du beginnst mit dem **Ethernet-Header** (Layer 2).

2. **Erster Wegweiser:** Ein Feld im Ethernet-Header (der **EtherType**) sagt dir, ob **IPv4** (`0x0800`), **IPv6** (`0x86DD`) oder etwas anderes folgt.

3. **Zweiter Wegweiser:** Ein Feld im **IP-Header** (das **Protocol**-Feld, z.B. `0x06` für **TCP**) sagt dir, welcher **Transport-Header** (Layer 4) als Nächstes kommt.



**Die Darstellungsbreite und Header-Länge**

Hier ist eine wichtige Klarstellung zur Größe der Header, die oft zu Verwirrung führt:

- **Darstellungsbreite (Layout):** Fast alle Standard-Protokoll-Diagramme (wie IPv4 oder TCP) werden mit einer Breite von **32 Bit** oder **4 Byte** dargestellt (da **32 Bit/8 Bit=4 Byte**). Dies dient nur der Übersichtlichkeit.

    - **Tatsächliche Länge (Größe):** Die tatsächliche Länge der Header ist vielfach größer als **32 Bit** (**4 Byte**):

        - Der IPv4-Header ist mindestens 20 Byte (160 Bit) lang.

        - Der TCP-Header ist mindestens 20 Byte (160 Bit) lang.

        - Der IPv6-Header ist fix 40 Byte (320 Bit) lang.

Deine Aufgabe ist es also, die Bits des Hex-Dumps Zeile für Zeile nach dem Bauplan abzuarbeiten, bis das Längenfeld des aktuellen Headers dir sagt, wo der nächste Header beginnt.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Endianness - Die Byte-Reihenfolge
Dies ist der wichtigste Aspekt für die korrekte Interpretation von Feldern, die mehr als ein Byte belegen (z.B. Ports, Sequence Number, Total Length). Netzwerke verwenden standardmäßig die sogenannte **Network Byte Order** (**Big-Endian**).


| Begriff | Reihenfolge | Interpretation | Beispiel (Hex: `12 34`) |
|---------|-------------|----------------|-------------------------|
| **Big-Endian** (Netzwerk-Standard) | Wichtigstes Byte zuerst | Byte `12` ist das höchste, Byte `34` das niedrigste | Dezimal: $12 * 256 + 34 = 4660$ | 
| **Little-Endian** (PC-Speicher) | Niedrigste Byte zuerst | Byte `34` ist das höchste, Byte `12` das niedrigste | Dezimal: $34 x 256 + 12 = 8716$|


**Fazit:** Bei der Header-Analyse müssen wir die Bytes in der Reihenfolge lesen, wie sie im Hex-Dump stehen. Ein Feld `0x0034` (wie unten) wird als $0034_{Hex}$ und nicht als $3400_{Hex}$ interpretiert.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Beispiel eines Hex-Dumps:

```text
Zeile | Ab hier beginnt der Header
  |     |
  v     v
0000    45 00 00 34 1a 2b 40 00 40 06 7c 3b 8b 11 14 0a
0010    c0 a8 01 11 00 19 f0 60 5a 2b 3c 4d c6 5d 76 10
0020    80 12 1f f0 1a 2c 00 00 02 04 05 b4 01 03 03 08
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Beispiele zur Berechnung



### Übung 1: Server-Antwrot (SYN/ACK)
Stell dir vor, dies ist die direkte Antwort des Servers auf das Paket, das wir gerade analysiert haben.

**Beispiel eines TCP/IPv4 Headers:**
```text
0000    45 00 00 34 1a 2b 40 00 40 06 7c 3b 8b 11 14 0a
0010    c0 a8 01 11 00 19 f0 60 5a 2b 3c 4d c6 5d 76 10
0020    80 12 1f f0 1a 2c 00 00 02 04 05 b4 01 03 03 08
```


```text
 0               1               2               3
 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-----------+
|Version|  IHL  |   TOS/DSCP    |  Total Length                   |   IPv4    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+           |
|        Identification         |Flags|     Fragment Offset       |   HEADER  | 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+           |
|      TTL      |    Protocol   |           Header Checksum       |           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-----------+
|                         Source IP Address                       |   TCP     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+           |
|                      Destination IP Address                     |   HEADER  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+           |
|               Option + Padding (nur wenn IHL > 5)               |           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-----------+
```

**Die Berechnung:**
- **Version und Internet Header Length:** `Version`: 0-3 Bits (**4 Bits** = Nibble); `IHL`: 4-7 Bits (**4 Bits**). Zusammen sind das **8 Bits** und ergeben 1 Byte. 
    - in unserem Beispiel ist das erste Byte `45`. Damit ist die Version die `4` (0-3 Bits) und der `IHL` mit der `5` die nächsten 4 Bits (4-7 Bits).
        - Der `IHL` bestimmt, wie groß der IPv4-Header ist und zeigt dir gleichzeitig an, wo der TCP-Header beginnt. Jede Zeile hat eine Größe von 4 Byte. Damit musst du die Zahl `5` mit `4 Byte` multiplizieren und kommst auf eine IPv4-Header-Größe von `20 Byte`. 
    - **Hexadezimale Schreibweise:** `0x45`

- **TOS/DSCP:** ist genau 1 Byte und kommt direkt nach der Version und des IHL. Die Hexadecimal ist somit die zweite Zweier-Zahlenfolge `00`.

- **Total Length:** Die Total Length sind die restlichen 2 Byte der ersten Zeile des Protokolls. Im Beispiel sind das die zwei Bytes `00 34` (oder `0x0034). Decimal sind das 52 Bytes an totaler Protokoll-Größe.

- **Identification:** `1a 2b` -> 

- **Flags und Fragment Offset:** Flags: Bits 16-18 (3 Bits) und Fragment Offset Bits 19-31 (13 Bits) mit der Hexadezimalen Folge von `40 00`, der Wert ist also `0x400`
    - **Wichtig:** Die `0x400` wird folgendermaßen aufgeteilt:
        - 1. Umwandlung von `0x400`in Binär:
            - `4` = `0100`
            - `0` = `0000`
            - `0` = `0000`
            - `0` = `0000`
            - Gesamtwert 16 Bits = `0100 0000 0000 0000`
        - 2. Maske aus Diagramm darüber legen:
            - **Ersten 3 Bits sind Flags:** `010`
            - **Resliche 13 Bits sind Offset:** `0000 0000 0000`
        - 3. Aufschlüsselung der Flag `010`:
            - Bit 1 (Reserved): `0` (ist immer 0)
            - Bit 2 (Don't Fragment, DF): `1`
            - Bit 3 (More Fragments, MF): `0`
        - 4. Aufschlüsselung des Offset `0000 0000 0000`:
            - Fragment Offset: = 0

- **Time To Life (TTL):** `40` oder `0x40` definiert die Sprungweite des Datenpakets (`TTL`).
    - Der `TTL`-Header zeigt an, wie viele "Sprünge" das Paket machen kann, bevor es revidiert wird. Das verhindert eine Rekursion.
    - `40` -> 64 Sprünge bevor das Paket verworfen wird.

- **Protocol:** `06`: stellt das verwendete Protokoll dar, je nach Wert des Inhalts. In unserem Beispiel ist es das `TCP` Protokoll.

- **Header Checksum:**  Der **Header Checksum** schützt den Header eines Datenpakets mit einem 16-Bit-Wert und stellt somit dessen Datenintegrität sicher. Wird vom Sender berechnet und vom Empfänger überprüft. Bei jedem Hop wird erneut verifiziert und, da sich TTL ändert, neu berechnet.

- **Source IP:** `8b 11 14 0a` oder `0x8b`.`0x11`.`0x14`.`0x0a` oder  `139`.`17`.`20`.`10`.

- **Destination IP:** `c0 a8 01 11` oder `0xc0`.`0xa8`.`0x01`.`0x11` oder `192`.`168`.`1`.`17`.

- Nach der Destination IP kommt ein optionales Padding, allerdings nur, wenn der **IHL > 5** ist.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Übersicht der Header Analyse
### I. IPv4-Header-Analyse (Zeilen 0000 - 0010)

| Feld | Position (Byte) | Hex-Wert | Dezimal-Wert | Interpretation |
|------|-----------------|----------|--------------|----------------|
| **Version/IHL** | 0 | `45`| Ver: $4$, IHL: $5$| IPv4 Paket, mit Header-Length von $20 Byte$ (`5x4 Bytes = 20 Bytes`). |
| **TOS/DSCP** | 1 | `00` | 0 | Normale Dienstklasse. |
| **Total Length** | 2-3 | `00 34` | 52 | **Gesamtlänge** des Pakets (Header + Daten): 52 Byte. |
| **Identification** | 4-5 | `1a 2b` | 26 43 | Eindeutige ID des Pakets. |
| **Flags/Offset** | 6-7 | `40 00` | - | **Flags:** `010` (Reserved: 0, DF: 1, MF: 0). DF = Don't Fragment ist gesetzt. **Offset:** 0. |
| **TTL** | 8 | `40`| 64 | Start-TTL von 64 (üblich für Linux-Systeme). |
| **Protokoll** | 9 | `06` | 6 | Der nächste Header ist **TCP**. |
| **Header Checksum** | 10-11 | `7c 3b` | - | Nur zur Integritätsprüfung des IPv4-Headers. |
| **Source-IP** | 12-15 | `8b 11 14 0a` | 139.17.20.10 | **Absender** (Server) |
| **Destination IP** | 16-19 | `c0 a8 01 11` | 192.168.1.17 | **Empfänger** (der Client). |


### II. TCP-Header-analyse (ab Byte 20, Zeile 0010)
Da der IPv4-Header 20 Byte lang ist (IHL=5), beginnt der TCP-Header direkt nach der Destination IP (`c0 a8 01 11`).
| Feld | Position (Byte) | Hex-Wert | Dezimal-Wert | Interpretation |
|------|-----------------|----------|--------------|----------------|
| **Source Port** | 20-21 | `00 19` | 25 | **Absender-Port** (Server): Port 25 (SMTP). |
| **Destination Port** | 22-23 | `f0 60` | 61535 | **Ziel-Port** (Client): Ephemeral Port. |
| **Sequence Number** | 24-27 | `5a 2b 3c 4d` | - | Die vom Server gewählte **Start-Sequenznummer**. |
| **ACK Number** | 28-31 | `c6 5d 76 10` | - | **Bestätigungsnummer** für den empfangenen Client-SYN. |
| **Data Offset/Flags** | 32-33 | `80 12` | - | **Data Offset:** `8` (Nibble 1) -> `8 x 4 Byte = 32 Byte` TCP-Header-Length. **Flags:** `... 0000 1001 0`.|
| **Flags:** (Detail) | 33 | - | - | **Ack** (`1`) und **SYN** (`1`) sind gesetzt (Bit 16 & 17). Dies bestätigt, dass es sich um eine SYN/ACK-Antwort handelt. |
| **Window Size** | 34-35 | `1f f0` | 8176 | Die Größe des Empfangspuffers, den der Serer dem Client anbietet. |
| **Checksum** | 36-37 | `1a 2c` | - | **Prüfsumme** für Integrität (Header und Pseudo-Header). |
| **Urgent Pointer** | 38-29 | `00 00` | 0 | Nicht genutzt. |
| **Options** | 40-51 | `02 04 05 b4 ...` | - | **Länge 12 Byte** (da Header-Länge 32 Byte ist, 20 Byte Mindestlänge fehlen, bleiben 12 Byte). hier sind die TCP-Optionen, z.B. **Maximum Segment Size (MSS)** und **Window Scale**. | 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Weitere Übungen
Die folgenden Übungen dienen dazu, die Analyse der Header-Kette zu vertiefen und unterschiedliche Protokolle zu erkennen.

### Übung 2: Datenübertragung (PSH/ACK)
Dies ist ein Paket, das nach einem erfolgreichen Verbindungsaufbau gesendet wird. Es enthält eine kleine Daten-Nutzlast, z.B. einen Teil einer HTTP-Anfrage.

***Aufgabe:*** *Analysiere den IPv4-Header, den TCP-Header und extrahiere die Daten-Nutzlast (Payload) als lesbaren Text.*

```text
0000 45 00 00 3e 7b 3a 40 00 80 06 1a 9f c0 a8 0a 32
0010 68 12 14 64 d0 01 00 50 11 22 33 44 55 66 77 88
0020 50 18 40 00 88 99 00 00 47 45 54 20 2f 20 48 54
0030 54 50 2f 31 2e 31 0d 0a
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Übung 3: Ein anderes Protokoll (ICMP "Ping")

Dieses Paket verwendet nicht TCP. Achte auf das "Protocol"-Feld im IP-Header, um zu wissen, wie du die Daten nach dem IP-Header interpretieren musst.

***Aufgabe:*** *Finde das Protokoll im IPv4-Header und analysiere den ICMP-Header sowie die Daten-Nutzlast.*

```text
0000 45 00 00 54 1c 01 00 00 40 01 7b 1a 0a 00 00 05
0010 08 08 08 08 08 00 f7 d5 12 34 00 01 61 62 63 64
0020 65 66 67 68 69 6a 6b 6c 6d 6e 6f 70 71 72 73 74
0030 75 76 77 78 79 7a 30 31 32 33 34 35 36 37 38 39
```



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

🗓️ **Letzte Aktualisierung:** November 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
