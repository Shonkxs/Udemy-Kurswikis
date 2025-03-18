### **📜 Videoskript: Fehlerhandling in Express – So baust du robuste Webserver & APIs!**

🎬 **Titel:** Fehlerhandling in Express – Die beste Methode für stabile APIs & Webserver  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **wie man Fehler sauber behandelt**, um **stabile Express-Anwendungen & REST-APIs** zu bauen.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Jede Webanwendung oder API wird irgendwann Fehler produzieren – sei es durch falsche Eingaben, Datenbankprobleme oder nicht gefundene Seiten. In diesem Video zeige ich dir, wie du in Express **ein sauberes Fehlerhandling einbaust**, damit deine Anwendung robust bleibt!"_

📌 **Agenda:**  
✅ Warum ist Fehlerhandling wichtig?  
✅ Grundlagen der Fehlerbehandlung in Express  
✅ Eigene Fehlerklassen für APIs definieren  
✅ Globale Fehlerbehandlung & Logging  
✅ Best Practices

---

## **🔹 2. Warum ist Fehlerhandling wichtig? (1:00 – 2:30)**

📌 **Was passiert ohne Fehlerhandling?**  
_"Ohne eine richtige Fehlerbehandlung können Express-Apps abstürzen oder Nutzern unschöne Fehlermeldungen anzeigen!"_

📌 **Schlechtes Beispiel:**

```javascript
app.get("/user/:id", (req, res) => {
  const user = undefined; // Simulierter Fehler
  res.send(user.name); // 🚨 Führt zu einem Absturz!
});
```

➡ **Fehlermeldung in der Konsole:**

```plaintext
TypeError: Cannot read properties of undefined (reading 'name')
```

🎯 **Lösung:** **Sauberes Fehlerhandling verhindert Abstürze & zeigt klare Fehlermeldungen!**

---

## **🔹 3. Grundlagen des Express-Fehlerhandlings (2:30 – 5:00)**

📌 **Einfaches Fehlerhandling mit `next(err)`**

```javascript
app.get("/error", (req, res, next) => {
  const error = new Error("Etwas ist schiefgelaufen!");
  error.status = 500;
  next(error); // Weiterleitung an den globalen Fehlerhandler
});
```

🎯 **Erklärung:**  
✅ **`next(error)`** leitet Fehler an den zentralen Fehlerhandler weiter.  
✅ **Kein direkter Absturz der App!**

📌 **Globale Fehler-Middleware hinzufügen**

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    error: {
      message: err.message,
    },
  });
});
```

🎯 **Nutzen:**  
✅ **Fängt alle Fehler ab & gibt eine saubere JSON-Fehlermeldung zurück!**

---

## **🔹 4. Fehlerklasse für saubere API-Fehler definieren (5:00 – 7:30)**

📌 **Problem:** **Alle Fehler haben aktuell denselben Standard-Fehleraufbau.**  
📌 **Lösung:** **Eigene Fehlerklasse für bessere API-Fehler!**

📌 **Fehlerklasse `ApiError.js` erstellen**

```javascript
class ApiError extends Error {
  constructor(status, message) {
    super(message);
    this.status = status;
  }
}

export default ApiError;
```

📌 **Fehlermeldung mit eigener Klasse auslösen**

```javascript
import ApiError from "./ApiError.js";

app.get("/protected", (req, res, next) => {
  return next(new ApiError(403, "Zugriff verweigert!"));
});
```

🎯 **Nutzen:**  
✅ **Saubere, konsistente Fehler in einer API**.  
✅ **Jeder Fehler hat einen eigenen Statuscode (403, 500, etc.).**

---

## **🔹 5. Globale Fehlerbehandlung & Logging (7:30 – 10:00)**

📌 **Erweiterte Fehlerbehandlung mit Logging**

```javascript
import fs from "fs";

app.use((err, req, res, next) => {
  fs.appendFileSync("errors.log", `${new Date()} - ${err.message}\n`);

  res.status(err.status || 500).json({
    error: {
      message: err.message,
    },
  });
});
```

🎯 **Nutzen:**  
✅ **Alle Fehler werden in `errors.log` gespeichert** – hilfreich für Debugging!  
✅ **Nutzer bekommen eine klare JSON-Fehlermeldung.**

---

## **🔹 6. 404-Fehler sauber handhaben (10:00 – 11:30)**

📌 **404-Route für nicht gefundene Seiten hinzufügen**

```javascript
app.all("*", (req, res, next) => {
  next(new ApiError(404, "Diese Seite existiert nicht!"));
});
```

🎯 **Nutzen:**  
✅ **Saubere 404-Fehlermeldung für jede nicht existierende Route.**

📌 **Erweiterte Version für HTML-Seiten**

```javascript
app.use((req, res) => {
  res.status(404).sendFile("public/404.html", { root: "." });
});
```

🎯 **Nutzen:**  
✅ **Eigene HTML-Fehlerseite für Web-Apps**.

---

## **🔹 7. Best Practices für Express-Fehlerhandling (11:30 – 12:30)**

📌 **🚀 Best Practices für stabiles Fehlerhandling**  
✅ **Nutze `next(error)`, um Fehler weiterzuleiten.**  
✅ **Verwende eine zentrale Fehlerklasse (`ApiError.js`).**  
✅ **Logge Fehler mit `fs.appendFileSync()` oder Winston/Logger.**  
✅ **Fange alle unbekannten Routen mit `app.all('*')` ab.**

📌 **Häufige Fehler vermeiden:**  
❌ **`res.send()` vor `next(error)` nutzen** → Sendet doppelte Antwort!  
❌ **Kein Logging implementieren** → Fehler gehen verloren.  
❌ **Falsche HTTP-Statuscodes** → Nutze **403 für "verboten"**, **404 für "nicht gefunden"**, **500 für Serverfehler**.

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du Fehler sauber in Express behandelst! Mit `next(error)`, einer zentralen Fehlerklasse & Logging machst du deine Webserver robuster und deine APIs professioneller!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie gehst du mit Fehlern in deinen Web-Apps um? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Middleware – Logging, Authentifizierung & Fehlerhandling kombinieren!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Fehlerhandling mit `async/await` und Datenbanken ergänzen?** 😊
