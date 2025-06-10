# 📘 Datenbankprojekt mit MariaDB

## 🧭 1. Einleitung

MariaDB ist ein relationales Datenbankmanagementsystem (RDBMS), das auf MySQL basiert und als Open-Source-Projekt weiterentwickelt wird. In der heutigen Softwareentwicklung ist der Umgang mit relationalen Datenbanken eine Kernkompetenz, da sie in nahezu jeder Anwendung zur Speicherung strukturierter Daten verwendet werden.

In dieser Dokumentation zeigen wir anhand eines konkreten Beispiels, wie man eine relationale Datenbank mit MariaDB plant, aufbaut und nutzt. Dabei lernst du:

- die Grundlagen relationaler Datenbanken,
- wie du eine eigene Datenbank mit SQL aufbaust,
- wie Tabellen miteinander in Beziehung gesetzt werden,
- wie man mit SQL-Daten abfragt, einfügt und verändert,
- Best Practices für den Umgang mit Datenbanken.

Unser Beispielprojekt basiert auf einem typischen Schulsystem mit Schüler:innen, Lehrpersonen, Fächern und Noten.

## 🧱 2. Grundlagen relationaler Datenbanken

Relationale Datenbanken speichern Daten in **Tabellen**, die miteinander in Beziehung stehen. Die wichtigsten Begriffe:

| Begriff         | Bedeutung                                                                  |
|-----------------|---------------------------------------------------------------------------|
| **Tabelle**     | Sammlung von Datensätzen mit gleicher Struktur (z. B. `schueler`, `noten`) |
| **Zeile**       | Ein einzelner Datensatz                                                   |
| **Spalte**      | Ein Datenfeld (z. B. `name`, `geburtsdatum`)                              |
| **Primary Key** | Eindeutiger Identifikator einer Zeile                                     |
| **Foreign Key** | Verknüpfung zu einem Primary Key einer anderen Tabelle                    |

Ein gutes Datenbankdesign sorgt für:

- **Konsistenz**: keine Widersprüche in den Daten,
- **Reduktionsfreiheit**: keine doppelten Daten (Normalisierung),
- **Skalierbarkeit**: auch bei vielen Datensätzen performant.

> ⚠️ **Beispiel**: Eine Tabelle `noten` sollte nicht die Namen der Schüler:innen enthalten – sondern nur deren `schueler_id` als Foreign Key.

## 💾 3. MariaDB installieren und verwenden

### 📥 Installation (lokal)

Für die Arbeit mit MariaDB benötigst du eine lokale Datenbankumgebung. Optionen:

- **Windows**: [XAMPP](https://www.apachefriends.org/index.html) oder MariaDB Installer
- **macOS**: Homebrew: `brew install mariadb`
- **Linux**: Paketmanager (z. B. `apt install mariadb-server`)

### 🖥️ Verbindung zur Datenbank

- **Command Line (CLI)**:

```bash
mariadb -u root -p
```

- **GUI-Tools**:

  - [DBeaver](https://dbeaver.io/) (empfohlen)
  - HeidiSQL
  - phpMyAdmin

## 📊 4. Erste SQL-Befehle – Datenbank erstellen

```sql
-- Datenbank erstellen
CREATE DATABASE schule_db;
USE schule_db;

-- Tabelle 'schueler' erstellen
CREATE TABLE schueler (
  schueler_id INT AUTO_INCREMENT PRIMARY KEY,
  vorname VARCHAR(50),
  nachname VARCHAR(50),
  geburtsdatum DATE,
  klasse_id INT
);
```

### Weitere wichtige SQL-Befehle:

| Befehl   | Zweck               |
|----------|---------------------|
| `INSERT` | Datensätze einfügen |
| `SELECT` | Daten abfragen      |
| `UPDATE` | Daten verändern     |
| `DELETE` | Daten löschen       |

## 📘 Fortsetzung

Wenn du willst, schreibe ich als Nächstes weiter an:

- Kapitel 5: Beispielprojekt – Schul-Datenbank (ER-Modell, SQL, Inserts)
- Kapitel 6: Komplexe Abfragen (`JOIN`, `GROUP BY` etc.)
- Kapitel 7–9: Best Practices, Vergleich, Glossar
