# 🧩 breakAndFixQuery

## 💥 Ziel dieser Datei

Diese Datei dient als kompakte Referenz zur **Analyse, Manipulation und Behebung von SQL-Queries** bei Sicherheitsanalysen – insbesondere im Rahmen von **SQL-Injection-Tests**.  
Sie zeigt typische Zeichen, mit denen eine SQL-Query absichtlich *gebrochen* oder *balanciert* werden kann, um Informationen zu extrahieren oder eine **kontrollierte Ausführung** zu ermöglichen.

---

## 🛠️ Wie man eine SQL-Query **bricht**

Diese Zeichen und Sequenzen werden verwendet, um bestehende SQL-Befehle **syntaktisch zu unterbrechen**, um eigene Payloads einzuschleusen. Das Ziel ist es, den ursprünglichen Query-String so zu manipulieren, dass man Zugang zu sensiblen Daten oder Kontrolle über die SQL-Engine erhält.

### Zeichen zum Brechen einer Query:

```sql
'        -- Einzelnes Hochkomma (String beenden)
"        -- Doppeltes Anführungszeichen
\        -- Escape-Zeichen
`        -- Backtick (für MySQL-Syntax)
#        -- Kommentarzeichen (MySQL)
'"       -- Kombination Hochkomma + Anführungszeichen
```

**Beispiel:**
`url`: mit ?id=2'

→ kann zu einem SQL-Fehler führen wie:

`sql`
```SELECT * FROM users WHERE id='2''; ```

=> Wenn Fehler sichtbar sind, können daraus Strukturinformationen über die Query oder Datenbank abgeleitet werden.

---

## 🔧 Wie man eine Query balanciert oder repariert

Sobald die Query unterbrochen wurde, ist es häufig erforderlich, sie korrekt zu balancieren, um eigene SQL-Kommandos einzuschleusen. Hier kommen sogenannte Fix-Operatoren zum Einsatz:

Zeichen & Konstrukte zum "Fixen" einer Query:
`sql`

| Zeichen | Erläuterung |
|:--------|:------------|
| --+     | Kommentar für MySQL, um restlichen Code auszukommentieren |
| -- -    | Gültige Variante eines MySQL-Kommentars |
| #       | Einzelnes Kommentarzeichen |
| /*      | Block-Kommentar (MySQL/SQL) |
| ;%00    | Prozent-Encoding für Null-Byte-Terminierung |


Beispiel mit Kommentar-Fix:
`url:` ```?id=1' --+```

→ Ergebnis (interpretiert von MySQL):
`sql`
```SELECT * FROM users WHERE id='1' --+'; ```

**Wichtig:** Alles nach --+ wird ignoriert → so kann man z. B. ein ' am Ende aushebeln.

---

## 🎯 Zusammenfassung: Angriffs- & Fix-Phasen

| Phase           | Ziel                                      | Beispiel    |
| --------------- | ----------------------------------------- | ----------- |
| Query "brechen" | Fehler provozieren / Injection erzwingen  | `'`, `"`    |
| Query "fixen"   | Query korrekt beenden & Payload ausführen | `--+`, `/*` |

---

## 🧪 Nützlich in Kombination mit

- [sqlInjectionToShell.md](/sqlInjection/SQLInjectionToShell.md) → zeigt komplette Angriffskette
- [UnionBasedAttack.md](/sqlInjection/UnionBasedAttack.md) → enthält Datenextraktion über UNION SELECT
- [Tools](/tools/) wie: Burp Suite, SQLMap, ZAP Proxy

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