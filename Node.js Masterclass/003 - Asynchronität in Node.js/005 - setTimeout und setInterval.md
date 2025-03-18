### **📜 Videoskript: `setTimeout` & `setInterval` in JavaScript – Einfach erklärt!**

🎬 **Titel:** `setTimeout` & `setInterval` – So steuerst du Zeit in JavaScript!  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** Verständnis für **setTimeout & setInterval**, deren **Unterschiede**, **Einsatzmöglichkeiten** und **Best Practices**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wie lässt man eine Funktion nach einer bestimmten Zeit ausführen? Oder wie erstellt man eine Schleife, die alle paar Sekunden etwas ausführt? Die Antwort: `setTimeout` & `setInterval`! Heute lernst du, wie du sie benutzt und was du beachten musst!"_

📌 **Agenda:**  
✅ `setTimeout` – Verzögerte Funktionsausführung  
✅ `setInterval` – Wiederholende Ausführung  
✅ Unterschiede & wann man was nutzt  
✅ Wie man `setTimeout` & `setInterval` stoppt  
✅ Best Practices & Fehlerquellen

---

## **🔹 2. Was ist `setTimeout`? (1:00 – 3:00)**

**🎥 Szene:** Sprecher zeigt eine einfache `setTimeout`-Demo.

🗣️ **Sprecher:**  
_"Mit `setTimeout` kannst du eine Funktion **nach einer bestimmten Zeit ausführen**, ohne dass dein Code blockiert wird!"_

📌 **Grundlegende Syntax:**

```javascript
setTimeout(function, delay);
```

🎯 **Parameter:**  
✅ **`function`** – Die Funktion, die nach der Wartezeit ausgeführt wird.  
✅ **`delay`** – Die Wartezeit in Millisekunden (1 Sekunde = 1000ms).

📌 **Beispiel: Nachricht nach 3 Sekunden anzeigen**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Hallo nach 3 Sekunden!");
}, 3000);

console.log("Ende");
```

🎯 **Erwartetes Verhalten:**  
1️⃣ "Start" wird sofort ausgegeben.  
2️⃣ "Ende" wird sofort ausgegeben.  
3️⃣ Nach 3 Sekunden erscheint "Hallo nach 3 Sekunden!".

---

## **🔹 3. Was ist `setInterval`? (3:00 – 5:00)**

**🎥 Szene:** Sprecher zeigt `setInterval` mit einer einfachen Animation oder einer Uhr.

🗣️ **Sprecher:**  
_"Während `setTimeout` einmalig etwas verzögert ausführt, führt `setInterval` eine Funktion in einem festen Intervall immer wieder aus!"_

📌 **Grundlegende Syntax:**

```javascript
setInterval(function, interval);
```

🎯 **Parameter:**  
✅ **`function`** – Die Funktion, die wiederholt aufgerufen wird.  
✅ **`interval`** – Die Zeit in Millisekunden zwischen den Aufrufen.

📌 **Beispiel: Eine Nachricht alle 2 Sekunden ausgeben**

```javascript
setInterval(() => {
  console.log("Diese Nachricht erscheint alle 2 Sekunden!");
}, 2000);
```

🎯 **Erwartetes Verhalten:**  
✅ Die Nachricht wird **alle 2 Sekunden wiederholt**, bis das Skript gestoppt wird.

📌 **Echte Anwendung: Eine Uhr mit `setInterval`**

```javascript
setInterval(() => {
  console.log(new Date().toLocaleTimeString());
}, 1000);
```

🎯 **Nutzen:** Perfekt für **Echtzeit-Daten, Timer oder Animationen**.

---

## **🔹 4. `setTimeout` & `setInterval` stoppen (5:00 – 7:00)**

**🎥 Szene:** Sprecher zeigt, wie man laufende Timer unterbricht.

🗣️ **Sprecher:**  
_"Manchmal willst du `setTimeout` oder `setInterval` abbrechen. Dafür gibt es `clearTimeout()` und `clearInterval()`!"_

📌 **Beispiel: `setTimeout` stoppen**

```javascript
let timeoutID = setTimeout(() => {
  console.log("Das sollte nicht erscheinen!");
}, 5000);

clearTimeout(timeoutID); // Storniert das Timeout
```

🎯 **Erwartetes Verhalten:** Die Funktion wird niemals ausgeführt.

📌 **Beispiel: `setInterval` stoppen**

```javascript
let count = 0;
let intervalID = setInterval(() => {
  console.log(`Sekunde ${count}`);
  count++;

  if (count === 5) {
    clearInterval(intervalID); // Nach 5 Durchläufen stoppen
    console.log("Interval gestoppt!");
  }
}, 1000);
```

🎯 **Erwartetes Verhalten:**  
✅ Zählt bis 4 und stoppt dann.  
✅ Perfekt für **Timer, Countdown oder begrenzte Wiederholungen**.

---

## **🔹 5. Unterschied zwischen `setTimeout` & `setInterval` (7:00 – 8:00)**

📌 **Vergleichstabelle: Wann nutzt man was?**

| **Feature**                  | **setTimeout** ⏳                              | **setInterval** 🔄                              |
| ---------------------------- | ---------------------------------------------- | ----------------------------------------------- |
| **Einmalige Ausführung**     | ✅ Ja                                          | ❌ Nein                                         |
| **Wiederholende Aktion**     | ❌ Nein                                        | ✅ Ja                                           |
| **Steuerung der Ausführung** | Manuell mit erneutem `setTimeout`              | Automatisch alle X ms                           |
| **Beispielanwendungen**      | Animationen, Verzögerungen, Benachrichtigungen | Echtzeit-Uhren, Live-Daten, regelmäßige Updates |

---

## **🔹 6. Best Practices & typische Fehler (8:00 – 10:00)**

📌 **🚀 Best Practices für `setTimeout` & `setInterval`**  
✅ **Variablen speichern:** Immer `let timer = setInterval(...)`, damit du es später stoppen kannst.  
✅ **Arrow-Funktionen nutzen:** `setTimeout(() => console.log("Hallo"), 1000);`  
✅ **Nicht zu viele `setInterval`s!** Bei mehreren Timern kann die Performance leiden.

📌 **⚠️ Häufige Fehler vermeiden**  
❌ **Vergessen, `setInterval` zu stoppen:**

```javascript
setInterval(() => {
  console.log("Dieser Timer läuft ewig...");
}, 1000);
```

✅ **Besser:**

```javascript
let count = 0;
let intervalID = setInterval(() => {
  count++;
  if (count === 5) clearInterval(intervalID);
}, 1000);
```

---

## **🔹 7. Fazit & Outro (10:00 – 12:00)**

🗣️ **Sprecher:**  
_"Jetzt weißt du, wie `setTimeout` und `setInterval` funktionieren! Nutze sie für Verzögerungen, Animationen oder Echtzeit-Updates – aber denke daran, Timer auch zu stoppen, wenn sie nicht mehr gebraucht werden!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche coolen Projekte hast du mit `setTimeout` oder `setInterval` gebaut? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"JavaScript Event Loop – Wie genau funktioniert asynchrones JavaScript?"_

---

🎯 **Fertig!** 🎯  
✅ **Willst du noch ein extra Beispiel für ein konkretes Projekt mit `setInterval` hinzufügen?** 😊
