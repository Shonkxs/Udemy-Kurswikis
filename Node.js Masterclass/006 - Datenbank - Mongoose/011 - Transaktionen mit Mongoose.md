### **📜 Videoskript: Transaktionen in Mongoose – Sicheres Datenhandling mit MongoDB**

🎬 **Titel:** Transaktionen in Mongoose – So sicherst du deine Datenbank-Operationen ab!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & Datenbank-Administratoren  
🎯 **Lernziel:** **Verstehen, wie man mit MongoDB-Transaktionen mehrere Datenbankoperationen in Mongoose sicher und atomar ausführt.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einem Fehler in einer Datenbankoperation.  
🗣️ **Sprecher:**  
_"Stell dir vor, du führst mehrere Datenbankoperationen durch, aber eine schlägt fehl – und die anderen bleiben trotzdem gespeichert. Das kann zu **Inkonsistenzen in deiner Datenbank** führen! Die Lösung? **Transaktionen in Mongoose!** In diesem Video zeige ich dir, wie du mehrere Operationen **atomar ausführst** und **Datenverlust verhinderst**."_

📌 **Agenda:**  
✅ Was sind Transaktionen in MongoDB?  
✅ Transaktionen in Mongoose mit `session` erstellen  
✅ Fehlerhandling & automatisches Rollback  
✅ Best Practices für sichere Transaktionen

---

## **🔹 2. Was sind Transaktionen in MongoDB? (1:00 – 2:30)**

📌 **Definition:**  
Eine **Transaktion** stellt sicher, dass **mehrere Datenbankoperationen entweder vollständig oder gar nicht** ausgeführt werden.

📌 **Warum sind Transaktionen wichtig?**  
✅ **Schützen vor inkonsistenten Daten**  
✅ **Stellen sicher, dass verknüpfte Dokumente korrekt aktualisiert werden**  
✅ **Ermöglichen sicheres Arbeiten mit mehreren Collections**

📌 **Beispiel ohne Transaktion (Problem!)**

```javascript
await User.create({ name: "Max" });
await Order.create({ userId: "123", totalAmount: 100 }); // Falls dieser Fehler auftritt, bleibt der User trotzdem gespeichert!
```

🎯 **Ergebnis:** **Teilweise gespeicherte Daten können Probleme verursachen!**

---

## **🔹 3. Eine Transaktion in Mongoose erstellen (2:30 – 5:30)**

📌 **Projektstruktur**

```plaintext
/my-express-app
 ├── models
 │   ├── User.js   # Benutzer-Schema
 │   ├── Order.js  # Bestell-Schema
 ├── server.js     # Express Server
 ├── package.json
```

📌 **Schema für Benutzer (`models/User.js`)**

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
});

export default mongoose.model("User", userSchema);
```

📌 **Schema für Bestellungen (`models/Order.js`)**

```javascript
const orderSchema = new mongoose.Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
  totalAmount: { type: Number, required: true },
});

export default mongoose.model("Order", orderSchema);
```

📌 **Transaktion in `server.js` implementieren**

```javascript
import mongoose from "mongoose";
import User from "./models/User.js";
import Order from "./models/Order.js";

const createOrderWithUser = async () => {
  const session = await mongoose.startSession();
  session.startTransaction(); // Transaktion starten

  try {
    const user = await User.create(
      [{ name: "Max", email: "max@example.com" }],
      { session }
    );
    const order = await Order.create(
      [{ userId: user[0]._id, totalAmount: 100 }],
      { session }
    );

    await session.commitTransaction(); // Alle Änderungen übernehmen
    session.endSession();
    console.log("✅ Benutzer & Bestellung gespeichert!");
  } catch (error) {
    await session.abortTransaction(); // Alle Änderungen zurücksetzen
    session.endSession();
    console.error(
      "❌ Fehler aufgetreten, Transaktion zurückgesetzt:",
      error.message
    );
  }
};

createOrderWithUser();
```

📌 **Erklärung:**  
✅ **`startSession()` startet eine Transaktion.**  
✅ **`commitTransaction()` speichert alle Änderungen.**  
✅ **`abortTransaction()` setzt alles zurück, falls ein Fehler auftritt.**

🎯 **Ergebnis:** **Entweder werden **beide Dokumente gespeichert** oder **keines** – keine inkonsistenten Daten!**

---

## **🔹 4. Automatische Wiederholungslogik für Transaktionen (5:30 – 7:30)**

📌 **Problem:**

- **Netzwerkfehler oder Verbindungsprobleme** können eine Transaktion fehlschlagen lassen.
- **Lösung:** Eine automatische Wiederholungslogik einbauen!

📌 **Transaktion mit Retry-Logik**

```javascript
const executeWithRetry = async (fn, maxRetries = 3) => {
  for (let i = 0; i < maxRetries; i++) {
    const session = await mongoose.startSession();
    session.startTransaction();

    try {
      const result = await fn(session);
      await session.commitTransaction();
      session.endSession();
      return result;
    } catch (error) {
      await session.abortTransaction();
      session.endSession();
      console.warn(`⚠️ Versuch ${i + 1} fehlgeschlagen:`, error.message);
      if (i === maxRetries - 1)
        throw new Error("Maximale Anzahl an Versuchen erreicht");
    }
  }
};

executeWithRetry(createOrderWithUser);
```

🎯 **Ergebnis:** **Falls eine Transaktion fehlschlägt, wird sie automatisch erneut versucht!**

---

## **🔹 5. Best Practices für Transaktionen (7:30 – 10:30)**

📌 **🚀 Best Practices für sichere Transaktionen:**  
✅ **Nur verwenden, wenn wirklich nötig (sie sind langsamer als normale Abfragen).**  
✅ **Immer Fehlerhandling mit `try/catch` & `abortTransaction()`.**  
✅ **Nur auf MongoDB **Replica Sets** verfügbar (kein Standalone-Modus).**  
✅ **Setze ein Zeitlimit für lange laufende Transaktionen.**

📌 **Maximale Transaktionsdauer setzen (`transactionLifetimeLimitSeconds`)**

```javascript
const sessionOptions = {
  readConcern: { level: "snapshot" },
  writeConcern: { w: "majority" },
};
session.startTransaction(sessionOptions);
```

🎯 **Ergebnis:** **Schnellere & stabilere Transaktionen!**

---

## **🔹 6. Fazit & Outro (10:30 – 12:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **sichere Transaktionen in Mongoose** erstellst, um mehrere Datenbankoperationen **atomar** durchzuführen. Damit kannst du verhindern, dass Daten verloren gehen oder inkonsistent bleiben!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon Transaktionen in MongoDB genutzt? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung – So beschleunigst du deine Abfragen!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für parallele Transaktionen oder Optimierung mit `writeConcern` ergänzen?** 😊
