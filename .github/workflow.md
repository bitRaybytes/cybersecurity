# ✅ Git & GitHub Workflow

## 🔧 1. Einmalige Einrichtung

```bash
# GPG-Schlüssel prüfen (sollte in deinem GitHub-Konto hinterlegt sein)
gpg --list-secret-keys --keyid-format=long

# Git-Konfiguration (einmalig oder global)
git config --global user.name "dein Username"
git config --global user.email "deine@email.com"
git config --global user.signingkey ABCDEFGHIJKL1234
git config --global commit.gpgsign true
```

---
 
## 🔁 2. Neues Feature entwickeln
```bash
# Auf aktuellen main-Stand wechseln
git checkout main
git pull origin main

# Neuen Branch erstellen (z. B. für neues Feature)
git checkout -b feature/neues-ui-design
```

---

## 💾 3. Dateien bearbeiten & committen
```bash
# Änderungen hinzufügen
git add .

# Commit mit GPG-Signatur
git commit -S -m "UI überarbeitet und responsive gemacht"
```

---

## 🚀 4. Neuen Branch pushen
```bash
git push origin feature/neues-ui-design
```

---

## 📦 5. Pull Request (PR) erstellen

- Gehe zu: https://github.com/bitRaybytes/cybersecurity
- GitHub zeigt automatisch: "Compare & Pull Request"
- Klicke darauf und fülle Titel & Beschreibung aus
- Bestätige mit: "Create pull request"

---

## ✅ 6. Code reviewen und mergen

- Code wird einmal reviewt und dann in der Main Branch übertragen.

---

## 🧹 7. Lokal aufräumen (optional)
```bash
# Nach Merge: Branch löschen
git branch -d feature/neues-ui-design
git pull origin main
```

---

## 🔒 Branch Protection Regeln (aktiv)

| Regel                        | Bedeutung                                 |
| ---------------------------- | ----------------------------------------- |
| ✅ Require PR before merging  | Kein direkter Push nach `main` erlaubt    |
| ✅ Require signed commits     | Alle Commits müssen mit GPG signiert sein |
| ✅ Block force pushes         | Kein `--force` Push                       |
| ✅ Restrict deletions/updates | Nur via PR & mit Berechtigung             |
| ✅ Require linear history     | Keine Merge-Commits, nur Fast-Forward     |

---

## 🛠 Typische Fehler & Lösungen

| ❌ Fehler                                          | 🛠 Lösung                                                                                            |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `push declined due to repository rule violations` | Du hast direkt auf `main` gepusht → **neuen Branch + PR**                                            |
| `gpg failed to sign the data`                     | GPG-Schlüssel fehlt oder nicht im Agent geladen → `gpg --list-secret-keys`, ggf. `gpg-agent` starten |
| `Updates were rejected`                           | Lokaler Branch ist veraltet oder Konflikte → `git pull --rebase` oder Konflikte manuell lösen        |


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

---

