### **📜 Videoskript: 404 Route in Express definieren – Fehlerseiten richtig handhaben**

🎬 **Titel:** Express 404 Route – So erstellst du eine benutzerfreundliche Fehlerseite!  
🎤 **Dauer:** ca. 8–10 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **wie man in Express eine 404-Fehlerseite definiert**, um **nicht gefundene Routen elegant zu handhaben**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Kennst du das? Du rufst eine Seite auf und bekommst einfach nur eine langweilige Fehlermeldung: „Cannot GET /xyz“. Das sieht unschön aus! In diesem Video zeige ich dir, wie du eine **benutzerfreundliche 404-Fehlerseite in Express** erstellst."_

📌 **Agenda:**  
✅ Warum eine 404-Seite wichtig ist  
✅ Eine 404-Route in Express definieren  
✅ Eine eigene HTML-Fehlerseite erstellen  
✅ Fehlerbehandlung optimieren

---

## **🔹 2. Warum eine 404-Route wichtig ist (1:00 – 2:00)**

📌 **Was passiert ohne eine 404-Route?**  
_"Wenn ein Benutzer eine nicht existierende Seite aufruft, gibt Express standardmäßig eine unfreundliche Fehlermeldung zurück."_

❌ **Ohne 404-Handling:**

```plaintext
Cannot GET /seite-nicht-vorhanden
```

🎯 **Warum eine eigene 404-Seite?**  
✅ **Bessere User Experience** – Nutzer wissen, was passiert ist.  
✅ **SEO-Optimierung** – Google mag saubere Fehlerseiten.  
✅ **Branding-Möglichkeit** – Nutze deine eigene Design-Sprache.

---

## **🔹 3. Eine einfache 404-Route in Express definieren (2:00 – 4:00)**

📌 **Standard-404-Handler:**

```javascript
import express from "express";

const app = express();
const port = 3000;

// Normale Routen
app.get("/", (req, res) => {
  res.send("Willkommen auf meiner Website!");
});

// 404-Route am Ende definieren!
app.all((req, res) => {
  res.status(404).send("Seite nicht gefunden");
});

app.listen(port, () =>
  console.log(`Server läuft auf http://localhost:${port}`)
);
```

🎯 **Erklärung:**  
✅ **`app.use()`** fängt alle nicht definierten Routen ab.  
✅ **`res.status(404)`** setzt den richtigen HTTP-Statuscode.  
✅ **Diese Route muss immer am Ende des Codes stehen!**

📌 **Testen:**  
➡ **Rufe eine nicht vorhandene Route auf:** http://localhost:3000/xyz

---

## **🔹 4. Eine benutzerfreundliche 404-Fehlerseite erstellen (4:00 – 6:00)**

📌 **Schritt 1: Eine eigene `404.html` Seite erstellen**  
➡ **Datei: `public/404.html`**

```html
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <title>Seite nicht gefunden</title>
    <style>
      body {
        text-align: center;
        font-family: Arial, sans-serif;
        padding: 50px;
      }
      h1 {
        color: red;
      }
      a {
        text-decoration: none;
        color: blue;
      }
    </style>
  </head>
  <body>
    <h1>Oops! Seite nicht gefunden (404)</h1>
    <p>Die Seite, die du suchst, existiert nicht.</p>
    <a href="/">Zurück zur Startseite</a>
  </body>
</html>
```

📌 **Schritt 2: 404-Seite mit Express ausliefern**

```javascript
app.all((req, res) => {
  res.status(404).sendFile("public/404.html", { root: "." });
});
```

🎯 **Nutzen:**  
✅ **Eine benutzerfreundliche Fehlerseite verbessert die User Experience.**  
✅ **Nutzer können zurück zur Startseite navigieren.**

---

## **🔹 5. Fehlerbehandlung optimieren (6:00 – 8:00)**

📌 **Erweitertes Error-Handling mit einer Middleware**

```javascript
app.all((req, res, next) => {
  const error = new Error("Seite nicht gefunden");
  error.status = 404;
  next(error);
});

// Globale Fehlerbehandlung
app.use((err, req, res, next) => {
  res.status(err.status || 500);
  res.json({
    error: {
      message: err.message,
    },
  });
});
```

🎯 **Nutzen:**  
✅ **Saubere Fehlerstruktur für APIs & Web-Apps**  
✅ **Flexibel erweiterbar für 500er-Fehler & Logging**

---

## **🔹 6. Fazit & Outro (8:00 – 9:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du in Express eine benutzerfreundliche 404-Route erstellst! Egal ob eine einfache Textantwort oder eine vollständige HTML-Seite – mit wenig Code kannst du deinen Nutzern eine bessere Erfahrung bieten!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie sieht deine perfekte 404-Seite aus? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Middleware in Express – Logging, Authentifizierung & Fehlerhandling!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für eine API mit Fehlerhandling ergänzen?** 😊
