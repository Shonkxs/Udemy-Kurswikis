### **📜 Videoskript: Automatisches Whitelisting nach Blacklisting – So entsperrst du gesperrte IPs in Express**

🎬 **Titel:** Automatisches Whitelisting nach Blacklisting – Dynamisches IP-Management in Express  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Fortgeschrittene Webentwickler & API-Entwickler  
🎯 **Lernziel:** Verstehen, **wie man automatisch gesperrte IPs nach einer bestimmten Zeit oder durch Verifizierung wieder entsperrt**, um **falsche Sperren zu vermeiden**.

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Code-Editor im Hintergrund.  
🗣️ **Sprecher:**  
_"Hast du schon ein Blacklisting-System für IPs in Express, aber möchtest verhindern, dass legitime Nutzer dauerhaft gesperrt bleiben? Die Lösung: **Automatisches Whitelisting!** In diesem Video zeige ich dir, wie du gesperrte IPs nach einer bestimmten Zeit oder durch eine manuelle Freigabe wieder entsperrst."_

📌 **Agenda:**  
✅ Warum ist ein dynamisches Blacklisting wichtig?  
✅ Temporäres Blacklisting mit automatischer Aufhebung nach X Minuten  
✅ Benutzer-Whitelisting mit manueller Entsperrung  
✅ Erweiterte Lösung mit Redis für verteilte Systeme  
✅ Best Practices für sicheres Whitelisting

---

## **🔹 2. Warum dynamisches Blacklisting & Whitelisting? (1:00 – 2:30)**

📌 **Problem:**  
❌ **Dauerhafte Sperrung kann legitime Nutzer ausschließen.**  
❌ **Dynamische IPs (z. B. Mobilgeräte) können versehentlich blockiert werden.**  
❌ **Manuelles Entsperren ist ineffizient für Admins.**

📌 **Lösung:**  
✅ **IP nach einer bestimmten Zeit automatisch entsperren.**  
✅ **Benutzer erhalten eine Möglichkeit, sich selbst zu entsperren (z. B. Captcha, E-Mail).**  
✅ **Verwaltung der Blacklist mit einer Datenbank oder Redis.**

🎯 **Ziel:** **Eine IP soll gesperrt werden, aber sich nach einer bestimmten Zeit oder manuell wieder freischalten können.**

---

## **🔹 3. Temporäres Blacklisting mit automatischer Aufhebung (2:30 – 5:00)**

📌 **Blacklisting für 30 Minuten speichern**

```javascript
import fs from "fs";

const blacklistedIPs = {}; // Speichert gesperrte IPs mit Zeitstempel

const ipBlacklistMiddleware = (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;

  if (blacklistedIPs[clientIP]) {
    const timePassed = Date.now() - blacklistedIPs[clientIP];
    if (timePassed > 30 * 60 * 1000) {
      // 30 Minuten
      delete blacklistedIPs[clientIP]; // IP automatisch entsperren
    } else {
      return res
        .status(403)
        .json({ error: "Deine IP wurde vorübergehend blockiert." });
    }
  }

  next();
};

app.use(ipBlacklistMiddleware);
```

🎯 **Ergebnis:**  
✅ **Nach 30 Minuten wird die IP automatisch aus der Blacklist entfernt.**  
✅ **Kein manuelles Eingreifen erforderlich.**

---

## **🔹 4. Benutzer-Whitelisting mit manueller Entsperrung (5:00 – 7:30)**

📌 **Whitelist-Datei verwalten**  
➡ **Datei: `whitelist.json`**

```json
["192.168.1.200"]
```

📌 **Middleware für IP-Überprüfung mit Whitelist**

```javascript
const loadWhitelist = () => {
  return JSON.parse(fs.readFileSync("whitelist.json", "utf8"));
};

const ipMiddleware = (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const whitelistedIPs = loadWhitelist();

  if (whitelistedIPs.includes(clientIP)) {
    return next(); // Whitelisted IPs werden immer zugelassen
  }

  if (blacklistedIPs[clientIP]) {
    return res.status(403).json({ error: "Deine IP wurde blockiert." });
  }

  next();
};

app.use(ipMiddleware);
```

📌 **IP zur Whitelist hinzufügen (Admin-Route)**

```javascript
app.post("/admin/whitelist", (req, res) => {
  const newIP = req.body.ip;
  let whitelist = loadWhitelist();

  if (!whitelist.includes(newIP)) {
    whitelist.push(newIP);
    fs.writeFileSync("whitelist.json", JSON.stringify(whitelist, null, 2));
    res.send(`IP ${newIP} wurde zur Whitelist hinzugefügt.`);
  } else {
    res.send(`IP ${newIP} ist bereits freigegeben.`);
  }
});
```

🎯 **Ergebnis:**  
✅ **Admins können IPs manuell freigeben.**  
✅ **Whitelist bleibt auch nach Serverneustart bestehen.**

---

## **🔹 5. Erweiterte Lösung mit Redis für verteilte Systeme (7:30 – 9:30)**

📌 **Warum Redis?**  
✅ **Verteilt Sperren & Freigaben über mehrere Server.**  
✅ **Automatische Aufhebung nach X Minuten ohne Neustart nötig.**

📌 **Redis & `ioredis` installieren**

```bash
npm install ioredis
```

📌 **Redis-Client einrichten**

```javascript
import Redis from "ioredis";
const redisClient = new Redis();
```

📌 **IP automatisch nach 30 Minuten entsperren**

```javascript
app.post("/login", async (req, res) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const attempts = await redisClient.incr(`failed_logins:${clientIP}`);

  if (attempts === 1) {
    await redisClient.expire(`failed_logins:${clientIP}`, 1800); // 30 Minuten
  }

  if (attempts >= 5) {
    await redisClient.set(`blacklist:${clientIP}`, "blocked", "EX", 1800); // Sperre für 30 Minuten
    return res.status(403).json({ error: "Deine IP wurde gesperrt." });
  }

  res.status(401).json({ error: "Login fehlgeschlagen!" });
});
```

📌 **Middleware für automatische Whitelist nach Sperrzeit**

```javascript
const ipBlacklistMiddleware = async (req, res, next) => {
  const clientIP = req.ip || req.connection.remoteAddress;
  const isBlacklisted = await redisClient.get(`blacklist:${clientIP}`);

  if (isBlacklisted) {
    return res
      .status(403)
      .json({ error: "Zugriff verweigert! Deine IP ist noch gesperrt." });
  }

  next();
};

app.use(ipBlacklistMiddleware);
```

🎯 **Ergebnis:**  
✅ **IPs werden nach 30 Minuten automatisch freigegeben.**  
✅ **Skalierbare Lösung für APIs mit mehreren Servern.**

---

## **🔹 6. Best Practices für sicheres Whitelisting (9:30 – 11:30)**

📌 **🚀 Best Practices:**  
✅ **Temporäres Blacklisting mit automatischer Entsperrung (30 Min, 1h, 24h).**  
✅ **Manuelles Whitelisting für echte Nutzer über Admin-Route.**  
✅ **Speichere Sperren & Freigaben in Redis oder einer Datenbank.**  
✅ **Captcha oder Zwei-Faktor-Authentifizierung als Alternative zu Sperren.**

📌 **Häufige Fehler vermeiden:**  
❌ **Dauerhaftes Sperren legitimer Nutzer ohne Reaktivierungsmöglichkeit.**  
❌ **Blacklist nicht persistent speichern → Sperren werden bei Neustart gelöscht.**  
❌ **Nur IP-Sperren nutzen → VPNs oder Proxy-Nutzer können die Sperre umgehen.**

---

## **🔹 7. Fazit & Outro (11:30 – 13:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **automatisches Whitelisting nach Blacklisting** umsetzt! Damit kannst du Nutzer nach einer gewissen Zeit wieder entsperren und false positives vermeiden."_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Welche Methode nutzt du für Sperren & Freigaben? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"Express Security – So schützt du deine API mit Helmet & CORS!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch eine Integration mit Captcha oder E-Mail-Verifizierung ergänzen?** 😊
