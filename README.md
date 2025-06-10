
# 📘 MariaDB-Dokumentation: Schulprojekt

## 1. Einleitung

MariaDB ist ein weit verbreitetes, freies relationales Datenbanksystem. Es basiert auf MySQL, wurde aber vollständig Open Source weiterentwickelt. Im schulischen Kontext setzen wir MariaDB ein, um Daten effizient zu speichern, zu verwalten und über SQL gezielt abzufragen.

## 2. Grundbegriffe

| Begriff | Erklärung |
|--------|-----------|
| **Tabelle** | Strukturierte Sammlung von Daten in Zeilen und Spalten |
| **Spalte (Attribut)** | Eigenschaft oder Merkmal eines Datensatzes |
| **Zeile (Datensatz)** | Eintrag in einer Tabelle |
| **Primärschlüssel (Primary Key)** | Eindeutige Identifikation einer Zeile |
| **Fremdschlüssel (Foreign Key)** | Verweis auf einen Schlüssel in einer anderen Tabelle |
| **Relation** | Beziehung zwischen zwei Tabellen |

## 3. Datenbankmodellierung

### 3.1 ER-Diagramm

Bevor man eine Datenbank erstellt, plant man mit einem **Entity-Relationship-Modell (ERM)**:

- **Entität**: z. B. Schüler:in, Fach, Lehrer:in
- **Beziehung**: z. B. „Schüler:in bekommt Note in Fach“
- **Attribute**: z. B. Vorname, Nachname, Geburtsdatum

### 3.2 Normalisierung

Durch **Normalisierung** vermeidet man doppelte Daten. Die wichtigsten Normalformen:

1. **1NF**: Nur atomare Werte (keine Listen)
2. **2NF**: Jedes Nicht-Schlüsselattribut hängt vollständig vom Schlüssel ab
3. **3NF**: Keine transitive Abhängigkeit zwischen Nicht-Schlüsselattributen

## 4. Installation & Tools

- **MariaDB** installieren (z. B. via XAMPP, MAMP, Docker)
- **DBeaver** als grafisches Verwaltungs-Tool verwenden
- Alternativ: **MySQL Workbench**, **HeidiSQL**, **phpMyAdmin**

## 5. Beispielprojekt: Schul-Datenbank

### 🧩 Ziel des Projekts

Wir bauen eine relationale Datenbank, mit der eine Schule ihre Daten verwalten kann. Die Datenbank enthält Informationen zu:

- Schüler:innen (`schueler`)
- Lehrpersonen (`lehrer`)
- Fächern (`faecher`)
- Noten (`noten`)
- Klassen (`klassen`)
- Unterricht (`unterricht`)
- Räume (`raeume`)
- Lehrer-Fächer-Zuordnung (`lehrer_faecher`)

### 🔗 Entity-Relationship-Modell (ER-Modell)

Ein vereinfachtes ER-Modell sieht so aus:

```
lehrer --------< lehrer_faecher >-------- faecher
    |                                    |
    |                                    |
unterricht --------< noten >-------- schueler
     |
  klasse
```

### 🧱 SQL: Datenbankstruktur

Ein Auszug aus dem SQL-Skript zur Erstellung der wichtigsten Tabellen:

```sql
CREATE TABLE klassen (
  klasse_id INT AUTO_INCREMENT PRIMARY KEY,
  bezeichnung VARCHAR(20)
);

CREATE TABLE schueler (
  schueler_id INT AUTO_INCREMENT PRIMARY KEY,
  vorname VARCHAR(50),
  nachname VARCHAR(50),
  geburtsdatum DATE,
  klasse_id INT,
  FOREIGN KEY (klasse_id) REFERENCES klassen(klasse_id)
);

CREATE TABLE faecher (
  fach_id INT AUTO_INCREMENT PRIMARY KEY,
  bezeichnung VARCHAR(50)
);

CREATE TABLE lehrer (
  lehrer_id INT AUTO_INCREMENT PRIMARY KEY,
  vorname VARCHAR(50),
  nachname VARCHAR(50)
);

CREATE TABLE lehrer_faecher (
  id INT AUTO_INCREMENT PRIMARY KEY,
  lehrer_id INT,
  fach_id INT,
  FOREIGN KEY (lehrer_id) REFERENCES lehrer(lehrer_id),
  FOREIGN KEY (fach_id) REFERENCES faecher(fach_id)
);
```

### 🗃️ Inserts (Beispieldaten)

```sql
INSERT INTO klassen (bezeichnung) VALUES ('1A'), ('1B');
INSERT INTO schueler (vorname, nachname, geburtsdatum, klasse_id)
VALUES ('Lina', 'Muster', '2007-03-04', 1);
INSERT INTO lehrer (vorname, nachname) VALUES ('Max', 'Huber');
INSERT INTO faecher (bezeichnung) VALUES ('Mathematik');
INSERT INTO lehrer_faecher (lehrer_id, fach_id) VALUES (1, 1);
```

## 6. Erweiterte SQL-Abfragen

### 📎 JOINs – Tabellen verknüpfen

Beispiel: Welche Schüler:innen haben in welchem Fach welche Note?

```sql
SELECT s.vorname, s.nachname, f.bezeichnung AS fach, n.note
FROM noten n
JOIN schueler s ON s.schueler_id = n.schueler_id
JOIN faecher f ON f.fach_id = n.fach_id;
```

### 📊 Gruppieren & Aggregieren

Beispiel: Durchschnittsnote pro Fach

```sql
SELECT f.bezeichnung, AVG(n.note) AS durchschnitt
FROM noten n
JOIN faecher f ON f.fach_id = n.fach_id
GROUP BY f.bezeichnung;
```

### 🧯 Fehlerquellen vermeiden

- `NULL`-Werte beachten bei Aggregationen
- Immer `FOREIGN KEYs` korrekt setzen
- Einfüge-Reihenfolge beachten (Eltern zuerst, Kinder danach)

## 7. Best Practices

| Best Practice                    | Erklärung |
|----------------------------------|-----------|
| Normalisierung beachten          | Redundanzen vermeiden |
| sprechende Namen verwenden       | z. B. `schueler_id` statt `id` |
| Primär- und Fremdschlüssel nutzen | für Datenintegrität |
| Transaktionen bei Änderungen     | bei komplexen Inserts/Updates |
| Regelmässige Backups             | bei echten Systemen Pflicht |

## 8. Vergleich: MariaDB vs. MySQL vs. SQLite

| Feature            | MariaDB         | MySQL            | SQLite           |
|--------------------|------------------|------------------|------------------|
| Lizenz             | Open Source      | Teilweise Open   | Open Source      |
| Skalierbarkeit     | Hoch             | Hoch             | Gering           |
| Anfängerfreundlich | Gut              | Mittel           | Sehr gut         |
| SQL-Kompatibilität | Hoch             | Hoch             | Eingeschränkt    |
| Verwendung         | Server           | Server           | Local / Mobile   |

Fazit: **MariaDB** ist für unser Projekt ideal – serverfähig, flexibel, offen und sehr MySQL-kompatibel.

## 9. Glossar

| Begriff          | Definition                                                                 |
|------------------|----------------------------------------------------------------------------|
| **RDBMS**         | Relational Database Management System                                      |
| **SQL**           | Structured Query Language – Abfragesprache für Datenbanken                |
| **PRIMARY KEY**   | Einzigartiger Identifikator für eine Zeile in einer Tabelle               |
| **FOREIGN KEY**   | Verweis auf einen Primary Key einer anderen Tabelle                        |
| **JOIN**          | SQL-Befehl zum Verknüpfen mehrerer Tabellen                               |
| **Normalisierung**| Technik zur Vermeidung von Datenredundanz und Inkonsistenz                |

## ✅ Abschluss

Mit dieser Dokumentation hast du die wichtigsten technischen Grundlagen von MariaDB kennengelernt und gelernt, wie man eine relationale Datenbank entwirft, erstellt, abfragt und erweitert. Das Schulbeispiel dient als solide Basis für eigene Projekte oder den Einstieg in komplexere Datenbankanwendungen.
