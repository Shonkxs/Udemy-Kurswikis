### **📜 Videoskript: Fehlerhandling mit Promises in JavaScript**

🎬 **Titel:** Fehlerhandling mit Promises – So vermeidest du Bugs in asynchronem Code!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** **Wie man Fehler in Promises richtig behandelt** und warum **try/catch nicht immer funktioniert**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Asynchroner Code in JavaScript ist super – aber was passiert, wenn ein Fehler auftritt? Promises können abstürzen, Anfragen können fehlschlagen und ohne richtiges Fehlerhandling kann dein Code unvorhersehbar werden! Heute zeige ich dir, wie du Fehler in Promises richtig behandelst und typische Fehler vermeidest."_

📌 **Agenda:**  
✅ Warum brauchen wir Fehlerhandling in Promises?  
✅ `catch()` vs. `try/catch`  
✅ `finally()` und Best Practices  
✅ Live-Coding mit echten API-Fehlern

---

## **🔹 2. Warum brauchen wir Fehlerhandling in Promises? (1:00 – 3:00)**

**🎥 Szene:** Sprecher erklärt anhand eines Beispiels, was passiert, wenn ein Promise fehlschlägt.

🗣️ **Sprecher:**  
_"Wenn ein synchroner Fehler auftritt, bekommen wir eine Fehlermeldung in der Konsole. Aber was ist mit asynchronen Fehlern?_"

📌 **Beispiel für einen normalen synchronen Fehler:**

```javascript
console.log("Start");
throw new Error("Oops, etwas ist schiefgelaufen!");
console.log("Ende"); // Wird nie erreicht
```

🎯 **Erwartetes Verhalten:** Der Fehler stoppt die gesamte Ausführung.

📌 **Jetzt mit einem Promise:**

```javascript
console.log("Start");

new Promise((resolve, reject) => {
  reject("Oops, Fehler im Promise!");
}).then((data) => console.log(data));

console.log("Ende");
```

🎯 **Problem:**  
✅ `Ende` wird trotzdem ausgeführt – der Fehler stoppt das Programm **nicht** sofort!  
✅ Ohne `catch()` bleibt der Fehler **ungesehen** und verursacht unerwartetes Verhalten.

💡 **Lösung:** **Fehler müssen in Promises explizit abgefangen werden!**

---

## **🔹 3. Fehler abfangen mit `catch()` (3:00 – 5:00)**

**🎥 Szene:** Vergleich von Promise-Fehlern mit und ohne `catch()`.

📌 **Falscher Code ohne Fehlerhandling:**

```javascript
new Promise((resolve, reject) => {
  reject("Fehler: Datenbankverbindung fehlgeschlagen!");
}).then((data) => console.log("Daten:", data)); // Fehler wird nicht abgefangen!
```

🎯 **Problem:** Der Fehler geht verloren – das Programm weiß nicht, dass etwas schiefgelaufen ist.

📌 **Korrekte Lösung mit `catch()`:**

```javascript
new Promise((resolve, reject) => {
  reject("Fehler: Datenbankverbindung fehlgeschlagen!");
})
  .then((data) => console.log("Daten:", data))
  .catch((error) => console.error("Promise abgelehnt:", error));
```

🎯 **Fazit:**  
✅ `catch()` fängt ALLE Fehler in der Promise-Kette ab.  
✅ Sollte immer ans Ende einer `then()`-Kette gesetzt werden!

---

## **🔹 4. `catch()` vs. `try/catch` (5:00 – 7:30)**

**🎥 Szene:** Sprecher erklärt, warum `try/catch` nicht immer funktioniert.

🗣️ **Sprecher:**  
_"Wir kennen `try/catch` aus synchronem Code – funktioniert das auch mit Promises?"_

📌 **Funktioniert nicht!**

```javascript
try {
  new Promise((resolve, reject) => {
    reject("Fehler: Server nicht erreichbar!");
  });
} catch (error) {
  console.error("Fehler:", error);
}
```

🎯 **Warum?**  
✅ `try/catch` funktioniert **nur für synchronen Code**.  
✅ Promises laufen asynchron – der Fehler ist nicht innerhalb des `try`-Blocks!

📌 **Richtige Lösung:**

```javascript
async function ladeDaten() {
  try {
    const daten = await new Promise((resolve, reject) => {
      reject("Fehler: Server nicht erreichbar!");
    });
    console.log(daten);
  } catch (error) {
    console.error("Fehler abgefangen:", error);
  }
}

ladeDaten();
```

🎯 **Fazit:**  
✅ `catch()` ist für **Promise-Ketten**.  
✅ `try/catch` funktioniert **nur in `async`-Funktionen** mit `await`!

---

## **🔹 5. `finally()` – Code ausführen, egal ob Fehler oder Erfolg (7:30 – 9:00)**

**🎥 Szene:** Beispiel für einen `finally()`-Block.

📌 **Anwendungsfall: Spinner oder Loading-Indikator anzeigen**

```javascript
console.log("Laden...");

new Promise((resolve, reject) => {
  setTimeout(() => reject("Fehler: Timeout!"), 2000);
})
  .then((data) => console.log("Erfolg:", data))
  .catch((error) => console.error("Fehlermeldung:", error))
  .finally(() => console.log("Laden beendet!"));
```

🎯 **Fazit:**  
✅ `finally()` wird **immer ausgeführt**, egal ob Erfolg oder Fehler.  
✅ Perfekt für Cleanup-Aufgaben (z. B. Ladeindikatoren ausschalten).

---

## **🔹 6. Praxisbeispiel: API-Fehlerhandling (9:00 – 11:00)**

**🎥 Szene:** Sprecher testet eine fehlerhafte API-Abfrage mit `fetch()`.

📌 **API-Fehler richtig abfangen:**

```javascript
async function fetchUserData() {
  try {
    const response = await fetch("https://api.example.com/user");

    if (!response.ok) {
      throw new Error(`HTTP-Fehler ${response.status}`);
    }

    const data = await response.json();
    console.log("Daten:", data);
  } catch (error) {
    console.error("API-Fehler:", error.message);
  } finally {
    console.log("Request abgeschlossen.");
  }
}

fetchUserData();
```

🎯 **Fazit:**  
✅ `if (!response.ok) throw new Error()` erkennt API-Fehler.  
✅ `try/catch` fängt alles ab.  
✅ `finally()` zeigt an, dass der Request fertig ist.

---

## **🔹 7. Fazit & Outro (11:00 – 12:00)**

🗣️ **Sprecher:**  
_"Jetzt weißt du, wie man Fehler in Promises richtig behandelt! Nutze immer `catch()` für Promise-Ketten und `try/catch` in `async`-Funktionen. Falls du noch mehr über asynchrones JavaScript lernen willst, schau dir mein nächstes Video zu `async & await` an!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie gehst du mit Fehlern in deinem Code um? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"async & await – So machst du JavaScript-Code lesbarer!"_

---

🎯 **Fertig!** 🎯  
Dieses Skript ist **strukturiert**, enthält **praktische Code-Beispiele** und erklärt die **wichtigsten Fehlerquellen**!

✅ **Möchtest du noch ein extra Beispiel oder ein Anwendungsprojekt ergänzen?** 😊
