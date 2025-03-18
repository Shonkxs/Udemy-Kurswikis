### **📜 Videoskript: Aggregation in Mongoose – Effiziente Datenanalyse mit MongoDB**

🎬 **Titel:** Aggregation in Mongoose – So wertest du Daten effizient aus!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & Datenbank-Optimierer  
🎯 **Lernziel:** **Verstehen, wie man mit der Aggregation-Pipeline in Mongoose leistungsstarke Abfragen und Analysen auf MongoDB-Daten durchführt.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor & einer großen MongoDB-Datenbank.  
🗣️ **Sprecher:**  
_"Hast du dich jemals gefragt, wie du **komplexe Datenanalysen direkt in MongoDB** durchführen kannst? Mit der **Aggregation-Pipeline in Mongoose** kannst du leistungsstarke Statistiken berechnen, Daten gruppieren und transformieren – direkt in der Datenbank! In diesem Video lernst du, wie du mit **Aggregation** große Datenmengen effizient analysierst!"_

📌 **Agenda:**  
✅ Was ist Aggregation in MongoDB?  
✅ Grundlegende Aggregation-Pipeline (`$match`, `$group`, `$sort`)  
✅ Fortgeschrittene Operatoren (`$lookup`, `$unwind`, `$project`)  
✅ Performance-Optimierung mit Indexen  
✅ Best Practices für Aggregation-Abfragen

---

## **🔹 2. Was ist die Aggregation-Pipeline in MongoDB? (1:00 – 2:30)**

📌 **Definition:**  
Die **Aggregation-Pipeline** ist eine leistungsstarke Möglichkeit, Daten **direkt in MongoDB zu verarbeiten und zu analysieren** – ähnlich wie `GROUP BY` oder `JOINs` in SQL.

📌 **Warum Aggregation nutzen?**  
✅ **Schnelle Datenverarbeitung** direkt in der Datenbank.  
✅ **Reduziert den Overhead von JavaScript-Verarbeitung in Node.js.**  
✅ **Ermöglicht komplexe Berechnungen & Datenmanipulationen.**

🎯 **Fazit:** **Aggregation spart Zeit & macht deine MongoDB-Abfragen effizienter!**

---

## **🔹 3. Grundlegende Aggregation – `$match`, `$group`, `$sort` (2:30 – 5:30)**

📌 **Beispieldaten: `models/Order.js`**

```javascript
import mongoose from "mongoose";

const orderSchema = new mongoose.Schema({
  customer: String,
  totalAmount: Number,
  status: String,
  createdAt: { type: Date, default: Date.now },
});

export default mongoose.model("Order", orderSchema);
```

📌 **Beispiel 1: Umsatz nach Status gruppieren (`$group`)**

```javascript
import Order from "./models/Order.js";

const getRevenueByStatus = async () => {
  const result = await Order.aggregate([
    { $group: { _id: "$status", totalRevenue: { $sum: "$totalAmount" } } },
  ]);
  console.log("📊 Umsatz nach Status:", result);
};

getRevenueByStatus();
```

✅ **`$group` fasst Daten nach `status` zusammen**.  
✅ **`$sum` berechnet den Gesamtumsatz pro Status**.

📌 **Beispiel 2: Nur abgeschlossene Bestellungen (`$match`)**

```javascript
const getCompletedOrders = async () => {
  const result = await Order.aggregate([
    { $match: { status: "completed" } },
    { $sort: { createdAt: -1 } }, // Neueste zuerst
  ]);
  console.log("✅ Abgeschlossene Bestellungen:", result);
};

getCompletedOrders();
```

✅ **`$match` filtert Bestellungen mit `status: "completed"`**.  
✅ **`$sort: { createdAt: -1 }` sortiert nach Datum absteigend**.

🎯 **Ergebnis:** **MongoDB verarbeitet Filter & Gruppierungen direkt – schnell & effizient!**

---

## **🔹 4. Fortgeschrittene Aggregation – `$lookup`, `$unwind`, `$project` (5:30 – 9:30)**

📌 **Beispieldaten: `models/Customer.js`**

```javascript
const customerSchema = new mongoose.Schema({
  name: String,
  email: String,
});

export default mongoose.model("Customer", customerSchema);
```

📌 **Beispiel 3: Kunden-Details zu Bestellungen hinzufügen (`$lookup`)**

```javascript
const getOrdersWithCustomers = async () => {
  const result = await Order.aggregate([
    {
      $lookup: {
        from: "customers", // Name der MongoDB-Sammlung
        localField: "customer",
        foreignField: "name",
        as: "customerDetails",
      },
    },
  ]);
  console.log("📌 Bestellungen mit Kunden:", result);
};

getOrdersWithCustomers();
```

✅ **`$lookup` verbindet Bestellungen mit Kundendaten – ähnlich einem SQL `JOIN`**.

📌 **Beispiel 4: Bestellungen mit mehreren Produkten entpacken (`$unwind`)**

```javascript
const orderSchema = new mongoose.Schema({
  customer: String,
  products: [{ name: String, price: Number }],
});

const getOrdersWithProducts = async () => {
  const result = await Order.aggregate([
    { $unwind: "$products" }, // Trennt Array in einzelne Objekte
  ]);
  console.log("📦 Bestellungen mit einzelnen Produkten:", result);
};

getOrdersWithProducts();
```

✅ **`$unwind` macht aus Arrays einzelne Einträge – ideal für Detailanalysen.**

📌 **Beispiel 5: Bestellungen mit formatierten Daten ausgeben (`$project`)**

```javascript
const getFormattedOrders = async () => {
  const result = await Order.aggregate([
    {
      $project: {
        customer: 1,
        totalAmount: 1,
        formattedDate: {
          $dateToString: { format: "%Y-%m-%d", date: "$createdAt" },
        },
      },
    },
  ]);
  console.log("🗓️ Formatierte Bestellungen:", result);
};

getFormattedOrders();
```

✅ **`$project` reduziert & formatiert Felder für saubere API-Responses**.

🎯 **Ergebnis:** **Daten sind aufbereitet & können direkt für Reports oder Dashboards genutzt werden!**

---

## **🔹 5. Performance-Optimierung für Aggregation (9:30 – 11:30)**

📌 **🚀 Best Practices für schnelle Aggregation:**  
✅ **Immer `$match` am Anfang setzen** (Reduziert die Datenmenge frühzeitig).  
✅ **Indexe auf Feldern setzen, die in `$match` verwendet werden.**  
✅ **Nicht benötigte Felder mit `$project` entfernen – spart Speicherplatz.**  
✅ **`$unwind` nur bei Arrays nutzen, wenn wirklich nötig (kann viele Dokumente erzeugen).**

📌 **Index auf `status` für schnellere Aggregation setzen (`models/Order.js`)**

```javascript
orderSchema.index({ status: 1 });
```

📌 **Performance testen mit `explain()`**

```javascript
const result = await Order.aggregate([
  { $match: { status: "completed" } },
]).explain("executionStats");
console.log(result);
```

🎯 **Ergebnis:** **Optimierte Abfragen sparen enorm viel Zeit!**

---

## **🔹 6. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit **Aggregation in Mongoose** leistungsstarke Datenanalysen durchführst! Mit diesen Techniken kannst du Bestellungen auswerten, Kundenverhalten analysieren und komplexe Reports direkt aus MongoDB abrufen."_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Aggregation-Operatoren nutzt du am meisten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB Indexierung – So beschleunigst du deine Abfragen!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Aggregation mit Echtzeit-Statistiken oder Dashboards ergänzen?** 😊
