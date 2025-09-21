# Dateisystemberechtigungen: Der Grundstein der Host-Sicherheit

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Linux-Dateisystemberechtigungen (chmod, chown)](#linux-dateisystemberechtigungen-chmod-chown)
    - [Die drei Berechtigungsklassen](#die-drei-berechtigungsklassen)
    - [`chmod` (Change Mode)](#chmod-change-mode)
    - [`chown` (Change Owner)](#chown-change-owner)
    - [Wichtige Sicherheitsaspekte in Linux](#wichtige-sicherheitsaspekte-in-linux)
- [Windows-Dateisystemberechtigungen (ACLs)](#windows-dateisystemberechtigungen-acls)
    - [Grundlegende Konzepte](#grundlegende-konzepte)
    - [Verwaltung von ACLs](#verwaltung-von-acls)
    - [`icacls` (Command-Line)](#icacls-command-line)
    - [Wichtige Sicherheitsaspekte in Windows](#wichtige-sicherheitsaspekte-in-windows)
- [Haftungsausschluss](#haftungsausschluss)




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einführung
**Dateisystemberechtigungen** sind ein essenzieller Bestandteil der Host-Sicherheit. Sie kontrollieren, wer auf Dateien und Verzeichnisse zugreifen kann und welche Aktionen (Lesen, Schreiben, Ausführen) erlaubt sind. Eine korrekte Konfiguration verhindert, dass unbefugte Benutzer oder Programme auf sensible Daten zugreifen, diese ändern oder löschen.

Stell dir Berechtigungen wie die Schlüssel zu einem Safe vor. Nur wer den richtigen Schlüssel hat, kann den Safe öffnen und auf seinen Inhalt zugreifen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Linux-Dateisystemberechtigungen (chmod, chown)
In Linux werden Berechtigungen über eine Kombination aus Benutzern, Gruppen und anderen kontrolliert. Jede Datei und jedes Verzeichnis hat spezifische Rechte für diese drei Kategorien, die über die Befehle `chmod` und `chown` verwaltet werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Die drei Berechtigungsklassen

| Berechtigung | Symbol | Oktalwert | Bedeutung |
|--------------|--------|-----------|-----------|
| Lesen | `r` | `4` | Daten können gelesen werden. |
| Schreiben | `w` | `2` | Daten können geändert oder gelöscht werden. |
| Ausführen | `x` | `1` | Skripte können ausgeführt werden. |

Die Berechtigungen werden in einer 10-stelligen Zeichenkette dargestellt (z. B. `-rwxr-xr-x`). Das erste Zeichen beschreibt den Dateityp, gefolgt von drei Gruppen von Berechtigungen: **Benutzer**, **Gruppe** und **Andere**.

```text
 -  rwx r-x r-x
(1) (2) (3) (4)

1. Dateityp: (`-` = Datei, `d` = Verzeichnis, `l` = Link)
2. Benutzer-Berechtigungen: Rechte für den Eigentümer.
3. Gruppen-Berechtigungen: Rechte für die Gruppe des Eigentümers.
4. Andere-Berechtigungen: Rechte für alle anderen Benutzer.
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### `chmod` (Change Mode)

Der `chmod`-Befehl ändert die Dateiberechtigungen. Die Rechte können numerisch (Oktal) oder symbolisch zugewiesen werden.

- **Numerische Methode (Oktal):**
    - `chmod 755 datei.txt`
        - `7` (4+2+1) = `rwx` (Lesen, Schreiben und Ausführen für den Benutzer).
        - `5` (4+1) = `r-x` (Lesen und Ausführen für die Gruppe).
        - `5` (4+1) = `r-x` (Lesen und Ausführen für alle anderen).
- **Symbolische Methode:**
    - `chmod u+r, g-w, o=r datei.txt`
        - `u+r` (Benutzer hinzufügen => Lesen)
        - `g-w` (Gruppe entfernt => Schreiben)
        - `o=r` (Andere => nur Lesen)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### `chown` (Change Owner)

Der `chown`-Befehl ändert den Eigentümer (`user`) und die Gruppe (`group`) einer Datei.

- `chown user:group datei.txt`
- `chown root:webdev /var/www/html`



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Sicherheitsaspekte in Linux

- **Prinzip des geringsten Privilegs:** Gewähre nur die minimal notwendigen Berechtigungen.
- **Vorsicht bei `777`:** Das Setzen von `chmod 777` macht eine Datei oder ein Verzeichnis für jeden uneingeschränkt les-, schreib- und ausführbar. Das ist ein erhebliches Sicherheitsrisiko.
- **Das `sudo`-Privileg:** Beschränke die Anzahl der Benutzer mit `sudo`-Rechten auf ein absolutes Minimum.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Windows-Dateisystemberechtigungen (ACLs)

In Windows werden Berechtigungen über **Access Control Lists** (**ACLs**) verwaltet. Eine ACL besteht aus mehreren **Access Control Entries** (**ACEs**), die festlegen, welche Benutzer oder Gruppen welche Berechtigungen für eine bestimmte Ressource haben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Grundlegende Konzepte
- **Prinzipal:** Ein Benutzer, eine Gruppe oder ein Computer, dem Berechtigungen zugewiesen werden.
- **Berechtigungen:**
    - **Basisberechtigungen:** Lesen, Schreiben, Ausführen, Ändern, Vollzugriff.
    - **Spezialberechtigungen:** Fein granulierte Berechtigungen, z. B. nur das Löschen einer Datei erlauben.
- **Vererbung:** Berechtigungen können von übergeordneten Ordnern auf untergeordnete Dateien und Ordner vererbt werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Verwaltung von ACLs

ACLs werden hauptsächlich über die grafische Benutzeroberfläche (Rechtsklick > Eigenschaften > Sicherheit) oder über die Kommandozeilen-Tools `icacls` und `cacls` verwaltet.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### `icacls` (Command-Line)
Der `icacls`-Befehl ist das moderne und mächtigere Werkzeug zur Verwaltung von ACLs.

- **Anzeigen von Berechtigungen:**
    - `icacls C:\sensible_daten`
- **Vererbung deaktivieren und Berechtigungen löschen:**
    - `icacls C:\daten /inheritance:d /remove:jeder`
- **Vollzugriff für einen bestimmten Benutzer gewähren:**
    - `icacls C:\webserver /grant:r administrator:(F)`


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Sicherheitsaspekte in Windows

- **Standard-Konten:** Schränke die Berechtigungen von Standard-Benutzern ein und verwenden keine Administratorkonten für alltägliche Aufgaben.
- **"Jeder"-Gruppe:** Achte darauf, dass die Gruppe "**Jeder**" (`Everyone`) keine unnötig weitreichenden Berechtigungen hat.
- **Vererbung:** Sei dir der Vererbung von Berechtigungen bewusst. Unsichere Berechtigungen auf einem übergeordneten Ordner können die Sicherheit aller untergeordneten Dateien gefährden.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Nützliche Links
- [https://wiki.ubuntuusers.de/Rechte/](https://wiki.ubuntuusers.de/Rechte/)
- [https://learn.microsoft.com/de-de/windows/win32/fileio/file-security-and-access-rights](https://learn.microsoft.com/de-de/windows/win32/fileio/file-security-and-access-rights)

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