### **📜 Videoskript: Automatisches Blacklisting bei zu vielen Loginversuchen mit Express**

🎬 **Titel:** Automatisches IP-Blacklisting bei zu vielen Loginversuchen – So schützt du deine API vor Brute-Force-Angriffen  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** Verstehen, **wie man wiederholte fehlgeschlagene Loginversuche erkennt und automatisch blockiert**, um **Brute-Force-Angriffe zu verhindern**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Wiederholte falsche Passworteingaben können ein Zeichen für einen Brute-Force-Angriff sein! Um dein System zu schützen, brauchst du ein **automatisches IP-Blacklisting**. In diesem Video zeige ich dir, wie du mit Express und Redis verdächtige IPs automatisch blockierst!"_

📌 **Agenda:**  
✅ Warum automatisches Blacklisting?  
✅ Login-Versuche mit `express-rate-limit` begrenzen  
✅ Fehlversuche speichern & IP automatisch blockieren  
✅ Erweiterte Lösung mit Redis für verteilte Systeme  
✅ Best Practices für sichere Login-Systeme

---

## **🔹 2. Warum automatisches Blacklisting? (1:00 – 2:30)**

📌 **Problem:**

- Ein Angreifer kann Millionen von Passwortkombinationen ausprobieren (_Brute-Force-Angriff_).
- Ohne Schutz kann das System kompromittiert oder überlastet werden.

📌 **Lösung:**  
✅ **Maximale Fehlversuche begrenzen (Rate-Limiting).**  
✅ **IPs mit zu vielen Fehlversuchen automatisch blockieren.**  
✅ **Blacklist in einer Datei oder Redis speichern.**

🎯 **Ziel:** **Sobald eine IP zu viele Loginversuche hat, wird sie automatisch blockiert!**

---

## **🔹 3. Login-Versuche mit `express-rate-limit` begrenzen (2:30 – 5:00)**

📌 **Schritt 1: `express-rate-limit` installieren**

```bash
npm install express-rate-limit
```

📌 **Schritt 2: Middleware für Login-Rate-Limit hinzufügen**

```javascript
import rateLimit from "express-rate-limit";

// Limitiert auf 5 fehlgeschlagene Versuche pro 10 Minuten
const loginLimiter = rateLimit({
  windowMs: 10 * 60 * 1000, // 10 Minuten
  max: 5, // Maximal 5 Versuche
  message: "Zu viele Loginversuche. Bitte versuche es später erneut.",
  standardHeaders: true,
  legacyHeaders: false,
});

app.post("/login", loginLimiter, (req, res) => {
  res.send("Login erfolgreich (wenn nicht gesperrt)");
});
```

🎯 **Ergebnis:**  
✅ **Blockiert zu viele Anfragen pro IP für `/login`**.  
✅ **Verhindert einfache Brute-Force-Angriffe.**

---

## **🔹 4. Dynamisches Blacklisting bei wiederholten Login-Fehlversuchen (5:00 – 7:30)**

📌 **Zusätzlich zu Rate-Limiting wollen wir IPs speichern und länger blockieren.**

📌 **Schritt 1: IP-Blacklist speichern (Dateibasierte Lösung)**  
➡ **Datei: `blacklist.json`**

```json
["192.168.1.100", "203.0.113.50"]
```

📌 **Schritt 2: Middleware für Blacklist-Check**

```javascript
import fs from "fs";

const loadBlacklist = () => {
  return JSON.parse(fs.readFileSync("blacklist.json", "utf8"));
};

const ipBlacklistMiddleware = (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const blacklistedIPs = loadBlacklist();

  if (blacklistedIPs.includes(clientIP)) {
    return res
      .status(403)
      .json({ error: "Zugriff verweigert! Deine IP wurde blockiert." });
  }

  next();
};

app.use(ipBlacklistMiddleware);
```

📌 **Schritt 3: Automatisch IPs zur Blacklist hinzufügen**

```javascript
const failedLogins = {};

app.post("/login", (req, res) => {
  const clientIP = req.ip || req.connection.remoteAddress;

  if (!failedLogins[clientIP]) {
    failedLogins[clientIP] = 1;
  } else {
    failedLogins[clientIP]++;
  }

  if (failedLogins[clientIP] >= 5) {
    let blacklist = loadBlacklist();
    if (!blacklist.includes(clientIP)) {
      blacklist.push(clientIP);
      fs.writeFileSync("blacklist.json", JSON.stringify(blacklist, null, 2));
    }
    return res
      .status(403)
      .json({ error: "Zu viele Fehlversuche. Deine IP wurde blockiert." });
  }

  res.status(401).json({ error: "Login fehlgeschlagen!" });
});
```

🎯 **Ergebnis:**  
✅ **Nach 5 fehlgeschlagenen Login-Versuchen wird die IP dauerhaft blockiert.**  
✅ **Blacklist wird gespeichert und bleibt nach einem Neustart erhalten.**

---

## **🔹 5. Erweiterte Lösung mit Redis für verteilte Systeme (7:30 – 9:30)**

📌 **Warum Redis?**  
✅ **Bessere Skalierbarkeit für mehrere Server.**  
✅ **Blacklist-Updates werden sofort synchronisiert.**

📌 **Schritt 1: Redis & `ioredis` installieren**

```bash
npm install ioredis
```

📌 **Schritt 2: Redis-Client einrichten**

```javascript
import Redis from "ioredis";
const redisClient = new Redis();
```

📌 **Schritt 3: Fehlgeschlagene Logins in Redis speichern**

```javascript
app.post("/login", async (req, res) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const attempts = await redisClient.incr(`failed_logins:${clientIP}`);

  if (attempts === 1) {
    await redisClient.expire(`failed_logins:${clientIP}`, 600); // Ablaufzeit: 10 Minuten
  }

  if (attempts >= 5) {
    await redisClient.sadd("blacklist", clientIP);
    return res
      .status(403)
      .json({ error: "Zu viele Fehlversuche. Deine IP wurde blockiert." });
  }

  res.status(401).json({ error: "Login fehlgeschlagen!" });
});
```

📌 **Schritt 4: IP-Blacklist mit Redis prüfen**

```javascript
const ipBlacklistMiddleware = async (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const isBlacklisted = await redisClient.sismember("blacklist", clientIP);

  if (isBlacklisted) {
    return res
      .status(403)
      .json({ error: "Zugriff verweigert! Deine IP wurde blockiert." });
  }

  next();
};

app.use(ipBlacklistMiddleware);
```

🎯 **Ergebnis:**  
✅ **Dynamisches Blacklisting über Redis.**  
✅ **Blacklist-Änderungen wirken sofort auf alle Server.**

---

## **🔹 6. Best Practices für sicheres Login-Blacklisting (9:30 – 11:30)**

📌 **🚀 Best Practices:**  
✅ **Fehlgeschlagene Login-Versuche mit `express-rate-limit` drosseln.**  
✅ **IP nach mehreren Fehlversuchen temporär oder dauerhaft blockieren.**  
✅ **Verteile Blacklist über Redis für skalierbare Systeme.**  
✅ **Zusätzliche Sicherheit: Captcha nach mehreren Fehlversuchen aktivieren.**

📌 **Häufige Fehler vermeiden:**  
❌ **Zu viele legitime Nutzer sperren → Kombiniere mit Captcha.**  
❌ **Blacklist nur im Speicher halten → Nutze Datei oder Redis für Persistenz.**

---

## **🔹 7. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **automatisches IP-Blacklisting** nach zu vielen Loginversuchen einführst! Damit schützt du deine API vor Brute-Force-Angriffen und bösartigen Nutzern!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Wie schützt du deine APIs vor Brute-Force-Angriffen? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Security – So schützt du deine API mit Helmet & CORS!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel mit JWT & Captcha-Integration ergänzen?** 😊
