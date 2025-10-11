# 🦈 Wireshark Cheat Sheet

> Ein schneller Überblick über die wichtigsten Filter, Shortcuts & Features in Wireshark.


## Inhaltsverzeichnis
- [Grundlagen](#grundlagen)
- [Capture Filter vs Display Filter](#capture-filter-bpf-syntax)
- [Häufige Display Filter](#häufige-display-filter)
- [IP & Ports](#ip--ports)
- [Verbindungen & Sessions](#verbindungen--sessions)
- [TLS/SSL](#tlsssl)
- [Capture Filter (BPF Syntax)](#capture-filter-bpf-syntax)
- [Nützliche Tastenkombinationen](#nützliche-tastenkombinationen)
- [Tools & Extras](#tools--extras)
- [Exportieren](#exportieren)
- [Tipps für Anfänger](#tipps-für-anfänger)
- [Nützliche Ressourcen](#nützliche-ressourcen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundlagen

| Begriff       | Beschreibung                               |
| ------------- | ------------------------------------------ |
| **Wireshark** | Open-Source-Tool zur Netzwerkanalyse       |
| **Capture**   | Live-Mitschnitt von Netzwerkverkehr        |
| **PCAP**      | "Packet Capture" – Datei mit Netzwerkdaten |
| **Interface** | Netzwerkkarte, über die aufgezeichnet wird |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Capture Filter vs Display Filter

| Typ                | Erklärung                       | Beispiel                       |
| ------------------ | ------------------------------- | ------------------------------ |
| **Capture Filter** | Vor dem Mitschnitt angewendet   | `port 80`                      |
| **Display Filter** | Nach dem Mitschnitt zur Analyse | `http.request.method == "GET"` |

> ⚠️ Capture Filter sind in BPF-Syntax geschrieben (wie in tcpdump), 
Display Filter sind Wireshark-spezifisch.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Häufige Display Filter
```bash
http
tcp
udp
dns
icmp
ssl || tls
ftp
smtp
dhcp
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## IP & Ports
```bash
ip.addr == 192.168.0.1
ip.src == 10.0.0.1
ip.dst == 8.8.8.8
tcp.port == 443
udp.port == 53
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Verbindungen & Sessions
```bash
tcp.stream eq 0                 # Erstverbindung
tcp.flags.syn == 1              # SYN-Pakete (Handshake)
tcp.analysis.retransmission
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## TLS/SSL

```bash
ssl.handshake
tls.record.version
tls.handshake.type == 1         # Client Hello
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Capture Filter (BPF Syntax)
```bash
host 192.168.0.1
net 192.168.0.0/24
port 80
tcp
udp
tcp port 443
tcp or udp
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Tastenkombinationen

| Shortcut           | Funktion                   |
| ------------------ | -------------------------- |
| `Ctrl + E`         | Start/Stop Capture         |
| `Ctrl + K`         | Einstellungen öffnen       |
| `Ctrl + F`         | Suchen                     |
| `Ctrl + Shift + F` | Erweiterte Suche           |
| `Ctrl + H`         | Paket folgen (TCP-Stream)  |
| `Ctrl + + / -`     | Zoom in/out                |
| `Ctrl + .`         | Nächstes Paket im Stream   |
| `Ctrl + ,`         | Vorheriges Paket im Stream |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Tools & Extras

| Funktion              | Beschreibung                                     |
| --------------------- | ------------------------------------------------ |
| **Follow TCP Stream** | Ganze Unterhaltung einer TCP-Verbindung anzeigen |
| **Statistics**        | Netzwerkstatistiken (Endpunkte, Gespräche etc.)  |
| **Name Resolution**   | IP-Adressen in Hostnamen umwandeln               |
| **Coloring Rules**    | Regeln zur farblichen Hervorhebung definieren    |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Exportieren

| Format    | Beschreibung                  |
| --------- | ----------------------------- |
| `.pcapng` | Standardformat von Wireshark  |
| `.csv`    | Export von Paketlisten        |
| `.txt`    | Nur Text                      |
| `.json`   | Für automatisierte Auswertung |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Tipps für Anfänger

- Beginne mit Display Filter – sicherer & einfacher.
- Nutze Farben, um Protokolle visuell hervorzuheben.
- Aktiviere Name Resolution nur bei Bedarf (kann verlangsamen).
- Speicher deine Mitschnitte früh – sie werden schnell groß!
- Nutze tcp.stream eq X zum Verfolgen einzelner Verbindungen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Ressourcen

| Thema                   | Link                                                                                                     |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| Wireshark Handbuch      | [https://www.wireshark.org/docs/wsug\_html\_chunked/](https://www.wireshark.org/docs/wsug_html_chunked/) |
| Display Filter Referenz | [https://wiki.wireshark.org/DisplayFilters](https://wiki.wireshark.org/DisplayFilters)                   |
| Capture Filter Referenz | [https://wiki.wireshark.org/CaptureFilters](https://wiki.wireshark.org/CaptureFilters)                   |
| Sample Captures (PCAPs) | [https://wiki.wireshark.org/SampleCaptures](https://wiki.wireshark.org/SampleCaptures)                   |





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
