### **📜 Videoskript: Soft Deletes mit Mongoose – Löschen ohne Datenverlust!**

🎬 **Titel:** Soft Deletes mit Mongoose – Wie du Daten sicher löschst, ohne sie zu verlieren  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** **Verstehen, wie man mit Soft Deletes in Mongoose Dokumente als "gelöscht" markiert, anstatt sie endgültig zu entfernen.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer versehentlich gelöschten Datenbank.  
🗣️ **Sprecher:**  
_"Hast du jemals aus Versehen Daten aus der Datenbank gelöscht und keine Möglichkeit gehabt, sie wiederherzustellen? Mit **Soft Deletes in Mongoose** kannst du Daten als **"gelöscht" markieren**, ohne sie wirklich zu entfernen – und sie später einfach wiederherstellen!"_

📌 **Agenda:**  
✅ Was sind Soft Deletes?  
✅ Soft Deletes mit einem `deleted`-Flag umsetzen  
✅ Automatisches Filtern gelöschter Daten (`pre` Middleware)  
✅ Gelöschte Daten wiederherstellen  
✅ Best Practices für Soft Deletes

---

## **🔹 2. Was sind Soft Deletes? (1:00 – 2:30)**

📌 **Definition:**

- **Soft Deletes bedeutet, dass Daten nicht wirklich gelöscht werden, sondern nur als "gelöscht" markiert werden.**
- **Ein zusätzliches Feld (`deleted` oder `deletedAt`) speichert den Löschstatus.**

📌 **Warum Soft Deletes nutzen?**  
✅ **Verhindert versehentlichen Datenverlust.**  
✅ **Bietet eine einfache Möglichkeit, gelöschte Daten wiederherzustellen.**  
✅ **Hilfreich für Audit-Logs & Recycle-Bin-Funktionen.**

🎯 **Fazit:** **Soft Deletes ermöglichen sicheres Datenmanagement!**

---

## **🔹 3. Soft Deletes mit `deleted`-Flag umsetzen (2:30 – 5:30)**

📌 **Benutzer-Schema mit `deleted`-Flag (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  deleted: { type: Boolean, default: false }, // Soft-Delete Flag
  deletedAt: { type: Date, default: null }, // Zeitpunkt der Löschung
});

export default mongoose.model("User", userSchema);
```

📌 **Soft Delete-Funktion (`server.js`)**

```javascript
import User from "./models/User.js";

const softDeleteUser = async (userId) => {
  const user = await User.findByIdAndUpdate(
    userId,
    { deleted: true, deletedAt: new Date() },
    { new: true }
  );
  console.log("🚮 Benutzer als gelöscht markiert:", user);
};

softDeleteUser("66123abc456def7890123456");
```

🎯 **Ergebnis:** **Benutzer ist als "gelöscht" markiert, bleibt aber in der Datenbank erhalten!**

---

## **🔹 4. Automatisches Filtern gelöschter Daten (`pre` Middleware) (5:30 – 7:30)**

📌 **Problem:**

- Standard-Queries (`find()`, `findOne()`) geben auch gelöschte Benutzer zurück.
- **Lösung:** `pre`-Middleware filtert gelöschte Dokumente automatisch aus.

📌 **Middleware in `models/User.js` hinzufügen**

```javascript
userSchema.pre(/^find/, function (next) {
  this.where({ deleted: false }); // Nur nicht-gelöschte Benutzer abrufen
  next();
});
```

📌 **Jetzt zeigt `find()` nur aktive Benutzer!**

```javascript
const users = await User.find();
console.log("👥 Aktive Benutzer:", users);
```

🎯 **Ergebnis:** **Gelöschte Benutzer tauchen nicht mehr in normalen Abfragen auf!**

---

## **🔹 5. Gelöschte Daten wiederherstellen (7:30 – 9:30)**

📌 **Wiederherstellung einer Soft Deleted Entity**

```javascript
const restoreUser = async (userId) => {
  const user = await User.findByIdAndUpdate(
    userId,
    { deleted: false, deletedAt: null },
    { new: true }
  );
  console.log("♻️ Benutzer wiederhergestellt:", user);
};

restoreUser("66123abc456def7890123456");
```

✅ **Setzt `deleted` auf `false` und löscht `deletedAt`.**

📌 **Alle gelöschten Benutzer abrufen (`find()`)**

```javascript
const getDeletedUsers = async () => {
  const deletedUsers = await User.find({ deleted: true });
  console.log("🗑️ Gelöschte Benutzer:", deletedUsers);
};

getDeletedUsers();
```

🎯 **Ergebnis:** **Gelöschte Benutzer können wiederhergestellt oder endgültig entfernt werden!**

---

## **🔹 6. Dauerhafte Löschung von Soft-Deleted Daten (9:30 – 11:30)**

📌 **Alle Benutzer, die länger als 30 Tage gelöscht sind, endgültig entfernen**

```javascript
const deleteOldUsers = async () => {
  const thirtyDaysAgo = new Date();
  thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);

  const result = await User.deleteMany({
    deleted: true,
    deletedAt: { $lte: thirtyDaysAgo },
  });
  console.log("❌ Endgültig gelöschte Benutzer:", result.deletedCount);
};

deleteOldUsers();
```

🎯 **Ergebnis:** **Nur Benutzer, die wirklich lange gelöscht sind, werden entfernt!**

---

## **🔹 7. Best Practices für Soft Deletes (11:30 – 12:30)**

📌 **🚀 Best Practices für Soft Deletes in MongoDB:**  
✅ **Nutze `deleted: false` als Standardfilter in `pre('find')` Middleware.**  
✅ **Speichere den Löschzeitpunkt (`deletedAt`) für bessere Nachverfolgung.**  
✅ **Füge ein automatisches Cleanup-Skript hinzu (`deleteMany` für alte Daten).**  
✅ **Biete eine API-Route für die Wiederherstellung (`restoreUser()`).**

📌 **Häufige Fehler vermeiden:**  
❌ **Daten versehentlich hart löschen – `deleteOne()` statt `findByIdAndUpdate()`.**  
❌ **Fehlende Wiederherstellungsoption – Benutzer können nicht reaktiviert werden.**  
❌ **Kein Logging für gelöschte Daten – Audit-Trail fehlt.**

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit **Soft Deletes in Mongoose** Daten sicher löschst, sie einfach wiederherstellen kannst und versehentlichen Datenverlust vermeidest!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Nutzt ihr Soft Deletes oder löscht ihr Daten endgültig? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung – So beschleunigst du deine Abfragen!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch eine Erweiterung mit `mongoose-delete` Plugin oder automatischer Archivierung ergänzen?** 😊
