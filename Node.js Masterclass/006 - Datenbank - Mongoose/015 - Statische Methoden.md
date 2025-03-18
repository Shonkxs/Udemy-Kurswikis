### **📜 Videoskript: Statische Methoden in Mongoose – Effiziente Model-Helper für deine Datenbank**

🎬 **Titel:** Statische Methoden in Mongoose – So erstellst du leistungsstarke Model-Funktionen  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** **Verstehen, wie man statische Methoden (`statics`) in Mongoose-Modellen nutzt, um wiederverwendbare und optimierte Abfragen bereitzustellen.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer komplexen MongoDB-Abfrage.  
🗣️ **Sprecher:**  
_"Schreibst du immer wieder die gleichen MongoDB-Abfragen? Mit **statischen Methoden in Mongoose** kannst du **wiederverwendbare Funktionen** direkt im Model definieren und deine Codebasis sauberer & effizienter machen! In diesem Video zeige ich dir, wie du sie richtig nutzt!"_

📌 **Agenda:**  
✅ Was sind statische Methoden in Mongoose?  
✅ Statische Methoden für Datenbankabfragen erstellen  
✅ Statische Methoden für komplexe Logik nutzen  
✅ Best Practices für performante Abfragen

---

## **🔹 2. Was sind statische Methoden in Mongoose? (1:00 – 2:30)**

📌 **Definition:**

- Statische Methoden (`statics`) sind **Methoden, die direkt auf dem Model aufgerufen werden**.
- Sie sind ideal für **häufig wiederkehrende Abfragen & Business-Logik**.

📌 **Vergleich:**  
❌ **Ohne statische Methoden:**

```javascript
const users = await User.find({ role: "admin" });
```

✅ **Mit statischer Methode:**

```javascript
const users = await User.findAdmins();
```

🎯 **Ergebnis:** **Kürzerer, sauberer & wiederverwendbarer Code!**

---

## **🔹 3. Statische Methoden für Datenbankabfragen (2:30 – 5:30)**

📌 **Beispiel: `findAdmins()` – Alle Admins abrufen (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  role: { type: String, enum: ["user", "admin"], default: "user" },
});

// Statische Methode für Admin-Suche
userSchema.statics.findAdmins = function () {
  return this.find({ role: "admin" });
};

export default mongoose.model("User", userSchema);
```

📌 **Nutzung in einer Route (`server.js`)**

```javascript
const getAdmins = async () => {
  const admins = await User.findAdmins();
  console.log("👑 Admins:", admins);
};

getAdmins();
```

🎯 **Ergebnis:** **Saubere & wiederverwendbare Funktion für Admin-Suche!**

---

## **🔹 4. Statische Methoden für komplexe Logik (5:30 – 8:00)**

📌 **Beispiel: `findRecentUsers()` – Benutzer, die in den letzten 7 Tagen erstellt wurden**

```javascript
userSchema.statics.findRecentUsers = function () {
  const lastWeek = new Date();
  lastWeek.setDate(lastWeek.getDate() - 7);
  return this.find({ createdAt: { $gte: lastWeek } });
};
```

📌 **Nutzung in einer API-Route (`server.js`)**

```javascript
app.get("/users/recent", async (req, res) => {
  const users = await User.findRecentUsers();
  res.json(users);
});
```

🎯 **Ergebnis:** **Einfacher Zugriff auf häufig benötigte Filterabfragen!**

---

## **🔹 5. Statische Methoden mit Aggregation (8:00 – 10:00)**

📌 **Beispiel: `getUserStatistics()` – Anzahl der Benutzer & Admins**

```javascript
userSchema.statics.getUserStatistics = function () {
  return this.aggregate([{ $group: { _id: "$role", count: { $sum: 1 } } }]);
};
```

📌 **Nutzung in einer Route (`server.js`)**

```javascript
const getUserStats = async () => {
  const stats = await User.getUserStatistics();
  console.log("📊 Benutzerstatistiken:", stats);
};

getUserStats();
```

🎯 **Ergebnis:** **Dynamische Statistiken direkt aus der Datenbank abrufen!**

---

## **🔹 6. Statische Methoden mit Parametern (10:00 – 11:30)**

📌 **Beispiel: `findByRole(role)` – Benutzer nach Rolle abrufen**

```javascript
userSchema.statics.findByRole = function (role) {
  return this.find({ role });
};
```

📌 **Nutzung mit Parameter (`server.js`)**

```javascript
const getUsersByRole = async (role) => {
  const users = await User.findByRole(role);
  console.log(`🔍 Benutzer mit Rolle ${role}:`, users);
};

getUsersByRole("user");
```

🎯 **Ergebnis:** **Flexible Methoden mit dynamischen Parametern!**

---

## **🔹 7. Best Practices für statische Methoden (11:30 – 12:30)**

📌 **🚀 Best Practices für effiziente statische Methoden:**  
✅ **Nutze sie für häufige Abfragen (`findByRole()`, `findRecentUsers()`).**  
✅ **Reduziere Code-Duplikation durch Wiederverwendung in Routen.**  
✅ **Vermeide Methoden für einmalige Abfragen – nutze stattdessen `find()`.**  
✅ **Nutze `aggregate()`, wenn Berechnungen nötig sind.**

📌 **Häufige Fehler vermeiden:**  
❌ **Zu viele komplexe Methoden in einem Model – hält die Codebasis unübersichtlich.**  
❌ **Unnötige statische Methoden für einfache `find()`-Abfragen.**  
❌ **Fehlendes `return this.find(...)` – Methode gibt nichts zurück!**

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **statische Methoden in Mongoose** nutzt, um **effiziente & wiederverwendbare Datenbankabfragen** zu erstellen. Damit kannst du deine API optimieren und deine Codebasis sauberer halten!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche statischen Methoden nutzt ihr in euren Projekten? Schreibt’s in die Kommentare!"  
✅ **Nächstes Video:** _"Virtuelle Felder in Mongoose – Berechnete Werte ohne Speicherung!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für `findOneAndUpdate()` als statische Methode ergänzen?** 😊
