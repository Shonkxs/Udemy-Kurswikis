### **📜 Videoskript: Verbindung mit MongoDB & Fehlerhandling mit Mongoose Event Listener**

🎬 **Titel:** Verbindung mit MongoDB & Fehlerhandling – So bleibt deine Express-App stabil!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** **Verstehen, wie man eine robuste Verbindung zu MongoDB mit Mongoose aufbaut und durch Event Listener Verbindungsabbrüche und Fehler sauber behandelt**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Deine Express-App verbindet sich mit MongoDB, aber was passiert, wenn die Verbindung plötzlich abbricht? Stürzt deine API dann ab? In diesem Video zeige ich dir, wie du mit **Mongoose Event Listenern** und einer **automatischen Wiederverbindung** eine **stabile Datenbankverbindung** sicherstellst!"_

📌 **Agenda:**  
✅ MongoDB & Mongoose installieren  
✅ Verbindung zu MongoDB herstellen  
✅ Event Listener für Verbindungsabbrüche nutzen  
✅ Automatische Wiederverbindung aktivieren  
✅ Best Practices für robuste MongoDB-Verbindungen

---

## **🔹 2. MongoDB & Mongoose installieren (1:00 – 2:30)**

📌 **Schritt 1: MongoDB lokal oder über Atlas nutzen**

- **Lokal:** Stelle sicher, dass MongoDB installiert ist (`mongod` läuft).
- **Cloud:** Erstelle eine MongoDB-Atlas-Datenbank und kopiere die **Connection-URL**.

📌 **Schritt 2: Mongoose installieren**

```bash
npm install mongoose dotenv
```

📌 **Schritt 3: Projektstruktur**

```plaintext
/my-express-app
 ├── config
 │   ├── db.js   # MongoDB-Verbindungslogik
 ├── server.js   # Hauptserver
 ├── package.json
 ├── .env        # Umgebungsvariablen (MongoDB-URI)
```

📌 **Schritt 4: `.env` Datei für Konfiguration erstellen**

```plaintext
MONGO_URI=mongodb://localhost:27017/meineDatenbank
PORT=5000
```

🎯 **Ergebnis:** **MongoDB ist bereit für die Verbindung mit Express!**

---

## **🔹 3. Verbindung zu MongoDB herstellen (2:30 – 4:30)**

📌 **Verbindungslogik in `config/db.js`**

```javascript
import mongoose from "mongoose";

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });

    console.log("✅ MongoDB erfolgreich verbunden!");
  } catch (error) {
    console.error("❌ Fehler bei der MongoDB-Verbindung:", error.message);
    process.exit(1); // App beenden, wenn keine Verbindung möglich ist
  }
};

export default connectDB;
```

📌 **Verbindung in `server.js` aufrufen**

```javascript
import express from "express";
import dotenv from "dotenv";
import connectDB from "./config/db.js";

dotenv.config();
const app = express();
const port = process.env.PORT || 5000;

// MongoDB verbinden
connectDB();

app.get("/", (req, res) => {
  res.send("API läuft...");
});

app.listen(port, () => {
  console.log(`🚀 Server läuft auf http://localhost:${port}`);
});
```

🎯 **Ergebnis:**  
✅ **MongoDB wird mit Express verbunden.**  
✅ **Die Verbindung wird beim Serverstart aufgebaut.**

---

## **🔹 4. Fehlerhandling mit Mongoose Event Listenern (4:30 – 7:30)**

📌 **Warum Event Listener?**

- Falls die Verbindung **unterbrochen wird**, müssen wir das erkennen.
- Die Anwendung sollte sich **automatisch erneut verbinden**, sobald MongoDB wieder verfügbar ist.

📌 **Mongoose Event Listener in `config/db.js` hinzufügen**

```javascript
import mongoose from "mongoose";

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });

    console.log("✅ MongoDB erfolgreich verbunden!");
  } catch (error) {
    console.error("❌ Fehler bei der MongoDB-Verbindung:", error.message);
    process.exit(1);
  }
};

// Event Listener für Verbindungsstatus
mongoose.connection.on("connected", () => {
  console.log("🔗 Mongoose verbunden mit MongoDB.");
});

mongoose.connection.on("error", (err) => {
  console.error(`🚨 Mongoose Verbindungsfehler: ${err.message}`);
});

mongoose.connection.on("disconnected", () => {
  console.warn(
    "⚠️ Mongoose Verbindung getrennt. Erneuter Verbindungsversuch..."
  );
  reconnectDB();
});

// Automatische Wiederverbindung
const reconnectDB = () => {
  setTimeout(() => {
    console.log("🔄 Versuche erneut, eine Verbindung zu MongoDB aufzubauen...");
    connectDB();
  }, 5000);
};

export default connectDB;
```

🎯 **Ergebnis:**  
✅ **Erkennt, wenn die Verbindung verloren geht.**  
✅ **Erneut Verbindung nach 5 Sekunden, wenn MongoDB offline war.**

---

## **🔹 5. Fehlerhandling in Routen (7:30 – 9:30)**

📌 **Datenmodell für MongoDB in `models/User.js` erstellen**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
});

export default mongoose.model("User", userSchema);
```

📌 **Fehlersichere Route für Benutzerabfrage in `server.js`**

```javascript
import User from "./models/User.js";

app.get("/users", async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    console.error("Fehler beim Abrufen der Benutzer:", error.message);
    res.status(500).json({ error: "Interner Serverfehler" });
  }
});
```

🎯 **Ergebnis:**  
✅ **Fehler werden abgefangen & die API stürzt nicht ab.**  
✅ **Nutzer erhalten eine verständliche Fehlermeldung.**

---

## **🔹 6. Best Practices für MongoDB-Fehlerhandling (9:30 – 11:30)**

📌 **🚀 Best Practices für robuste MongoDB-Verbindungen:**  
✅ **Automatische Wiederverbindung mit `setTimeout`.**  
✅ **Nutze `mongoose.connection.on('disconnected')`, um Fehler zu erkennen.**  
✅ **Vermeide, sensible Daten direkt im Code zu speichern – nutze `.env`.**  
✅ **Behandle Fehler in Routen mit `try/catch`.**  
✅ **Nutze `mongoose.set('debug', true)`, um Abfragen in der Konsole zu überwachen.**

📌 **Häufige Fehler vermeiden:**  
❌ **Keine Event Listener → App erkennt Verbindungsabbrüche nicht.**  
❌ **Fehlendes Error Handling in API-Routen → Nutzer erhalten unklare Fehler.**  
❌ **Kein Logging → Probleme bleiben unbemerkt.**

---

## **🔹 7. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **MongoDB mit Express verbindest und Verbindungsabbrüche mit Mongoose Event Listenern erkennst**. Damit bleibt deine App stabil, auch wenn MongoDB mal nicht verfügbar ist!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon mal Probleme mit MongoDB-Verbindungsabbrüchen gehabt? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung & Performance-Optimierung!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Logging mit `winston` oder `morgan` ergänzen?** 😊
