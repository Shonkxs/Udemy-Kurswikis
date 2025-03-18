### **📜 Videoskript: async & await in JavaScript – Einfach erklärt!**

🎬 **Titel:** async & await – Asynchrones JavaScript einfach erklärt!  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** **async & await** verstehen und korrekt in JavaScript verwenden.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Kennst du das Problem mit verschachtelten `then()`-Ketten in JavaScript? Mit `async & await` wird asynchroner Code viel einfacher und lesbarer! Heute erkläre ich dir, was `async & await` ist, warum wir es brauchen und wie du es richtig nutzt!"_

📌 **Agenda:**  
✅ Warum brauchen wir `async & await`?  
✅ Der Unterschied zu Promises  
✅ Praktische Beispiele  
✅ Fehlerhandling mit `try/catch`

---

## **🔹 2. Warum brauchen wir async & await? (1:00 – 3:00)**

**🎥 Szene:** Animation eines typischen JavaScript-Ablaufs (Synchrone vs. Asynchrone Ausführung).

🗣️ **Sprecher:**  
_"JavaScript ist **einzelthreadig**, das bedeutet: Code wird Zeile für Zeile ausgeführt. Doch was passiert, wenn eine Funktion länger dauert – zum Beispiel das Laden von Daten aus einer API?"_

📌 **Problem mit synchronem Code:**

```javascript
console.log("Start");
const daten = fetch("https://api.example.com/user");
console.log("Daten geladen:", daten); // Funktioniert nicht!
console.log("Ende");
```

🎯 **Problem:** Der `fetch()`-Aufruf ist asynchron – die Daten sind noch nicht da!

📌 **Lösung: Früher mit `then()` (Promises):**

```javascript
console.log("Start");

fetch("https://api.example.com/user")
  .then((response) => response.json())
  .then((data) => console.log("Daten geladen:", data))
  .catch((error) => console.error("Fehler:", error));

console.log("Ende");
```

🎯 **Problem:** Verschachtelte `then()`-Ketten (sogenannte **Promise Hells**) sind unübersichtlich!  
💡 **Lösung:** `async & await` macht den Code **lesbarer und strukturierter**!

---

## **🔹 3. Was ist async & await? (3:00 – 5:30)**

**🎥 Szene:** Sprecher erklärt das Prinzip mit einem einfachen Beispiel.

🗣️ **Sprecher:**  
_"`async & await` ist eine modernere und elegantere Art, mit asynchronem Code umzugehen. Lass uns den gleichen Code mit `async & await` schreiben!"_

📌 **Code-Vergleich: Mit `async & await`**

```javascript
async function ladeDaten() {
  console.log("Start");

  const response = await fetch("https://api.example.com/user");
  const daten = await response.json();

  console.log("Daten geladen:", daten);
  console.log("Ende");
}

ladeDaten();
```

🎯 **Fazit:**  
✅ `async` macht eine Funktion **asynchron**.  
✅ `await` **wartet**, bis ein Promise aufgelöst wird, **bevor die nächste Zeile ausgeführt wird**.  
✅ Kein `then()` mehr – der Code sieht **synchron** aus, bleibt aber **asynchron**!

---

## **🔹 4. Praktische Beispiele (5:30 – 9:30)**

### **👉 Beispiel 1: API-Daten abrufen mit `async & await`**

**🎥 Szene:** Live-Coding einer API-Abfrage.

🖥️ **Code:**

```javascript
async function getUser() {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/users/1"
    );
    const user = await response.json();
    console.log("Benutzer:", user);
  } catch (error) {
    console.error("Fehler beim Abrufen der Daten:", error);
  }
}

getUser();
```

🎯 **Fazit:**  
✅ `try/catch` fängt Fehler ab.  
✅ `await` sorgt dafür, dass die Daten erst geladen werden, bevor sie verarbeitet werden.

---

### **👉 Beispiel 2: Mehrere asynchrone Aufrufe hintereinander**

**🎥 Szene:** Vergleich zwischen sequenziellen und parallelen API-Aufrufen.

📌 **Sequenziell (langsamer, da die Aufrufe nacheinander erfolgen):**

```javascript
async function getData() {
  const user = await fetch("https://jsonplaceholder.typicode.com/users/1").then(
    (res) => res.json()
  );
  const posts = await fetch(
    "https://jsonplaceholder.typicode.com/posts?userId=1"
  ).then((res) => res.json());

  console.log("User:", user);
  console.log("Posts:", posts);
}
getData();
```

🎯 **Problem:** Die zweite Anfrage (`posts`) startet erst, wenn die erste (`user`) abgeschlossen ist.

📌 **Parallel ausführen mit `Promise.all()` (schneller! 🚀):**

```javascript
async function getDataParallel() {
  const [user, posts] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users/1").then((res) =>
      res.json()
    ),
    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then((res) =>
      res.json()
    ),
  ]);

  console.log("User:", user);
  console.log("Posts:", posts);
}
getDataParallel();
```

🎯 **Fazit:** `Promise.all()` ermöglicht parallele Anfragen – **deutlich schneller!** 🚀

---

## **🔹 5. Fehlerhandling mit `try/catch` (9:30 – 11:00)**

🗣️ **Sprecher:**  
_"Asynchroner Code kann fehlschlagen – z. B. durch Netzwerkfehler. Deshalb brauchen wir `try/catch`, um Fehler abzufangen!"_

📌 **Fehlerhandling mit `try/catch`:**

```javascript
async function getUserSafe() {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/users/1"
    );

    if (!response.ok) {
      throw new Error(`Fehler: ${response.status}`);
    }

    const user = await response.json();
    console.log("Benutzer:", user);
  } catch (error) {
    console.error("Fehler:", error.message);
  }
}

getUserSafe();
```

🎯 **Fazit:**  
✅ `catch` fängt Fehler ab, z. B. wenn das Internet ausfällt.  
✅ `response.ok` prüft den HTTP-Statuscode.

---

## **🔹 6. Fazit & Outro (11:00 – 12:30)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie `async & await` JavaScript-Code lesbarer macht! Verwende es für API-Abfragen, Datenverarbeitung und mehr. Falls du tiefer einsteigen willst, check meine nächsten Videos zu Promises und WebSockets!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wo nutzt du `async & await` am meisten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"JavaScript Promises – So funktioniert asynchroner Code wirklich!"_

---

🎯 **Fertig!** 🎯  
Dieses Skript ist **klar strukturiert**, enthält **Code-Demos** und **praktische Anwendungsfälle**.

✅ **Möchtest du noch ein extra Beispiel oder eine spezifische API-Nutzung ergänzen?** 😊
