# Qubes OS

> Hier folgen bald mehr Infos zu `QubesOS`

- dom0 ist `adminVM` mit full-root-Rechten.
    - dom0 sollte abgeschottet sein von den anderen Containern.
    - dom0 sollte nicht geupdatet werden.
    - es wird nicht über `dom0` gebootet.
- Qubes ist geeignet für Cybersecurity, da es verschachtelte Container aufbaut.
- sys-firewall
-sys-usb für USB Ports !Nicht anfassen!
- sys-net ist Mainnet aus der Netzwerkkarte. !Nicht anfassen!
    - fungiert als Netzwerk
    - StandaloneVM sys-net 
- QubesOs-Container laufen in Templates, die man erstellt bzw. konfiguriert.
    - Default-Stack um einen Rahmen zu schaffen.

- Templates erhalten keine Netzwerkverbindung (n/a)
- Vault hat kein Internet. Ist quasi die Datenbank mit Credentials, Textdateien.
    - Quasi die "Festplatte"



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
