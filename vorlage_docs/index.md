# Datenanalyse mit NumPy & Pandas

Willkommen zur Lernsituation **Datenanalyse mit NumPy und Pandas**!

Diese Lernmaterialien richten sich an angehende **Fachinformatiker/innen für Daten- und Prozessanalyse** und vermitteln die grundlegenden Techniken der Datenanalyse mit Python.

---

## 📚 Struktur der Materialien

### Infoblätter (Nachschlagewerke)

Die Infoblätter dienen als **Referenz** und erklären Konzepte und Syntax.

=== "NumPy"
    | Infoblatt | Thema |
    |-----------|-------|
    | [NumPy Grundlagen](infoblaetter/numpy-grundlagen.md) | Arrays, Datentypen, Erstellung |
    | [NumPy Indexierung](infoblaetter/numpy-indexierung.md) | Slicing, Fancy Indexing |
    | [NumPy Funktionen](infoblaetter/numpy-funktionen.md) | Statistische Funktionen |
    | [NumPy Broadcasting](infoblaetter/numpy-broadcasting.md) | Vektorisierte Berechnungen |

=== "Pandas"
    | Infoblatt | Thema |
    |-----------|-------|
    | [Pandas Grundlagen](infoblaetter/pandas-grundlagen.md) | DataFrame & Series |
    | [Pandas Datenzugriff](infoblaetter/pandas-datenzugriff.md) | loc, iloc, Boolean Indexing |
    | [Pandas Aggregation](infoblaetter/pandas-aggregation.md) | groupby, agg, pivot_table |
    | [Pandas Transformation](infoblaetter/pandas-transformation.md) | map, apply, neue Spalten |
    | [Datenbereinigung](infoblaetter/datenbereinigung.md) | NaN, Duplikate, Ausreißer |

---

### Arbeitsblätter (Übungen)

Die Arbeitsblätter enthalten **praktische Aufgaben** mit steigendem Schwierigkeitsgrad.

=== "NumPy Arbeitsblätter"
    | Nr. | Arbeitsblatt | Thema | Datensatz |
    |-----|-------------|-------|-----------|
    | NP-01 | [Einführung](arbeitsblaetter/np-01-einfuehrung.md) | Arrays, Shapes, Datentypen | – |
    | NP-02 | [Indexierung](arbeitsblaetter/np-02-indexierung.md) | Slicing, Fancy Indexing | Taxi-Daten |
    | NP-03 | [Statistik](arbeitsblaetter/np-03-statistik.md) | Aggregation, Funktionen | Taxi-Daten |
    | NP-04 | [Filtern](arbeitsblaetter/np-04-filtern.md) | Boolean Indexing, Vektorisierung | Taxi-Daten |
    | NP-05 | [Fallstudie](arbeitsblaetter/np-05-fallstudie.md) | Komplette Analyse | Studentendaten |

=== "Pandas Arbeitsblätter"
    | Nr. | Arbeitsblatt | Thema | Datensatz |
    |-----|-------------|-------|-----------|
    | PD-01 | [Einführung](arbeitsblaetter/pd-01-einfuehrung.md) | DataFrames, CSV, Exploration | Games |
    | PD-02 | [Datenzugriff](arbeitsblaetter/pd-02-datenzugriff.md) | loc, iloc, Boolean Indexing | MBA |
    | PD-03 | [Aggregation](arbeitsblaetter/pd-03-aggregation.md) | groupby, agg, pivot_table | MBA |
    | PD-04 | [Verbinden](arbeitsblaetter/pd-04-verbinden.md) | merge, concat, Joins | – |
    | PD-05 | [Transformation](arbeitsblaetter/pd-05-transformation.md) | map, apply, Bereinigung | MBA |
    | PD-06 | [Fallstudie](arbeitsblaetter/pd-06-fallstudie.md) | Komplette Analyse | Shark Attacks |

---

## 🎯 Lernziele

Nach Bearbeitung der Materialien kannst du:

!!! success "NumPy"
    - [x] NumPy-Arrays erstellen und manipulieren
    - [x] Mehrdimensionale Arrays indexieren und slicen
    - [x] Statistische Berechnungen durchführen
    - [x] Boolean Indexing für Filterung anwenden
    - [x] Vektorisierte Berechnungen statt Schleifen nutzen

!!! success "Pandas"
    - [x] DataFrames aus CSV-Dateien laden
    - [x] Daten mit loc, iloc und Boolean Indexing auswählen
    - [x] Daten gruppieren und aggregieren
    - [x] Transformationen und Bereinigungen durchführen
    - [x] Pivot-Tabellen für Kreuztabellen erstellen

---

## 📁 Datensätze

Die folgenden Datensätze werden in den Übungen verwendet:

| Datensatz | Beschreibung | Verwendet in |
|-----------|--------------|--------------|
| `taxi_tripdata.csv` | NYC Taxi-Fahrten | NumPy NP-02 bis NP-04 |
| `student-mat.csv` | Schülerleistungen | NumPy NP-05 |
| `games.csv` | Videospiel-Daten | Pandas PD-01 |
| `mba_decisions.csv` | MBA-Bewerbungen | Pandas PD-02 bis PD-04 |
| `global_shark_attacks.csv` | Hai-Angriffe weltweit | Pandas PD-05 |

---

## 🚀 Empfohlener Lernpfad

1. NumPy Grundlagen (NP-01 bis NP-03)
2. NumPy Vertiefung (NP-04, NP-05)
3. Pandas Grundlagen (PD-01 bis PD-04)
4. Pandas Vertiefung (PD-05, PD-06)

---

## 💡 Tipps für erfolgreiches Lernen

!!! tip "Praktische Tipps"
    1. **Code selbst schreiben** – Nicht nur lesen, sondern aktiv tippen!
    2. **Experimentieren** – Ändere Werte und beobachte, was passiert
    3. **Fehler machen** – Aus Fehlermeldungen lernt man am meisten
    4. **Infoblätter nutzen** – Sie sind dein Nachschlagewerk
    5. **Fragen stellen** – Bei Unklarheiten nachfragen

---

## 🔧 Voraussetzungen

Für die Bearbeitung benötigst du:

- [ ] Python 3.8 oder höher
- [ ] NumPy (`pip install numpy`)
- [ ] Pandas (`pip install pandas`)
- [ ] Jupyter Notebook oder VS Code mit Python-Extension

```python
# Installation prüfen
import numpy as np
import pandas as pd

print(f"NumPy Version: {np.__version__}")
print(f"Pandas Version: {pd.__version__}")
```

---

Viel Erfolg bei der Bearbeitung! 🎓
