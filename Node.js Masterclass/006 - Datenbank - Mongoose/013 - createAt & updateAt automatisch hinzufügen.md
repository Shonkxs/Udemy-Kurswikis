### **📜 Videoskript: `createdAt` & `updatedAt` automatisch in Mongoose hinzufügen**

🎬 **Titel:** Automatische Zeitstempel in Mongoose – `createdAt` & `updatedAt` richtig nutzen  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** **Verstehen, wie man mit Mongoose automatisch `createdAt` und `updatedAt` verwaltet, um Datenhistorie & Änderungszeiten nachzuvollziehen.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer Datenbank ohne Zeitstempel.  
🗣️ **Sprecher:**  
_"Willst du wissen, wann ein Dokument in deiner MongoDB-Datenbank erstellt oder zuletzt aktualisiert wurde? Mit **automatischen Zeitstempeln (`createdAt` & `updatedAt`) in Mongoose** kannst du diese Informationen **ohne manuelle Updates** speichern!"_

📌 **Agenda:**  
✅ Warum sind `createdAt` & `updatedAt` wichtig?  
✅ Automatische Zeitstempel in Mongoose aktivieren  
✅ `updatedAt` wird automatisch aktualisiert  
✅ Best Practices & häufige Fehler vermeiden

---

## **🔹 2. Warum sind `createdAt` & `updatedAt` wichtig? (1:00 – 2:30)**

📌 **Nutzen von Zeitstempeln:**  
✅ **Nachvollziehbarkeit** → Wann wurde ein Dokument erstellt oder geändert?  
✅ **Datenaktualität** → Hilfreich für Caching & API-Responses  
✅ **Datenhistorie** → Gut für Log- & Tracking-Systeme

📌 **Typische Anwendungsfälle:**  
✅ **Erstellungszeit für Benutzer (`createdAt`)**  
✅ **Letzte Änderung von Blogposts (`updatedAt`)**  
✅ **Automatische Sortierung nach Aktualisierungsdatum**

🎯 **Fazit:** **Mit `createdAt` & `updatedAt` bleibt deine Datenbank organisiert & nachvollziehbar!**

---

## **🔹 3. Automatische Zeitstempel in Mongoose aktivieren (2:30 – 4:30)**

📌 **Einfache Implementierung mit `timestamps: true`**  
➡ **Datei: `models/User.js`**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    name: { type: String, required: true },
    email: { type: String, required: true, unique: true },
  },
  { timestamps: true }
); // Automatisch `createdAt` & `updatedAt` hinzufügen

export default mongoose.model("User", userSchema);
```

📌 **Was passiert hier?**  
✅ **`timestamps: true` erzeugt automatisch zwei Felder:**

- `createdAt`: Zeitpunkt der Erstellung
- `updatedAt`: Zeitpunkt der letzten Änderung

📌 **Ein neues Dokument speichern**

```javascript
import User from "./models/User.js";

const createUser = async () => {
  const user = await User.create({ name: "Max", email: "max@example.com" });
  console.log("✅ Benutzer erstellt:", user);
};

createUser();
```

🎯 **Ergebnis:** **MongoDB speichert automatisch `createdAt` & `updatedAt`!**

---

## **🔹 4. `updatedAt` wird automatisch aktualisiert (4:30 – 6:30)**

📌 **Problem:** Wie wird `updatedAt` geändert?

- **Mongoose aktualisiert `updatedAt` automatisch, wenn das Dokument geändert wird!**

📌 **Benutzer aktualisieren (`updatedAt` ändert sich automatisch)**

```javascript
const updateUser = async (userId) => {
  const user = await User.findByIdAndUpdate(
    userId,
    { name: "Max Mustermann" },
    { new: true }
  );
  console.log("📝 Benutzer aktualisiert:", user);
};

updateUser("66123abc456def7890123456");
```

📌 **Erwartetes Ergebnis:**  
✅ `updatedAt` erhält das neue Änderungsdatum.  
✅ `createdAt` bleibt unverändert.

📌 **`updatedAt` wird **NICHT** aktualisiert, wenn `timestamps: false` gesetzt ist!**

```javascript
const userSchema = new mongoose.Schema(
  {
    name: String,
    email: String,
  },
  { timestamps: false }
); // KEINE automatischen Zeitstempel
```

🎯 **Ergebnis:** **Mongoose kümmert sich automatisch um `updatedAt` – kein manueller Aufwand nötig!**

---

## **🔹 5. Benutzerdefinierte Namen für `createdAt` & `updatedAt` (6:30 – 8:00)**

📌 **Problem:** Manche APIs erfordern spezifische Feldnamen (z. B. `created_at` statt `createdAt`).

📌 **Benutzerdefinierte Zeitstempelnamen setzen**

```javascript
const userSchema = new mongoose.Schema(
  {
    name: String,
    email: String,
  },
  { timestamps: { createdAt: "created_at", updatedAt: "updated_at" } }
);
```

📌 **Datenbank-Eintrag sieht so aus:**

```json
{
  "_id": "66123abc456def7890123456",
  "name": "Max",
  "email": "max@example.com",
  "created_at": "2024-03-18T12:00:00.000Z",
  "updated_at": "2024-03-18T12:30:00.000Z"
}
```

🎯 **Ergebnis:** **Flexibilität bei API-Anforderungen & Namenskonventionen!**

---

## **🔹 6. Best Practices für `createdAt` & `updatedAt` (8:00 – 9:30)**

📌 **🚀 Best Practices für Zeitstempel in MongoDB:**  
✅ **`timestamps: true` immer aktivieren, außer für statische Daten.**  
✅ **Benutzerdefinierte Namen setzen, wenn APIs es erfordern.**  
✅ **Nutze `sort({ updatedAt: -1 })` für aktuelle Daten zuerst.**  
✅ **Falls `updatedAt` manuell gesetzt werden muss, nutze:**

```javascript
await User.findByIdAndUpdate(userId, { updatedAt: new Date() });
```

📌 **Häufige Fehler vermeiden:**  
❌ **Vergessen, `timestamps: true` zu setzen → Keine Zeitstempel!**  
❌ **`updatedAt` mit `updateMany()` nicht aktualisiert? → `{ timestamps: true }` explizit setzen!**  
❌ **Manuelles `createdAt`-Ändern → API-Logik könnte durcheinanderkommen.**

---

## **🔹 7. Fazit & Outro (9:30 – 11:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit **Mongoose automatisch `createdAt` & `updatedAt` setzt** und wie du damit deine API-Daten sauber und nachvollziehbar hältst!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie nutzt ihr Zeitstempel in euren Projekten? Schreibt’s in die Kommentare!"  
✅ **Nächstes Video:** _"Soft Deletes mit Mongoose – Löschen ohne Datenverlust!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für eine `lastModifiedBy`-Funktion (wer hat zuletzt geändert?) ergänzen?** 😊
