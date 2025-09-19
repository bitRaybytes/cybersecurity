# 📝 Pentest/CTF Report Template

## 1. Allgemeine Informationen
- Titel:
- Autor:
- Datum:
- Übung/Challenge/CTF:
- Zielsystem/Scope:
- Verwendete Tools:

## 2. Zielsetzung
- Kurze Beschreibung:
- Was war die Aufgabe / das Szenario?
- Was ist das angestrebte Ziel (z. B. Root-Rechte, Flag finden, Exploit testen)?

## 3. Reconnaissance (Informationsbeschaffung)
- Passive Recon: (z. B. whois, nslookup, OSINT)
- Active Recon: (z. B. nmap, dirb, nikto)
- Beispielausgabe (gekürzt):
```bash
nmap -sC -sV -p- 192.168.0.10

22/tcp   open  ssh     OpenSSH 7.9
80/tcp   open  http    Apache httpd 2.4.29
```

## 4. Enumeration
- Welche Dienste laufen?
- Versionen und mögliche Schwachstellen?
- Screenshots oder Code-Snippets:
```bash
gobuster dir -u [http://192.168.0.10](http://192.168.0.10) -w /usr/share/wordlists/dirb/common.txt
```

## 5. Exploitation
- Ausgeführte Schritte & Befehle
- Metasploit-Module oder manuelle Exploits
- Beispiel:
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.0.10
run
```

Ergebnis (z. B. Shell-Zugriff erhalten, Flag gefunden):

## 6. Post-Exploitation
- Welche Rechte erlangt? (User → Root)
- Persistenz / Lateral Movement getestet?
- Sensible Daten gefunden?

## 7. Ergebnisse / Flags
- Flag 1: THM{abcdef123456}
- Flag 2: HTB{root_access_granted}

## 8. Schutzmaßnahmen (optional)
- Patchen der Schwachstelle
- Härtung der Dienste
- Netzwerksegmentierung / Firewall-Regeln

## 9. Lessons Learned
- Was lief gut?
- Was war schwierig?
- Welche Tools/Techniken neu gelernt?

## 10. Anhang
- Vollständige Scans (nmap, gobuster, wfuzz)
- Screenshots
- Links / Referenzen

---------

## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

<div align=right>

[↑ Hoch](#-pentestctf-report-template)

</div>

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---