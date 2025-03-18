### **📜 Videoskript: ZIP-Dateien in Node.js erstellen & entpacken**

🎬 **Titel:** ZIP-Dateien in Node.js – Dateien & Ordner komprimieren & entpacken!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Node.js-Entwickler  
🎯 **Lernziel:** Verstehen, wie man **ZIP-Dateien mit Node.js erstellt und entpackt** – ideal für **Backups, Datei-Uploads und Archivierung**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Musst du oft Dateien archivieren oder große Datenmengen als ZIP verschicken? In diesem Video zeige ich dir, wie du ZIP-Dateien direkt in Node.js **erstellt und entpackst** – ganz ohne externe Programme!"_

📌 **Agenda:**  
✅ Warum ZIP-Dateien in Node.js nutzen?  
✅ Dateien & Ordner komprimieren  
✅ ZIP-Archive entpacken  
✅ Alternative Methoden & Best Practices

---

## **🔹 2. Warum ZIP-Dateien in Node.js nutzen? (1:00 – 2:30)**

**🎥 Szene:** Sprecher erklärt, warum ZIP-Archive nützlich sind.

🗣️ **Sprecher:**  
_"ZIP-Dateien helfen, Speicherplatz zu sparen und mehrere Dateien einfach zu transportieren. Viele Webanwendungen und Cloud-Dienste nutzen ZIP-Formate für Datei-Uploads, Backups oder Datenexporte."_

📌 **Anwendungsfälle:**  
✅ **Backups automatisieren**  
✅ **Datei-Uploads optimieren**  
✅ **Mehrere Dateien einfach versenden**

🎯 **Fazit:** **ZIP-Dateien sparen Speicher und erleichtern den Datenaustausch!**

---

## **🔹 3. ZIP-Dateien mit `archiver` erstellen (2:30 – 5:30)**

📌 **Schritt 1: Modul installieren**

```bash
npm install archiver
```

📌 **Schritt 2: Einzelne Datei komprimieren**

```javascript
import fs from "fs";
import archiver from "archiver";

const output = fs.createWriteStream("example.zip");
const archive = archiver("zip", { zlib: { level: 9 } });

output.on("close", () => {
  console.log(`ZIP-Datei erstellt: ${archive.pointer()} Bytes`);
});

archive.pipe(output);
archive.file("test.txt", { name: "test.txt" });
archive.finalize();
```

🎯 **Erklärung:**  
✅ `archiver` erstellt ein ZIP-Archiv mit maximaler Komprimierung (`level: 9`).  
✅ `.file()` fügt eine Datei hinzu.  
✅ `.finalize()` beendet den ZIP-Prozess.

📌 **Mehrere Dateien & Ordner komprimieren**

```javascript
archive.directory("meinOrdner/", false);
archive.finalize();
```

🎯 **Nutzen:**

- **Perfekt für Backups & Webanwendungen**.
- **Einfaches Archivieren von ganzen Projekten**.

---

## **🔹 4. ZIP-Dateien mit `unzipper` entpacken (5:30 – 8:00)**

📌 **Schritt 1: Modul installieren**

```bash
npm install unzipper
```

📌 **Schritt 2: ZIP-Archiv entpacken**

```javascript
import fs from "fs";
import unzipper from "unzipper";

fs.createReadStream("example.zip")
  .pipe(unzipper.Extract({ path: "entpackt" }))
  .on("close", () => console.log("ZIP-Datei erfolgreich entpackt!"));
```

🎯 **Erklärung:**  
✅ `fs.createReadStream()` liest das ZIP-Archiv.  
✅ `unzipper.Extract()` extrahiert es in den Ordner `entpackt`.

📌 **Einzelne Dateien aus ZIP extrahieren**

```javascript
fs.createReadStream("example.zip")
  .pipe(unzipper.Parse())
  .on("entry", (entry) => {
    console.log(`Datei gefunden: ${entry.path}`);
    entry.autodrain(); // Falls nicht extrahiert wird, Speicherverbrauch vermeiden
  });
```

🎯 **Nutzen:**

- **Prüfen, welche Dateien in einem ZIP-Archiv sind, bevor man sie extrahiert**.

---

## **🔹 5. Alternative Methoden für ZIP-Dateien (8:00 – 10:30)**

📌 **Methode 1: ZIP mit `child_process` (Linux/macOS)**

```javascript
import { exec } from "child_process";

exec("zip -r archive.zip meinOrdner", (err, stdout, stderr) => {
  if (err) console.error("Fehler:", stderr);
  else console.log("ZIP erfolgreich erstellt!");
});
```

🎯 **Nutzen:**  
✅ **Nutzt native ZIP-Tools des Betriebssystems**.  
✅ **Schnell & effizient für Linux/macOS**.

📌 **Methode 2: Entpacken mit `child_process`**

```javascript
exec("unzip archive.zip -d entpackt", (err, stdout, stderr) => {
  if (err) console.error("Fehler:", stderr);
  else console.log("ZIP erfolgreich entpackt!");
});
```

🎯 **Nutzen:**  
✅ **Nützlich für Shell-Skripte & Deployment-Prozesse**.

---

## **🔹 6. Fehlerbehandlung & Best Practices (10:30 – 12:00)**

📌 **Fehlermanagement für ZIP-Operationen**

```javascript
archive.on("error", (err) => {
  console.error("Fehler beim Erstellen der ZIP-Datei:", err);
});
```

```javascript
fs.createReadStream("example.zip").on("error", (err) =>
  console.error("Fehler beim Entpacken:", err)
);
```

🎯 **Fazit:**  
✅ **Immer `.on('error')` verwenden**, um Abstürze zu vermeiden.  
✅ **Prüfen, ob eine Datei existiert**, bevor man sie packt oder entpackt.

📌 **🚀 Best Practices für ZIP in Node.js**  
✅ **Nutze `archiver` für komplexe ZIP-Archive.**  
✅ **Verwende `unzipper` für schnelles Extrahieren.**  
✅ **`child_process` ist perfekt für schnelle Shell-Skripte.**

---

## **🔹 7. Fazit & Outro (12:00 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du ZIP-Dateien in Node.js **erstellen & entpacken** kannst! Egal ob Backups, Datei-Uploads oder Archivierung – mit `archiver` und `unzipper` hast du alles unter Kontrolle!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Für welche Projekte würdest du ZIP-Dateien nutzen? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Node.js File-System – Arbeiten mit `fs` und `path`!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für verschlüsselte ZIP-Archive ergänzen?** 😊
