# 🐧 Linux Masterclass - Daten-Pipelining für die Cybersicherheit

## Inhaltsverzeichnis
- [Einleitung: Die Notwendigkeit der Shell-Analyse](#einleitung-die-notwendigkeit-der-shell-analyse)
- [Grundlagen: Pipelining und Umleitung](#grundlagen-pipelining-und-umleitung)
    - [Die Pipe: | (Pipelining)](#die-pipe--pipelining)
    - [Umleitungen: > und >> (Redirection)](#umleitungen--und--redirection)
- [Interprozesskommunikation (IPC) für Forensiker](#interprozesskommunikation-ipc-für-forensiker)
- [Analyse-Werkzeuge und Prozessdiagnose](#analyse-werkzeuge-und-prozessdiagnose)

- [Stufe 1: Die Filter und Mustererkennung](#stufe-1-die-filter-und-mustererkennung)
    - [1.1 `grep`: Die drei Suchmodi (BRE, ERE, F)](#11-grep-die-drei-suchmodi-bre-ere-f)
    - [1.2 tr: Zeichenkonvertierung](#12-tr-zeichenkonvertierung)
- [Stufe 2: Textmodifikation und Feldextraktion](#stufe-2-textmodifikation-und-feldextraktion)
    - [2.1 `awk`: Der spaltenbasierte Berichtsgenerator](#21-awk-der-spaltenbasierte-berichtsgenerator)
    - [2.2 `sed`: Der Stream Editor](#22-sed-der-stream-editor)
- [Stufe 3: Sortierung und Massenverarbeitung](#stufe-3-sortierung-und-massenverarbeitung)
    - [3.1 `sort` & `uniq`: Aggregation von Duplikaten](#31-sort--uniq-aggregation-von-duplikaten)
    - [3.2 `xargs`: Der Befehls-Argument-Konstruktor](#32-xargs-der-befehls-argument-konstruktor)
- [Stufe 4: Spezialwerkzeuge für die Cybersicherheit](#stufe-4-spezialwerkzeuge-für-die-cybersicherheit)
    - [4.1 `cURL` & `wget`: HTTP-Interaktion für Pentester](#41-curl--wget-http-interaktion-für-pentester)
        - [`cURL` (Client URL Request Library)](#curl-client-url-request-library)
        - [`wget` (Web Get)](#wget-web-get)
- [4.2 `xxd`: Binäre Analyse und Reverse Engineering](#42-xxd-binäre-analyse-und-reverse-engineering)
- [Praxisbeispiel: Komplexer Log-Analyse-Pipeline](#praxisbeispiel-komplexer-log-analyse-pipeline)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung: Die Notwendigkeit der Shell-Analyse

In der Sicherheitsanalyse ist **Daten-Pipelining** das Fundament jeder automatisierten Untersuchung.   
Ob Malware-Funde, Netzwerk-Logs oder Speicher-Dumps – die Fähigkeit, **Textströme zu verketten, zu filtern und umzuleiten**, entscheidet über Effizienz und Präzision bei der Beweissicherung.

Beispielhafte Einsatzfelder:
- **Incident Response:** Analyse von `/var/log/auth.log` in Echtzeit mit Pipes und Filtern.  
- **Reverse Engineering:** Extraktion von Signaturen aus Binärdateien mit `xxd | grep`.  
- **Pentesting:** Automatisiertes Testen hunderter Hosts durch `xargs` und `nmap`.  




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundlagen: Prozesse, Streams und File Deskriptoren

Jeder Linux-Prozess hat standardmäßig **drei File Deskriptoren (FDs):**

| FD | Bezeichnung | Beschreibung |
|----|--------------|---------------|
| `0` | stdin | Standardeingabe (z. B. Tastatur oder Pipe) |
| `1` | stdout | Standardausgabe (z. B. Terminal oder Datei) |
| `2` | stderr | Standardfehlerausgabe (z. B. Terminal oder Logfile) |


> **Sicherheitsrelevanz:**  
> Angreifer leiten Fehlerausgaben oft um, um Spuren zu verwischen (`2>/dev/null`).  
> In der Forensik sollte man daher gezielt prüfen, **wohin stderr zeigt**.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Die Pipe: `|` (Pipelining)

Der **Pipelining-Operator** (`|`) verbindet den `stdout` eines Befehls direkt mit dem `stdin` des nachfolgenden Befehls ohne Zwischendatei. Dies ermöglicht die Bildung von Befehlsketten zur schrittweisen Datenverarbeitung.

- **Sicherheitsrelevanz:** Unverzichtbar für die Live-Analyse von Systemprozessen oder Log-Daten, ohne Zwischenergebnisse auf der Festplatte speichern zu müssen.

```bash
cat syslog | grep "CRON" | awk '{print $1, $2, $5}'
```
So entstehen **Einweg-Datenströme**, die temporär im Kernel-Buffer (`pipefs`) gespeichert sind.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Umleitungen: `>` und `>>` (Redirection)

Diese Operatoren leiten den `stdout` eines Befehls entweder in eine Datei oder den `stdin` von einer Datei um.
| Operator | Ziel | Beschreibung | Beispiel |
|----------|------|--------------|----------|
| `>` | stdout → Datei | Überschreibt Datei | `ls > output.txt` |
| `>>` | stdout → Datei | Hängt an Datei an | `ls >> output.txt` |
| `<` | stdin ← Datei | Liest aus Datei | `cat < input.txt` |
| `2>` | stderr → Datei | Umleitung von Fehlermeldungen | `grep root /etc/* 2>errors.log` |


#### Kombinierte Ausgabe
```bash
cmd > all.log 2<&1
```

Leitet sowohl `stdout` (**1**) als auch `stderr` (**2**) in dieselbe Datei.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Spezialbefehl `tee`: Ausgabe duplizieren
Mit `tee` lässt sich eine Ausgabe gleichzeitig anzeigen und speichern:

```bash
cat /var/log/auth.log | grep "Failed" | tee failed.log | wc -l
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Interprozesskommunikation (IPC) für Forensiker

### Named Pipes (FIFOs)

Neben anonymen Pipes (`|`) existieren **Named Pipes**, die als Datei unter `/tmp/` oder `/dev/` sichtbar sind:
```bash
mkfifo /tmp/sniff.pipe
tcpdump -w - > /tmp/sniff.pipe &
cat /tmp/sniff.pipe | strings | grep "HTTP"
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### UNIX Sockets & Prozessüberwachung

Viele Dienste (z. B. `syslog`, `sshd`, `nginx`) verwenden UNIX Domain Sockets zur Kommunikation.
Diese sind über `lsof` oder `ss -lx` sichtbar:

```bash
sudo lsof -U | grep nginx
ss -lx | grep docker
```

Im Incident Response oft entscheidend, um kompromittierte Dienste oder Backdoor-Sockets zu entdecken.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Analyse-Werkzeuge und Prozessdiagnose
### Dateideskriptoren sichtbar machen: `lsof` & `/proc`
```bash
lsof -p <PID>
ls -l /proc/<PID>/fd/
```
Jeder offene Stream eines Prozesses erscheint als Eintrag in `/proc/<PID>/fd/`.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Live-Debugging von Pipelines

Mit `ps`, `pstree` und `/proc` kann man aktiv laufende Pipelines untersuchen:
```bash
ps -ef | grep '[p]ipe'
pstree -p | grep bash
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Stufe 1: Die Filter und Mustererkennung

Diese Befehle sind für die erste Stufe der Datenanalyse zuständig: die schnelle Reduzierung des Datenstroms auf relevante Zeilen oder Zeichen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 1.1 `grep`: Die drei Suchmodi

`grep` (`Global Regular Expression Print`) ist der primäre Filterbefehl. Seine Effizienz hängt stark von der korrekten Verwendung der Regulären Ausdrücke (`Regex`) ab.

| Option | Alias | Name | Regex-Typ | Beschreibung & Relevanz |
|--------|-------|------|-----------|-------------------------|
| (Standard) | N/A | `grep` | BRE (Basic Regex) | Klammern `()` und Quantifizierer `{}` müssen mit Backslash maskiert werden (z. B. `\(\)` ). |
| `-E` | `egrep` | Extended `grep` | ERE (Extended Regex) | Ermöglicht die Verwendung komplexer Muster. |
| `-F` | `fgrep` | Fixed `grep` | Feste Zeichenkette | Sucht nach String, nicht nach Mustern. Am schnellsten, ideal für die Suche nach bekannten Hash-Werten oder IP-Adressen. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### BRE vs. ERE: Der Unterschied liegt in der Maskierung
| Funktion | BRE (Standard) | ERE (`grep -E`) |
| :--- | :--- | :--- |
| ODER-Verknüpfung | N/A | (`Pipe`) |
| Gruppierung | `\( \)` | `( )` |
| Quantifizierung | `\{1,3\}` | `{1,3}` |

#### Pentesting-Beispiele

1. Suche nach hartcodierten Anmeldeinformationen (`fgrep`):
```bash
grep -rF "password =" /var/www/html/ # Schnellsuche nach literalem String
```

2. Erweiterte Log-Filterung (egrep):
```bash
# Finde jeden Statuscode, der mit 4 oder 5 beginnt, und zeige 5 Zeilen Kontext davor
cat access.log | grep -E -B 5 "(4[0-9]{2}|5[0-9]{2})"
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 1.2 tr: Zeichenkonvertierung

**tr** (`Translate` or `delete characters`) arbeitet ausschließlich mit einzelnen Zeichen und wird zur Normalisierung oder Bereinigung von Daten verwendet.

- **Sicherheitsrelevanz:** Entfernen von Steuercodes, Normalisierung von Benutzernamen oder Konvertierung von Hashes vor dem Brute-Force-Angriff.

| Klasse | Beschreibung | Beispiel |
|--------|--------------|----------|
| `[:lower:]` | Kleinbuchstaben | `tr '[:upper:]' '[:lower:]'`: Groß nach Klein. |
| `[:digit:]` | Ziffern (0-9) | `tr -d '[:digit:]'`: Alle Zahlen löschen. |
| `[:punct:]` | Satzzeichen | `tr -d '[:punct:]'`: Entfernen von Sonderzeichen. |
| `[:space:]` | Whitespace (Leerzeichen, Tab) | `tr -s ' ' '\n'`: Reduziert mehrfache Leerzeichen auf Newlines. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele
1. Entfernen von Nicht-Hexadezimal-Zeichen aus einem Speicher-Dump:
```bash
cat mem_dump.txt | tr -cd '[:xdigit:]' # Behält nur 0-9 und a-f bei
```

2. Verschleiern eines Strings (ROT13-ähnlich):
```bash
echo "Secret" | tr 'a-zA-Z' 'n-za-mN-ZA-M'
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Stufe 2: Textmodifikation und Feldextraktion

Diese Befehle ermöglichen eine Transformation des Datenstroms in strukturierte oder veränderte Formate.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2.1 `awk`: Der spaltenbasierte Berichtsgenerator

`awk` ist ein vollständiger Textprozessor und ideal für die Verarbeitung von zeilen- und spaltenbasierten Daten.

**Syntax:** `awk [flags] 'BEGIN {commands} /pattern/ {action} END {commands}' [input file]`



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Wichtige Flags und Blöcke
| Flag | Beschreibung |
|------|--------------|
| `-F 'sep'` | Definiert den **Field Separator** (**FS**). Wichtig für CSV, Logfiles etc. |
| `-v var=val` | Definiert eine externe Variable, die innerhalb des Skripts verwendet werden kann. |
| `BEGIN{...}` | Code, der einmal vor dem Lesen der ersten Zeile ausgeführt wird (z. B. Header-Druck, Variablen-Setup). |
| `END{...}` | Code, der einmal nach dem Lesen der letzten Zeile ausgeführt wird (z. B. Zusammenfassung, Durchschnittberechnung). |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Interne Variablen

| Variable | Beschreibung | Sicherheitsrelevanz |
|----------|--------------|---------------------|
| `NR` | Number of Records (Aktuelle Zeilennummer) | Wird für die Zählung von Log-Einträgen oder das Hinzufügen einer ID verwendet. |
| `NF` | Number of Fields (Anzahl der Felder in der aktuellen Zeile). | Nützlich, um unvollständige Log-Einträge zu erkennen. |
| `FS` | Field Separator (Standard: Whitespace). | Muss oft auf Komma (CSV) oder Doppelpunkt (z. B. `/etc/passwd`) gesetzt werden. |
| `OR` | Record Separator (Standard: Newline). | Kann z.B. auf ein doppeltes Newline gesetzt werden, um Absätze zu verarbeiten. |
| `OFS` | Output Field Separator (Standard: Leerzeichen). | Definiert das Trennzeichen zwischen den ausgegebenen Feldern. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele

1. Konvertierung der /etc/passwd Datei:
```bash
# Extrahiere Benutzer (Feld 1) und Shell (Feld 7) und formatiere als CSV
awk -F: 'BEGIN {OFS=","} {print $1, $7}' /etc/passwd
```

2. Erkennung von ungewöhnlich langen Requests im Log:
```bash
# Initialisiert die Zählung, druckt Zeilen (Anfrage > 15 Felder), beendet mit Summe.
awk '{if (NF > 15) {print "LANGER REQUEST: "$0; count++}} END {print "Total ungewöhnlich lange Requests: " count}' access.log
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 2.2 `sed`: Der Stream Editor

`sed` ist ein nicht-interaktiver Stream Editor zur zeilenweisen Transformation von Text. Es ist der Standardbefehl für das Suchen und Ersetzen.

Syntax: `sed [flags] 'adresse/pattern/command/arg' [input file]`



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Modi (Commands) und Argumente

| Command | Beschreibung |
|---------|--------------|
| `s` | **Substitute** (Ersetzen). Die am häufigsten verwendete Funktion. |
| `y` | **Yank**/**Transliterate** (Zeichen zu Zeichen ersetzen, ähnlich wie `tr`). |
| `d` | **Delete** (Löscht die aktuelle Zeile). |
| `p` | **Print** (Druckt die aktuelle Zeile, oft in Kombination mit `-n`). |


| Argument/Flag | Beschreibung |
|---------------|--------------|
| `/g` | **Global**: Ersetzt alle Vorkommen in der Zeile (Standard: nur das erste). |
| `/i` | **Ignore Case**: Case-Insensitive Suche. |
| `n` | **Number**: Ersetzt nur das n-te Vorkommen des Musters (z. B. `/2` ersetzt das zweite Vorkommen). |
| `1,5` | **Adressierung/Range**: Führt den Befehl nur für die Zeilen 1 bis 5 aus. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Command Semantik und Erklärungen

Der `sed`-Befehl verwendet standardmäßig BRE. Für fortgeschrittenes Ersetzen muss man Gruppen mit `\(\)` definieren.

- **Gruppierung (`\(\)` und Back-Referenzen `\1`, `\2`):** Erlaubt es, Teile eines gefundenen Musters zur Wiederverwendung zu speichern. Der Teil innerhalb der ersten Klammergruppe wird zu `\1`, der zweite zu `\2` usw.

**Beispielerklärung:** `sed 's/\(^\b[[:alpha:] ]*\)\([[:digit:]]*\)/\=\> \1\[\2\]/g' file.txt`

1. Muster-Teil 1 (`\1`): `\(^\b[[:alpha:] ]*\)`
    - `^`: Anker am Zeilenanfang.

    - `\b`: Wortgrenze (obwohl oft besser mit `\<` und `\>`).

    - `[[:alpha:] ]*`: Beliebige Buchstaben und Leerzeichen (0-mal oder öfter).

    - **Ergebnis:** Fängt den gesamten alphabetischen Teil am Zeilenanfang (z.B. den Namen) ein.

2. Muster-Teil 2 (`\2`): `\([[:digit:]]*\)`

    - `[[:digit:]]*`: Fängt direkt im Anschluss alle Ziffern (z.B. eine ID oder ein Alter) ein.

3. **Ersatz:** `\=\> \1\[\2\]`

    - Ersetzt den gesamten gefundenen String durch den String `=>` , gefolgt vom gespeicherten Namen (`\1`), und die Zahl in eckigen Klammern (`[\2]`).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele

1. Entfernen von Kommentaren und Leerzeichen aus Config-Dateien:
```bash
cat nginx.conf | sed '/^#/d;/^$/d'
# Löscht Zeilen, die mit # beginnen, und dann alle leeren Zeilen
```


2. Umwandeln von Log-Format zur Daten-Maskierung:
```bash
# Ersetzt alle IP-Adressen (z.B. 10.x.x.x) durch "PRIVATE_IP"
sed -E 's/(10\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3})/PRIVATE_IP/g' access.log
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Stufe 3: Sortierung und Massenverarbeitung
### 3.1 `sort` & `uniq`: Aggregation von Duplikaten

Diese Befehle werden fast immer zusammen verwendet, um Daten zu aggregieren und Zählungen durchzuführen.
Der Unterschied

- `sort`: Sortiert Zeilen basierend auf einem Sortierschlüssel. Zwingend erforderlich für die Funktion von `uniq`.

- `uniq`: Entfernt oder zählt aufeinanderfolgende identische Zeilen. Ohne vorheriges sort funktioniert die Zählung nicht korrekt, da Duplikate möglicherweise nicht direkt nacheinander stehen.


| Option | Befehl | Kurzbeschreibung | Anwendungsfall |
|--------|--------|------------------|----------------|
| `-n` | `sort` | Numerische Sortierung (wichtig für `uniq -c` Ausgaben). | Sortieren von Zähldaten (Häufigkeit). |
| `-r` | `sort` | umgekehrte Reihenfolge (Descending). | Finden der Top-N-Ergebnisse. |
| `-k N` | `sort` | Sortiert nach dem N-ten Schlüssel/Feld. | Sortieren nach Spalteninhalt in Logfiles. |
| `-c` | `uniq` | Zeigt die Anzahl der Vorkommen jeder Zeile an. | Ermitteln der Häufigkeit von IPs, User-Agents oder Fehlern. |
| `-d`| `uniq` | Zeigt nur die doppelt vorhandenen Zeilen an. | Identifizierung von wiederholten Log-Einträgen. |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele

1. Ranking der häufigsten Command Inejction-Payloads:
```bash
# Extrahiert alle Payload-Strings, sortiert, zählt Duplikate, sortiert nach Häufigkeit
cat requests.log | grep -E "ping|whoami|cat /etc/passwd" | sort | uniq -c | sort -nr
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.2 `xargs`: Der Befehls-Argument-Konstruktor

`xargs` ist der Schlüssel zur Skalierbarkeit. Es nimmt die Ausgabe von `stdin` (zeilenweise) und wandelt sie in Argumente für den nachfolgenden Befehl um.

Semantik: `xargs` ist oft notwendig, weil `cmd1 | cmd2` den `stdout` von `cmd1` als Input für `cmd2` sendet, aber viele Befehle Argumente (z.B. Dateinamen) in ihrer Kommandozeile erwarten.

| Option | Befehl | Anwendungsfall |
|--------|--------|----------------|
| `-I {}` | Definiert einen Platzhalter (hier: `{}`), der das Eingabeelement an beliebiger Stelle im Befehl platziert. | Muss verwendet werden, wenn das Argument nicht am Ende des Befehls stehen soll. |  
| `-P N` | Führt den nachfolgenden Befehl mit N parallelen Prozessen aus. | Massenscans, Multi-Threading für Brute-Force-Aktionen. |  
| `-n N` | Übergibt maximal N Argumente pro Befehlsaufruf. | Beschränkung der Anzahl der Dateien, die rm in einem Durchlauf löscht. |  
| `-L n` | Verarbeitet N Zeilen der Eingabe auf einmal. | Übergibt z.B. 10 IP-Adressen gleichzeitig an einen Scan-Befehl. |  
| `-0` | Verwendet das NULL-Zeichen als Trennzeichen. | Zwingend erforderlich in Kombination mit `find -print0`, um Probleme mit Dateinamen, die Leerzeichen enthalten, zu vermeiden. |  




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele:

1. Paralleler Subdomain-Scan:
```bash
# Liste der Subdomains wird zeilenweise übergeben, 20 parallel
cat subdomains.txt | xargs -I {} -P 20 sh -c 'host {} | grep "has address"'
```


2. Sichere Massenlöschung von alten Malware-Spuren:
```bash
# Verwendet -print0 und -0 zur Handhabung unsicherer Dateinamen
find /var/log/temp -type f -mtime +90 -print0 | xargs -0 rm -v
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Stufe 4: Spezialwerkzeuge für die Cybersicherheit
### 4.1 `cURL` & `wget`: HTTP-Interaktion für Pentester

Diese Befehle sind für die Interaktion mit Webdiensten in Skripten unerlässlich.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### `cURL` (Client URL Request Library)

Wird primär für das Senden benutzerdefinierter, komplexer Anfragen (POST, Header-Manipulation, Cookies) verwendet.

| Option | Befehl | Anwendungsfall |
|--------|--------|----------------|
| `-X METHOD` | Definiert die HTTP-Methode (GET, POST, PUT, etc.). | HTTP Verb Tampering. |
| `-H 'Header: Value'` | Fügt einen benutzerdefinierten Header hinzu. | Spoofing von User-Agent, Hinzufügen von Authorization Tokens, Host Header Injection. |
| `-d 'data'` | Sendet Daten im POST-Body. | Testing von API-Endpunkten und Formularübermittlungen. |
| `-I` | Ruft nur die Header ab (Head-Request). | Schnelle Statusprüfung und Analyse von Sicherheitsheadern (z. B. HSTS, CSP). |
| `-L` | Folgt HTTP-Weiterleitungen (3xx Statuscodes). | Wichtig bei der Überprüfung von Umleitungslogiken. |
| `-k` | Erlaubt unsichere SSL/TLS-Verbindungen. | Testen von Diensten mit selbstsignierten Zertifikaten. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


##### Pentesting-Beispiel: Testen einer Blind SQL Injection via Cookie-Header
```bash
# Sende eine Anfrage mit einem manipulierten Cookie zur Überprüfung auf Blind SQLi
curl -s -X GET '[https://target.com/index.php](https://target.com/index.php)' -H 'Cookie: tracking_id=123 OR 1=1'
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### `wget` (Web Get)

Wird primär für den rekursiven Download ganzer Webseiten oder das Spiegeln von Inhalten verwendet.

| Option | Befehl | Anwendungsfall |
|--------|--------|----------------|
| `-r` | Recursive (Rekursiver Download). | Download der gesamten Struktur eines Webservers. |
| `-l N` | Setzt die maximale Rekursionstiefe auf N. | Begrenzt die Tiefe beim Klonen einer Website. |
| `-p` | Page requisites (Läd alle notwendigen Dateien). | Stellt sicher, dass das geklonte Ziel lokal funktioniert. |
| `-nc` | No clobber (Überschreibt keine bestehenden Dateien). | Wichtig, um laufende Scans nicht zu unterbrechen. |
| `-A list` | Accept list (Akzeptiert nur Dateitypen in der Liste). | Suche nach bestimmten Dateien wie `.zip`, `.bak`, `.conf`. |


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


##### Pentesting-Beispiel: Suche nach öffentlich zugänglichen Konfigurationsdateien
```bash
# Rekursiv klonen, Tiefe 2, nur nach .bak und .conf suchen
wget -r -l 2 -A ".bak,.conf,.zip" [http://target.com/](http://target.com/)
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 4.2 `xxd`: Binäre Analyse und Reverse Engineering

`xxd` erstellt eine Hex-Dump-Darstellung einer Datei oder eines Datenstroms oder kehrt diesen Prozess um (Revert).

- **Sicherheitsrelevanz:** Analyse von Shellcode, Untersuchung von Malware-Binärdateien, Erkennung versteckter Zeichen in Textdateien.


| Option | Befehl | Anwendungsfall |
|--------|--------|----------------|
| `-p` | Plain Hex Dump (Keine Adressen, keine ASCII-Spalte). | Ideal zur direkten Konvertierung von Binärdaten in Strings zur weiteren Analyse oder zum Pipelining. | 
| `-r` | Revert (Kehrt den Prozess um). | Konvertiert einen Hex-String zurück in die Binärdatei. | 
| `-c N` | Setzt die Spaltenbreite auf N Bytes. | Formatierung der Ausgabe, um die Untersuchung von Shellcode-Blöcken zu erleichtern (z.B. `-c 16`). | 
| `-g N` | Gruppiert N Bytes in der Hex-Ausgabe. | Visuelles Hervorheben von Wortgrenzen (z.B. 4 Bytes). | 


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


#### Pentesting-Beispiele:

1. Shellcode zur direkten Ausführung vorbereiten:
```bash
# Nimmt den Hex-Dump, entfernt Leerzeichen/Newlines (tr), und führt ihn in eine Variable ein
SHELLCODE=$(cat shellcode.hex | xxd -p | tr -d '\n')
```

2. Überprüfung von Nicht-druckbaren Zeichen:
```bash
# Zeigt Hex- und ASCII-Darstellung einer Datei an
xxd /etc/shadow | less
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Praxisbeispiel: Komplexer Log-Analyse-Pipeline

Ziel: Finde die 10 häufigsten User-Agents von externen IPs, die in einem access.log einen Fehler (Status > 400) verursacht haben, und gib die Häufigkeit und den Agenten aus.

```bash
# 1. Datenquelle: Lese die Access-Log-Datei
cat access.log | \
# 2. Filter (awk): Verarbeite zeilenweise.
# $9 > 400 (Statuscode größer 400) UND $1 !~ /^192\.168\./ (KEINE internen IPs).
awk '($9 ~ /4[0-9]{2}|5[0-9]{2}/) && ($1 !~ /^192\.168\./) {
    # 3. Extraktion: Extrahiere nur den User-Agent ($12 bis zum Ende)
    for (i=12; i<=NF; i++) {
        agent = agent " " $i; 
    }
    print agent;
    agent = "" # Agent Variable zurücksetzen für die nächste Zeile
}' | \
# 4. Filter (tr): Entferne die doppelten Anführungszeichen am Anfang und Ende des Strings
tr -d '"' | \
# 5. Sortierung (sort): Sortiere die User-Agents alphabetisch
sort | \
# 6. Zählung (uniq): Zähle die Vorkommen jedes Agents und gib die Zählung davor aus
uniq -c | \
# 7. Sortierung (sort): Sortiere numerisch (n) und absteigend (r) nach der Zählspalte
sort -nr | \
# 8. Begrenzung (head): Gib nur die Top 10 Ergebnisse aus
head -10
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [TryHackMe: LinuxModules](https://tryhackme.com/room/linuxmodules)
- [GeeksForGeeks: Sed Command in Linux/Unix With Examples](https://www.geeksforgeeks.org/linux-unix/sed-command-in-linux-unix-with-examples/)
- [Basic vs Extended Regular Expressions](https://www.gnu.org/software/grep/manual/html_node/Basic-vs-Extended.html)
- [`cURL` - Linux Man Page](https://linux.die.net/man/1/curl)
- [`xargs` - Linux Man Page](https://linux.die.net/man/1/xargs)
- [`sort` - Linux Man Page](https://linux.die.net/man/1/sort)
- [`uniq` - Linux Man Page](https://linux.die.net/man/3/uniq)


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