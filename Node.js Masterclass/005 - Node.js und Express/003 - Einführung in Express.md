### **📜 Videoskript: Einführung in Express – Dein erster Webserver mit Node.js**

🎬 **Titel:** Einführung in Express – So baust du deinen ersten Webserver mit Node.js!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **was Express ist**, warum es genutzt wird und wie man **einen einfachen Webserver erstellt**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Willst du einen Webserver mit Node.js aufsetzen, ohne dich mit kompliziertem Code herumzuschlagen? Dann brauchst du **Express**! In diesem Video zeige ich dir, wie du mit Express **einen schnellen Webserver** aufbaust, API-Routen erstellst und Middleware nutzt!"_

📌 **Agenda:**  
✅ Installation & erste Schritte  
✅ Ein einfacher Webserver mit Express  
✅ Routing & Middleware

## **🔹 3. Express installieren & einrichten (2:30 – 5:00)**

📌 **Schritt 1: Express installieren**

```bash
npm init -y  # Erstellt eine package.json
npm install express  # Installiert Express
```

📌 **Schritt 2: Einfachen Webserver erstellen**

```javascript
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Willkommen auf meinem Express-Server!");
});

app.listen(port, () => {
  console.log(`Server läuft auf http://localhost:${port}`);
});
```

🎯 **Erklärung:**  
✅ `express()` erstellt eine neue Express-App.  
✅ `.get()` definiert eine Route für Anfragen an `/`.  
✅ `.listen()` startet den Server auf Port **3000**.

📌 **Server starten**

```bash
node server.js
```

➡ **Öffne http://localhost:3000 in deinem Browser!**

🎯 **Fazit:** **Express erlaubt es dir, in wenigen Zeilen einen Server zu erstellen!**

---

## **🔹 7. Fazit & Outro (11:00 – 12:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, was Express ist, warum es so beliebt ist und wie du in wenigen Minuten einen eigenen Webserver aufsetzt! Jetzt bist du bereit, eigene APIs & Webanwendungen mit Express zu bauen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon mit Express gearbeitet? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Routing & Middleware in Express – So baust du saubere APIs!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für eine Express-API mit Datenbank ergänzen?** 😊
