### **📜 Videoskript: IP-Blacklisting in Express – Schutz vor unerwünschten Zugriffen!**

🎬 **Titel:** IP-Blacklisting mit Express – Blockiere unerwünschte Nutzer & Angreifer  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** Verstehen, **wie man IP-Adressen in Express blockiert**, um **böswillige Nutzer, Bots oder DDoS-Angriffe zu verhindern**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Hast du jemals bemerkt, dass bestimmte IPs wiederholt deine API angreifen oder Spam-Anfragen senden? Dann brauchst du **IP-Blacklisting!** Heute zeige ich dir, wie du in Express bestimmte IPs blockierst, um deine Anwendung vor Missbrauch zu schützen."_

📌 **Agenda:**  
✅ Warum IP-Blacklisting?  
✅ Manuelles Blacklisting mit einer Middleware  
✅ Dynamisches Blacklisting & Speicherung in einer Datei  
✅ Erweiterte Blacklisting-Lösungen mit Redis  
✅ Best Practices für API-Sicherheit

---

## **🔹 2. Warum ist IP-Blacklisting wichtig? (1:00 – 2:30)**

📌 **Bedrohungen durch nicht verifizierte IPs:**  
❌ **Bots & Scraper** greifen APIs automatisiert ab.  
❌ **DDoS-Angriffe** verursachen Serverüberlastungen.  
❌ **Hacker-IPs** versuchen Brute-Force-Angriffe auf Login-Seiten.

🎯 **Lösung:**  
✅ **Blacklist spezifische IPs, um Angriffe zu verhindern.**  
✅ **Erweiterbar mit Rate-Limiting & Authentifizierung.**

---

## **🔹 3. Manuelles IP-Blacklisting mit einer Middleware (2:30 – 5:00)**

📌 **Liste mit blockierten IPs definieren**

```javascript
const blacklistedIPs = ["192.168.1.100", "203.0.113.50"];
```

📌 **Middleware für IP-Blockierung**

```javascript
const ipBlacklistMiddleware = (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;

  if (blacklistedIPs.includes(clientIP)) {
    return res
      .status(403)
      .json({ error: "Zugriff verweigert! Deine IP wurde blockiert." });
  }

  next(); // Weiter zur nächsten Middleware oder Route
};

app.use(ipBlacklistMiddleware);
```

🎯 **Erklärung:**  
✅ **Vergleicht die IP des Clients mit der Blacklist.**  
✅ **Wenn die IP blockiert ist, wird die Anfrage mit `403 Forbidden` abgelehnt.**  
✅ **Sonst wird die Anfrage normal weiterverarbeitet.**

📌 **Testen:**  
➡ **Anfrage von einer blockierten IP → Zugriff verweigert!**

---

## **🔹 4. Dynamisches Blacklisting & Speicherung in einer Datei (5:00 – 7:30)**

📌 **IP-Blacklist in einer Datei speichern**  
➡ **Datei: `blacklist.json`**

```json
["192.168.1.100", "203.0.113.50"]
```

📌 **Middleware für dynamische Blacklist**

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

🎯 **Nutzen:**  
✅ **Ermöglicht das Hinzufügen neuer IPs ohne Neustart der Anwendung.**  
✅ **Einfach verwaltbar durch `blacklist.json`.**

📌 **IP zur Blacklist hinzufügen (Admin-Route)**

```javascript
app.post("/admin/blacklist", (req, res) => {
  const newIP = req.body.ip;
  const blacklist = loadBlacklist();

  if (!blacklist.includes(newIP)) {
    blacklist.push(newIP);
    fs.writeFileSync("blacklist.json", JSON.stringify(blacklist, null, 2));
    res.send(`IP ${newIP} wurde zur Blacklist hinzugefügt.`);
  } else {
    res.send(`IP ${newIP} ist bereits blockiert.`);
  }
});
```

📌 **Testen:**  
➡ **Sende eine POST-Anfrage mit einer IP an `/admin/blacklist`**

```json
{ "ip": "203.0.113.51" }
```

➡ **Die IP wird nun automatisch blockiert!**

---

## **🔹 5. Erweiterte IP-Blacklisting-Lösung mit Redis (7:30 – 9:30)**

📌 **Warum Redis?**  
✅ **Ideal für verteilte Systeme mit mehreren Servern.**  
✅ **Blacklist-Updates werden sofort für alle Instanzen übernommen.**

📌 **Schritt 1: Redis installieren**

```bash
npm install ioredis
```

📌 **Schritt 2: Redis-Client einrichten**

```javascript
import Redis from "ioredis";
const redisClient = new Redis();
```

📌 **Schritt 3: Middleware für IP-Blacklist mit Redis**

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

📌 **Schritt 4: IP zur Blacklist in Redis hinzufügen**

```javascript
app.post("/admin/blacklist", async (req, res) => {
  const newIP = req.body.ip;
  await redisClient.sadd("blacklist", newIP);
  res.send(`IP ${newIP} wurde zur Blacklist hinzugefügt.`);
});
```

🎯 **Nutzen:**  
✅ **Skalierbare Lösung für verteilte Systeme mit mehreren API-Servern.**  
✅ **Blacklist-Änderungen wirken sofort auf alle Server.**

---

## **🔹 6. Best Practices für sicheres IP-Blacklisting (9:30 – 11:30)**

📌 **🚀 Best Practices für IP-Blocking:**  
✅ **Speichere Blacklists extern (Datei, Redis, Datenbank)**.  
✅ **Nutze Admin-Routen, um IPs zur Blacklist hinzuzufügen.**  
✅ **Kombiniere Blacklisting mit Rate-Limiting (`express-rate-limit`).**  
✅ **Erstelle eine automatische Blacklist für zu viele Fehlversuche.**

📌 **Häufige Fehler vermeiden:**  
❌ **Keine dynamische Blacklist → Neustart bei Änderungen nötig.**  
❌ **Nur IP-Blocking ohne weitere Sicherheitsmechanismen.**  
❌ **Zu aggressive Blockierung → Könnte echte Nutzer sperren.**

---

## **🔹 7. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **IP-Blacklisting mit Express** umsetzt – von einer einfachen Liste bis hin zu einer skalierbaren Lösung mit Redis! Damit kannst du **Angriffe & unerwünschte Zugriffe auf deine API verhindern**!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du schon IP-Blocking in deinen Projekten genutzt? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Security – So schützt du deine API mit Helmet & CORS!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für automatisches Blacklisting bei zu vielen Login-Fehlversuchen ergänzen?** 😊
