### **📜 Videoskript: Das Mongoose Model als CRUD-Operator – Datenbankoperationen einfach gemacht!**

🎬 **Titel:** Mongoose Model als CRUD-Operator – Daten sicher in MongoDB verwalten  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** **Verstehen, wie man das Mongoose Model als CRUD-Operator nutzt, um Daten in MongoDB zu erstellen, zu lesen, zu aktualisieren und zu löschen.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Du willst Daten in MongoDB speichern, abrufen, aktualisieren oder löschen? Mongoose macht das mit **CRUD-Operationen** ganz einfach! In diesem Video zeige ich dir, wie du mit dem Mongoose Model CRUD-Operationen durchführst, um eine vollständige Datenbankverwaltung zu ermöglichen."_

📌 **Agenda:**  
✅ Was ist ein Mongoose Model?  
✅ CRUD-Operationen (Create, Read, Update, Delete) mit Mongoose  
✅ Erweiterte Methoden wie `findOneAndUpdate()` & `deleteMany()`  
✅ Best Practices für sichere Datenbankoperationen

---

## **🔹 2. Was ist ein Mongoose Model? (1:00 – 2:30)**

📌 **Definition:**  
Ein **Mongoose Model** ist eine JavaScript-Klasse, die **MongoDB-Dokumente verwaltet**. Es basiert auf einem **Schema** und erlaubt **CRUD-Operationen direkt in der Datenbank**.

📌 **Projektstruktur:**

```plaintext
/my-express-app
 ├── models
 │   ├── User.js   # Mongoose Model für Benutzer
 ├── server.js     # Express Server
 ├── package.json
```

📌 **Schema & Model in `models/User.js` definieren**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: [true, "Name ist erforderlich"],
      trim: true,
    },
    email: {
      type: String,
      required: [true, "E-Mail ist erforderlich"],
      unique: true,
      match: [/^[^\s@]+@[^\s@]+\.[^\s@]+$/, "Ungültige E-Mail-Adresse"],
    },
    age: { type: Number, min: [0, "Alter kann nicht negativ sein"] },
  },
  { timestamps: true }
);

const User = mongoose.model("User", userSchema);
export default User;
```

🎯 **Ergebnis:** **Wir haben ein Model für CRUD-Operationen!**

---

## **🔹 3. Daten mit Mongoose erstellen (CREATE) (2:30 – 4:30)**

📌 **Einen neuen Benutzer in der Datenbank speichern**

```javascript
import User from "./models/User.js";

const createUser = async () => {
  try {
    const user = await User.create({
      name: "Max Mustermann",
      email: "max@example.com",
      age: 30,
    });
    console.log("✅ Benutzer erstellt:", user);
  } catch (error) {
    console.error("❌ Fehler beim Erstellen:", error.message);
  }
};

createUser();
```

📌 **Alternative: `new User().save()`**

```javascript
const user = new User({ name: "Anna", email: "anna@example.com", age: 25 });
await user.save();
```

🎯 **Ergebnis:** **Ein neues Dokument wird in MongoDB gespeichert!**

---

## **🔹 4. Daten abrufen (READ) (4:30 – 6:30)**

📌 **Alle Benutzer abrufen (`find()`)**

```javascript
const getUsers = async () => {
  try {
    const users = await User.find();
    console.log("👥 Alle Benutzer:", users);
  } catch (error) {
    console.error("❌ Fehler beim Abrufen:", error.message);
  }
};

getUsers();
```

📌 **Einen Benutzer per E-Mail finden (`findOne()`)**

```javascript
const getUserByEmail = async (email) => {
  try {
    const user = await User.findOne({ email });
    console.log("🔎 Gefundener Benutzer:", user);
  } catch (error) {
    console.error("❌ Fehler beim Suchen:", error.message);
  }
};

getUserByEmail("max@example.com");
```

🎯 **Ergebnis:** **Wir können gezielt nach Benutzern suchen!**

---

## **🔹 5. Daten aktualisieren (UPDATE) (6:30 – 8:30)**

📌 **Einen Benutzer nach ID aktualisieren (`findByIdAndUpdate()`)**

```javascript
const updateUser = async (userId) => {
  try {
    const updatedUser = await User.findByIdAndUpdate(
      userId,
      { age: 35 },
      { new: true, runValidators: true } // Gibt das aktualisierte Dokument zurück
    );
    console.log("✏️ Benutzer aktualisiert:", updatedUser);
  } catch (error) {
    console.error("❌ Fehler beim Aktualisieren:", error.message);
  }
};

updateUser("66123abc456def7890123456");
```

📌 **Erklärung:**  
✅ `new: true` → Gibt das **aktualisierte Dokument** zurück.  
✅ `runValidators: true` → Wendet Schema-Validierungen an.

🎯 **Ergebnis:** **Daten können gezielt aktualisiert werden!**

---

## **🔹 6. Daten löschen (DELETE) (8:30 – 10:30)**

📌 **Einen Benutzer nach ID löschen (`findByIdAndDelete()`)**

```javascript
const deleteUser = async (userId) => {
  try {
    const deletedUser = await User.findByIdAndDelete(userId);
    console.log("🗑️ Benutzer gelöscht:", deletedUser);
  } catch (error) {
    console.error("❌ Fehler beim Löschen:", error.message);
  }
};

deleteUser("66123abc456def7890123456");
```

📌 **Mehrere Benutzer löschen (`deleteMany()`)**

```javascript
const deleteUsersByAge = async (age) => {
  try {
    const result = await User.deleteMany({ age: { $lt: age } });
    console.log("🗑️ Benutzer unter", age, "gelöscht:", result.deletedCount);
  } catch (error) {
    console.error("❌ Fehler beim Massenlöschen:", error.message);
  }
};

deleteUsersByAge(18);
```

🎯 **Ergebnis:** **Daten werden gezielt entfernt!**

---

## **🔹 7. Best Practices für CRUD mit Mongoose (10:30 – 12:00)**

📌 **🚀 Best Practices für Mongoose CRUD-Operationen:**  
✅ **`try/catch` nutzen, um Fehler abzufangen.**  
✅ **`runValidators: true` verwenden, um Validierungen bei Updates durchzusetzen.**  
✅ **Immer `await` nutzen, um asynchrone Operationen zu steuern.**  
✅ **Sensible Daten (Passwörter) mit `select: false` vor ungewolltem Abruf schützen.**

📌 **Fehlersichere API-Route für CRUD-Operationen in Express**

```javascript
app.get("/users", async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    res.status(500).json({ error: "Fehler beim Abrufen der Benutzer" });
  }
});
```

🎯 **Ergebnis:** **Saubere, stabile & fehlerfreie Datenbankoperationen!**

---

## **🔹 8. Fazit & Outro (12:00 – 13:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **CRUD-Operationen mit dem Mongoose Model** durchführst. Mit diesen Techniken kannst du deine MongoDB-Datenbank effizient verwalten!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Mongoose-Methoden nutzt du am meisten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung & Performance-Optimierung!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch eine Erweiterung mit Aggregation oder Transaktionen ergänzen?** 😊
