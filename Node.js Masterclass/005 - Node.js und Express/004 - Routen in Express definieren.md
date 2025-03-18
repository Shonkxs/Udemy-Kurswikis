### **📜 Videoskript: Routen in Express definieren – Alles, was du wissen musst!**

🎬 **Titel:** Express Routing – So definierst du saubere & effiziente API-Routen!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **wie man in Express Routen definiert**, um **strukturierte und skalierbare Webanwendungen & APIs** zu bauen.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wie kannst du in Express saubere API-Routen erstellen? Ob für einfache Webanwendungen oder komplexe REST-APIs – Routing ist das Herzstück jeder Express-App! In diesem Video zeige ich dir, wie du Routen definierst, verwaltest und optimierst!"_

📌 **Agenda:**  
✅ Was sind Routen in Express?  
✅ Einfache GET- und POST-Routen erstellen  
✅ Routen mit Parametern & Query-Strings  
✅ Routen modularisieren  
✅ Best Practices

---

## **🔹 2. Was sind Routen in Express? (1:00 – 2:30)**

📌 **Definition:**  
_"Routen in Express sind Endpunkte, über die dein Server auf HTTP-Anfragen reagiert. Jede Route kann auf eine bestimmte URL und Methode (GET, POST, PUT, DELETE) reagieren."_

📌 **Beispiel für eine Route:**

```javascript
app.get("/users", (req, res) => {
  res.send("Liste aller Benutzer");
});
```

🎯 **Fazit:** **Routen steuern, wie dein Server mit Anfragen umgeht.**

---

## **🔹 3. Einfache Routen definieren (2:30 – 5:00)**

📌 **Schritt 1: Express installieren (falls nicht geschehen)**

```bash
npm install express
```

📌 **Schritt 2: Express-Server mit einer GET-Route erstellen**

```javascript
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Willkommen auf meiner Express-Seite!");
});

app.listen(port, () => {
  console.log(`Server läuft auf http://localhost:${port}`);
});
```

🎯 **Erklärung:**  
✅ **`app.get()`** → Reagiert auf GET-Anfragen.  
✅ **`res.send()`** → Sendet eine einfache Antwort zurück.  
✅ **`app.listen()`** → Startet den Server auf Port **3000**.

📌 **Server starten:**

```bash
node server.js
```

➡ **Öffne http://localhost:3000 in deinem Browser!**

---

## **🔹 4. Routen mit Parametern & Query-Strings (5:00 – 7:30)**

📌 **Dynamische Routen mit Parametern (`:id`)**

```javascript
app.get("/user/:id", (req, res) => {
  res.send(`Benutzer mit der ID: ${req.params.id}`);
});
```

➡ **Testen:** http://localhost:3000/user/42

📌 **Query-Strings verarbeiten (`?name=Markus`)**

```javascript
app.get("/search", (req, res) => {
  res.send(`Suchergebnis für: ${req.query.name}`);
});
```

➡ **Testen:** http://localhost:3000/search?name=Markus

🎯 **Nutzen:**  
✅ **Dynamische URLs für Benutzerprofile & Artikel**  
✅ **Filtern & Suchen mit Query-Strings**

---

## **🔹 5. POST- und PUT-Routen für APIs (7:30 – 9:30)**

📌 **Express kann auch JSON-Daten verarbeiten**  
➡ **Middleware aktivieren, um JSON-Daten zu lesen:**

```javascript
app.use(express.json());
```

📌 **POST-Route für neue Benutzer**

```javascript
app.post("/users", (req, res) => {
  const newUser = req.body;
  res.status(201).send(`Neuer Benutzer erstellt: ${JSON.stringify(newUser)}`);
});
```

➡ **Testen mit Postman oder cURL:**

```bash
curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"name":"Lisa"}'
```

📌 **PUT-Route zum Aktualisieren von Benutzerdaten**

```javascript
app.put("/users/:id", (req, res) => {
  res.send(`Benutzer ${req.params.id} wurde aktualisiert!`);
});
```

🎯 **Nutzen:**  
✅ **POST für neue Ressourcen, PUT für Updates**  
✅ **Ideal für REST-APIs & Datenbanken**

---

## **🔹 6. Routen modularisieren & strukturieren (9:30 – 11:30)**

📌 **Problem:** **Wenn du viele Routen hast, wird dein Code unübersichtlich.**  
📌 **Lösung:** **Nutze `express.Router()` für saubere Struktur!**

📌 **Schritt 1: Eine eigene Router-Datei `routes/users.js` erstellen**

```javascript
import express from "express";
const router = express.Router();

router.get("/", (req, res) => res.send("Liste aller Benutzer"));
router.post("/", (req, res) => res.send("Neuer Benutzer erstellt"));
router.put("/:id", (req, res) =>
  res.send(`Benutzer ${req.params.id} aktualisiert`)
);
router.delete("/:id", (req, res) =>
  res.send(`Benutzer ${req.params.id} gelöscht`)
);

export default router;
```

📌 **Schritt 2: Router in `server.js` einbinden**

```javascript
import usersRouter from "./routes/users.js";

app.use("/users", usersRouter);
```

➡ **Alle User-Routen funktionieren jetzt unter `/users`!**

🎯 **Nutzen:**  
✅ **Sauberer & übersichtlicher Code**  
✅ **Perfekt für größere Projekte mit vielen Routen**

---

## **🔹 7. Best Practices für Routen in Express (11:30 – 12:30)**

📌 **🚀 Best Practices für saubere Routen:**  
✅ **Nutze separate Router-Dateien für verschiedene Bereiche** (`/users`, `/products`).  
✅ **Vermeide zu lange URLs – nutze sinnvolle API-Struktur**.  
✅ **Nutze Middleware für Authentifizierung & Logging**.

📌 **Häufige Fehler vermeiden:**  
❌ **Vergessen, `express.json()` zu aktivieren** → POST/PUT-Daten können nicht gelesen werden.  
❌ **Fehlendes `next()` in Middleware** → Request bleibt hängen.  
❌ **Unstrukturierter Code in `server.js`** → Nutze Router-Dateien für Ordnung.

---

## **🔹 8. Fazit & Outro (12:30 – 13:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du in Express saubere Routen definierst – von einfachen GET-Routen bis hin zu REST-API-Methoden wie POST, PUT und DELETE! Jetzt kannst du strukturierte APIs und Webanwendungen mit Express bauen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Express-Routen brauchst du in deinen Projekten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Middleware in Express – Logging, Auth & Fehlerhandling!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Authentifizierung oder Validierung mit `express-validator` ergänzen?** 😊
