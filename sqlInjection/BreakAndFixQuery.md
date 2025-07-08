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

