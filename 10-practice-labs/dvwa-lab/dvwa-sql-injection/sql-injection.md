#  DVWA-Challenge SQL-Injection 
**Security Level: low**

## Inhaltsverzeichnis

1. [Docker mit DVWA starte](#-1-docker-mit-dvwa-starten)
2. [Auf DVWA im Browser zugreifen](#-2-auf-dvwa-im-browser-zugreifen)
3. [Kleines DVWA Setup](#-3-kleines-dvwa-setup)
4. [Challenge SQL-Injection Accepted](#-4-challenge-sql-injection-accepted)
5. [Haftungsausschluss](#️-haftungsausschluss)


## 🐳 1. Docker mit DVWA starten

**Hinweis:** Um Docker zu starten, ist es notwendig, das Programm herunterzuladen und zu installieren. Wie du Docker und alle Abhängigkeiten auf Kali Linux herunterlädts, [erfährst du hier](/cybersercurity/09-tools-cheatsheet/docker-infos.md). 

Wir arbeiten mit Docker, um DVWA in einer sicherer Umgebung zu testen.
Für die Applikation wählen wir den Port 80. Sollte dieser nicht funktioniere, probiere 8080

Gehe hierzu in dein Kali Linux Terminal und gib folgenden Befehl ein:

```bash
docker run --rm -ir -p 80:80 vulnerables/web-dvwa
```

![Docker Containter mit DVWA starten](/10-practice-labs/ressources/pictures/dockerStart.png)

Das lädt die Image-Datei direkt im Docker-Container-
Wenn alles erfolgreich war, dann solltest du nun dieses Terminal vorfinden:

![DVWA erfogreich im Docker gestartet](/10-practice-labs/ressources/pictures/dockerDvwaRun.png)

Du kannst im nächsten Schritt deinen Browser öffnen.

---

## 🐞💻 2. Auf DVWA im Browser zugreifen

In der URL-Leiste kannst du nun folgendes Seite aufrufen:

```http
http://localhost:80
```

Wenn alles erfolgreich war, hast du nun Zugriff auf diese Seite:


![DVWA Login-Page](/10-practice-labs/ressources/pictures/dvwa-localhost-login.png)

---

## 🔧 3. Kleines DVWA Setup

Wenn du DVWA zum ersten Mal nutzt bzw. in deiner VM das erste Mal installierst, dann musst du zunächst eine "Datenbank" kreieren.
Klicke hierzu auf der `/login.php`-Seite auf den Button `login`


Das ist einfacher als es klingt. Klicke dazu einfach auf Login und du kommst auf die `setup.php`-Webseite.
Scrolle hier einfach bis ganz nach unten und klicke anschließend auf den Butten `Create / Reset Database`.


![DVWA Datenbank initialisieren](/10-practice-labs/ressources/pictures/dvwa-createDb.png)


Nachdem du auf den Button `Create / Reset Database`geklickt hast, kannst du die `http://localhost/login.php` erneut laden und dich mit Benutzernamen und Passwort anmelden.

Hier noch einmal der Benutzername und das Passwort: `admin`:`password`.


Jetzt sollte die `index.php`-Seite laden und du kannst direkt mit deiner ersten Challenge anfangen. 
Klicke auf der linken Navigationsleiste auf den Reiter `SQL-Injection`:

![Challenge starten](/10-practice-labs/ressources/pictures/dvwa-sqi-start.png)

**Hinweis:** Über die Navigationsleiste kannst du auch die Einstellungen zu deinem `Security Level` ändern. Die Default-Einstellung ist `low`. Deine Einstellung siehst du unten links unter deinem Usernamen.

---

## 💉 4. Challenge SQL-Injection accepted

### 🔍 Überblick

1. In dieses Eingabefeld kannst du deine [SQL-Injections](/03-web-security/sql-injection/sql-injection-cheatsheet.md) eingeben.
2. Hilfreiche Links zum Thema SQL-Injections
3. Hilfe Buttons, wenn du mal nicht weiterkommst.
    - `View Source`: zeigt den Source Code der .php Datei an.
    - `View Help` : zeigt dir die Hilfe an; Vorsicht Spoiler Gefahr!

![Challenge starten](/10-practice-labs/ressources/pictures/dvwa-sqi-injection-info.png)

### View Help Page

Über den Button `View Help` erhältst du nähere Infos zu dieser Challenge. Hier erfährst du unter anderem, dass diese Challenge 5 User in der Datenbank hat, mit IDs von 1 bis 5 und deine Mission ist es, die Passwörter via SQL-Injection zu stehlen.


### 🧪 Erste Abfragen starten

Schauen wir uns die Webseite einmal an.
Wir sehen ein Eingabfeld und ein Button mit dem Namen `Submit`.
Links neben dem Eingabefeld steht der Begriff `User ID:`.

Ist das vielleicht ein Indiz darauf, dass wir eine Zahl, also eine ID eingeben können?
vielleicht auch einen Buchstaben? Probieren wir es aus:

Geben wir als erste irgendeinen Buchstaben ein. Sagen wir `a`, da dies der erste Buchstabe im Alphabet ist.

![SQL-Injection probieren](/10-practice-labs/ressources/pictures/dvwa-sqli-1.png)

Schauen wir uns mal an, was nach dem submitten der Daten passiert:

![SQL-Injection Auswertung Buchstabe](/10-practice-labs/ressources/pictures/dvwa-sqli-2.png)

Wie du siehst, verändert sich die URL, doch eine Fehlermeldung? Fehlanzeige. Also probieren wir mal das alles mit einer Zahl aus.

![SQL-Injection Auswertung Zahl](/10-practice-labs/ressources/pictures/dvwa-sqli-3.png)

Interessant!

Also mit der Zahl `1` haben wir ein Ergebnis erhalten.
Das Ergebnis, welches wir erhalten habt zeigt, dass in der Datenbank ein Datensatz mit der Zahl 1 die `ID: 1`, der `First Name: admin` und der `Surname: admin` verknüpft sind.

**Was heißt das jetzt?** 
Nichts - denn letztlich haben wir mit einer solchen Abfrage eine ganz normale und für die Anwendung vorgesehene Abfrage durchgeführt. Wir haben einfach eine ID verwendet und haben sogar herausgefunden, dass dieser Datensatz entpsrechend vorhanden ist.

Machen wir weiter!

### Beginnen wir mit der SQL-Injection

> Solltest du noch nicht wissen, was SQL-Injection sind, dann sieh dir bitte unseren kurzen Leitfaden zu diesem Thema an:
[SQL-Injection Cheat Sheet](/03-web-security/sql-injection/sql-injection-cheatsheet.md).

#### Ein einfacher Payload

Ein Apostroph `'` ist ein einfacher Payload. In der Syntax beendet ein Apostroph einen Eingabewert. Was wir damit herausfinden können?

Dadurch können wir vielleicht einen Fehler erzeugen und herausfinden, welche Syntax-Sprache für diese Art von SQL-Abfrage genutzt wird.

Hier erfährst du mehr über den [Break von SQLi](03-web-security/break-fix/break_and_fix_query.md)

Probieren wir es aus. Schreib ein `'` (Apostroph) in das Eingabefeld und drück auf `Submit`.

Du solltest nun folgenden Fehler erhalten:

![SQL-Injection Syntax Fehlermeldung](/10-practice-labs/ressources/pictures/dvwa-sqli-4.png)

Diese Fehlermeldung sagt, dass du einen Fehler in der MariaDB Syntax hast. Du Siehst auch im letzten Satz, wo der Syntax-Fehler liegt (`at Line 1`). Die Zeichenkette `'''''` (5x ') bedeutet, dass diese Abfrage (speziell nur das `'`) ein Fehler erzeugt.

Du kannst dir das so vorstellen:
Die Abfrage lautet vermutlich irgendwas mit:

```sql
SELECT Id, username, lastname FROM users WHERE ID=($id);
```
Wobei `$id` nur der Platzhalter für die Eingabevariable ist, die das Programm nach Übermittlung erhält und ihr den Wert zuweist.

#### Schauen wir uns einmal den Source 

Solltest du noch die Fehlermeldung der letzte Grafik sehen, gehe bitte eine Seite in deinem Browser zurück und lade die SQL-Injection-Seite.

Klicke anschließend unten rechts auf `View Source`, um dir den Source Code anzusehen. 

![SQL-Injection Source Code](/10-practice-labs/ressources/pictures/dvwa-sqli-5.png)

Wir wir sehen, hatte wir mit unserer SQL-Syntax nicht so ganz unrecht. Die richtige Syntax siehst du im Bild oben.

#### Iterieren wir bekannte SQL-Injections

Eine bekannte Abfrage ist `1' OR '1'='1`. Gib diese Abfrage in das Eingabefeld ein und submitte deine Anfrage. Was ist passiert?
![SQL-Injection ERfolgreicher Payload](/10-practice-labs/ressources/pictures/dvwa-sqli-6.png)

Erste Iteration, direkter Erfolg! Das ist Glück. In der Praxis sollte das natürlich kein Standard sein, da dies zu erheblichen Sicherheitseinbußen führen kann, wenn Angreifen durch Angriffe sensible Daten stehlen und sie ggfs. im Internet offenlegen bzw. verkaufen.

#### Herausfinden, wie viele Entitäten die Datenbank hat.

Finden wir mit dem `Order By`-Befehl heraus, wie viele Entitätstypen im SQL-Befehl abgefragt werden.

**Was sagt uns das?**
*Kurzer Exkurs zu Datenbank*

Eine Datenbank in SQL ist folgendermaßen aufgebaut:
Es existiert eine Datenbank XY, in der Datenbank-Tabellen vorhanden sind (Entitäten). 
Du kannst dir die Datenbank als Bibliothek vorstellen, die alle deine Informationen speichert, und auch ordnet, je nach dem, wie gut du mit Datenbanken umgehen kannst.

Beispiel: 

```text
Datenbank XY hat 3 Datenbanktabelle. User, Produkte, Rechnungen
```

Wenn du je mit Excel gearbeitet hast, dann kannst du dir vorstellen, wie eine Enität aufgebaut ist. Die Spalten oben in einem Excel-Arbeitsblatt (A, B, C usw.) repräsentieren die Spalten mit ihren jeweiligen Namen. Natürlich sind die Spalten (Entitätstypen) in einer Datenbanktabelle (Entität) individuell benannt, denn sie haben ihre bestimmten Zweck.

Diese Entitäten (User, Produkte, Rechnungen) haben nun ihre jeweiligen Spalten, die jeweilige Daten/Informationen gespeichert haben.

Zum Beispiel könnte die Datenbanktabelle `User` die Entitätstypen `| ID | Name | Nachname | E-Mail | Passwort |` beinhalten, und jede einzelne Zeile wäre ein Datensatz, dem ein bestimmter Wert zugewiesen ist.

Das bedeutet für uns, dass wir unseren SQL-Befehl individuell gestalten könnten, wenn wir Abfragen erzeugen möchten.
So könnte eine Abfrage beispielsweise aussehen:

```sql
SELECT Name, E-Mail, Passwort FROM User;
```

Beachte, dass diese Abfrage sehr einfach gehalten ist und auch keine weiteren Konditionen bzw. Bedingungen abgefragt werden.

Nur angenommen, wir haben 100 Datensätze, von denen wir nur bestimmte herausfiltern wollen und sie sogar in aufsteigender Reihenfolge soriteren.

Dazu verwendet man die `WHERE` und `ORDER BY`-Befehle (es gibt auch andere Befehle, aber für uns reicht das erst mal).
`WHERE` (zu Deutsch: Wo) sagt dem SQL-Befehl, in welchen Zeilen er in der Abfrage suchen soll und der `ORDER BY`-Befehl sortiert die Datensätze (*wichtig:* Ein Attribut muss für `ORDER BY` angegeben werden). Lass uns das einmal anschauen.

```sql
SELECT Name, E-Mail, Passwort FROM User WHERE user.name LIKE "Adm%" ORDER BY user.name ASC;
```

Der Befehl sagt: WÄHLE Name, E-Mail und Passwort (Entitätstypen) VON User (Datenbanktabelle), WO der Name vom User WIE "Adm%" ist und sortiere mir die Datensätze in aufsteigender Reihenfolge (ASC = Ascending). Gegenteil wäre DESC (Descending)

*Exkurs ENDE*
In einer SQL-Injection kann uns `Order By` helfen, herauszufinden, wie viele Spalten die Datenbankabfrage im Backend hat.
Mit dieser SQL-Injection erhalten wir die Anzahl, der in der SQL-Abfrage hinterlegten Entitätstypen (Spalten).

Probieren wir es direkt aus, gib dazu im Eingabefeld folgende Abfrage ein:
```sql
' Order By XY#
```
**oder**
```sql
' Order By XY--
```

Nach dem `Order By` iterieren wir mit Zahlen weiter. Also zum Beispiel`Order By 1--`. Die `--` leitet einen Kommentar ein, was bedeutet, dass alles nach dem Kommentar auf der rechten Seite nicht in der Syntax berücksichtigt wird. Beachte hier bitte, dass die Syntax zu dieser SQL-Sprache in `DVWA` einen Kommentar mit `#` einleitet. Richtig wäre in unserem Prozess zwar `' Order By 1#`, aber es ist nicht in jeder SQL-Syntax ein Kommentar.

**Aufgabe:** Probiere zuerst `' Order By 1--` aus und schau was passiert. Danach kannst du `' Order By 1#` verwenden.

**Beachte:** Je nach Syntax der bevorzugten Datenbank, musst du `#` (wie MariaDB) oder `--` (MySQL) probieren, um einen Kommentar nach deiner SQL-Injection einzuleiten.

Beispiel:
```sql
SELECT * From User Where user.name LIKE "Admin%" #AND user.password = "test";
```
Alles, in dieser Abfrage nach dem "Hashtag" kommt, wird im Interpreter ignoriert.

Hast du bemerkt, wie sich die URL verändert hat?

```http
localhost/vulnerabilitier/sqli/?id?'+order+by+1%23&Submit=Submit#
```

Dabei ist die Zeichenkette `%23` nur der Hexwert vom `#` ASCII-Zeichen.

Wenn du nun 1. `'Order By 1#` eingibst, erhältst du nur eine neue URL und die Seite verändert sich im Frontend nicht.
Das gleiche passiert, wenn du `'Order By 2#` eingibst.

Und siehe da, bei `'Order By 3#` ist Schluss und wir erhalten folgende Fehlermeldung:

![SQL-Injection Order By Fehlermeldung](/10-practice-labs/ressources/pictures/dvwa-sqli-8.png)


**Das heißt:** Unsere Datenbank-Abfrage hat also 2 Spalten, da die letzte Abfrage mit der `3#` eine Fehlermeldung erzeugt hat.

#### SQL-Injection UNION-Based-Attack

Hier findest du mehr zum Thema [UNION-BASED-ATTACKS](/14-vulnerabilities/sqlInjection/union_based_attack.md)

Nutzen wir jetzt einmal unser wissen, welches wir über eine einfache Recherche erhalten haben: dem Source Code.

Wir wissen bereits, dass die Abfrage in `php` folgendermaßen aufgebaut ist: 
```php
$query = "Select first_name, last_name FROM users WHERE user_id = '$id';"
```

Dabei ist das Abfrage-Statement in eine Variable namens `$query` gesetzt worden (vereinfacht Arbeiten am Code).

In dieser SQL-Injection Phase nutzen wir den `UNION`-Befehl, um über unsere Paylouds Zugriff auf die Datenbank zu erhalten oder direkt mehr Informationen herauszuholen.

Letztlich ist dies nur eine vereinfachte "Demonstration" einer SQL-Injection auf einer bereits vorhandenen Seite, die darauf ausgelegt ist. Lass uns aber 2-3 Befehle davor testen, um einfach zu sehen, wie sie der `UNION`-Befehl verhält.

Mit `UNION-Based-Attacks` kann man eine Menge über Datenbank herausfinden.

Du willst wissen, wie die Datenbanken heißt, in der du dich gerade befindest?
Gib dazu einfach in das Eingabefeld folgenden Befehl ein:

```sql
test' UNION SELECT 1,database() FROM information_schema.tables #
```

Dann solltest du folgende Information vorliegen haben:

![UNION Based Attack Payload](/10-practice-labs/ressources/pictures/dvwa-sqli-9.png)

Mit nachfolgendem Befehl findest du heraus, wie die Datenbanktabellen in der Datenbank heißen:
```sql
test' UNION SELECT 1, group_concat(schema_name) FROM information_schema.schemata #
```

![UNION Based Attack Payload](/10-practice-labs/ressources/pictures/dvwa-sqli-10.png)


Der dritte Befehl zeigt dir, wie die Spaltennamen in den vorgefundenen Datebanktabellen heißen:
Gib dazu im Eingabfeld folgenden Befehl ein:

```sql
test' UNION SELECT 1, group_concat(column_name) FROM information_schema.columns #
```

Jetzt solltest du folgendes erhalten:
![UNION Based Attack Payload](/10-practice-labs/ressources/pictures/dvwa-sqli-11.png)

Du siehst nun klar, wie die Spaltennamen heißen.
Das können wir für weitere Paylouds nutzen, um auf sensible Nutzerdaten Zugriff zu erhalten.

Fallen dir interesante Spaltennamen auf?
`Password` scheint doch ganz interessant zu sein. Dazu noch der `user` und Boom! du hast Zugriff auf die Passwörter aller User.

Probieren wir es direkt aus und beenden unsere SQL-Injection.

#### Der entscheidende Payloud

Was passiert, wenn wir die Spaltennamen nutzen und die im Befehl vorhandene Zahl `1` sowie `group_concat(column_name)` ersetzen mit folgendem Befehl:

```sql
test' UNION SELECT user, password FROM users 
```

Sieh an, was wir herausgefunden haben:

![UNION Based Attack Payload](/10-practice-labs/ressources/pictures/dvwa-sqli-12.png)

Wir haben nun die 5 Datensätze herausgefunden und die Challenge damit erfolgreich bestanden.
Herzlichen Glückwunsch zu deiner ersten erfolgreichen SQL-Injection.

Folgende Schritte kannst du nun umsetzen:
- Weitermachen, mit dem, was du bisher an Erfahrung gesammelt hast.
- Docker beenden: im Kali Linux Terminal den Befehl `exit` eingeben.
- Einstellungen den Schwierigkeitsgrad deinem Wissenstand anpassen und erneut testen.

---

## ⚠️ Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 

Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** Juli 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
