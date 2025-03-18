### **📜 Videoskript: Express-Routing richtig strukturieren – Saubere & skalierbare APIs!**

🎬 **Titel:** Express-Routing richtig strukturieren – So baust du saubere & skalierbare APIs!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** Verstehen, **wie man Routing in Express sauber strukturiert**, um **skalierbare und wartbare Webanwendungen** zu entwickeln.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Hast du eine Express-App mit vielen Routen, die langsam unübersichtlich wird? Dann ist es Zeit, dein Routing besser zu strukturieren! In diesem Video zeige ich dir, wie du deine Express-Routen in Module aufteilst und eine **saubere & skalierbare API-Struktur** erstellst!"_

📌 **Agenda:**  
✅ Warum sollte man Routing strukturieren?  
✅ `express.Router()` für modulare Routen nutzen  
✅ Routing-Dateien für größere Projekte  
✅ Controller für eine noch bessere Trennung  
✅ Best Practices für skalierbare APIs

---

## **🔹 2. Warum Routing sauber strukturieren? (1:00 – 2:30)**

📌 **Problem:**  
_"Wenn du alle Routen in `server.js` oder `app.js` definierst, wird dein Code schnell unübersichtlich!"_

📌 **Schlechtes Beispiel (alle Routen in `server.js`)**

```javascript
import express from "express";

const app = express();
const port = 3000;

app.get("/users", (req, res) => res.send("Liste aller Benutzer"));
app.get("/products", (req, res) => res.send("Alle Produkte"));
app.post("/orders", (req, res) => res.send("Bestellung erstellt"));
app.delete("/orders/:id", (req, res) =>
  res.send(`Bestellung ${req.params.id} gelöscht`)
);

app.listen(port, () =>
  console.log(`Server läuft auf http://localhost:${port}`)
);
```

❌ **Problem:** **Bei vielen Routen wird `server.js` unlesbar!**

🎯 **Lösung:** **Routen in separate Dateien auslagern & `express.Router()` nutzen!**

---

## **🔹 3. Routen mit `express.Router()` modularisieren (2:30 – 5:00)**

📌 **Schritt 1: Projektstruktur anlegen**

```plaintext
/my-express-app
 ├── routes
 │   ├── users.js
 │   ├── products.js
 │   ├── orders.js
 ├── controllers
 │   ├── userController.js
 │   ├── productController.js
 │   ├── orderController.js
 ├── server.js
 ├── package.json
```

📌 **Schritt 2: `users.js` als eigene Router-Datei erstellen**  
➡ **Datei:** `routes/users.js`

```javascript
import express from "express";

const router = express.Router();

router.get("/", (req, res) => res.send("Liste aller Benutzer"));
router.get("/:id", (req, res) => res.send(`Benutzer ${req.params.id}`));

export default router;
```

📌 **Schritt 3: Router in `server.js` einbinden**

```javascript
import express from "express";
import usersRouter from "./routes/users.js";

const app = express();
const port = 3000;

app.use("/users", usersRouter);

app.listen(port, () =>
  console.log(`Server läuft auf http://localhost:${port}`)
);
```

🎯 **Ergebnis:**  
✅ **`/users` zeigt die Benutzerliste**  
✅ **`/users/42` zeigt einen spezifischen Benutzer**  
✅ **Saubere Trennung von Code & Modulen!**

---

## **🔹 4. Noch bessere Struktur mit Controllern (5:00 – 7:30)**

📌 **Warum Controller nutzen?**  
_"Auch in `routes/users.js` wird der Code schnell unübersichtlich, wenn jede Route ihre eigene Logik enthält. Die Lösung: **Controller auslagern**!"_

📌 **Schritt 1: `userController.js` erstellen**  
➡ **Datei:** `controllers/userController.js`

```javascript
export const getUsers = (req, res) => {
  res.send("Liste aller Benutzer");
};

export const getUserById = (req, res) => {
  res.send(`Benutzer ${req.params.id}`);
};
```

📌 **Schritt 2: Controller in `routes/users.js` verwenden**  
➡ **Datei:** `routes/users.js`

```javascript
import express from "express";
import { getUsers, getUserById } from "../controllers/userController.js";

const router = express.Router();

router.get("/", getUsers);
router.get("/:id", getUserById);

export default router;
```

🎯 **Nutzen:**  
✅ **Bessere Lesbarkeit** – Routen enthalten nur Routing-Logik.  
✅ **Einfachere Wartung** – Logik ist in `controllers/` zentralisiert.

---

## **🔹 5. Middleware in Routen integrieren (7:30 – 9:30)**

📌 **Middleware für Authentifizierung hinzufügen**

```javascript
const authMiddleware = (req, res, next) => {
  console.log("Authentifizierung läuft...");
  next();
};
```

📌 **Middleware in `users.js` integrieren**

```javascript
router.get("/", authMiddleware, getUsers);
```

🎯 **Nutzen:**  
✅ **Trennung von Authentifizierungs- & Geschäftslogik.**

---

## **🔹 6. Best Practices für skalierbares Routing (9:30 – 11:30)**

📌 **🚀 Best Practices für große Express-Projekte:**  
✅ **Jede Entität (Users, Orders, Products) bekommt eine eigene `routes/` Datei**.  
✅ **Geschäftslogik gehört in `controllers/` statt direkt in die Route.**  
✅ **Nutze Middleware für Logging, Authentifizierung & Validierung.**  
✅ **Erstelle separate `services/` für komplexe Geschäftslogik.**

📌 **Häufige Fehler vermeiden:**  
❌ **Alle Routen in `server.js` lassen** → Unlesbarer Code.  
❌ **Controller-Logik direkt in der Route schreiben** → Schwer wartbar.  
❌ **Fehlendes Fehlerhandling** → Nutze `next(error)` für saubere Fehlerbehandlung.

---

## **🔹 7. Fazit & Outro (11:30 – 12:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du Routing in Express **modular & sauber strukturierst**! Mit `express.Router()`, Controllern & Middleware kannst du skalierbare APIs bauen, die leicht wartbar sind."_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie strukturierst du deine Express-Routen? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Middleware – Logging, Authentifizierung & Fehlerhandling!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für `express-validator` oder `async/await` für API-Routen ergänzen?** 😊
