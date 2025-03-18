### **📜 Videoskript: MongoDB lokal installieren**

🎬 **Titel:** MongoDB lokal installieren – Schritt-für-Schritt-Anleitung  
🎤 **Dauer:** ca. 10–12 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Entwickler  
🎯 **Lernziel:** **Verstehen, wie man MongoDB auf einem lokalen Rechner installiert und konfiguriert, um mit der Entwicklung zu beginnen.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit MongoDB-Logo im Hintergrund.  
🗣️ **Sprecher:**  
_"Willkommen zu diesem Video, in dem ich dir zeige, wie du **MongoDB lokal installieren** kannst. MongoDB ist eine beliebte NoSQL-Datenbank, die sich hervorragend für die Entwicklung moderner Anwendungen eignet. Lass uns direkt loslegen!"_

📌 **Agenda:**  
✅ Warum MongoDB?  
✅ Voraussetzungen für die Installation  
✅ Schritt-für-Schritt-Installation auf Windows, macOS und Linux  
✅ Erste Schritte mit MongoDB

---

## **🔹 2. Warum MongoDB? (1:00 – 2:00)**

📌 **Vorteile von MongoDB:**  
✅ **Flexibilität:** Dokumentenbasierte Datenbank, die JSON-ähnliche Dokumente speichert  
✅ **Skalierbarkeit:** Horizontale Skalierung für große Datenmengen  
✅ **Leistungsfähigkeit:** Hohe Performance bei Lese- und Schreiboperationen  
✅ **Beliebtheit:** Weit verbreitet und gut dokumentiert

---

## **🔹 3. Voraussetzungen für die Installation (2:00 – 3:00)**

📌 **Systemanforderungen:**  
✅ **Betriebssystem:** Windows, macOS oder Linux  
✅ **Speicherplatz:** Mindestens 200 MB freier Speicherplatz  
✅ **RAM:** Mindestens 2 GB RAM empfohlen

📌 **Benötigte Software:**  
✅ **Webbrowser:** Zum Herunterladen der Installationsdateien  
✅ **Terminal oder Eingabeaufforderung:** Zum Ausführen von Befehlen

---

## **🔹 4. MongoDB auf Windows installieren (3:00 – 5:00)**

📌 **Schritte:**

1. **Download:** Gehe zur [MongoDB Download-Seite](https://www.mongodb.com/try/download/community) und lade die Windows-Version herunter.
2. **Installation:** Führe die heruntergeladene .msi-Datei aus und folge den Installationsanweisungen.
3. **Konfiguration:** Setze den Haken bei "Install MongoDB as a Service" und wähle die Standardoptionen.
4. **Überprüfung:** Öffne die Eingabeaufforderung und gib `mongo --version` ein, um die Installation zu überprüfen.

---

## **🔹 5. MongoDB auf macOS installieren (5:00 – 7:00)**

📌 **Schritte:**

1. **Homebrew installieren:** Falls noch nicht installiert, öffne das Terminal und gib `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` ein.
2. **MongoDB installieren:** Gib im Terminal `brew tap mongodb/brew` und anschließend `brew install mongodb-community` ein.
3. **Starten:** Starte MongoDB mit dem Befehl `brew services start mongodb/brew/mongodb-community`.
4. **Überprüfung:** Gib `mongo --version` im Terminal ein, um die Installation zu überprüfen.

---

## **🔹 6. MongoDB auf Linux installieren (7:00 – 9:00)**

📌 **Schritte:**

1. **Repository hinzufügen:** Öffne das Terminal und füge das MongoDB-Repository hinzu (Befehle variieren je nach Distribution, siehe [MongoDB-Dokumentation](https://docs.mongodb.com/manual/administration/install-on-linux/)).
2. **Installation:** Aktualisiere die Paketliste und installiere MongoDB mit `sudo apt-get install -y mongodb-org` (für Ubuntu/Debian).
3. **Starten:** Starte den MongoDB-Dienst mit `sudo systemctl start mongod`.
4. **Überprüfung:** Gib `mongo --version` im Terminal ein, um die Installation zu überprüfen.

---

## **🔹 7. Erste Schritte mit MongoDB (9:00 – 11:00)**

📌 **Verbindung herstellen:**  
✅ **Mongo Shell:** Öffne die Mongo Shell mit dem Befehl `mongo` im Terminal.  
✅ **Erste Datenbank erstellen:** Erstelle eine neue Datenbank mit `use meineDatenbank`.  
✅ **Dokument einfügen:** Füge ein Dokument ein mit `db.meineSammlung.insertOne({name: "Max Mustermann", alter: 29})`.  
✅ **Daten abfragen:** Frage die Daten ab mit `db.meineSammlung.find()`.

---

## **🔹 8. Fazit & Outro (11:00 – 12:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, wie du **MongoDB lokal installieren** kannst. Jetzt bist du bereit, deine eigenen Projekte mit MongoDB zu starten. Viel Erfolg!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "Hast du MongoDB erfolgreich installiert? Welche Projekte planst du damit? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB mit Mongoose – Einführung und erste Schritte!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für eine erweiterte Konfiguration ergänzen?** 😊
