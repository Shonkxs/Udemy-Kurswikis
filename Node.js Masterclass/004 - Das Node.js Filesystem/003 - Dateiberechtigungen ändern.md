### **📜 Videoskript: Dateiberechtigungen in Node.js ändern – chmod, chown & Co.**

🎬 **Titel:** Dateiberechtigungen in Node.js ändern – chmod, chown & Co. einfach erklärt!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Node.js-Entwickler & DevOps  
🎯 **Lernziel:** Verstehen, wie man **Dateiberechtigungen** in Node.js mit dem `fs`-Modul ändert und verwaltet.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wer darf eine Datei lesen, schreiben oder ausführen? In Unix- und Linux-Systemen steuern **Dateiberechtigungen**, wer was mit einer Datei tun kann. Heute zeige ich dir, wie du mit Node.js **Dateiberechtigungen ändern** kannst – mit `chmod`, `chown` und mehr!"_

📌 **Agenda:**  
✅ Dateiberechtigungen verstehen (`rwx`, `chmod`, `chown`)  
✅ Dateirechte mit Node.js auslesen  
✅ Berechtigungen mit `fs.chmod()` ändern  
✅ Besitzer mit `fs.chown()` setzen  
✅ Best Practices & Fehlervermeidung

---

## **🔹 2. Was sind Dateiberechtigungen? (1:00 – 2:30)**

**🎥 Szene:** Sprecher zeigt eine Unix-Berechtigungsübersicht (`ls -l`).

🗣️ **Sprecher:**  
_"In Unix-basierten Systemen hat jede Datei und jeder Ordner drei Gruppen von Berechtigungen: **Eigentümer, Gruppe, Andere**."_

📌 **Beispiel für `ls -l` in Linux/Mac:**

```bash
-rwxr-xr--  1 user  staff  1234 Mar 18 10:00 script.sh
```

🎯 **Aufschlüsselung:**  
✅ **rwx** (Eigentümer) – Lesen, Schreiben, Ausführen  
✅ **r-x** (Gruppe) – Lesen, Ausführen  
✅ **r--** (Andere) – Nur Lesen

📌 **Numerische Darstellung (`chmod 755`):**  
| Wert | Bedeutung | Beispiel (`chmod 755`) |
|------|------------|----------------|
| **4** | Lesen (r) | ✅ **r--** |
| **2** | Schreiben (w) | ✅ **-w-** |
| **1** | Ausführen (x) | ✅ **--x** |
| **7** | Lesen, Schreiben, Ausführen | ✅ **rwx** |
| **5** | Lesen & Ausführen | ✅ **r-x** |

🎯 **Warum ist das wichtig?**  
✅ **Sicherheit:** Verhindert ungewollte Zugriffe.  
✅ **Skripte & Programme:** Müssen ausführbar sein (`chmod +x`).  
✅ **Dateizugriffe regulieren:** Wer darf lesen, schreiben, ausführen?

---

## **🔹 3. Dateiberechtigungen mit Node.js auslesen (2:30 – 4:00)**

📌 **Berechtigungen einer Datei prüfen mit `fs.stat()`**

```javascript
import fs from "fs";

fs.stat("test.txt", (err, stats) => {
  if (err) {
    console.error("Fehler beim Lesen der Datei:", err);
    return;
  }
  console.log("Berechtigungen (Oktal):", (stats.mode & 0o777).toString(8));
});
```

🎯 **Erklärung:**  
✅ `fs.stat()` gibt Datei-Informationen zurück.  
✅ `stats.mode & 0o777` filtert nur die Berechtigungen heraus.  
✅ `toString(8)` konvertiert in die Oktalnotation (`644`, `755` etc.).

---

## **🔹 4. Dateiberechtigungen mit `fs.chmod()` ändern (4:00 – 6:00)**

📌 **Eine Datei schreibgeschützt setzen (`chmod 444`)**

```javascript
fs.chmod("test.txt", 0o444, (err) => {
  if (err) {
    console.error("Fehler beim Setzen der Berechtigung:", err);
    return;
  }
  console.log("Berechtigungen geändert auf 444 (Nur Lesen)");
});
```

🎯 **Nutzen:**  
✅ `0o444` setzt die Berechtigungen auf **Nur Lesen**.  
✅ Berechtigt **niemanden**, die Datei zu schreiben.

📌 **Eine Datei ausführbar machen (`chmod 755`)**

```javascript
fs.chmod("script.sh", 0o755, (err) => {
  if (err) {
    console.error("Fehler:", err);
    return;
  }
  console.log("Datei ist nun ausführbar!");
});
```

🎯 **Nutzen:**  
✅ Erlaubt dem **Besitzer, zu schreiben & auszuführen**.  
✅ **Andere können nur lesen & ausführen**.

---

## **🔹 5. Besitzer & Gruppe mit `fs.chown()` ändern (6:00 – 8:00)**

📌 **Besitzer und Gruppe ändern (Root-Berechtigung nötig!)**

```javascript
fs.chown("test.txt", 1001, 1002, (err) => {
  if (err) {
    console.error("Fehler:", err);
    return;
  }
  console.log("Besitzer & Gruppe geändert!");
});
```

🎯 **Parameter:**  
✅ `1001` – **Neue Benutzer-ID (UID)**  
✅ `1002` – **Neue Gruppen-ID (GID)**

⚠ **Achtung:** `fs.chown()` benötigt **Root-Rechte** auf Linux/Mac!

📌 **Aktuellen Besitzer und Gruppe anzeigen**

```javascript
fs.stat("test.txt", (err, stats) => {
  if (err) {
    console.error("Fehler:", err);
    return;
  }
  console.log("Besitzer (UID):", stats.uid);
  console.log("Gruppe (GID):", stats.gid);
});
```

🎯 **Nutzen:** Perfekt für **Benutzerverwaltung & Zugriffsrechte** in **Server-Umgebungen**!

---

## **🔹 6. Best Practices & häufige Fehler (8:00 – 10:00)**

📌 **🚀 Best Practices für sicheres Arbeiten mit Berechtigungen**  
✅ **Nutze `chmod` für Skripte & Logs**, um Manipulation zu vermeiden.  
✅ **Setze sensible Dateien auf `600`** (`chmod 600 config.env`).  
✅ **Vermeide `777`** – Jeder könnte die Datei ändern & ausführen!

📌 **⚠️ Häufige Fehler vermeiden**  
❌ **Vergessen, Berechtigungen zu prüfen** → **Nutze `fs.stat()` vor Änderungen.**  
❌ **Falsche Oktalwerte verwenden** → **Immer `0o` vor `chmod()`-Werten setzen!**  
❌ **`chown()` ohne Root-Rechte nutzen** → **Erfordert `sudo` oder `root`.**

---

## **🔹 7. Fazit & Outro (10:00 – 12:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du Dateiberechtigungen in Node.js verwaltest! Von `chmod` über `chown` bis zur Rechteprüfung mit `fs.stat()` – jetzt kannst du den Zugriff auf Dateien sicher steuern!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon mal ein Skript mit `chmod` ausführbar gemacht? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Node.js & Dateisystem – So nutzt du das `fs`-Modul richtig!"_

---

🎯 **Fertig!** 🎯  
✅ **Willst du noch ein Beispiel für Server-Berechtigungen ergänzen?** 😊
