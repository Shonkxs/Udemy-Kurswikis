### **📜 Videoskript: Virtuelle Felder in Mongoose – Daten berechnen, ohne sie zu speichern!**

🎬 **Titel:** Virtuelle Felder in Mongoose – So berechnest du Daten dynamisch ohne Speicherung  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** **Verstehen, wie man virtuelle Felder (`virtuals`) in Mongoose nutzt, um dynamische Daten zu berechnen, ohne sie in der Datenbank zu speichern.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer MongoDB-Datenbank.  
🗣️ **Sprecher:**  
_"Hast du dich jemals gefragt, wie du zusätzliche Daten zu einem Dokument hinzufügen kannst, ohne sie wirklich in der Datenbank zu speichern? Die Lösung heißt **virtuelle Felder in Mongoose!** Damit kannst du **dynamische Werte berechnen**, ohne die Datenbankgröße zu erhöhen. In diesem Video zeige ich dir, wie du virtuelle Felder richtig nutzt!"_

📌 **Agenda:**  
✅ Was sind virtuelle Felder?  
✅ Virtuelle Felder für zusammengesetzte Werte  
✅ Virtuelle Felder für Formatierungen  
✅ Virtuelle Felder in API-Responses anzeigen  
✅ Best Practices & häufige Fehler

---

## **🔹 2. Was sind virtuelle Felder? (1:00 – 2:30)**

📌 **Definition:**

- Virtuelle Felder in Mongoose **werden nicht in der Datenbank gespeichert**, sondern **on-the-fly berechnet**.
- Ideal für **zusammengesetzte Daten, Formatierungen oder Berechnungen**.

📌 **Warum virtuelle Felder nutzen?**  
✅ **Reduzieren den Speicherplatz**, da keine redundanten Daten gespeichert werden.  
✅ **Ermöglichen benutzerdefinierte API-Responses**, ohne die DB zu verändern.  
✅ **Dynamische Werte ohne Mehraufwand in Abfragen berechnen.**

🎯 **Fazit:** **Virtuelle Felder helfen, Daten effizient zu verwalten & zu präsentieren!**

---

## **🔹 3. Virtuelle Felder für zusammengesetzte Werte (2:30 – 5:30)**

📌 **Beispiel: `fullName` aus `firstName` & `lastName` generieren (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  firstName: { type: String, required: true },
  lastName: { type: String, required: true },
  email: { type: String, required: true, unique: true },
});

// Virtuelles Feld für den vollständigen Namen
userSchema.virtual("fullName").get(function () {
  return `${this.firstName} ${this.lastName}`;
});

export default mongoose.model("User", userSchema);
```

📌 **Erklärung:**  
✅ **Das Feld `fullName` wird automatisch berechnet.**  
✅ **Kein Speichern nötig – MongoDB bleibt schlank!**

📌 **Test in einer Route (`server.js`)**

```javascript
import User from "./models/User.js";

const getUser = async (userId) => {
  const user = await User.findById(userId);
  console.log("👤 Benutzer:", user.fullName);
};

getUser("66123abc456def7890123456");
```

🎯 **Ergebnis:** **`fullName` wird berechnet, obwohl es nicht in der DB existiert!**

---

## **🔹 4. Virtuelle Felder für Formatierungen (5:30 – 7:30)**

📌 **Beispiel: `createdAt` als formatiertes Datum anzeigen**

```javascript
import { format } from "date-fns";

userSchema.virtual("formattedDate").get(function () {
  return format(this.createdAt, "dd.MM.yyyy HH:mm");
});
```

📌 **Erklärung:**  
✅ **Statt `createdAt` im ISO-Format wird ein schönes Datum angezeigt.**

📌 **Test in einer Route (`server.js`)**

```javascript
const user = await User.findById("66123abc456def7890123456");
console.log("📅 Erstellt am:", user.formattedDate);
```

🎯 **Ergebnis:** **Daten sind sofort API-freundlich formatiert!**

---

## **🔹 5. Virtuelle Felder in API-Responses anzeigen (7:30 – 9:30)**

📌 **Problem:** Standardmäßig zeigt Mongoose **keine virtuellen Felder in `toJSON()` oder `toObject()`.**  
📌 **Lösung:** `toJSON: { virtuals: true }` aktivieren!

📌 **Schema mit virtuellen Feldern exportieren**

```javascript
const userSchema = new mongoose.Schema(
  {
    firstName: String,
    lastName: String,
  },
  { timestamps: true, toJSON: { virtuals: true } }
);
```

📌 **Jetzt erscheinen virtuelle Felder in API-Responses:**

```javascript
app.get("/users/:id", async (req, res) => {
  const user = await User.findById(req.params.id);
  res.json(user); // `fullName` wird jetzt angezeigt!
});
```

📌 **Erwartete API-Response:**

```json
{
  "_id": "66123abc456def7890123456",
  "firstName": "Max",
  "lastName": "Mustermann",
  "fullName": "Max Mustermann",
  "createdAt": "2024-03-18T12:00:00.000Z",
  "formattedDate": "18.03.2024 12:00"
}
```

🎯 **Ergebnis:** **Virtuelle Felder erscheinen automatisch in API-Responses!**

---

## **🔹 6. Virtuelle Setter für zusätzliche Logik (9:30 – 11:30)**

📌 **Problem:** Wie speichert man dynamisch berechnete Werte?  
📌 **Lösung:** **Virtuelle Setter ändern Daten beim Speichern.**

📌 **Beispiel: `fullName` automatisch in `firstName` & `lastName` aufteilen**

```javascript
userSchema
  .virtual("fullName")
  .get(function () {
    return `${this.firstName} ${this.lastName}`;
  })
  .set(function (value) {
    const parts = value.split(" ");
    this.firstName = parts[0];
    this.lastName = parts.slice(1).join(" ");
  });
```

📌 **Jetzt kann man `fullName` direkt setzen!**

```javascript
const user = new User();
user.fullName = "Anna Schmidt";
console.log(user.firstName); // "Anna"
console.log(user.lastName); // "Schmidt"
```

🎯 **Ergebnis:** **Volle Flexibilität bei der Datenverarbeitung!**

---

## **🔹 7. Best Practices für virtuelle Felder (11:30 – 12:30)**

📌 **🚀 Best Practices für virtuelle Felder in MongoDB:**  
✅ **Nutze `toJSON: { virtuals: true }`, um virtuelle Felder in API-Responses anzuzeigen.**  
✅ **Vermeide zu viele Berechnungen in virtuellen Feldern – MongoDB ist keine Rechenmaschine.**  
✅ **Nutze `virtual.set()`, wenn du Eingaben transformieren möchtest.**  
✅ **Verwende `date-fns` oder `moment` für saubere Datumsformatierungen.**

📌 **Häufige Fehler vermeiden:**  
❌ **Virtuelle Felder in Abfragen nutzen (`find({ fullName: "Max Mustermann" })` geht nicht).**  
❌ **Vergessen, `toJSON: { virtuals: true }` zu setzen – dann erscheinen sie nicht in der API.**  
❌ **Teure Berechnungen in virtuellen Feldern ausführen (z. B. Summen aus `populate()`).**

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **virtuelle Felder in Mongoose** nutzt, um dynamische Daten zu berechnen, ohne sie in der Datenbank zu speichern. Damit kannst du APIs sauberer & effizienter gestalten!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche virtuellen Felder nutzt du in deinen Projekten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Aggregation in Mongoose – Daten effizient auswerten!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für virtuelle Felder mit `populate()` ergänzen?** 😊
