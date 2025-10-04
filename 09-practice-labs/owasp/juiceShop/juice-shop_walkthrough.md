# OWASP - Juice Shop Walkthrough

> **Vorsicht: Spoiler Gefahr!**


## Inhaltsverzeichnis
- [Einleitung](#einleitung)
- [So startest du OWASP - Juice Shop](#so-startest-du-owasp---juice-shop)
- [Scoreboard und Herausforderungen](#scoreboard-und-herausforderungen)
- [Die erste XSS Herausforderung lösen](#die-erste-xss-herausforderung-lösen)
- [Haftungsausschluss](#haftungsausschluss)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Einleitung

Der [**OWASP Juice Shop:** https://owasp.org/www-project-juice-shop/](https://owasp.org/www-project-juice-shop/) ist eine bewusst unsichere Webanwendung, die als sichere Lernumgebung für **Webanwendungs-Sicherheit** dient. Das Projekt wird von der **Open Worldwide Application Security Project (OWASP)** Foundation gehostet und bildet die **OWASP Top 10** ab, eine Liste der kritischsten Sicherheitsrisiken für Webanwendungen.

Im Prinzip ist es ein Online-Shop, der eine Vielzahl realer Sicherheitslücken enthält. Dein Ziel ist es, diese Schwachstellen zu finden und auszunutzen, um die versteckten **Challenges** zu lösen. Das integrierte Scoreboard zeigt dir, wie weit du gekommen bist, ohne dir den Spaß am Entdecken zu nehmen.

Juice Shop ist ideal, um folgende Fähigkeiten zu trainieren:

- **SQL Injection:** Manipulieren von Datenbankabfragen.

- **Cross-Site Scripting (XSS):** Einschleusen von bösartigem Code.

- **Broken Authentication:** Umgehen von Anmeldeverfahren.

- **Insecure Direct Object References (IDOR):** Direkter Zugriff auf private Ressourcen.

**Hinweis:** Dieser Walkthrough ist kein vollständiger Guide, sondern zeigt nur einige Themen auf, um dir den Einstieg zu erleichtern. Der eigentliche Spaß liegt darin, selbst zu experimentieren und die versteckten Schwachstellen zu finden.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## So startest du OWASP - Juice Shop

Du kannst OWASP - Juice Shop über Docker starten. Eine genaue Installationsanleitung erhältst du [hier](/09-practice-labs/owasp/juiceShop/owasp-juice-shop_install.md).

So startest du die Docker Image:
```bash
# startet Juice-Shop auf Port 3000 im Docker
docker run -d -p 3000:3000 bkimminich/juice-shop

# Aktive Docker Images auflisten
docker ps
```

![Docker Image starten](/09-practice-labs/ressourcen/pictures/owasp/juiceshop/juiceshop.png)


Nachdem du den Befehl zum Starten des Docker Images eingegeben hast, kannst du in deinem Browser über `http://localhost:3000` auf den OWASP-Juice-Shop zugreifen. Zum Analysieren der Webanwendung sind die Developer Tools deines Browsers sehr hilfreich. Für komplexere Angriffe empfiehlt sich ein Proxy wie die Burp Suite oder OWASP ZAP.

![OWASP Juice Shop über den Browser starten](/09-practice-labs/ressourcen/pictures/owasp/juiceshop/juiceshop2.png)


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Scoreboard und Herausforderungen

Das Herzstück des Juice Shops ist das **Scoreboard**, das du über die URL `localhost:3000/#/score-board` erreichst. Es dient als zentrale Anlaufstelle, um deinen Fortschritt zu verfolgen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Das Scoreboard

Das Scoreboard ist eine dynamische Übersichtsseite, die alle verfügbaren Herausforderungen auflistet. Jede gelöste Challenge wird automatisch als erfolgreich markiert. Die Herausforderungen sind nach Schwierigkeitsgrad in Sterne (von 1 einfach bis 6 herausfordernd) unterteilt.


### Die Herausforderungen (Challenges)

Jede Herausforderung im Juice Shop ist so konzipiert, dass sie eine bestimmte Sicherheitslücke aus der realen Welt darstellt. Im Grunde musst du eine Aufgabe lösen, die das Ausnutzen einer Schwachstelle erfordert. Zum Beispiel:

- **Bestimmte Informationen finden:** Finde eine versteckte Datei oder ein geheimes Passwort.

- **Einen bestimmten Benutzer einloggen:** Melde dich ohne gültiges Passwort als ein anderer Benutzer an.

- **Bestimmte Daten manipulieren:** Ändere den Preis eines Produkts.

Sobald du die Aufgabe erfolgreich ausgeführt hast, wird eine Benachrichtigung im Juice Shop angezeigt, die bestätigt, dass die Challenge gelöst wurde. Das entsprechende Flag auf dem Scoreboard wird dann freigeschaltet.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Die erste XSS Herausforderung lösen

[**Cross-Site Scripting**](/03-web-security/angriffe/schwachstellen/xss.md) (**XSS**) ist eine Methode, bei der Angreifer schadhaften Code in eine Webanwendung einschleusen. Der Code wird nicht vom Server ausgeführt, sondern an den Browser des Opfers ausgeliefert, wo er ausgeführt wird. Das Ziel ist es, sensible Informationen vom Client abzugreifen (z. B. Session-Cookies) oder den Nutzer auf eine bösartige Seite umzuleiten.

Um eine XSS-Schwachstelle in einer Webanwendung zu finden, musst du herausfinden, ob die Seite Eingaben korrekt validiert und filtert. Ein einfacher Test genügt oft, um zu sehen, ob die Anwendung anfällig ist. Dazu versuchen wir, einen einfachen JavaScript-Befehl zu injizieren, der ein Pop-up-Fenster erzeugt.

Gehe auf die Suche nach einem Eingabefeld (zum Beispiel in der Suchfunktion) und gib folgenden Befehl ein:

```js
<iframe src="javascript:alert('XSS')">
```

Wenn die Webseite angreifbar ist (was der OWASP-Juice Shop ist), dann erhältst du ein PopUp mit dem Text `XSS`.

![XSS-Angriff auf OWASP-Juice Shop zeigt PopUp wie erwartet](/09-practice-labs/ressourcen/pictures/owasp/juiceshop/juiceshop3.png)


**Herzlichen Glückwunsch**, du hast die erste XSS-Herausforderung gemeistert!

Sieh im **Scoreboard** nach, wenn du gerade nicht weißt, welche Herausforderung du umsetzen sollst.  
Viel Spaß beim Hacken und Lernen.



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

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---
