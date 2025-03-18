### **📜 Videoskript: API-Rate-Limiting mit Express – So schützt du deine API vor Missbrauch!**

🎬 **Titel:** API-Rate-Limiting mit Express – Schutz vor DDoS & API-Missbrauch  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** Verstehen, **wie man Rate-Limiting in Express implementiert**, um **DDoS-Angriffe & übermäßige API-Nutzung zu verhindern**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Jede API kann durch zu viele Anfragen überlastet werden – sei es durch böswillige Angriffe oder unerwartet hohe Nutzung. Die Lösung? **API-Rate-Limiting!** Heute zeige ich dir, wie du mit Express eine **Ratenbegrenzung** einführst, um deine API zu schützen!"_

📌 **Agenda:**  
✅ Warum ist Rate-Limiting wichtig?  
✅ Rate-Limiting mit `express-rate-limit`  
✅ Benutzer- & IP-basiertes Throttling  
✅ Erweiterte Konfiguration (Redis für verteilte Systeme)  
✅ Best Practices

---

## **🔹 2. Warum ist Rate-Limiting wichtig? (1:00 – 2:30)**

📌 **Was passiert ohne Rate-Limiting?**  
❌ **DDoS-Angriffe** können Server überlasten.  
❌ **Unbegrenzte API-Anfragen** können zu hohen Kosten führen (z. B. externe APIs).  
❌ **Missbrauch durch Bots & Scraper** kann sensible Daten gefährden.

🎯 **Lösung:**  
✅ **Rate-Limiting begrenzt die Anzahl der Anfragen pro Nutzer/IP.**  
✅ **Schützt API-Ressourcen & verbessert die Stabilität.**

---

## **🔹 3. Grundlegendes Rate-Limiting mit `express-rate-limit` (2:30 – 5:00)**

📌 **Schritt 1: Paket installieren**

```bash
npm install express-rate-limit
```

📌 **Schritt 2: Middleware in Express hinzufügen**

```javascript
import express from "express";
import rateLimit from "express-rate-limit";

const app = express();
const port = 3000;

// Basic Rate Limiting: Maximal 100 Anfragen pro 15 Minuten pro IP
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 Minuten
  max: 100, // 100 Anfragen pro IP
  message: "Zu viele Anfragen, bitte versuche es später erneut.",
  headers: true, // Zeigt Rate-Limit-Header in der Response
});

// Rate-Limiting für alle Routen aktivieren
app.use(limiter);

app.get("/", (req, res) => {
  res.send("Willkommen in meiner API!");
});

app.listen(port, () =>
  console.log(`Server läuft auf http://localhost:${port}`)
);
```

📌 **Testen:**  
➡ **Nach 100 Anfragen in 15 Minuten wird die IP blockiert!**  
➡ **Antwort:**

```json
{
  "message": "Zu viele Anfragen, bitte versuche es später erneut."
}
```

🎯 **Nutzen:**  
✅ **Begrenzt die API-Nutzung pro IP**  
✅ **Schützt vor einfachen DDoS-Attacken**

---

## **🔹 4. Rate-Limiting für bestimmte Routen (5:00 – 7:30)**

📌 **Beispiel: Strikteres Limit für Login-Route**

```javascript
const loginLimiter = rateLimit({
  windowMs: 5 * 60 * 1000, // 5 Minuten
  max: 5, // Nur 5 Versuche pro IP
  message: "Zu viele Login-Versuche, bitte warte 5 Minuten.",
});

app.post("/login", loginLimiter, (req, res) => {
  res.send("Login erfolgreich (wenn es nicht limitiert wurde)");
});
```

🎯 **Nutzen:**  
✅ **Verhindert Brute-Force-Angriffe auf Login-Systeme.**  
✅ **Erhöht die Sicherheit von Benutzerkonten.**

---

## **🔹 5. Benutzer- & IP-basiertes Throttling (7:30 – 9:30)**

📌 **Problem:** Rate-Limiting per IP kann ineffektiv sein, wenn viele Benutzer dieselbe IP nutzen (z. B. Firmen, Mobilnetze).  
📌 **Lösung:** Rate-Limiting für **bestimmte Benutzer-IDs** mit `keyGenerator`.

📌 **Rate-Limiting für Benutzer-IDs statt IPs**

```javascript
const userLimiter = rateLimit({
  windowMs: 10 * 60 * 1000, // 10 Minuten
  max: 50, // 50 Anfragen pro Benutzer
  keyGenerator: (req) => (req.user ? req.user.id : req.ip),
  message: "Limit erreicht! Bitte warte kurz.",
});

app.use("/api", userLimiter);
```

🎯 **Nutzen:**  
✅ **Schützt APIs basierend auf Benutzerkonten statt IPs.**  
✅ **Ideal für APIs mit Authentifizierung!**

---

## **🔹 6. Erweiterte Rate-Limiting-Lösung mit Redis (9:30 – 11:30)**

📌 **Problem:**

- Wenn deine API **auf mehreren Servern läuft**, wird das Rate-Limiting nur lokal gespeichert.
- Lösung? **Redis als verteiltes Limit-System nutzen!**

📌 **Schritt 1: Redis & `rate-limit-redis` installieren**

```bash
npm install ioredis rate-limit-redis
```

📌 **Schritt 2: Redis-Client in `server.js` einrichten**

```javascript
import Redis from "ioredis";
import { RateLimit } from "rate-limit-redis";

const redisClient = new Redis();

const redisLimiter = rateLimit({
  store: new RateLimit({
    store: redisClient,
    windowMs: 15 * 60 * 1000,
    max: 100,
  }),
  message: "Zu viele Anfragen! Versuch es später erneut.",
});

app.use(redisLimiter);
```

🎯 **Nutzen:**  
✅ **Verteilt Limits über mehrere Server** (perfekt für große APIs).  
✅ **Hält Limits auch nach Serverneustart.**

---

## **🔹 7. Best Practices für API-Rate-Limiting (11:30 – 13:00)**

📌 **🚀 Best Practices für sicheres Rate-Limiting:**  
✅ **Setze verschiedene Limits für unterschiedliche Routen** (`/login` vs. `/api`).  
✅ **Nutze Redis für verteilte Systeme mit mehreren Servern.**  
✅ **Zeige in der API-Antwort Header mit Rate-Limits (`Retry-After`).**  
✅ **Logging & Monitoring für verdächtige IPs aktivieren.**

📌 **Häufige Fehler vermeiden:**  
❌ **Zu hohe Limits → Kein Schutz gegen Abuse.**  
❌ **Zu niedrige Limits → Frustrierte echte Nutzer.**  
❌ **Nur IP-Limits nutzen → Nutzer hinter Firewalls können gesperrt werden.**

---

## **🔹 8. Fazit & Outro (13:00 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **API-Rate-Limiting mit Express** umsetzt! Mit `express-rate-limit` kannst du deine API gegen DDoS, Missbrauch & Brute-Force absichern und gleichzeitig echte Nutzer schützen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon API-Limits in deinen Projekten genutzt? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Security – So schützt du deine API mit Helmet & CORS!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für IP-Blacklisting oder Captcha-Integration ergänzen?** 😊
