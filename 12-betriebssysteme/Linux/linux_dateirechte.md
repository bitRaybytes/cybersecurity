# 🛡️ Linux-Dateirechte: Grundlagen und erweiterte Konzepte


## Inhaltsverzeichnis
- [1. Das Linux-Zugriffsmodell: UGO (User, Group, Other)](#1-das-linux-zugriffsmodell-ugo-user-group-other)
- [2. Die Standardrechte: rwx (Read, Write, Execute)](#2-die-standardrechte-rwx-read-write-execute)
    - [2.1 Symbolische und Numerische Darstellung](#21-symbolische-und-numerische-darstellung)
    - [2.2 Oktale Notation (chmod)](#22-oktale-notation-chmod)
- [3. Die Rechte-Maske `umask` (Default Permissions)](#3-die-rechte-maske-umask-default-permissions)
- [4. Die 10-stellige Darstellung (`ls -l`)](#4-die-10-stellige-darstellung-ls--l)
- [5. Symbolische Rechtevergabe (`chmod` mit u/g/o/a)](#5-symbolische-rechtevergabe-chmod-mit-ugoa)
    - [5.1 Die Operatoren](#51-die-operatoren)
    - [5.2 Praxisbeispiele für symbolische Rechte](#52-praxisbeispiele-für-symbolische-rechte)
- [6. Verwaltung von Eigentümer und Gruppe (`chown` / `chgrp`)](#6-verwaltung-von-eigentümer-und-gruppe-chown--chgrp)
    - [6.1 `chown` (Change Owner)](#61-chown-change-owner)
    - [6.2 chgrp (Change Group)](#62-chgrp-change-group)
- [7. Spezialrechte (Extended Permissions)](#7-spezialrechte-extended-permissions)
- [8. Erweiterte Zugriffssteuerung: ACLs](#8-erweiterte-zugriffssteuerung-acls)
- [9. Sicherheitsimplikationen & Angriffsvektoren](#9-sicherheitsimplikationen--angriffsvektoren)
- [10. Best Practices für sichere Rechtevergabe](#10-best-practices-für-sichere-rechtevergabe)
- [Fazit](#fazit)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 1. Das Linux-Zugriffsmodell: UGO (User, Group, Other)

Linux- und Unix-Systeme verwenden ein hierarchisches Berechtigungsmodell, um zu definieren, wer auf eine Datei oder ein Verzeichnis zugreifen darf. Dieses Modell basiert auf drei Hauptkategorien von Entitäten, bekannt als **UGO**:

| Kategorie | Kürzel | Beschreibung |
|-----------|--------|--------------|
| User (Eigentümer) | `u` | Der einzelne Benutzer, dem die Datei gehört (der Ersteller oder ein per `chown` zugewiesener Benutzer). |
| Group (Gruppe) | `g` | Die Gruppe von Benutzern, der die Datei zugewiesen ist. Alle Mitglieder dieser Gruppe erben dieselben Rechte. | 
| Other (Andere) | `o` | Alle anderen Benutzer auf dem System, die weder Eigentümer noch Mitglied der zugeordneten Gruppe sind. |
| All (Alle) | `a` | Ein Platzhalter, der für die gleichzeitige Anwendung von Rechten auf alle drei UGO-Kategorien steht. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 2. Die Standardrechte: rwx (Read, Write, Execute)

Jede der drei UGO-Kategorien kann eine Kombination aus drei primären Berechtigungen zugewiesen werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2.1 Symbolische und Numerische Darstellung

Die Rechte werden sowohl symbolisch (Buchstaben) als auch numerisch (oktale Werte) dargestellt. Die numerische Darstellung basiert auf einer binären Gewichtung $2^2=4, 2^1=2, 2^0=1$.

| Recht | Symbolisch | Numerischer Wert | Bedeutung |
|-------|------------|------------------|-----------|
| Lesen | `r` (Read) | $4$ | Inhalt anzeigen. |
| Schreiben | `w` (Write) | $2$ | Inhalt ändern/überschreiben. |
| Ausführen | `x` (Execute) | $1$ | Bei Dateien: Ausführen als Programm. Bei Verzeischnissen: In das Verzeichnis wechseln. | 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 2.2 Oktale Notation (`chmod`)

Die numerischen Werte werden addiert, um die Rechte für eine Kategorie zu definieren, und dann zu einem dreistelligen Code kombiniert, der die Rechte für User, Group und Other in dieser Reihenfolge festlegt.

| Beschreibung | chmod (oktal) | umask (oktal) | Symbolische Rechte |
|--------------|---------------|---------------|--------------------|
| Lesen, Schreiben, Ausführen (Vollzugriff) | $7 (4+2+1)$ | $0$ | `rwx` |
| Lesen und Schreiben | $6 (4+2)$ | $1$ | `rw-` |
| Lesen und Ausführen | $5 (4+1)$ | $2$ | `r-x` | 
| Nur Lesen | $4 (4)$ | $3$ | `r--` |
| Schreiben und Ausführen | $3 (2+1)$ | $4$ | `-wx` |
| Nur Schreiben | $2 (2)$ | $5$ | `-w-` |
| Nur Ausführen | $1 (1)$ | $6$ | `--x` |
| Keine Rechte | $0 (0)$ | $7$ | `---` |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### Beispiele für die Anwendung mit `chmod` (Oktal):

| Befehl | Resultierene Rechte (UGO) | Erklärung |
|--------|---------------------------|-----------|
| `chmod 755 script.sh` | `rwx r-x r-x` | Eigentümer: Alle Rechte ($7$); Gruppe & Andere: Lesen und Ausführen ($5$). |
| `chmod 640 secrets.txt` | `rw- r-- ---` | Eigentümer: Lesen/Schreiben ($6$); Gruppe: Nur Lesen ($4$); Andere: Keine Rechte ($0$). |




## 3. Die Rechte-Maske: `umask` (Default Permissions)

Die `umask` (User File-Creation Mask) ist ein essenzielles Sicherheitskonzept. Sie definiert die **maximalen Rechte**, die abgezogen werden sollen, wenn eine neue Datei oder ein neues Verzeichnis erstellt wird.

`umask` wird typischerweise in den Shell-Startskripten (`.bashrc`, `.profile`) gesetzt und ist der Schlüssel zur Sicherstellung, dass neu erstellte Daten standardmäßig restriktiv sind.

### Funktionsweise der `umask`
1. **Basisrechte (Maximalrechte):**
    - **Dateien:** Maximal $666$ (`rw-rw-rw-`). Ausführungsrechte ($1$) werden standardmäßig entfernt, um zu verhindern, dass Skripte unbeabsichtigt ausgeführt werden.
    - **Verzeichnisse:** Maximal $777$ (`rwxrwxrwx`).
2. **Subtraktion:** Die umask wird von den Basisrechten subtrahiert.

### Beispiel:
| Eigenschaft | Oktal | Erklärung |
|-------------|-------|-----------|
| Aktuelle umask | `0022` oder `022` | Zieht $w$ von Gruppe und Andere ab. |
| Verzeichnis Basis | $777$ | (`rwxrwxrwx`) |
| Resultierende Rechte | $755$ ($777$-$022$) | Neue Verzeichnisse: `rwxr-xr-x` (Sicherer Standard). |
| Datei Basis | $666$ | (`rw-rw-rw-`)|
| Resultierende Rechte| $644$ ($666$-$022$) | Neue Dateien: `rw-r--r--` (Sicherer Standard). |

**Security-Hinweis:** Eine `umask` von `000` (z. B. auf einem Server) würde dazu führen, dass alle neuen Dateien mit $666$ erstellt werden, was ein hohes Sicherheitsrisiko darstellt (**World-Writable**).

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 4. Die 10-stellige Darstellung (`ls -l`)

Die Rechte einer Datei werden am deutlichsten durch den Output des Befehls `ls -l` dargestellt. Die ersten zehn Zeichen beschreiben die Zugriffsrechte:

```text
d rwx r-x r-- . 1 user group 4096 Jul 10 10:00 directory/
| | | |
| | | V-- 3. Block: Andere (Other) -> r--
| | V--- 2. Block: Gruppe (Group) -> r-x
| V---- 1. Block: Eigentümer (User) -> rwx
V------ 1. Zeichen: Dateityp -> d (Directory)
```

| Position | Zeichen | Bedeutung |
|----------|---------|-----------|
| 1. | `d`/`-`/`l` | Dateityp (`d`=Verzeichnis, `-`=Datei, `l`=symbolischer Link) |
| 2.-4. | `rwx` | Eigentümer-Rechte |
| 5.-7. | `r-x` | Gruppen-Rechte |
| 8.-10. | `r--` | Andere-Rechte |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 5. Symbolische Rechtevergabe (`chmod` mit u/g/o/a)

Die symbolische Methode verwendet Buchstaben (`u`, `g`, `o`, `a`) und Operatoren (`+`, `-`, `=`), um Rechte hinzuzufügen, zu entfernen oder neu zu setzen. Dies ist nützlich, wenn man nur eine bestimmte Berechtigung ändern möchte, ohne die anderen zu überschreiben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 5.1 Die Operatoren

| Operator | Bedeutung |
|----------|-----------|
| `+` | Fügt das angegebene Rechte hinzu (Addition). |
| `-` | Entfernt das angegebene Recht (Substraktion). |
| `=` | Setzt die Rechte für die angegebene Kategorie exakt auf diesen Wert (Überschreiben). |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 5.2 Praxisbeispiele für symbolische Rechte
| Befehl | Erklärung | Ergebnis auf  `rwxrwxr-x` |
|--------|-----------|---------------------------|
| `chmod u+x file.txt` | Fügt dem Eigentümer das Ausführungsrecht hinzu. | Wenn $u$ kein $x$ hatte: z.B. `rw-rwxr-x` → `rwxrwxr-x` |
| `chmod g-w projekt/` | Entfernt das Schreibrecht von der Gruppe. | `rwxrwxr-x` → `rwxr-xr-x` |
| `chmod o=rwx file.txt` | Setzt die Rechte für Andere explizit auf rwx. | Unabhängig von vorherigen: `...r--` → `...rwx` |
| `chmod a+w file.txt` | Fügt allen (User, Group, Other) das Schreibrecht hinzu. | Oktal: $777$ (`rwx` `rwx` `rwx`) |
| `chmod u=rw,g=r,o= file` | Setzt Eigentümer auf $6$, Gruppe auf $4$, Andere auf $0$. | Oktal: $640$ (`rw-` `r--` `---`) |





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 6. Verwaltung von Eigentümer und Gruppe (`chown` / `chgrp`)

Um die Kontrolle über eine Datei zu übernehmen, müssen Sie den Eigentümer und/oder die zugehörige Gruppe ändern. Diese Befehle erfordern typischerweise Root-Rechte (durch `sudo`).




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 6.1 `chown` (Change Owner)

Ändert den Eigentümer einer Datei oder eines Verzeichnisses.

| Befehl | Syntax | Erklärung |
|--------|--------|-----------|
| Eigentümer ändern | `sudo chown [neuer_eigentümer] [datei]` | Ändert den Eigentümer. |
| Eigentümer und Gruppe ändern | `sudo chown [eigentümer]:[gruppe] [datei]` | Ändert sowohl den Eigentümer als auch die Gruppe. |
| Nur Gruppe ändern | `sudo chown :[neue_gruppe] [datei]` | Ändert nur die Gruppe (ist äquivalent zu `chgrp`). |
| Rekursiv | `sudo chown -R [eigentümer]:[gruppe] [verzeichnis]` | Wendet die Änderung auf das Verzeichnis und alle Unterelemente an. |




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

#### Praxisbeispiele für `chown`:

```bash
# Ändert den Eigentümer von 'web_app.log' zu 'admin'
sudo chown admin web_app.log

# Ändert Eigentümer zu 'devops' und Gruppe zu 'projekte'
sudo chown devops:projekte config.yaml

# Ändert die Gruppe aller Dateien im Ordner 'data' rekursiv zu 'archiv'
sudo chown -R :archiv data/
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### 6.2 chgrp (Change Group)

Ändert nur die Gruppe einer Datei oder eines Verzeichnisses.

```bash
# Ändert die Gruppe von 'report.pdf' zu 'manager'
sudo chgrp manager report.pdf
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 7. Spezialrechte (Extended Permissions)

Zusätzlich zu den Standardrechten können vier spezielle Oktalwerte (von $0$ bis $7$) vor den UGO-Rechten hinzugefügt werden, um den vierstelligen Oktalcode zu bilden (z. B. `chmod 4755`).

| Recht | Numerischer Wert | Anwendung |
|-------|------------------|-----------|
| SUID (Set User ID) | $4$ | Bei der Ausführung wird das Programm mit den Rechten des Dateieigentümers (meist Root) ausgeführt. |
| SGID (Set Group ID) | $2$ | Bei Verzeichnissen: Neue Dateien erben die Gruppe des Verzeichnisses. |
| Sticky Bit | $1$ | Nur der Eigentümer einer Datei kann diese in einem Verzeichnis löschen oder umbenennen, selbst wenn andere Schreibrechte haben (wichtig für `/tmp`). |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Visuelle Darstellung: Aufbau der Zugriffsarten:
```text
 ________________________________________________________
| SUID | SGID | STID | r | w | x | r | w | x | r | w | x |
|--------------------|-----------|-----------|-----------|
|  Spezielle Rechte  |   Owner   |   Group   |   Other   |
|____________________|___________|___________|___________|
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Beispiel für SUID-Setzung (vierstellig):

```bash
# Setzt SUID (4) + rwx (7) für User, r-x (5) für Group/Other
# Resultat: -rwsr-xr-x
sudo chmod 4755 priv_script.sh
```

> **Achtung:** Ein falsch gesetztes SUID kann zu Privilege Escalation führen!



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Beispiel: Sticky Bit

```bash
sudo chmod 1777 /shared/tmp/
# drwxrwxrwt
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 8. Erweiterte Zugriffssteuerung: ACLs
Das traditionelle UGO-Modell hat eine Einschränkung: Es erlaubt nur eine Benutzergruppe. Eine **Access Control List** (**ACL**) bietet eine erweiterte, granulare Zugriffsverwaltung, die es ermöglicht, Rechte für beliebige zusätzliche Benutzer oder Gruppen zu definieren.

ACLs sind für komplexe Server-Umgebungen unerlässlich, in denen mehr als eine Gruppe auf bestimmte Ressourcen mit unterschiedlichen Berechtigungen zugreifen muss.



| Befehl | Funktion |
|--------|----------|
| `getfacl` | Zeigt die aktuellen ACLs für eine Datei oder ein Verzeichnis an. |
| `setfacl` | Ändert die ACLs (Hinzufügen/Entfernen von Benutzern/Gruppen). |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Beispiel für ACL-Setzung:

```bash
# Erlaubt dem Benutzer 'auditor' vollständigen Zugriff auf das Log-Verzeichnis
setfacl -m u:auditor:rwx /var/log/app/
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 9. Sicherheitsimplikationen & Angriffsvektoren

Fehlerhafte Berechtigungen sind eine der häufigsten Ursachen für Privilege Escalation und Data Exposure in Linux-Systemen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Typische Schwachstellen:

| Art | Beschreibung | Beispiel |
|-----|--------------|----------|
| World-Writable Files | Jeder Benutzer kann Datei verändern. | `/etc/shadow` → `rw-rw-rw-` |
| SUID Root Binary | Benutzer kann Root-Rechte erlangen. | `chmod 4755 vulnerable_bin` |
| Offene Verzeichnisse | Andere Benutzer können fremde Dateien löschen. | Fehlendes Sticky-Bit in `/tmp` |
| Unsichere Skripte | Skript ruft Befehle mit Root-Rechten auf. | `bash`-SUID ohne Kontrolle | 



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Beispiel: SUID-Exploit (theoretisch)
Ein falsch gesetztes Binary kann missbraucht werden, um eine Shell mit Root-Rechten zu erhalten:

```bash
ls -l /usr/local/bin/vuln_bin
# -rwsr-xr-x 1 root root 12345 ...
/usr/local/bin/vuln_bin
# Exploitiert durch manipulierte Umgebungsvariablen
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 10. Best Practices für sichere Rechtevergabe

- **Prinzip der minimalen Rechte (Least Privilege)** → Gib nur die Rechte, die notwendig sind.

- **Vermeide `777`-Rechte** → Jeder hat Schreibrechte → großes Risiko.

- **SUID nur bei absoluter Notwendigkeit setzen** → z. B. `/usr/bin/passwd`, aber nicht bei benutzerdefinierten Tools.

- **Regelmäßige Rechte-Audits durchführen**

```Bash
sudo find / -perm -4000 -type f 2>/dev/null
sudo find / -perm -2000 -type f 2>/dev/null
```

- **Logische Trennung von Benutzergruppen und Anwendungen**

- **Verwendung von umask konfigurieren** → Steuert Standardrechte neuer Dateien (z. B. `umask 027` für restriktive Defaults).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Fazit

Ein tiefes Verständnis von Linux-Dateirechten ist essenziell für IT-Sicherheitsexperten. Fehlkonfigurationen in Rechten führen häufig zu Exploits, Data Leaks oder Privilege Escalations.
Richtig angewendet, stellen sie jedoch eine der wichtigsten Verteidigungslinien auf Systemebene dar.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Nützliche Links
- [UbuntuUsers.de: Rechte](https://wiki.ubuntuusers.de/Rechte/)
- [Muirlandoracle - Blog: Unix File Permissions](https://muirlandoracle.co.uk/2020/03/05/unix-file-permissions/#SUID)
- [UbuntuUsers.de: ACL](https://wiki.ubuntuusers.de/ACL/)



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