### **📜 Videoskript: Einführung in das `fs`-Modul in Node.js**

🎬 **Titel:** Das `fs`-Modul in Node.js – Dateien lesen, schreiben & löschen einfach erklärt!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** Verstehen, wie das `fs`-Modul in Node.js genutzt wird, um Dateien und Verzeichnisse zu verwalten.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wie kannst du in Node.js Dateien lesen, schreiben und löschen? Das geht mit dem `fs`-Modul! Heute zeige ich dir, wie du mit dem **Dateisystem in Node.js** arbeitest – mit vielen praktischen Beispielen!"_

📌 **Agenda:**  
✅ Was ist das `fs`-Modul?  
✅ Dateien lesen & schreiben  
✅ Dateien und Ordner verwalten  
✅ Synchron vs. Asynchron  
✅ Best Practices & Fehlerhandling

---

## **🔹 2. Was ist das `fs`-Modul? (1:00 – 2:30)**

**🎥 Szene:** Sprecher erklärt, dass `fs` für "File System" steht und standardmäßig in Node.js enthalten ist.

🗣️ **Sprecher:**  
_"Das `fs`-Modul ist ein Kernmodul in Node.js, das dir erlaubt, mit dem Dateisystem zu arbeiten. Es ermöglicht dir, Dateien zu **lesen, schreiben, aktualisieren, löschen und zu manipulieren** – und das sowohl synchron als auch asynchron!"_

📌 **Modul importieren:**

```javascript
const fs = require("fs");
```

🎯 **Fazit:**  
✅ `fs` ist ein **integriertes Node.js-Modul** – keine zusätzliche Installation nötig!

---

## **🔹 3. Dateien lesen mit `fs.readFile` (2:30 – 4:30)**

**🎥 Szene:** Sprecher zeigt eine **asynchrone** Datei-Lesung mit `fs.readFile`.

📌 **Beispiel: Eine Datei asynchron lesen**

```javascript
fs.readFile("test.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Fehler beim Lesen:", err);
    return;
  }
  console.log("Dateiinhalt:", data);
});
```

🎯 **Fazit:**  
✅ `readFile()` liest die Datei **asynchron**, ohne den Haupt-Thread zu blockieren.  
✅ `utf8` sorgt dafür, dass der Inhalt als lesbarer Text ausgegeben wird.  
✅ Falls ein Fehler auftritt, wird er in `err` gespeichert.

📌 **Synchrone Alternative (`fs.readFileSync`)**

```javascript
const data = fs.readFileSync("test.txt", "utf8");
console.log("Dateiinhalt:", data);
```

🎯 **Unterschied:**  
✅ `fs.readFileSync()` blockiert den Code, bis die Datei geladen ist.  
✅ Sollte nur verwendet werden, wenn **keine Verzögerung** im Programmfluss entsteht.

---

## **🔹 4. Dateien schreiben mit `fs.writeFile` (4:30 – 6:30)**

**🎥 Szene:** Sprecher zeigt, wie man eine Datei erstellt und beschreibt.

📌 **Beispiel: Eine Datei asynchron schreiben**

```javascript
fs.writeFile("output.txt", "Hallo, das ist ein neuer Inhalt!", (err) => {
  if (err) {
    console.error("Fehler beim Schreiben:", err);
    return;
  }
  console.log("Datei wurde erfolgreich erstellt!");
});
```

🎯 **Fazit:**  
✅ `fs.writeFile()` erstellt eine **neue Datei oder überschreibt** eine bestehende.

📌 **Synchrone Alternative (`fs.writeFileSync`)**

```javascript
fs.writeFileSync("output.txt", "Hallo, das ist ein neuer Inhalt!");
console.log("Datei wurde erfolgreich erstellt!");
```

🎯 **Unterschied:**  
✅ `fs.writeFileSync()` blockiert den Code, bis die Datei gespeichert ist.

---

## **🔹 5. Dateien anhängen mit `fs.appendFile` (6:30 – 7:30)**

📌 **Neuen Text zu einer Datei hinzufügen, ohne sie zu überschreiben**

```javascript
fs.appendFile("output.txt", "\nNeuer Inhalt hinzugefügt!", (err) => {
  if (err) {
    console.error("Fehler beim Anhängen:", err);
    return;
  }
  console.log("Text erfolgreich angehängt!");
});
```

🎯 **Nutzen:** Perfekt für **Log-Dateien oder fortlaufende Speicherungen**.

---

## **🔹 6. Dateien löschen mit `fs.unlink` (7:30 – 8:30)**

📌 **Beispiel: Datei löschen**

```javascript
fs.unlink("output.txt", (err) => {
  if (err) {
    console.error("Fehler beim Löschen:", err);
    return;
  }
  console.log("Datei erfolgreich gelöscht!");
});
```

🎯 **Achtung:** Vor dem Löschen sicherstellen, dass die Datei nicht mehr benötigt wird!

---

## **🔹 7. Verzeichnisse erstellen & löschen (8:30 – 10:00)**

📌 **Neues Verzeichnis erstellen:**

```javascript
fs.mkdir("neuerOrdner", { recursive: true }, (err) => {
  if (err) {
    console.error("Fehler beim Erstellen:", err);
    return;
  }
  console.log("Ordner erfolgreich erstellt!");
});
```

📌 **Verzeichnis löschen:**

```javascript
fs.rmdir("neuerOrdner", (err) => {
  if (err) {
    console.error("Fehler beim Löschen:", err);
    return;
  }
  console.log("Ordner erfolgreich gelöscht!");
});
```

🎯 **Nutzen:** Automatische Verzeichnisverwaltung für **Backups, Logs oder File-Uploads**.

---

## **🔹 8. Best Practices & Fehlerhandling (10:00 – 12:00)**

📌 **🚀 Best Practices für das `fs`-Modul**  
✅ **Immer `utf8` angeben**, um lesbaren Text zu erhalten.  
✅ **Fehlermanagement mit `if (err) {}`**, um Abstürze zu verhindern.  
✅ **Asynchron bevorzugen**, um den Haupt-Thread nicht zu blockieren.  
✅ **`fs.existsSync()` nutzen, um vor dem Zugriff zu prüfen, ob eine Datei existiert.**

📌 **Beispiel für sicheres Lesen & Schreiben:**

```javascript
const dateipfad = "test.txt";

if (fs.existsSync(dateipfad)) {
  fs.readFile(dateipfad, "utf8", (err, data) => {
    if (err) {
      console.error("Fehler beim Lesen:", err);
      return;
    }
    console.log("Dateiinhalt:", data);
  });
} else {
  console.log("Datei existiert nicht!");
}
```

🎯 **Fazit:** Immer prüfen, ob eine Datei existiert, um Fehler zu vermeiden!

---

## **🔹 9. Fazit & Outro (12:00 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du mit dem `fs`-Modul Dateien und Verzeichnisse verwaltest! Nutze diese Techniken für Log-Dateien, Konfigurationsdateien oder das Speichern von Nutzerdaten. Falls du tiefer einsteigen willst, schau dir mein nächstes Video zu File-Uploads mit Multer an!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon einmal eine eigene Datei-Verwaltung in Node.js gebaut? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"File Uploads mit Multer – So speicherst du Dateien in Node.js!"_

---

🎯 **Fertig!** 🎯  
✅ **Möchtest du noch ein spezielles Beispiel oder ein Projekt mit dem `fs`-Modul ergänzen?** 😊
