### **📜 Videoskript: SQL vs. NoSQL – Welches Datenbanksystem solltest du nutzen?**

🎬 **Titel:** SQL vs. NoSQL – Welches Datenbanksystem ist das richtige für dein Projekt?  
🎤 **Dauer:** ca. 12–15 Minuten  
👨‍🏫 **Zielgruppe:** Anfänger & Fortgeschrittene Entwickler  
🎯 **Lernziel:** **Verstehen, wie sich SQL- und NoSQL-Datenbanken unterscheiden, welche Vor- und Nachteile sie haben und wann du welche Technologie nutzen solltest.**

---

## **🔹 1. Intro (0:00 – 1:00)**

**🎥 Szene:** Sprecher vor dem Bildschirm mit Diagrammen im Hintergrund.  
🗣️ **Sprecher:**  
_"SQL oder NoSQL? Relationale oder dokumentenbasierte Datenbanken? Diese Frage stellt sich jeder Entwickler früher oder später. In diesem Video erkläre ich dir die **Unterschiede zwischen SQL und NoSQL**, wann du welches System nutzen solltest und welche Vor- und Nachteile es gibt!"_

📌 **Agenda:**  
✅ Was sind SQL- und NoSQL-Datenbanken?  
✅ Wie unterscheiden sie sich in Struktur & Skalierbarkeit?  
✅ Vor- & Nachteile beider Systeme  
✅ Wann sollte man SQL oder NoSQL wählen?  
✅ Best Practices für deine Entscheidung

---

## **🔹 2. Was sind SQL- & NoSQL-Datenbanken? (1:00 – 2:30)**

📌 **SQL-Datenbanken** _(Relationale Datenbanken)_

- **Strukturierte Daten** – Tabellen mit Spalten & Zeilen
- **Feste Schemata** – Jede Tabelle hat vordefinierte Felder
- **Nutzen SQL (Structured Query Language)** für Abfragen
- **Beispiele:** MySQL, PostgreSQL, Microsoft SQL Server

📌 **NoSQL-Datenbanken** _(Nicht-relational, dokumentenbasiert, key-value, etc.)_

- **Flexible Datenstrukturen** – JSON, Key-Value, Graph-Datenbanken
- **Dynamische Schemata** – Keine festen Tabellenstrukturen
- **Horizontal skalierbar** – Ideal für große Datenmengen & verteilte Systeme
- **Beispiele:** MongoDB, Redis, Cassandra, Firebase

🎯 **Zusammenfassung:**  
✅ **SQL = Strukturiert & organisiert**  
✅ **NoSQL = Flexibel & skalierbar**

---

## **🔹 3. Strukturunterschiede zwischen SQL & NoSQL (2:30 – 5:00)**

📌 **SQL – Tabellenbasiert (Relationale Datenbank)**  
➡ **Beispiel: MySQL-Tabelle für Benutzer**

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
```

- **Feste Spaltenstruktur** (Name, E-Mail sind erforderlich)
- **Relationale Verknüpfungen möglich** (JOINs mit anderen Tabellen)

📌 **NoSQL – Dokumentenbasiert (MongoDB)**  
➡ **Beispiel: MongoDB-Dokument für Benutzer**

```json
{
  "_id": "123abc",
  "name": "Max",
  "email": "max@example.com",
  "hobbies": ["Gaming", "Kochen"]
}
```

- **Flexible Struktur:** Jedes Dokument kann andere Felder haben
- **Keine Tabellen, keine JOINs – Daten sind oft in einem Dokument gespeichert**

🎯 **Fazit:**  
✅ **SQL ist gut für komplexe Abfragen & Beziehungen.**  
✅ **NoSQL ist ideal für flexible, sich ändernde Datenstrukturen.**

---

## **🔹 4. Skalierbarkeit & Performance (5:00 – 7:30)**

📌 **SQL (Vertikale Skalierung, ACID-konform)**  
✅ Starke Konsistenz durch **ACID-Prinzip** (Atomicity, Consistency, Isolation, Durability)  
✅ Vertikale Skalierung (**Mehr CPU & RAM**) nötig, um Leistung zu steigern  
❌ **Begrenzte Skalierbarkeit**, teuer bei großen Datenmengen

📌 **NoSQL (Horizontale Skalierung, BASE-Prinzip)**  
✅ **BASE (Basically Available, Soft state, Eventually consistent)** für schnelle Performance  
✅ **Horizontal skalierbar** – Kann auf mehrere Server verteilt werden  
✅ **Besser für Big Data & Echtzeitanwendungen**  
❌ **Keine garantierte Konsistenz (z. B. Eventual Consistency in MongoDB)**

🎯 **Zusammenfassung:**  
✅ **SQL = Stark in Konsistenz & strukturierten Daten**  
✅ **NoSQL = Besser für Skalierung & dynamische Workloads**

---

## **🔹 5. Vor- & Nachteile von SQL & NoSQL (7:30 – 9:30)**

| **Merkmal**         | **SQL-Datenbanken**                    | **NoSQL-Datenbanken**                     |
| ------------------- | -------------------------------------- | ----------------------------------------- |
| **Struktur**        | Tabellenbasiert, stark typisiert       | Dokumenten- / Key-Value-basiert, flexibel |
| **Flexibilität**    | Feste Schemata                         | Kein festes Schema                        |
| **Skalierung**      | Vertikal (mehr Leistung pro Server)    | Horizontal (mehr Server hinzufügen)       |
| **Abfragen**        | Komplexe Abfragen mit SQL & JOINs      | Einfachere Abfragen mit NoSQL-Methoden    |
| **Konsistenz**      | ACID-konform (hohe Datenintegrität)    | BASE (Eventual Consistency)               |
| **Anwendungsfälle** | Banking, ERP, CRM, strukturierte Daten | Social Media, Big Data, Echtzeit-Apps     |

🎯 **Fazit:**  
✅ **Nutze SQL, wenn Konsistenz & relationale Daten wichtig sind**.  
✅ **Nutze NoSQL, wenn Skalierbarkeit & Flexibilität Priorität haben**.

---

## **🔹 6. Wann sollte man SQL oder NoSQL nutzen? (9:30 – 11:30)**

📌 **SQL ist besser für:**  
✅ **E-Commerce-Systeme** (Produkte, Bestellungen, Kundenverwaltung)  
✅ **Banking & Finanzsektor** (Transaktionen mit hoher Konsistenz)  
✅ **Unternehmenssoftware (ERP, CRM)**

📌 **NoSQL ist besser für:**  
✅ **Social Media & Echtzeit-Daten** (z. B. Chat-Apps mit Firebase)  
✅ **Big Data & Analyse-Plattformen** (z. B. Empfehlungssysteme)  
✅ **Content Management Systeme (CMS)** (z. B. flexible Artikelstruktur)

🎯 **Zusammenfassung:**  
**→ Brauchst du starre, relationale Strukturen? Nutze SQL.**  
**→ Brauchst du flexible, skalierbare Strukturen? Nutze NoSQL.**

---

## **🔹 7. Best Practices für die Wahl der richtigen Datenbank (11:30 – 12:30)**

📌 **🚀 Best Practices:**  
✅ **Nutze SQL, wenn du viele Abfragen & Beziehungen benötigst.**  
✅ **Nutze NoSQL, wenn du große, verteilte Systeme baust.**  
✅ **Kombiniere SQL & NoSQL für Hybrid-Lösungen (z. B. MySQL für Bestellungen, Redis für Sessions).**

📌 **Häufige Fehler vermeiden:**  
❌ **SQL verwenden, obwohl sich das Schema oft ändert** (besser NoSQL).  
❌ **NoSQL verwenden, wenn relationale JOINs nötig sind** (besser SQL).  
❌ **Kein Indexing in NoSQL nutzen → Performance-Probleme!**

---

## **🔹 8. Fazit & Outro (12:30 – 14:00)**

🗣️ **Sprecher:**  
_"Heute hast du gelernt, was der Unterschied zwischen **SQL & NoSQL** ist, wann du welche Datenbank nutzen solltest und welche Vorteile jede Technologie hat. Wähle weise, denn die richtige Datenbank kann dein Projekt entscheidend beeinflussen!"_

🎬 **Outro mit Call-to-Action:**  
✅ **Frage an die Zuschauer:** "SQL oder NoSQL – was nutzt du in deinen Projekten? Schreib’s in die Kommentare!"  
✅ **Nächstes Video:** _"MongoDB vs. MySQL – Praxisvergleich der beiden Datenbanktechnologien!"_

---

🎯 **Fertig!** 🎯  
✅ **Soll ich noch ein Beispiel für eine Hybrid-Lösung (SQL + NoSQL) ergänzen?** 😊
