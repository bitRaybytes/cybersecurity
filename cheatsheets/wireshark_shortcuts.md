# 🦈 Wireshark Cheat Sheet

>Ein schneller Überblick über die wichtigsten Filter, Shortcuts & Features in Wireshark.

---

## 📍 Grundlagen

| Begriff       | Beschreibung                               |
| ------------- | ------------------------------------------ |
| **Wireshark** | Open-Source-Tool zur Netzwerkanalyse       |
| **Capture**   | Live-Mitschnitt von Netzwerkverkehr        |
| **PCAP**      | "Packet Capture" – Datei mit Netzwerkdaten |
| **Interface** | Netzwerkkarte, über die aufgezeichnet wird |

---

## 🎯 Capture Filter vs Display Filter

| Typ                | Erklärung                       | Beispiel                       |
| ------------------ | ------------------------------- | ------------------------------ |
| **Capture Filter** | Vor dem Mitschnitt angewendet   | `port 80`                      |
| **Display Filter** | Nach dem Mitschnitt zur Analyse | `http.request.method == "GET"` |

> ⚠️ Capture Filter sind in BPF-Syntax geschrieben (wie in tcpdump), 
Display Filter sind Wireshark-spezifisch.

---

## 🔍 Häufige Display Filter

- http
- tcp
- udp
- dns
- icmp
- ssl || tls
- ftp
- smtp
- dhcp

---

## 🧪 IP & Ports

- ip.addr == 192.168.0.1
- ip.src == 10.0.0.1
- ip.dst == 8.8.8.8
- tcp.port == 443
- udp.port == 53

---

## 🧪 Verbindungen & Sessions

- tcp.stream eq 0                 # Erstverbindung
- tcp.flags.syn == 1              # SYN-Pakete (Handshake)
- tcp.analysis.retransmission

---

## 🧪 TLS/SSL

- ssl.handshake
- tls.record.version
- tls.handshake.type == 1         # Client Hello

---

## ⚙️ Capture Filter (BPF Syntax)

- host 192.168.0.1
- net 192.168.0.0/24
- port 80
- tcp
- udp
- tcp port 443
- tcp or udp

---

## 💡 Nützliche Tastenkombinationen

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

---

## 🔧 Tools & Extras

| Funktion              | Beschreibung                                     |
| --------------------- | ------------------------------------------------ |
| **Follow TCP Stream** | Ganze Unterhaltung einer TCP-Verbindung anzeigen |
| **Statistics**        | Netzwerkstatistiken (Endpunkte, Gespräche etc.)  |
| **Name Resolution**   | IP-Adressen in Hostnamen umwandeln               |
| **Coloring Rules**    | Regeln zur farblichen Hervorhebung definieren    |

---

## 🗂️ Exportieren

| Format    | Beschreibung                  |
| --------- | ----------------------------- |
| `.pcapng` | Standardformat von Wireshark  |
| `.csv`    | Export von Paketlisten        |
| `.txt`    | Nur Text                      |
| `.json`   | Für automatisierte Auswertung |

---

## 🧪 Tipps für Anfänger

- Beginne mit Display Filter – sicherer & einfacher.
- Nutze Farben, um Protokolle visuell hervorzuheben.
- Aktiviere Name Resolution nur bei Bedarf (kann verlangsamen).
- Speicher deine Mitschnitte früh – sie werden schnell groß!
- Nutze tcp.stream eq X zum Verfolgen einzelner Verbindungen.

---

## 🔗 Nützliche Ressourcen

| Thema                   | Link                                                                                                     |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| Wireshark Handbuch      | [https://www.wireshark.org/docs/wsug\_html\_chunked/](https://www.wireshark.org/docs/wsug_html_chunked/) |
| Display Filter Referenz | [https://wiki.wireshark.org/DisplayFilters](https://wiki.wireshark.org/DisplayFilters)                   |
| Capture Filter Referenz | [https://wiki.wireshark.org/CaptureFilters](https://wiki.wireshark.org/CaptureFilters)                   |
| Sample Captures (PCAPs) | [https://wiki.wireshark.org/SampleCaptures](https://wiki.wireshark.org/SampleCaptures)                   |


---

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!