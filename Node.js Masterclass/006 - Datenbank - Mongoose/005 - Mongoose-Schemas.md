### **📜 Videoskript: Mongoose Schemas – So definierst du Datenmodelle mit Validierung & Fehlerhandling**

🎬 **Titel:** Mongoose Schemas – Validierung, Fehlerhandling & Beziehungen einfach erklärt!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** **Verstehen, wie man Mongoose Schemas mit Validierung, benutzerdefinierten Fehlermeldungen und Beziehungen erstellt.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Hast du dich jemals gefragt, wie du in Mongoose **Fehlermeldungen für ungültige Daten definierst**? Oder wie du **Beziehungen zwischen Dokumenten** erstellst? In diesem Video lernst du alles über **Mongoose Schemas mit Validierung, Fehlerhandling und Relationen**!"_

📌 **Agenda:**  
✅ Was sind Mongoose Schemas?  
✅ Datenvalidierung mit benutzerdefinierten Fehlermeldungen  
✅ Standardwerte & automatische Zeitstempel  
✅ Relationen zwischen Dokumenten mit `ref`  
✅ Best Practices für optimierte Mongoose Schemas

---

## **🔹 2. Was sind Mongoose Schemas? (1:00 – 2:30)**

📌 **Definition:**  
_Mongoose Schemas definieren die Struktur von Dokumenten in einer MongoDB-Sammlung._

📌 **Warum Schemas?**  
✅ **Struktur für MongoDB-Daten definieren**  
✅ **Datenvalidierung direkt im Modell**  
✅ **Standardwerte & automatische Zeitstempel setzen**

🎯 **Fazit:** **Mongoose gibt MongoDB Struktur & Validierung!**

---

## **🔹 3. Mongoose Schema mit Validierung & Fehlermeldungen (2:30 – 5:30)**

📌 **Grundschema für Benutzer (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: [true, "Name ist erforderlich"],
      trim: true,
      minlength: [3, "Name muss mindestens 3 Zeichen lang sein"],
    },
    email: {
      type: String,
      required: [true, "E-Mail ist erforderlich"],
      unique: true,
      match: [/^[^\s@]+@[^\s@]+\.[^\s@]+$/, "Ungültige E-Mail-Adresse"],
    },
    age: {
      type: Number,
      min: [0, "Alter kann nicht negativ sein"],
      max: [120, "Alter kann nicht über 120 sein"],
    },
    password: {
      type: String,
      required: [true, "Passwort ist erforderlich"],
      minlength: [6, "Passwort muss mindestens 6 Zeichen lang sein"],
      select: false, // Wird bei Abfragen standardmäßig nicht zurückgegeben
    },
  },
  { timestamps: true }
); // Automatische `createdAt` & `updatedAt` Zeitstempel

export default mongoose.model("User", userSchema);
```

📌 **Erklärung:**  
✅ `required: [true, "Fehlermeldung"]` → Setzt eine benutzerdefinierte Fehlermeldung.  
✅ `minlength` / `maxlength` → Validiert die Länge eines Strings.  
✅ `match: [Regex, "Fehlermeldung"]` → Überprüft Format mit regulären Ausdrücken.  
✅ `select: false` → Passwort wird nicht standardmäßig zurückgegeben.

🎯 **Ergebnis:** **Jeder Validierungsfehler zeigt eine individuelle Fehlermeldung!**

---

## **🔹 4. Fehlermeldungen beim Speichern abfangen (5:30 – 7:30)**

📌 **Benutzer mit falschen Daten speichern & Fehler abfangen**

```javascript
import User from "./models/User.js";

const testUser = new User({
  name: "Al",
  email: "invalid-email",
  password: "123",
});

testUser
  .save()
  .then(() => console.log("Benutzer gespeichert"))
  .catch((err) => console.error("Fehler beim Speichern:", err.message));
```

📌 **Erwartete Fehler:**  
❌ **Name zu kurz:** `Name muss mindestens 3 Zeichen lang sein`  
❌ **Ungültige E-Mail:** `Ungültige E-Mail-Adresse`  
❌ **Passwort zu kurz:** `Passwort muss mindestens 6 Zeichen lang sein`

🎯 **Ergebnis:** **Daten mit Fehlern werden nicht gespeichert, Nutzer erhalten eine klare Fehlermeldung.**

---

## **🔹 5. Beziehungen zwischen Dokumenten mit `ref` (7:30 – 9:30)**

📌 **Benutzer können mehrere Artikel haben (1:n Beziehung)**  
➡ **Datei: `models/Post.js`**

```javascript
import mongoose from "mongoose";

const postSchema = new mongoose.Schema(
  {
    title: { type: String, required: [true, "Titel ist erforderlich"] },
    content: { type: String, required: [true, "Inhalt darf nicht leer sein"] },
    author: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: [true, "Autor ist erforderlich"],
    },
  },
  { timestamps: true }
);

export default mongoose.model("Post", postSchema);
```

📌 **Post mit Referenz auf einen Benutzer speichern**

```javascript
const user = await User.findOne({ email: "max@example.com" });

const newPost = new Post({
  title: "Mein erster Post",
  content: "Das ist mein Beitrag!",
  author: user._id,
});

await newPost.save();
console.log("Post gespeichert:", newPost);
```

📌 **Post mit Autor abrufen (`populate`)**

```javascript
const post = await Post.findOne().populate("author", "name email");
console.log("Post mit Autor:", post);
```

🎯 **Ergebnis:**  
✅ **MongoDB speichert nur die `user._id`, Mongoose ersetzt sie durch Benutzerdaten!**

---

## **🔹 6. Best Practices für Mongoose Schemas (9:30 – 11:30)**

📌 **🚀 Best Practices für MongoDB-Datenmodelle:**  
✅ **Nutze `trim: true` für Strings, um Leerzeichen zu entfernen.**  
✅ **Indexiere häufige Suchfelder für Performance (`index: true`).**  
✅ **Setze `select: false`, um sensible Daten zu schützen.**  
✅ **Nutze `timestamps: true` für automatische `createdAt` & `updatedAt`.**

📌 **Optimiertes User-Schema mit Best Practices**

```javascript
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
    password: { type: String, required: true, minlength: 6, select: false },
  },
  { timestamps: true }
);
```

🎯 **Ergebnis:** **Saubere, schnelle & sichere Datenbankstrukturen!**

---

## **🔹 7. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **Mongoose Schemas mit Validierung, Fehlerhandling und Beziehungen** definierst. Mit diesen Techniken bleiben deine MongoDB-Daten sauber und deine API sicher!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Validierungen nutzt du in deinen Mongoose Schemas? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung & Performance-Optimierung!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch eine Erweiterung mit Virtual Fields oder statischen Methoden ergänzen?** 😊
