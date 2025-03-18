### **📜 Videoskript: Die HTTP-Methoden – So kommunizieren Webserver & Clients**

🎬 **Titel:** HTTP-Methoden erklärt – GET, POST, PUT, DELETE & mehr!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **was HTTP-Methoden sind**, wie sie funktionieren und wann man welche Methode benutzt.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wie kommuniziert dein Browser mit einem Server? Wie sendet eine API Daten? Die Antwort: **HTTP-Methoden**! Heute zeige ich dir, welche Methoden es gibt, wie sie funktionieren und wann du welche nutzen solltest!"_

📌 **Agenda:**  
✅ Was sind HTTP-Methoden?  
✅ Die wichtigsten Methoden (GET, POST, PUT, DELETE, PATCH)  
✅ Praktische Beispiele mit Express  
✅ Best Practices für APIs

---

## **🔹 2. Was sind HTTP-Methoden? (1:00 – 2:30)**

📌 **Definition:**  
_"HTTP-Methoden sind Befehle, die bestimmen, **wie ein Client mit einem Server kommuniziert**."_

📌 **Die 5 wichtigsten Methoden:**  
| Methode | Zweck | Beispiel |
|----------|------------|--------------|
| **GET** | Daten abrufen | `GET /users` |
| **POST** | Neue Daten senden | `POST /users` |
| **PUT** | Bestehende Daten ersetzen | `PUT /users/1` |
| **DELETE** | Daten löschen | `DELETE /users/1` |
| **PATCH** | Teilweise Daten ändern | `PATCH /users/1` |

🎯 **Fazit:** **Jede Methode hat ihren Zweck in einer REST-API!**

---

## **🔹 3. GET – Daten abrufen (2:30 – 4:00)**

📌 **Beispiel mit Express:**

```javascript
import express from "express";
const app = express();

app.get("/users", (req, res) => {
  res.json([
    { id: 1, name: "Markus" },
    { id: 2, name: "Sophie" },
  ]);
});

app.listen(3000, () => console.log("Server läuft auf Port 3000"));
```

🎯 **Erklärung:**  
✅ **GET wird verwendet, um Daten zu lesen.**  
✅ **Daten werden in JSON zurückgegeben.**

📌 **Testen:**  
➡ **Öffne http://localhost:3000/users in deinem Browser!**

---

## **🔹 4. POST – Neue Daten senden (4:00 – 5:30)**

📌 **POST-Route in Express:**

```javascript
app.use(express.json()); // Middleware, um JSON-Daten zu parsen

app.post("/users", (req, res) => {
  const newUser = req.body;
  console.log("Neuer Benutzer:", newUser);
  res.status(201).json(newUser);
});
```

🎯 **Erklärung:**  
✅ **POST wird für das Erstellen neuer Ressourcen verwendet.**  
✅ **`req.body` enthält die gesendeten Daten.**  
✅ **Status 201 bedeutet „Erfolgreich erstellt“.**

📌 **Testen mit Postman oder cURL:**

```bash
curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"name":"Lisa"}'
```

---

## **🔹 5. PUT – Daten komplett ersetzen (5:30 – 7:00)**

📌 **PUT-Route für das Aktualisieren von Nutzerdaten:**

```javascript
app.put("/users/:id", (req, res) => {
  const userId = req.params.id;
  const updatedUser = req.body;
  console.log(`Benutzer ${userId} wurde ersetzt:`, updatedUser);
  res.json(updatedUser);
});
```

🎯 **Erklärung:**  
✅ **PUT ersetzt eine Ressource komplett** – Alte Daten werden überschrieben.  
✅ **Wird meist für Updates verwendet, wenn alle Felder bekannt sind.**

📌 **PUT vs. PATCH:**

- **PUT ersetzt alle Daten einer Ressource.**
- **PATCH ändert nur bestimmte Felder.**

---

## **🔹 6. DELETE – Daten entfernen (7:00 – 8:30)**

📌 **DELETE-Route für das Löschen eines Nutzers:**

```javascript
app.delete("/users/:id", (req, res) => {
  const userId = req.params.id;
  console.log(`Benutzer ${userId} wurde gelöscht.`);
  res.status(204).send(); // Kein Inhalt zurückgeben
});
```

🎯 **Erklärung:**  
✅ **DELETE entfernt eine Ressource**.  
✅ **Status 204 bedeutet „Erfolgreich gelöscht, aber keine Daten zurückgeben“**.

📌 **Testen mit cURL:**

```bash
curl -X DELETE http://localhost:3000/users/1
```

---

## **🔹 7. PATCH – Teilweise Aktualisierung (8:30 – 10:00)**

📌 **PATCH-Route für das Aktualisieren eines Namens:**

```javascript
app.patch("/users/:id", (req, res) => {
  const userId = req.params.id;
  const updatedFields = req.body;
  console.log(`Benutzer ${userId} wurde aktualisiert:`, updatedFields);
  res.json(updatedFields);
});
```

🎯 **Erklärung:**  
✅ **PATCH wird verwendet, um nur einzelne Felder zu ändern.**  
✅ **Ideal für kleine Updates (z. B. nur den Namen ändern).**

📌 **PUT vs. PATCH in der Praxis:**

- **PUT-Anfrage:**
  ```json
  { "name": "Max", "email": "max@example.com" }
  ```
- **PATCH-Anfrage:**
  ```json
  { "name": "Max" }
  ```

🎯 **Best Practice:** **PATCH nutzen, wenn nur kleine Änderungen nötig sind!**

---

## **🔹 8. Best Practices für HTTP-Methoden (10:00 – 11:30)**

📌 **🚀 Best Practices für APIs:**  
✅ **GET sollte niemals Daten verändern!**  
✅ **Nutze POST für neue Ressourcen, PUT für komplette Updates, PATCH für Teilupdates.**  
✅ **Antwortstatus verwenden (`201` für POST, `204` für DELETE).**

📌 **Häufige Fehler vermeiden:**  
❌ **DELETE sollte keine Daten zurückgeben (nur Status 204).**  
❌ **GET sollte keine Daten schreiben (Sicherheit & Caching!).**  
❌ **PUT sollte vollständige Daten enthalten, PATCH nur geänderte Felder.**

---

## **🔹 9. Fazit & Outro (11:30 – 12:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie HTTP-Methoden funktionieren und wie du sie in einer REST-API anwendest! Mit GET, POST, PUT, DELETE & PATCH kannst du Daten lesen, schreiben, aktualisieren und löschen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche HTTP-Methoden nutzt du am häufigsten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"REST-APIs mit Express – Best Practices für saubere API-Entwicklung!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel mit Authentifizierung & HTTP-Headern ergänzen?** 😊
