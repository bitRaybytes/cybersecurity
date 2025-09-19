# 🕵️ Steganografie - Das Verstecken von Payloads im Medium

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Funktionsweise von Steganografie](#funktionsweise-von-steganografie)
- [Arten der Steganografie](#arten-der-steganografie)
- [Bekannte Methoden](#bekannte-methoden)
- [Vergleich mit Kryptografie](#vergleich-mit-kryptografie)
- [Anwendungsbereiche](#anwendungsbereiche)
- [Steganalyse](#steganalyse)
- [Nützliche Tools und Links](#nützliche-tools-und-links)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

Steganografie ist die Kunst des Verbergens von Informationen in einem scheinbar harmlosen Trägermedium wie Bild, Audio, Video oder Text.
Das Ziel: nicht nur den Inhalt, sondern auch die Existenz der Nachricht zu verbergen.

👉 **Unterschied zur Kryptografie:**
- Kryptografie verschlüsselt eine Nachricht → sie ist unlesbar, aber ihre Existenz ist offensichtlich.
- Steganografie versteckt die Nachricht → niemand soll merken, dass eine Botschaft vorhanden ist.

**Moderne Beispiele sind:**
- Digitale Einbettung von Daten in Bilddateien (z. B. über die LSB-Methode, Least Significant Bit).
- Unsichtbare Wasserzeichen in Dokumenten oder Druckern.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Funktionsweise von Steganografie

- **Container (Trägermedium):** Datei, in der die geheime Nachricht versteckt wird (Bild, Audio, Text …)
- **Nachricht (Payload):** die Information, die verborgen werden soll (z. B. Text, Malware, Schlüssel).
- **Steganogramm:** das Endprodukt (Container + eingebettete Nachricht).

> Angreifer nutzen Steganografie oft, um schadhafte Payloads unauffällig in Medien zu verstecken.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Arten der Steganografie
| Art der Steganografie | Erläuterung                                                                  |
| --------------------- | ---------------------------------------------------------------------------- |
| **Text**              | Nachrichten in Textformatierungen, Abständen oder Unicode-Zeichen versteckt. |
| **Bild**              | Verändern einzelner Pixel (z. B. LSB-Methode, Farbmodulation).               |
| **Audio**             | Daten im Rauschen oder in unhörbaren Frequenzen eingebettet.                 |
| **Video**             | Kombination aus Bild- und Audio-Steganografie.                               |
| **Netzwerk**          | Versteckte Daten in Protokoll-Headern oder anderen Teilen von Netzwerkpaketen (selten genutzten Flags). |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Bekannte Methoden

- **Least Significant Bit (LSB):** Austausch der letzten Bits eines Pixels/Samples.
- **Spread Spectrum:** Nachricht über viele kleine Änderungen im Medium verteilt.
- **Maskierung & Filterung:** Nachricht im Rauschen, Wasserzeichen oder Transparenz versteckt.
- **Linguistische Steganografie:** Wörterwahl, Satzstruktur oder Tippfehler als Code.
- **Whitespace-Steganografie:** Nutzung von Leerzeichen, Tabs oder unsichtbaren Unicode-Zeichen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Vergleich mit Kryptografie
| Steganografie                                        | Kryptografie                                        |
| ---------------------------------------------------- | --------------------------------------------------- |
| Verbirgt die Existenz einer Nachricht                | Verbirgt den Inhalt einer Nachricht                 |
| Kann zusätzlich verschlüsselte Nachrichten enthalten | Erzeugt „sichtbar verschlüsselte“ Daten             |
| Wird oft mit Kryptografie kombiniert                 | Allein eingesetzt leicht als „verdächtig“ erkennbar |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Anwendungsbereiche

1. **Malware / Cybercrime:** Kriminielle nutzen Steganografie, um Schadsoftware oder andere bösartige Codes unauffällig zu übertragen.
2. **Geheime Kommunikation:** Menschen nutzen sie, um Informationen zu übermitteln, ohne dass Dritte Verdacht schöpfen.
3. **Wasserzeichen & Copyright:** Moderne Drucker können unsichtbare Codes auf Ausdrucke drucken, die durch Steganografie Daten enthalten.
4. **CTFs / Pentesting:** Beliebtes Thema in CTF-Challenges.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Steganalyse

Die Steganalyse ist aus dem Wort Steganografie und Analyse entstanden. Dabei nutzen Analysten verschiedene Tools und Verfahren, damit die versteckten Nachrichten gefunden werden können.

Analysten können auf breit aufgestellte Analystools (z. B. Exif-Tool, HexEd.it) zurückgreifen, um Anomalien in Dateien zu erkennen, die auf versteckte Informationen hindeuten könnten.

Typische Ansätze sind:
- **Statistische Analyse:** Pixelmuster, Bitverteilungen oder Rauschanomalien prüfen.
- **Hash-/Checksum-Vergleich:** Abweichungen von Originaldateien erkennen.
- **Machine Learning:** Training von Modellen zur Erkennung manipulierter Dateien.
- **Visuelle Analyse:** Bilder in anderen Farbkanälen betrachten (z. B. RGB-Splitting).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Tools und Links

- [Exif-Tool by Phil Harvey](https://exiftool.org/) - Metadaten-Analyse
- [hexed.it](https://hexed.it/) - Online Hex-Editor
- [Steghide](https://steghide.sourceforge.net/) - Stego-Tool (Bilder, Audio)
- [stegseek](https://github.com/RickdeJager/stegseek) - schneller Brute-Forcer für Steghide
- [stegsolve](https://kb.offsec.nl/tools/forensics/stegsolve/) - Analyse von Farbkanälen
- [zsteg](https://github.com/zed-0xff/zsteg) - PNG/BMP Steganalyse
- [WikiPedia: Steganografie](https://de.wikipedia.org/wiki/Steganographie#Open_Code)
- [kaspersky: What is Steganography?](https://www.kaspersky.de/resource-center/definitions/what-is-steganography)



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

