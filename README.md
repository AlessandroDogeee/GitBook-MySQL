
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

### Aufgaben zu Kapitel 2: Grundbegriffe

1. Wie definierst du in SQL einen Primärschlüssel für eine Spalte?  
2. Wie setzt man in einer Tabelle einen Fremdschlüssel auf eine andere Tabelle?

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

### Aufgaben zu Kapitel 4: Installation & Tools

1. Wie kannst du in MariaDB eine Datenbank sichern (Backup)? Nenne kurz das gängige Tool oder den Befehl.

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

### Aufgaben zu Kapitel 5: Beispielprojekt
1. Wie lautet der SQL-Befehl, um eine neue Tabelle namens klassen zu erstellen?

2. Mit welchem SQL-Befehl fügst du einen neuen Schüler in die Tabelle schueler ein?

3. Nenne den SQL-Befehl, um alle Daten aus der Tabelle lehrer abzurufen.


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

### Aufgaben zu Kapitel 6: Erweiterte SQL-Abfragen
1. Wie verknüpft man zwei Tabellen in einer Abfrage mit JOIN? Nenne das Grundformat des Befehls.

2. Welcher Befehl zeigt den Durchschnittswert einer Spalte in SQL an?


## 7. Best Practices

| Best Practice                    | Erklärung |
|----------------------------------|-----------|
| Normalisierung beachten          | Redundanzen vermeiden |
| sprechende Namen verwenden       | z. B. `schueler_id` statt `id` |
| Primär- und Fremdschlüssel nutzen | für Datenintegrität |
| Transaktionen bei Änderungen     | bei komplexen Inserts/Updates |
| Regelmässige Backups             | bei echten Systemen Pflicht |

### Aufgaben zu Kapitel 7: Best Practices
1. Nenne den SQL-Befehl, um Daten in der Tabelle faecher zu ändern (Update).

2. Welcher Befehl löscht alle Datensätze aus einer Tabelle, ohne die Tabelle selbst zu löschen?

   
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

# Lösungen mit Kapitelzuordnung

Hier findest du die Lösungen zu den Aufgaben, geordnet nach Kapiteln, mit direkten und präzisen Antworten.

## Kapitel 2: Grundbegriffe

### Aufgabe 1: Primärschlüssel definieren
Definiere einen Primärschlüssel für eine Tabelle.

```sql
PRIMARY KEY (spaltenname)
```

### Aufgabe 2: Fremdschlüssel setzen
Setze einen Fremdschlüssel, der auf eine andere Tabelle verweist.

```sql
FOREIGN KEY (fremdspalte) REFERENCES andere_tabelle(spalte)
```

### Aufgabe 3: Ziel der Normalisierung
Beschreibe das Ziel der Normalisierung in Datenbanken.

**Lösung:**  
Redundanzen und Inkonsistenzen vermeiden.

### Aufgabe 4: Beispiel Entität
Nenne ein Beispiel für eine Entität in einer Schul-Datenbank.

**Lösung:**  
Schüler:in (oder Fach, Lehrer:in).

## Kapitel 4: Installation & Tools

### Aufgabe 5: Datenbank sichern mit `mysqldump`
Erstelle ein Backup einer Datenbank mit `mysqldump`.

```bash
mysqldump -u benutzername -p datenbankname > backup.sql
```

## Kapitel 5: Beispielprojekt: Schul-Datenbank

### Aufgabe 6: Tabelle `klassen` erstellen
Erstelle eine Tabelle `klassen` mit einer ID und einer Bezeichnung.

```sql
CREATE TABLE klassen (
  klasse_id INT AUTO_INCREMENT PRIMARY KEY,
  bezeichnung VARCHAR(20)
);
```

### Aufgabe 7: Neuen Schüler einfügen
Füge einen neuen Schüler in die Tabelle `schueler` ein.

```sql
INSERT INTO schueler (vorname, nachname, geburtsdatum, klasse_id)
VALUES ('Max', 'Muster', '2008-01-01', 1);
```

### Aufgabe 8: Alle Lehrer abfragen
Erstelle eine Abfrage, um alle Lehrer anzuzeigen.

```sql
SELECT * FROM lehrer;
```

## Kapitel 6: Erweiterte SQL-Abfragen

### Aufgabe 9: Tabellen verbinden mit JOIN
Verbinde zwei Tabellen mit einem JOIN.

```sql
SELECT * FROM tabelle1
JOIN tabelle2 ON tabelle1.spalte = tabelle2.spalte;
```

### Aufgabe 10: Durchschnitt berechnen
Berechne den Durchschnitt einer Spalte.

```sql
SELECT AVG(spaltenname) FROM tabelle;
```

### Aufgabe 11: Daten ändern mit UPDATE
Ändere Daten in einer Tabelle mit einer Bedingung.

```sql
UPDATE tabellenname
SET spalte = wert
WHERE bedingung;
```

### Aufgabe 12: Alle Daten löschen mit DELETE
Lösche alle Daten aus einer Tabelle.

```sql
DELETE FROM tabellenname;
```

## ✅ Abschluss

Mit dieser Dokumentation hast du die wichtigsten technischen Grundlagen von MariaDB kennengelernt und gelernt, wie man eine relationale Datenbank entwirft, erstellt, abfragt und erweitert. Das Schulbeispiel dient als solide Basis für eigene Projekte oder den Einstieg in komplexere Datenbankanwendungen.


