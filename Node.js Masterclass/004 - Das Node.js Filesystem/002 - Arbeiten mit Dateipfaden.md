### **📜 Videoskript: Arbeiten mit Dateipfaden in Node.js – Das `path`-Modul einfach erklärt!**

🎬 **Titel:** Dateipfade in Node.js – Das `path`-Modul richtig nutzen!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** Verständnis für **das `path`-Modul in Node.js**, um **Dateipfade plattformunabhängig** zu verwalten.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Arbeiten mit Dateipfaden kann knifflig sein – besonders wenn du zwischen Windows, Mac und Linux wechselst. Zum Glück gibt es das `path`-Modul in Node.js! Heute zeige ich dir, wie du damit sicher und flexibel Dateipfade verwalten kannst!"_

📌 **Agenda:**  
✅ Warum brauchen wir das `path`-Modul?  
✅ Grundlagen & wichtige Methoden  
✅ Absolute & relative Pfade  
✅ Plattformübergreifende Kompatibilität  
✅ Praktische Beispiele

---

## **🔹 2. Warum brauchen wir das `path`-Modul? (1:00 – 2:30)**

**🎥 Szene:** Sprecher erklärt Unterschiede zwischen Windows (`\`) und Linux/Mac (`/`) Pfaden.

🗣️ **Sprecher:**  
_"Dateipfade sind in Windows und Linux unterschiedlich. Windows nutzt `\`, während Linux & Mac `/` verwenden. Das kann Probleme verursachen, wenn du deinen Code auf mehreren Systemen ausführst!"_

📌 **Beispiel für plattformabhängige Pfade:**

```javascript
// Windows
const pfad = "C:\\Benutzer\\Dokumente\\datei.txt";

// Linux / Mac
const pfad = "/home/user/dokumente/datei.txt";
```

🎯 **Problem:**  
✅ Harte Kodierung von Pfaden führt zu Fehlern beim Wechsel zwischen Betriebssystemen.  
✅ Lösung: Das `path`-Modul, das immer den richtigen Trennstrich (`/` oder `\`) setzt.

---

## **🔹 3. Das `path`-Modul importieren & erste Schritte (2:30 – 4:00)**

**🎥 Szene:** Sprecher zeigt, wie das `path`-Modul in Node.js importiert wird.

📌 **Modul importieren:**

```javascript
import path from "path";
```

🎯 **Fazit:**  
✅ `path` ist ein **integriertes Node.js-Modul** – keine zusätzliche Installation nötig!  
✅ Erlaubt es, **plattformsichere Dateipfade zu erstellen**.

📌 **Beispiel: Basisfunktionen mit `path`**

```javascript
console.log("Trennzeichen:", path.sep);
console.log("Aktuelles Verzeichnis:", import.meta.dirname);
console.log("Aktuelle Datei:", import.meta.filename);
```

🎯 **Nutzen:**  
✅ `path.sep` gibt das richtige Trennzeichen für das aktuelle System zurück (`/` oder `\`).  
✅ `import.meta.dirname` und `import.meta.filename` sind praktische Variablen, um Dateipfade zu bestimmen.

---

## **🔹 4. Wichtige Methoden des `path`-Moduls (4:00 – 7:00)**

**🎥 Szene:** Sprecher zeigt Beispiele für `path.join()`, `path.resolve()`, `path.basename()` usw.

📌 **1️⃣ `path.join()` – Pfade sicher zusammenfügen**

```javascript
const pfad = path.join(import.meta.dirname, "ordner", "datei.txt");
console.log("Zusammengesetzter Pfad:", pfad);
```

🎯 **Nutzen:**  
✅ Setzt automatisch das richtige **Trennzeichen** für das Betriebssystem.

📌 **2️⃣ `path.resolve()` – Absoluten Pfad generieren**

```javascript
const absoluterPfad = path.resolve("ordner", "datei.txt");
console.log("Absoluter Pfad:", absoluterPfad);
```

🎯 **Nutzen:**  
✅ Wandelt einen **relativen Pfad** in einen **absoluten Pfad** um.  
✅ Startet vom aktuellen Arbeitsverzeichnis (`process.cwd()`).

📌 **3️⃣ `path.basename()` – Dateiname aus Pfad extrahieren**

```javascript
const datei = path.basename("/home/user/dokumente/text.txt");
console.log("Dateiname:", datei);
```

🎯 **Nutzen:** Gibt nur den **Dateinamen** zurück (`text.txt`), nicht den gesamten Pfad.

📌 **4️⃣ `path.extname()` – Dateiendung extrahieren**

```javascript
const endung = path.extname("bild.jpg");
console.log("Dateiendung:", endung); // .jpg
```

🎯 **Nutzen:** Ideal für **MIME-Typen oder Dateityp-Überprüfung**.

📌 **5️⃣ `path.dirname()` – Ordnerpfad ohne Dateinamen**

```javascript
const ordner = path.dirname("/home/user/dokumente/text.txt");
console.log("Ordnerpfad:", ordner);
```

🎯 **Nutzen:** Gibt nur das **übergeordnete Verzeichnis** zurück.

---

## **🔹 5. Praktische Anwendungen (7:00 – 10:00)**

📌 **Beispiel: Dynamischer Pfad für Dateioperationen**

```javascript
import fs from "fs";
const dateipfad = path.join(import.meta.dirname, "daten", "test.txt");
fs.writeFileSync(dateipfad, "Hallo Welt!");
console.log("Datei gespeichert in:", dateipfad);
```

🎯 **Nutzen:** **Plattformunabhängige Dateispeicherung** ohne fest kodierte Pfade.

---

## **🔹 6. Fazit & Outro (11:30 – 12:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit dem `path`-Modul plattformunabhängige und sichere Dateipfade erstellst! Nutze es, um mit Node.js auf Dateien zuzugreifen, CLI-Tools zu bauen oder dynamische Dateisysteme zu verwalten."_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie nutzt du das `path`-Modul in deinen Projekten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"File-Handling in Node.js – Das `fs`-Modul im Detail!"_

---

🎯 **Fertig!** 🎯
