### **📜 Videoskript: Arbeiten mit Readable & Writable Streams in Node.js**

🎬 **Titel:** Streams in Node.js – Große Datenmengen effizient verarbeiten!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Node.js-Entwickler  
🎯 **Lernziel:** Verstehen, wie **Readable & Writable Streams** in Node.js funktionieren und wie man sie für **Dateiverarbeitung, APIs und Echtzeit-Datenströme** nutzt.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Hast du schon mal mit riesigen Dateien gearbeitet, die dein Speicher zum Absturz bringen? Oder Daten von einer API empfangen, die nicht auf einmal geliefert werden? Genau hier helfen **Streams in Node.js**! Heute zeige ich dir, wie du mit **Readable & Writable Streams** effizient große Datenmengen verarbeiten kannst!"_

📌 **Agenda:**  
✅ Was sind Streams?  
✅ Readable Streams (Lesen von Daten)  
✅ Writable Streams (Schreiben von Daten)  
✅ Piping & Stream-Kombinationen  
✅ Fehlerbehandlung & Best Practices

---

## **🔹 2. Was sind Streams? (1:00 – 2:30)**

**🎥 Szene:** Sprecher erklärt das Konzept von Streams mit einer Animation von fließendem Wasser.

🗣️ **Sprecher:**  
_"Stell dir einen Stream wie ein Wasserrohr vor. Statt Daten auf einmal zu laden, fließen sie Stück für Stück. Das macht Streams extrem effizient!"_

📌 **Warum Streams nutzen?**  
✅ Verarbeiten von **großen Dateien ohne Speicherüberlastung**  
✅ **Echtzeit-Datenverarbeitung** (z. B. Live-Video, Websockets)  
✅ **Daten von einer API empfangen & direkt weiterverarbeiten**

📌 **Arten von Streams in Node.js:**  
| Stream-Typ | Beschreibung | Beispiel |
|------------|-------------|-----------|
| **Readable** | Daten lesen | Datei einlesen |
| **Writable** | Daten schreiben | Datei speichern |
| **Duplex** | Lesen & Schreiben | WebSocket |
| **Transform** | Daten während des Streams verändern | ZIP-Dateien, Komprimierung |

🎯 **Fazit:** **Streams sparen Speicher und verbessern Performance!**

---

## **🔹 3. Readable Streams – Daten lesen (2:30 – 5:30)**

📌 **Beispiel: Große Datei in Chunks lesen**

```javascript
import fs from "fs";

const readStream = fs.createReadStream("großeDatei.txt", { encoding: "utf8" });

readStream.on("data", (chunk) => {
  console.log("Chunk erhalten:", chunk);
});

readStream.on("end", () => {
  console.log("Lesen abgeschlossen!");
});

readStream.on("error", (err) => {
  console.error("Fehler beim Lesen:", err);
});
```

🎯 **Erklärung:**  
✅ `fs.createReadStream()` liest eine Datei **Stück für Stück** (Chunks).  
✅ `on('data', chunk)` gibt jedes gelesene Stück aus.  
✅ `on('end')` zeigt an, wenn das Lesen abgeschlossen ist.

📌 **Wann ist das nützlich?**

- **Große Dateien verarbeiten**, ohne alles auf einmal zu laden.
- **Streaming von Videos oder Musik**.
- **Echtzeit-Daten von APIs** empfangen.

---

## **🔹 4. Writable Streams – Daten schreiben (5:30 – 7:30)**

📌 **Beispiel: Große Datei schreiben**

```javascript
const writeStream = fs.createWriteStream("output.txt");

writeStream.write("Hallo, dies ist ein Stream!\n");
writeStream.write("Noch eine Zeile!\n");

writeStream.end(() => {
  console.log("Datei wurde erfolgreich geschrieben!");
});

writeStream.on("error", (err) => {
  console.error("Fehler beim Schreiben:", err);
});
```

🎯 **Erklärung:**  
✅ `fs.createWriteStream()` schreibt Daten **stückweise** in eine Datei.  
✅ `.write()` sendet einzelne Teile an die Datei.  
✅ `.end()` signalisiert, dass keine weiteren Daten kommen.

📌 **Typische Anwendungsfälle:**

- **Große Log-Dateien schreiben**.
- **Streaming-Uploads in Cloud-Dienste**.
- **Daten aus einer API in eine Datei speichern**.

---

## **🔹 5. Streams kombinieren mit `pipe()` (7:30 – 9:30)**

📌 **Datei lesen & direkt in eine neue Datei schreiben**

```javascript
const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);

writeStream.on("finish", () => {
  console.log("Datei wurde erfolgreich kopiert!");
});
```

🎯 **Erklärung:**  
✅ **`pipe()` verbindet Streams**, sodass die Daten direkt durchgeleitet werden.  
✅ Kein **manuelles Schreiben** mehr nötig!

📌 **Weitere Anwendungen:**

- **ZIP- und Komprimierungsstreams**
- **Live-Video- oder Audio-Streaming**

---

## **🔹 6. Transform Streams – Daten während des Streams verändern (9:30 – 11:00)**

📌 **Beispiel: Daten in Großbuchstaben umwandeln**

```javascript
import { Transform } from "stream";

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  },
});

fs.createReadStream("input.txt")
  .pipe(upperCaseTransform)
  .pipe(fs.createWriteStream("output_uppercase.txt"));
```

🎯 **Erklärung:**  
✅ **Transform Streams** können Daten im Stream verändern.  
✅ Perfekt für **Formatierung, Komprimierung & Datenvalidierung**.

---

## **🔹 7. Fehlerbehandlung & Best Practices (11:00 – 12:30)**

📌 **Fehlermanagement für Streams**  
Streams arbeiten asynchron und können Fehler verursachen. Um Abstürze zu vermeiden, **müssen alle Fehler behandelt werden**!

✅ **Fehlermanagement für `Readable` und `Writable` Streams:**

```javascript
const readStream = fs.createReadStream("nicht_existierende_datei.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.on("error", (err) => {
  console.error("Fehler beim Lesen:", err);
});

writeStream.on("error", (err) => {
  console.error("Fehler beim Schreiben:", err);
});
```

🎯 **Fazit:**

- **Immer `.on('error')` verwenden**, um Fehler in Streams abzufangen.
- **Fehlende Dateien prüfen**, bevor sie gelesen werden.
- **Genügend Schreibrechte sicherstellen**, bevor Daten gespeichert werden.

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit `Readable` und `Writable` Streams große Datenmengen effizient verarbeiten kannst! Nutze Streams für Dateiverarbeitung, API-Daten oder Echtzeit-Anwendungen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wo nutzt du Streams in deinen Projekten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Node.js & File-System – Arbeiten mit `fs` und `path`!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für Netzwerk-Streaming oder WebSockets ergänzen?** 😊
