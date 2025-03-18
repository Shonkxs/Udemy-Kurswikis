Hier ist ein **ausführliches Videoskript** für das Thema **"Was ist eine REST-API?"**. Es erklärt REST-APIs verständlich mit Beispielen und Code-Demos.

---

## **📜 Videoskript: Was ist eine REST-API?**

🎬 **Titel:** Was ist eine REST-API? Alles, was du wissen musst!  
🎤 **Dauer:** ca. 10–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene  
🎯 **Lernziel:** Verständnis für das Konzept einer REST-API und wie man sie nutzt

---

### **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit API-Diagramm.  
🗣️ **Sprecher:**  
_"Jeden Tag nutzt du unbewusst hunderte von APIs – sei es beim Login mit Google, beim Wetter-Check oder beim Online-Shopping. Aber was ist eigentlich eine REST-API? Heute erkläre ich dir genau das – mit einfachen Beispielen und einem Code-Demo!"_

📌 **Agenda:**
✅ Was ist eine API?  
✅ Was bedeutet REST?  
✅ Wie funktioniert eine REST-API?  
✅ Praktisches Beispiel mit Node.js

---

### **🔹 2. Was ist eine API? (1:00 – 2:30)**

**🎥 Szene:** Animierte Grafik: Ein Benutzer fragt eine API nach Daten.  
🗣️ **Sprecher:**  
*"API steht für *Application Programming Interface*. Eine API ist eine Schnittstelle, die es zwei Systemen erlaubt, miteinander zu kommunizieren."*

🖥️ **💡 Beispiel:**

- **Du** bestellst in einem Restaurant (Client).
- **Der Kellner** nimmt deine Bestellung auf (API).
- **Die Küche** bereitet dein Essen zu (Server).
- **Der Kellner bringt dein Essen zurück** (API liefert Daten zurück).

🎯 **Fazit:** Eine API ist ein **Vermittler** zwischen zwei Systemen.

---

### **🔹 3. Was bedeutet REST? (2:30 – 5:00)**

**🎥 Szene:** Sprecher mit REST-Definition und HTTP-Methoden-Tabelle.

🗣️ **Sprecher:**  
*"REST steht für *Representational State Transfer* – ein Architekturstil für APIs, der einfache, standardisierte Regeln nutzt."*

📌 **REST-Prinzipien:**  
1️⃣ **Client-Server-Architektur** (Trennung von Frontend & Backend)  
2️⃣ **Zustandslosigkeit** (Jede Anfrage enthält alle Infos, kein Session-Tracking)  
3️⃣ **Cachefähigkeit** (Antworten können zwischengespeichert werden)  
4️⃣ **Einheitliche Schnittstellen** (Standardisierte HTTP-Methoden)

📌 **Wichtige HTTP-Methoden:**  
| Methode | Bedeutung |
|----------|------------|
| **GET** | Daten abrufen |
| **POST** | Neue Daten erstellen |
| **PUT** | Daten aktualisieren |
| **DELETE** | Daten löschen |

🎯 **Fazit:** REST-APIs nutzen **HTTP-Methoden**, um Daten zu senden und abzurufen.

---

### **🔹 4. Wie funktioniert eine REST-API? (5:00 – 7:30)**

⚠️ jsonplaceholder nutzen

**🎥 Szene:** Live-Demo einer REST-API in Postman oder Browser.
🗣️ **Sprecher:**  
_"Lass uns eine echte REST-API in Aktion sehen!"_

✅ **API-Endpunkte einer User-Verwaltung:**

```
GET      /users          // Alle Nutzer abrufen
GET      /users/:id      // Einzelnen Nutzer abrufen
POST     /users          // Neuen Nutzer erstellen
PUT      /users/:id      // Nutzer aktualisieren
DELETE   /users/:id      // Nutzer löschen
```

🖥️ **Beispiel mit JSON-Response:**

```json
GET /users/1

{
  "id": 1,
  "name": "Max Mustermann",
  "email": "max@example.com"
}
```

🎯 **Fazit:** Eine REST-API antwortet in der Regel mit **JSON-Daten**, die vom Client verarbeitet werden.

---

---

### **🔹 7. Fazit & Outro (11:30 – 12:00)**

**🎥 Szene:** Sprecher fasst das Gelernte zusammen.  
🗣️ **Sprecher:**  
_"Heute haben wir gelernt, dass REST-APIs standardisierte HTTP-Methoden nutzen, JSON-Daten liefern und überall verwendet werden! Falls du tiefer einsteigen willst, schau dir meine Videos zur API-Entwicklung mit Express an. Like & abonniere für mehr Node.js-Tutorials – bis zum nächsten Mal! 👋"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon einmal eine eigene REST-API gebaut? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Erste REST-API mit Express.js – Dein erster eigener API-Server!"_

---

🎯 **Fertig!** 🎯  
Dieses Skript ist **strukturiert**, enthält **klare Code-Beispiele** und hat **eine Call-to-Action für Interaktion**.

✅ **Möchtest du noch ein vertieftes Beispiel hinzufügen?** 😊
