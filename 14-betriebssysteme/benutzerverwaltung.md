# 📑 Benutzerverwaltung

Die Benutzerverwaltung ist ein zentraler Bestandteil jedes Betriebssystems.
Sie stellt sicher, dass Benutzer, Prozesse und Dateien sauber voneinander getrennt sind, Ressourcen geschützt bleiben und Zugriffe kontrolliert erfolgen.

## Inhaltsverzeichnis
- [Grundprinzipien](#grundprinzipien)
- [Arten der Authentifizierung](#arten-der-authentifizierung)
- [Passwortsicherheit](#passwortsicherheit)
- [Benutzerverwaltung im Netzwerk](#benutzerverwaltung-im-netzwerk)
- [Dateizugriff und Rechte](#dateizugriff-und-rechte)
- [ACLs - Access Control Lists](#acls---access-control-lists)
- [Remote-Zugriff und Verwaltung](#remote-zugriff-und-verwaltung)
- [Best Practices Benutzerverwaltung](#best-practices-benutzerverwaltung)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Grundprinzipien

- **Benutzerauthentifizierung (Login)**
    - → Jeder User hat eine eigene Arbeitsumgebung.

- **Abschirmung**
    - → Prozesse eines Benutzers können andere nicht beeinflussen.

- **Dateizugriff**
    - → Nur mit ausdrücklicher Erlaubnis des Besitzers.

- **Quota**
    - → Begrenzung von Speicherplatz, Nutzungszeit oder Ressourcen.

- **Abrechnungsinformationen**
    - → Logfiles erfassen Anmeldezeiten, CPU-Auslastung, Speicherverbrauch.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Arten der Authentifizierung

```text
+------------------+-----------------------------------+
| Faktor           | Beispiel                          |
+------------------+-----------------------------------+
| Wissen           | Passwort, PIN, Challenge-Response |
| Besitz           | Token, Smartcard, SecureID        |
| Inhärenz         | Fingerabdruck, Iris, Stimme       |
| Kombination      | 2FA / MFA                         |
+------------------+-----------------------------------+
```

- **Passwörter** → einfach, aber angreifbar (Phishing, Brute-Force).
- **Einmalpasswörter (OTP/TAN)** → kurzlebig, sicherer.
- **Token** → Smartcards, Hardware-Keys (z. B. YubiKey).
- **Biometrie** → Fingerprint, FaceID, Schreibdynamik.
- **MFA** → Kombination von Wissen + Besitz + Inhärenz.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Passwortsicherheit

- Mindestlänge: mind. 10–12 Zeichen (BSI/NIST).
- Speicherung: nur gehasht (z. B. bcrypt, Argon2).
- Keine Wiederverwendung über Accounts hinweg.
- Einsatz von Blacklist (verbotene Begriffe).
- Nur ändern, wenn Kompromittierung vermutet wird.
- Nutzung von Passwortmanagern empfohlen.

Beispiel Passwortsicherheit vs. Angriffszeit:

```text
+----------------+-------------------+
| Passwortlänge  | Angriffszeit*     |
+----------------+-------------------+
| 8 Zeichen      | ~ Stunden/Tage    |
| 10 Zeichen     | ~ Jahrzehnte      |
| 12 Zeichen     | ~ Jahrtausende    |
+----------------+-------------------+
* abhängig von Hashverfahren & Hardware
```

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Benutzerverwaltung im Netzwerk

- Arbeitsgruppe
    - → Keine zentrale Verwaltung, geeignet für kleine Netze.

- Domänen-Service
    - → Zentrale Verwaltung (z. B. Active Directory, LDAP, FreeIPA).
    - → Vorteile: Übersicht, Sicherheit, zentrale Policies.
    - → Nachteil: Abhängigkeit von zentralen Diensten.

Beispiel Zentrale Benutzerverwaltung:
```diff
+----------------------+
|   Domain Controller  |
+----------------------+
       |   |   |
  +----+   |   +----+
  |        |        |
Client1  Client2  Client3
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Dateizugriff und Rechte

**Linux-Berechtigungen (chmod):**
```lua
r (read)    = 4
w (write)   = 2
x (execute) = 1
```

Kombiniert für:

- Eigentümer
- Gruppe
- Andere

**Beispiele für Berechtigungsvergabe:**
- `chmod 755 file.txt` → rwx r-x r-x
- `chmod 721 file.txt` → rwx -w- --x
- `chmod 070 file.txt` → --- rwx ---
- `chmod 111 file.txt` → --x --x --x
- `chmod 760 file.txt` → rwx rw- ---

**Spezialrechte:**

- **SUID (4):** Ausführung mit Rechten des Eigentümers.
- **SGID (2):** Gruppenrechte vererben.
- **Sticky Bit (1):** Löschen nur durch Eigentümer.

**Aufbau der Zugriffsarten:**

```yaml
 ________________________________________________________
| SUID | SGID | STID | r | w | x | r | w | x | r | w | x |
|--------------------|-----------|-----------|-----------|
|  Spezielle Rechte  |   Owner   |   Group   |   Other   |
|____________________|___________|___________|___________|
```

**Beispielhafter Aufbau einer Ausgabe in der Shell:**
```text
user@hostSystem:~/Documents/foo/bar$ ll
drwxrwxrwx  1   user    512 Aug 12  15:21  /baz
-rwxr-xr--  1   user    33  Mär 13  09:12  file.txt 
| |  |  | 
| |  |  V
| |  V  Other -> nur Rechte zum Lesen r--
| V  Group -> Rechte nur zum lesen und ausführen r-x
| User -> Alle Rechte zum Lesen, Schreiben/Bearbeiten und Ausführen rwx
V
d = Verzeichnis (Directory), kein d, wenn Datei
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## ACLs - Access Control Lists

ACLs erlauben feinere Steuerung:

- Rechte für einzelne User statt nur Gruppen.
- Beispiel: Nur ein bestimmter User darf schreiben, Gruppe aber nicht.
- Unterstützt z. B. unter EXT4, XFS, Btrfs.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Remote-Zugriff und Verwaltung

-RAS (Remote Access Server) → klassisch über Einwahl + RADIUS.
-SSH → Standard für sichere Fernzugriffe, basiert auf Public-Key-Verfahren.
-RDP (Remote Desktop, Windows) → GUI-Zugriff, Auth. über Windows-Konten.
-RSAT → Admin-Tools für Windows-Domänen.
-Teamviewer / AnyDesk → Drittanbieter-Lösungen.

ASCII: SSH-Handshake (vereinfacht)

```arduino
Client -----> Server : Verbindung anfordern
Server -----> Client : Zertifikat senden
Client -----> Server : Authentifizierung (Passwort/Key)
Beide Seiten: Schlüssel für Sitzung aushandeln
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Best Practices Benutzerverwaltung

- Prinzip der minimalen Rechte (Least Privilege).
- Keine gemeinsamen Accounts.
- 2FA/MFA für kritische Accounts.
- Regelmäßige Log-Auswertung & Audits.
- Zeit- und Ortsbeschränkungen (z. B. nur Arbeitszeiten, bestimmte IPs).
- Einsatz zentraler Benutzerverwaltung in größeren Netzen.


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