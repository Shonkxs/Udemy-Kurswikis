### **📜 Videoskript: Indizes in Mongoose – So beschleunigst du deine MongoDB-Abfragen!**

🎬 **Titel:** Mongoose Indizes – So optimierst du deine MongoDB-Datenbank für schnelle Abfragen  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & Datenbank-Optimierer  
🎯 **Lernziel:** **Verstehen, wie Indizes in MongoDB und Mongoose funktionieren, um die Performance von Datenbankabfragen zu verbessern.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer langsamen Datenbankabfrage.  
🗣️ **Sprecher:**  
_"Dauern deine MongoDB-Abfragen zu lange? Dann fehlt dir wahrscheinlich ein Index! In diesem Video zeige ich dir, wie du mit **Mongoose Indizes** deine Abfragen **beschleunigst**, **Kollisionsprobleme vermeidest** und **beste Performance aus MongoDB herausholst**."_

📌 **Agenda:**  
✅ Was sind Indizes in MongoDB?  
✅ Indizes mit Mongoose erstellen (`index()`, `unique`, `compound indexes`)  
✅ Performanz-Boost mit Indizes testen  
✅ Best Practices für effiziente Indizes

---

## **🔹 2. Was sind Indizes in MongoDB? (1:00 – 2:30)**

📌 **Definition:**  
Ein **Index** in MongoDB ist eine **Datenstruktur**, die das **schnelle Suchen** von Dokumenten ermöglicht – ähnlich wie ein Inhaltsverzeichnis in einem Buch.

📌 **Warum sind Indizes wichtig?**  
✅ **Schnellere Suchanfragen (`find()`, `findOne()`)**  
✅ **Bessere Sortiergeschwindigkeit (`sort()`)**  
✅ **Weniger Speicherverbrauch durch gezielte Datenzugriffe**

📌 **Ohne Index (Langsame Suche)**

```javascript
await User.find({ email: "max@example.com" }); // Durchsucht ALLE Dokumente!
```

📌 **Mit Index (Schnelle Suche)**

```javascript
await User.find({ email: "max@example.com" }).hint({ email: 1 });
```

🎯 **Ergebnis:** **MongoDB muss nicht jedes Dokument durchgehen, sondern findet Daten direkt!**

---

## **🔹 3. Einfache Indizes in Mongoose (2:30 – 4:30)**

📌 **Index auf `email`-Feld setzen (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true, index: true },
  age: { type: Number, min: 0 },
});

export default mongoose.model("User", userSchema);
```

📌 **Was passiert hier?**  
✅ `unique: true` → Kein doppelter Wert erlaubt.  
✅ `index: true` → MongoDB erstellt automatisch einen Index für `email`.

🎯 **Ergebnis:** **Schnellere Suche nach E-Mail & bessere Konsistenz!**

---

## **🔹 4. Compound Indizes – Mehrere Felder indexieren (4:30 – 6:30)**

📌 **Warum?**  
Manchmal braucht man schnelle Suchanfragen auf **mehr als einem Feld** (z. B. `name + email`).

📌 **Compound Index auf `name` & `email` erstellen**

```javascript
userSchema.index({ name: 1, email: 1 });
```

📌 **Was passiert hier?**  
✅ **Index optimiert Suchen nach `name` UND `email`.**  
✅ **Sortieren nach `name` läuft schneller.**  
✅ **Besonders nützlich für häufige `find()`-Queries mit beiden Feldern.**

📌 **Effiziente Abfrage mit Compound Index**

```javascript
await User.find({ name: "Max", email: "max@example.com" });
```

🎯 **Ergebnis:** **MongoDB findet Daten noch schneller, da beide Felder indiziert sind!**

---

## **🔹 5. TTL (Time-To-Live) Indizes – Automatische Datenlöschung (6:30 – 8:30)**

📌 **Warum?**

- Perfekt für **Session-Daten, Logs oder temporäre Einträge**.
- Daten löschen sich **automatisch** nach einer bestimmten Zeit.

📌 **Index mit Ablaufzeit (`expiresAfterSeconds`) setzen**

```javascript
const sessionSchema = new mongoose.Schema({
  sessionId: String,
  createdAt: { type: Date, default: Date.now, expires: 3600 }, // Automatisch nach 1h löschen
});

export default mongoose.model("Session", sessionSchema);
```

📌 **Erklärung:**  
✅ **TTL-Index (`expires`) sorgt für automatische Löschung nach 1 Stunde.**  
✅ **Gut für Sitzungsverwaltung oder Logs.**

🎯 **Ergebnis:** **MongoDB hält die Datenbank automatisch sauber!**

---

## **🔹 6. Indizes für Geodaten (`2dsphere`) – Standortbasierte Suchen (8:30 – 10:30)**

📌 **Anwendungsfall:**

- **Suchen nach Orten in der Nähe (z. B. Restaurants, Läden, User-Standorte)**.
- **Geospatiale Abfragen schneller machen**.

📌 **Geo-Index auf `location`-Feld setzen (`models/Place.js`)**

```javascript
const placeSchema = new mongoose.Schema({
  name: String,
  location: {
    type: { type: String, enum: ["Point"], required: true },
    coordinates: { type: [Number], required: true },
  },
});

placeSchema.index({ location: "2dsphere" }); // Index für Geodaten

export default mongoose.model("Place", placeSchema);
```

📌 **Schnelle Geo-Abfrage – Orte in einem Umkreis von 5 km finden**

```javascript
const places = await Place.find({
  location: {
    $near: {
      $geometry: { type: "Point", coordinates: [13.405, 52.52] },
      $maxDistance: 5000,
    },
  },
});
```

🎯 **Ergebnis:** **Perfekt für standortbasierte Apps!**

---

## **🔹 7. Indizes auf Performanz testen (10:30 – 12:00)**

📌 **Index-Analyse mit `explain()`**

```javascript
const result = await User.find({ email: "max@example.com" }).explain(
  "executionStats"
);
console.log(result);
```

📌 **Was zeigt das Ergebnis?**  
✅ **`totalKeysExamined` → Wie viele Indexeinträge geprüft wurden**  
✅ **`executionTimeMillis` → Wie lange die Abfrage dauerte**  
✅ **`indexUsed` → Welcher Index verwendet wurde**

🎯 **Ergebnis:** **Überprüfe, ob MongoDB wirklich den Index nutzt!**

---

## **🔹 8. Best Practices für Indizes in Mongoose (12:00 – 13:30)**

📌 **🚀 Best Practices für Indizes:**  
✅ **Setze nur Indizes für Felder, die oft für Suchen verwendet werden.**  
✅ **Nutze `compound indexes`, wenn du oft nach mehreren Feldern suchst.**  
✅ **Vermeide zu viele Indizes – sie verlangsamen `insert()` & `update()`.**  
✅ **Nutze `explain()` für Performance-Analysen.**

📌 **Häufige Fehler vermeiden:**  
❌ **Zu viele Indizes → Schreiboperationen werden langsamer.**  
❌ **Kein `index: true` auf häufig gesuchten Feldern → Langsame Abfragen.**  
❌ **Vergessen, `expires` für temporäre Daten zu nutzen → Datenbank wird zu groß.**

---

## **🔹 9. Fazit & Outro (13:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **Mongoose Indizes nutzt**, um deine MongoDB-Abfragen zu beschleunigen! Mit diesen Techniken kannst du die Performance deiner Datenbank massiv verbessern!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Art von Index nutzt du am häufigsten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Aggregation – So wertest du Daten effizient aus!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Sharding oder Volltext-Indizes ergänzen?** 😊
