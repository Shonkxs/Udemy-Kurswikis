### **📜 Videoskript: Statische Dateien in Express bereitstellen – CSS, Bilder & mehr!**

🎬 **Titel:** Statische Dateien in Express bereitstellen – So hostest du CSS, Bilder & JavaScript!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Webentwickler  
🎯 **Lernziel:** Verstehen, **wie man in Express statische Dateien bereitstellt**, um **statische Webseiten oder APIs mit Bildern, CSS und JavaScript-Dateien** zu unterstützen.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Willst du eine Website mit Express hosten oder statische Dateien wie Bilder, CSS oder JavaScript bereitstellen? In diesem Video zeige ich dir, wie du in Express **statische Inhalte für deine Webanwendung** verfügbar machst!"_

📌 **Agenda:**  
✅ Was sind statische Dateien?  
✅ Express für statische Dateien konfigurieren  
✅ Mehrere statische Ordner nutzen  
✅ Sicherheit & Best Practices

---

## **🔹 2. Was sind statische Dateien? (1:00 – 2:30)**

📌 **Definition:**  
_"Statische Dateien sind **unveränderte Inhalte**, die der Server direkt an den Client sendet – z. B. HTML, CSS, JavaScript, Bilder oder Videos."_

📌 **Beispiele für statische Dateien:**

- **CSS-Dateien** → Layouts & Styles
- **JavaScript-Dateien** → Interaktive Funktionen
- **Bilder (PNG, JPG, SVG)** → Logos & Icons
- **PDFs & Videos** → Downloads & Medieninhalte

🎯 **Fazit:** **Express kann solche Dateien direkt bereitstellen – ganz ohne eine API!**

---

## **🔹 3. Statische Dateien mit `express.static()` bereitstellen (2:30 – 5:00)**

📌 **Schritt 1: Ordnerstruktur anlegen**

```plaintext
/my-express-app
 ├── public
 │   ├── index.html
 │   ├── style.css
 │   ├── script.js
 │   ├── images/
 │   │   ├── logo.png
 ├── server.js
 ├── package.json
```

📌 **Schritt 2: Express-Server mit statischen Dateien einrichten**

```javascript
import express from "express";

const app = express();
const port = 3000;

// Statischen Ordner bereitstellen
app.use(express.static("public"));

app.listen(port, () => {
  console.log(`Server läuft auf http://localhost:${port}`);
});
```

📌 **Schritt 3: HTML-Seite mit statischen Dateien testen**  
➡ **Öffne `http://localhost:3000/index.html`**  
➡ **CSS & Bilder werden automatisch geladen!**

🎯 **Erklärung:**  
✅ **`express.static('public')`** → Macht alle Dateien in `public/` zugänglich.  
✅ **Kein zusätzliches Routing nötig – Express erledigt alles automatisch!**

---

## **🔹 4. Mehrere statische Ordner verwalten (5:00 – 7:30)**

📌 **Was tun, wenn deine statischen Dateien in verschiedenen Ordnern liegen?**

```javascript
app.use(express.static("public"));
app.use(express.static("assets"));
```

🎯 **Nutzen:**  
✅ **Dateien aus `public/` & `assets/` sind beide erreichbar.**  
✅ **Ideal, wenn man Frontend & Backend trennt.**

📌 **Testen:**  
➡ **`http://localhost:3000/style.css` (aus `public/`)**  
➡ **`http://localhost:3000/images/logo.png` (aus `assets/`)**

---

## **🔹 5. Einen eigenen statischen URL-Pfad definieren (7:30 – 9:00)**

📌 **Problem:** Standardmäßig sind Dateien direkt unter `/` abrufbar.  
📌 **Lösung:** Ein eigenes **URL-Präfix** für statische Inhalte festlegen.

```javascript
app.use("/static", express.static("public"));
```

🎯 **Jetzt werden die Dateien unter `/static/` bereitgestellt:**  
➡ **`http://localhost:3000/static/index.html`**  
➡ **`http://localhost:3000/static/images/logo.png`**

📌 **Nutzen:**  
✅ **Besser für Sicherheit & Struktur**.  
✅ **Vermeidet Konflikte mit API-Routen (z. B. `/api` vs. `/static`).**

---

## **🔹 6. Sicherheit & Best Practices (9:00 – 11:30)**

📌 **🚀 Best Practices für statische Dateien in Express:**  
✅ **Nutze `express.static()` für unveränderte Inhalte**.  
✅ **Trenne API-Routen von statischen Dateien (`/api/*` vs. `/static/*`).**  
✅ **Verwende einen separaten Ordner (`public/`) statt Root-Verzeichnis.**  
✅ **Verhindere den Zugriff auf versteckte Dateien (.env, .git, etc.).**

📌 **⚠️ Sicherheitsproblem: Verhindere Zugriff auf versteckte Dateien**

```javascript
app.use(
  "/public",
  express.static("public", {
    dotfiles: "ignore", // Verhindert Zugriff auf .env, .git usw.
  })
);
```

🎯 **Nutzen:**  
✅ **Schützt sensible Dateien vor unbefugtem Zugriff.**

---

## **🔹 7. Fazit & Outro (11:30 – 12:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit Express statische Dateien bereitstellst – von HTML, CSS und JavaScript bis hin zu Bildern und Videos! Damit kannst du komplette Webseiten hosten oder deine APIs mit statischen Inhalten erweitern."_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche statischen Dateien hostest du in deinen Express-Apps? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Middleware in Express – Logging, Authentifizierung & Fehlerhandling!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel mit `etag` oder Caching für bessere Performance ergänzen?** 😊
