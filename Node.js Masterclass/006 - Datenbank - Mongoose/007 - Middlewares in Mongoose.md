### **📜 Videoskript: Middlewares in Mongoose – Daten vor und nach dem Speichern verändern!**

🎬 **Titel:** Middlewares in Mongoose – So nutzt du `pre` & `post` Hooks für Datenverarbeitung  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** **Verstehen, wie Mongoose Middlewares (`pre` & `post` Hooks) funktionieren und wie man sie zur Datenmanipulation und Fehlerbehandlung nutzt.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wusstest du, dass du Daten automatisch vor dem Speichern ändern oder nach einer Abfrage manipulieren kannst? In diesem Video zeige ich dir, wie du mit **Mongoose Middlewares** (`pre` & `post` Hooks) Prozesse wie **Datenvalidierung, Passwort-Hashing oder Logging** automatisierst!"_

📌 **Agenda:**  
✅ Was sind Mongoose Middlewares?  
✅ `pre`-Middleware für Manipulation vor dem Speichern  
✅ `post`-Middleware für Verarbeitung nach Abfragen  
✅ Fehlerhandling mit Middlewares  
✅ Best Practices für effiziente Middlewares

---

## **🔹 2. Was sind Mongoose Middlewares? (1:00 – 2:30)**

📌 **Definition:**  
_Mongoose Middlewares sind Funktionen, die **vor (`pre`) oder nach (`post`) einer Datenbankoperation** ausgeführt werden._

📌 **Warum Middlewares nutzen?**  
✅ **Automatisches Daten-Preprocessing** (z. B. Passwort-Hashing).  
✅ **Logging von Datenänderungen**.  
✅ **Datenvalidierung und Bereinigung vor Speicherung**.  
✅ **Performance-Optimierung nach einer Abfrage**.

🎯 **Fazit:** **Middlewares helfen, Code zu organisieren und wiederkehrende Prozesse zu automatisieren!**

---

## **🔹 3. `pre`-Middleware – Daten vor dem Speichern manipulieren (2:30 – 5:30)**

📌 **Anwendungsfälle:**  
✅ **Passwort vor dem Speichern hashen**.  
✅ **Daten bereinigen (z. B. Strings trimmen, Kleinbuchstaben erzwingen)**.

📌 **Beispiel: Passwort vor dem Speichern hashen (`models/User.js`)**

```javascript
import mongoose from "mongoose";
import bcrypt from "bcrypt";

const userSchema = new mongoose.Schema({
  name: { type: String, required: true, trim: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true, select: false },
});

// Pre-Middleware: Hash das Passwort vor dem Speichern
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next(); // Nur wenn Passwort geändert wurde
  try {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
  } catch (error) {
    next(error);
  }
});

export default mongoose.model("User", userSchema);
```

📌 **Erklärung:**  
✅ `pre('save', callback)` → Wird **vor** dem Speichern (`save()`) ausgeführt.  
✅ `isModified('password')` → Verhindert doppeltes Hashing bei Updates.  
✅ **Ergebnis:** Das Passwort wird automatisch gehasht, bevor es gespeichert wird.

🎯 **Ergebnis:** **Nutzerpasswörter sind sicher, ohne dass wir Hashing explizit in jeder Route schreiben müssen!**

---

## **🔹 4. `post`-Middleware – Daten nach einer Operation verändern (5:30 – 7:30)**

📌 **Anwendungsfälle:**  
✅ **Loggen, wenn ein Dokument erstellt oder gelöscht wird**.  
✅ **Zusätzliche Felder hinzufügen (z. B. virtuelle Felder).**

📌 **Beispiel: Logging nach Benutzererstellung (`models/User.js`)**

```javascript
userSchema.post("save", function (doc, next) {
  console.log(`📢 Neuer Benutzer erstellt: ${doc.email}`);
  next();
});
```

📌 **Beispiel: `find`-Ergebnisse automatisch formatieren**

```javascript
userSchema.post("find", function (docs) {
  docs.forEach((doc) => (doc.name = doc.name.toUpperCase()));
});
```

🎯 **Ergebnis:** **Jeder gefundene Benutzer hat automatisch seinen Namen in Großbuchstaben.**

---

## **🔹 5. Fehlerhandling mit Middlewares (7:30 – 9:30)**

📌 **Problem:** Fehler während eines Vorgangs (z. B. E-Mail existiert bereits).  
📌 **Lösung:** **Middleware für Fehlerhandling nutzen!**

📌 **Globale Fehlerhandling-Middleware für E-Mail-Duplikate**

```javascript
userSchema.post("save", function (error, doc, next) {
  if (error.name === "MongoServerError" && error.code === 11000) {
    next(new Error("Diese E-Mail-Adresse wird bereits verwendet!"));
  } else {
    next(error);
  }
});
```

🎯 **Ergebnis:** **Benutzer erhalten eine verständliche Fehlermeldung bei doppelter E-Mail!**

---

## **🔹 6. `pre('remove')` – Daten verarbeiten vor dem Löschen (9:30 – 11:30)**

📌 **Anwendungsfälle:**  
✅ **Alle zugehörigen Daten eines Benutzers löschen (z. B. Posts, Kommentare)**.

📌 **Beispiel: Lösche alle `Post`-Dokumente, wenn ein Benutzer gelöscht wird**  
➡ **Datei: `models/User.js`**

```javascript
import Post from "./Post.js";

userSchema.pre("remove", async function (next) {
  await Post.deleteMany({ author: this._id });
  console.log(`🗑️ Alle Posts von ${this.email} wurden gelöscht`);
  next();
});
```

📌 **Test: Benutzer mit ID löschen**

```javascript
const user = await User.findById(userId);
await user.remove();
```

🎯 **Ergebnis:** **Sobald ein Benutzer gelöscht wird, werden auch alle seine Posts entfernt!**

---

## **🔹 7. Best Practices für Middlewares in Mongoose (11:30 – 12:30)**

📌 **🚀 Best Practices für Mongoose Middlewares:**  
✅ **Nutze `pre('save')` für Datenvalidierung und Transformation.**  
✅ **Nutze `post('find')`, um Ergebnisse zu verändern.**  
✅ **Verwende `pre('remove')`, um abhängige Daten zu löschen.**  
✅ **Fehlerhandling in `post('save', error, doc, next)` Middleware integrieren.**

📌 **Häufige Fehler vermeiden:**  
❌ **Fehlendes `next()` → Die Middleware blockiert den Prozess!**  
❌ **Globale `post('find')` Middleware kann die Performance beeinflussen.**

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **Mongoose Middlewares nutzt**, um Daten automatisch zu verändern und Fehler sauber zu behandeln. Mit diesen Techniken kannst du Daten sicher speichern, verarbeiten und verwalten!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Mongoose Middlewares nutzt du am meisten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Aggregation – So wertest du Daten effizient aus!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch eine Erweiterung mit Virtual Fields oder statischen Methoden ergänzen?** 😊
