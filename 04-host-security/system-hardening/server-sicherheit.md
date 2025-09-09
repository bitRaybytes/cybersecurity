# 🛡️ System-Hardening: Die Absicherung von Servern
## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Grundlegende Prinzipien des Hardening](#grundlegende-prinzipien-des-hardening)
- [Linux Server-Hardening](#linux-server-hardening)
    - [1. Benutzerverwaltung und Berechtigungen](#1-benutzerverwaltung-und-berechtigungen)
    - [2. Netzwerk- und Firewall-Konfiguration](#2-netzwerk--und-firewall-konfiguration)
    - [3. Paket- und System-Updates](#3-paket--und-system-updates)
    - [4. Auditing und Logging](#4-auditing-und-logging)
- [Windows Server-Hardening](#windows-server-hardening)
    - [1. Benutzerverwaltung und Richtlinien](#1-benutzerverwaltung-und-richtlinien)
    - [2. Dienste und Systemkonfiguration](#2-dienste-und-systemkonfiguration)
    - [3. Patch-Management und Software-Hygiene](#3-patch-management-und-software-hygiene)
    - [4. Auditing und Logging](#4-auditing-und-logging-1)
- [Automatisierung des Hardening-Prozesses](#automatisierung-des-hardening-prozesses)
    - [Praktische Beispiele](#praktische-beispiele)
- [Nützliche Links](#nützliche-links)
- [Haftungsausschluss](#haftungsausschluss)


## Einführung
System-Hardening ist ein proaktiver Prozess zur Verbesserung der Sicherheit eines Servers, indem Angriffsflächen reduziert werden. Es geht darum, das System so zu konfigurieren, dass es weniger anfällig für Cyberangriffe ist. Ein gehärtetes System entfernt unnötige Funktionen und Dienste, schließt bekannte Schwachstellen und implementiert strenge Sicherheitsrichtlinien.

Ein Server im Standardzustand ist wie eine offene Tür. Hardening macht daraus einen Panzerschrank.

```text
       +-------------------+
       |   Standard-Server |
       |  (Viele offene    |
       |   Ports, Dienste) |
       +-------------------+
               |
        (Hardening-Prozess)
               |
               v
       +-------------------+
       | Gehärteter Server |
       |  (Nur notwendige  |
       |   Dienste aktiv)  |
       +-------------------+
```

## Grundlegende Prinzipien des Hardening
System-Hardening basiert auf dem **Prinzip des geringsten Privilegs**. Das bedeutet, einem Benutzer, Dienst oder System nur die minimal notwendigen Berechtigungen zu geben, die für seine Funktion erforderlich sind.

- **Minimierung der Angriffsfläche:** Deaktivieren oder Deinstallieren aller nicht benötigten Dienste, Anwendungen und Komponenten.
- **Patch-Management:** Regelmäßiges Einspielen von Sicherheitsupdates und Patches, um bekannte Schwachstellen zu beheben.
- **Sichere Konfiguration:** Anpassung von System- und Anwendungseinstellungen an die sichersten verfügbaren Optionen.
- **Zugriffskontrolle:** Implementierung von starken Authentifizierungsmechanismen und Zugriffsrichtlinien.
- **Monitoring und Auditing:** Ständige Überwachung des Systems auf ungewöhnliche Aktivitäten und Speicherung von Protokollen (Logs) für die forensische Analyse.

## Linux Server-Hardening
### 1. Benutzerverwaltung und Berechtigungen
- **Passwortrichtlinien:**
    - Verwendung von `pam_pwquality` zur Erzwingung komplexer Passwörter.
    - Deaktivierung von Passwörtern mit Ablaufdatum für Systemkonten.
- **Sichere Anmeldeverfahren:**
    - Deaktivierung der Root-Anmeldung via SSH.
    - Verwendung von SSH-Schlüsselpaaren anstelle von Passwörtern.
- **Datei- und Verzeichnisberechtigungen:**
    - Setzen strenger Berechtigungen (`chmod`, `chown`).
    - Verwendung von `sticky bits` und `setuid`/`setgid` mit Vorsicht.

### 2. Netzwerk- und Firewall-Konfiguration
- **Dienste:** Deaktivierung nicht benötigter Dienste (`systemctl disable`).
- **Firewall:** Konfiguration von `iptables` oder `ufw`.
    - `ufw default deny incoming`
    - Erlauben nur benötigter Ports (z. B. 22/TCP für SSH).

### 3. Paket- und System-Updates
- **Automatische Updates:** Konfiguration von Unattended-Upgrades.
- **Verifizierung:** Nutzung von `gpg` und Paket-Signaturen.

### 4. Auditing und Logging
- **auditd:** Überwachung von Systemaktivitäten.
- **Zentrale Protokollierung:** Weiterleitung von Logs an ein SIEM-System.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Windows Server-Hardening
### 1. Benutzerverwaltung und Richtlinien
- **Passwortrichtlinien:**
    - Verwendung der **Group Policy** zur Erzwingung von Passwortkomplexität und Ablaufdatum.

- **Kontosperrung:**
    - Konfiguration von Kontosperrungsrichtlinien, um Brute-Force-Angriffe zu erschweren.

- **Lokale Administrator-Konten:**
    - Umbenennung oder Deaktivierung des Standard-Administratorkontos.

### 2. Dienste und Systemkonfiguration
- **Dienstemanager:** Deaktivieren von unnötigen Diensten.

- **Firewall:**
    - Konfiguration der Windows Defender Firewall (oder einer Drittanbieter-Firewall).
    - Festlegen strikter Inbound-Regeln.

- **Remote-Desktop (RDP):**
    - Absicherung von RDP durch Ändern des Standardports.
    - Erzwingen von Network Level Authentication (NLA).

### 3. Patch-Management und Software-Hygiene
- **Windows Update:** Sicherstellen, dass automatische Updates aktiv sind und zuverlässig installiert werden.
- **Anwendungssteuerung:** Nutzung von AppLocker oder Windows Defender Application Control (WDAC).


### 4. Auditing und Logging
- **Ereignisprotokolle:** Konfiguration der Überwachung, um kritische Ereignisse wie Anmeldefehler, Benutzeränderungen und Prozessstart zu protokollieren.

- **PowerShell:** Skripte zur Automatisierung von Audit-Aufgaben.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Automatisierung des Hardening-Prozesses

Manuelles Hardening ist fehleranfällig und zeitaufwändig.  Automatisierung ist daher unerlässlich.

- **Ansible:** Nutzt Playbooks, um Konfigurationen auf vielen Systemen gleichzeitig durchzuführen.
- **Puppet & Chef:** Client-Server-Architekturen zur Konfigurationsverwaltung.
- **Skripte:** Einsatz von Shell-Skripten (Bash, PowerShell) zur Automatisierung einfacher Hardening-Aufgaben.


### Praktische Beispiele
- **Linux:** Ein Bash-Skript, das alle nicht benötigten Pakete deinstalliert und die UFW-Firewall mit Standardregeln aktiviert.
- **Windows:** Ein PowerShell-Skript, das die RDP-Portnummer ändert und eine neue Firewall-Regel erstellt.


## Nützliche Links
- [https://de.wikipedia.org/wiki/H%C3%A4rten_(Computer)](https://de.wikipedia.org/wiki/H%C3%A4rten_(Computer))

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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---