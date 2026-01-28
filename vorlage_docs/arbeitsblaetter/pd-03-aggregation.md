# Pandas – Aggregation & Gruppierung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Daten mit `groupby()` gruppieren und aggregieren
- verschiedene Aggregatfunktionen anwenden
- mehrere Aggregationen gleichzeitig ausführen
- Pivot-Tabellen für Kreuztabellen erstellen

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Aggregation](../infoblaetter/pandas-aggregation.md) – groupby, agg, pivot_table
    - [:material-book-open-variant: Pandas Datenzugriff](../infoblaetter/pandas-datenzugriff.md)
    
    Lies das Infoblatt **zuerst**, bevor du die Aufgaben bearbeitest. Dort findest du alle Syntax-Beispiele und Erklärungen zu Aggregationsfunktionen.

---

## Einführung

Gruppieren und Aggregieren folgt dem **Split-Apply-Combine** Paradigma:

1. **Split** – Teile Daten nach Gruppen auf
2. **Apply** – Wende eine Funktion auf jede Gruppe an
3. **Combine** – Füge die Ergebnisse zusammen

**Bearbeite alle Aufgaben in einem Jupyter Notebook.**

**Spaltenübersicht für den MBA-Datensatz:**

| Spalte | Beschreibung |
|--------|--------------|
| Application_ID | Eindeutige Bewerber-ID |
| Gender | Geschlecht (Male/Female) |
| International | Internationaler Student (True/False) |
| GPA | Notendurchschnitt (0-4) |
| Major | Studienfach |
| Work_Experience | Berufserfahrung in Jahren |
| Decision | Entscheidung (Admit/Deny/Waitlist) |

!!! abstract "Datensatz herunterladen"
    [:material-download: mba_decisions.csv](../assets/files/mba_decisions.csv){ .md-button }

---

## Aufgaben

### Aufgabe 1 – Datensatz laden und verstehen

Lade den MBA-Datensatz und verschaffe dir einen Überblick.

- [ ] Lade die Datei `mba_decisions.csv` mit Pandas
- [ ] Gib die Shape und die Spaltennamen aus
- [ ] Zeige die ersten Zeilen des DataFrames an
- [ ] Analysiere die kategorischen Spalten (`Gender`, `International`, `Major`, `Decision`) mit `value_counts()`

!!! tip "Hilfe"
    - Datei laden: `pd.read_csv('pfad/datei.csv')`
    - Shape anzeigen: `df.shape`
    - Spaltennamen: `df.columns.tolist()`
    - Häufigkeiten: `df['spalte'].value_counts()`

---

### Aufgabe 2 – Grundlagen von groupby()

Verstehe die Grundlagen der Gruppierung mit `groupby()`.

- [ ] Gruppiere den DataFrame nach `Decision` und speichere das GroupBy-Objekt
- [ ] Untersuche das GroupBy-Objekt: Gib Typ, Anzahl der Gruppen und die Gruppen-Keys aus
- [ ] Berechne die **Durchschnittswerte** aller numerischen Spalten pro Gruppe
- [ ] Berechne den **Durchschnitts-GPA** und die **durchschnittliche Work_Experience** separat pro `Decision`

!!! tip "Hilfe"
    - Gruppieren: `df.groupby('spalte')`
    - Anzahl Gruppen: `grouped.ngroups`
    - Gruppen-Keys: `grouped.groups.keys()`
    - Mittelwert aller Spalten: `grouped.mean(numeric_only=True)`
    - Einzelne Spalte aggregieren: `df.groupby('spalte')['wert_spalte'].mean()`

---

### Aufgabe 3 – Verschiedene Aggregatfunktionen

Wende verschiedene Aggregatfunktionen auf gruppierte Daten an.

- [ ] Berechne für den GPA pro `Decision`: **Anzahl, Mittelwert, Standardabweichung, Minimum, Maximum und Median**
- [ ] Berechne die **Summe der Work_Experience** pro Gruppe
- [ ] Ermittle die **Anzahl der Bewerber** pro Gruppe mit `size()`
- [ ] Berechne die **Quartile** (25%, 50%, 75%) des GPA pro Decision

!!! tip "Hilfe"
    - Mehrere Funktionen: `df.groupby('spalte')['wert'].agg(['count', 'mean', 'std', 'min', 'max', 'median'])`
    - Summe: `grouped['spalte'].sum()`
    - Gruppengröße: `grouped.size()`
    - Quantile/Perzentile: `grouped['spalte'].quantile([0.25, 0.5, 0.75])`

---

### Aufgabe 4 – Mehrere Spalten gruppieren

Gruppiere nach mehreren Spalten gleichzeitig.

- [ ] Berechne den **Durchschnitts-GPA** gruppiert nach `Gender` UND `Decision`
- [ ] Wandle das Ergebnis mit `reset_index()` in einen normalen DataFrame um und benenne die Spalten sinnvoll
- [ ] Nutze `unstack()` für eine pivot-artige Darstellung (Gender als Zeilen, Decision als Spalten)

!!! tip "Hilfe"
    - Mehrere Gruppierungsspalten: `df.groupby(['spalte1', 'spalte2'])`
    - MultiIndex zu DataFrame: `grouped_result.reset_index()`
    - Spalten umbenennen: `df.columns = ['name1', 'name2', ...]`
    - Pivot-artige Darstellung: `series.unstack()`

---

### Aufgabe 5 – agg() mit mehreren Funktionen

Berechne verschiedene Kennzahlen in einem Schritt.

- [ ] Berechne für den GPA pro Decision: Mittelwert, Standardabweichung und Anzahl mit `agg()`
- [ ] Verwende die **Dictionary-Syntax**, um für GPA Mittelwert und Std zu berechnen, für Work_Experience Mittelwert und Maximum, und für Application_ID die Anzahl
- [ ] Nutze **Named Aggregations** für übersichtliche Spaltennamen (z.B. `avg_gpa`, `std_gpa`, `max_exp`)

!!! tip "Hilfe"
    - Mehrere Funktionen auf eine Spalte: `df.groupby('spalte')['wert'].agg(['mean', 'std', 'count'])`
    - Dictionary-Syntax: `df.groupby('spalte').agg({'sp1': ['mean', 'std'], 'sp2': 'sum'})`
    - Named Aggregations: `df.groupby('spalte').agg(neuer_name=('spalte', 'funktion'), ...)`

---

### Aufgabe 6 – Eigene Aggregatfunktionen

Erstelle und verwende eigene Aggregatfunktionen.

- [ ] Berechne die **GPA-Spannweite** (Maximum - Minimum) pro Decision mit einer Lambda-Funktion
- [ ] Berechne den **Anteil der Bewerber mit GPA > 3.5** pro Decision (in Prozent)
- [ ] Schreibe eine eigene Funktion `iqr(series)`, die den Interquartilsabstand (Q3 - Q1) berechnet
- [ ] Schreibe eine Funktion `coeff_of_variation(series)`, die den Variationskoeffizienten (std/mean * 100) berechnet
- [ ] Wende beide Funktionen auf den GPA pro Decision an

!!! tip "Hilfe"
    - Lambda für Spannweite: `lambda x: x.max() - x.min()`
    - Anteil berechnen: `lambda x: (x > schwelle).mean() * 100`
    - Quantile in Funktion: `series.quantile(0.75) - series.quantile(0.25)`
    - Eigene Funktionen mit agg: `grouped.agg([funktion1, funktion2])`

!!! question "Eigenständige Aggregationsübungen"
    Löse ohne Musterlösung:
    
    1. Berechne für jede Kombination von `Gender` und `International` den Durchschnitts-GPA und die Anzahl
    2. Finde den Major mit der höchsten durchschnittlichen Work_Experience
    3. Erstelle eine Aggregation, die für jede Decision zeigt: min, max, mean und Spannweite (max-min) des GPA
    4. Berechne den Anteil der Aufnahmen (Decision=='Admit') pro Gender - nutze eine Lambda-Funktion
    5. Gruppiere nach Gender und berechne: niedrigster GPA eines Aufgenommenen, höchster GPA eines Abgelehnten

---

### Aufgabe 7 – Pivot-Tabellen

Pivot-Tabellen erstellen Kreuztabellen mit Aggregation.

- [ ] Erstelle eine Pivot-Tabelle mit dem **durchschnittlichen GPA** nach Gender (Zeilen) und Decision (Spalten)
- [ ] Erweitere die Tabelle um eine **Gesamtzeile und -spalte** mit dem Parameter `margins=True`
- [ ] Erstelle eine Pivot-Tabelle mit **mehreren Aggregatfunktionen** (mean und count) für den GPA
- [ ] Erstelle eine Pivot-Tabelle mit **mehreren Werte-Spalten** (GPA und Work_Experience)

!!! tip "Hilfe"
    - Grundstruktur: `pd.pivot_table(df, values='wert', index='zeilen', columns='spalten', aggfunc='mean')`
    - Gesamtsummen: `margins=True, margins_name='Gesamt'`
    - Mehrere Funktionen: `aggfunc=['mean', 'count']`
    - Mehrere Werte: `values=['spalte1', 'spalte2']`

---

## Vertiefende Aufgaben

!!! tip "Optionale Aufgaben zur Vertiefung"
    Die folgenden Aufgaben sind **optional** und vertiefen das Gelernte. Sie eignen sich besonders für:
    
    - **Crosstab für Häufigkeitstabellen und Kreuztabellen**
    - **Fortgeschrittene Pivot-Operationen**
    - **Prüfungsvorbereitung** durch eigenständiges Arbeiten

---

### Aufgabe 8 – Crosstab für Häufigkeiten

`pd.crosstab()` ist spezialisiert auf Häufigkeitstabellen.

- [ ] Erstelle eine **Häufigkeitstabelle** mit der Anzahl der Bewerber nach Gender und Decision
- [ ] Berechne die **Zeilen-Prozente** (Anteil pro Gender) – nutze `normalize='index'`
- [ ] Berechne die **Spalten-Prozente** (Anteil pro Decision) – nutze `normalize='columns'`
- [ ] Füge der Tabelle **Summenzeilen und -spalten** hinzu

!!! tip "Hilfe"
    - Grundstruktur: `pd.crosstab(df['zeilen_spalte'], df['spalten_spalte'])`
    - Zeilen-Prozente: `normalize='index'` (multipliziere mit 100 für %)
    - Spalten-Prozente: `normalize='columns'`
    - Summen: `margins=True, margins_name='Summe'`

---

### Aufgabe 9 – Praktische Analysen

Führe komplexere praktische Analysen durch.

- [ ] Erstelle eine **komplette Aufnahmestatistik** pro Decision mit: Anzahl, GPA-Statistiken (mean, median, min, max) und durchschnittlicher Experience
- [ ] Berechne die **Gesamt-Aufnahmequote** (Anteil Admit an allen Bewerbern)
- [ ] Berechne die **Aufnahmequote pro Gender** und **pro International-Status**
- [ ] Teile den GPA in **Kategorien** ein (<3.0, 3.0-3.3, 3.3-3.6, 3.6-3.8, 3.8-4.0) mit `pd.cut()` und berechne die Aufnahmequote pro Kategorie
- [ ] Erstelle eine **Interaktions-Pivot-Tabelle**: Aufnahmequote nach GPA-Kategorie und Gender

!!! tip "Hilfe"
    - Named Aggregation: `df.groupby('spalte').agg(name=('spalte', 'funktion'), ...)`
    - Aufnahmequote: `(df['Decision'] == 'Admit').mean() * 100`
    - Kategorien erstellen: `pd.cut(df['spalte'], bins=[...], labels=[...])`
    - Lambda in Pivot: `aggfunc=lambda x: (x == 'Wert').mean() * 100`

---

### Aufgabe 10 – Komplexe Analyseaufgaben

!!! warning "Ohne Musterlösung"
    Bearbeite diese Aufgaben selbstständig.

**Aufgabe A: Vollständige Statistik-Tabelle**

- [ ] Erstelle eine Übersichtstabelle, die für jede `Decision` folgende Werte zeigt:
    - Anzahl Bewerber
    - Durchschnitt, Median, Std von GPA
    - Durchschnitt, Median, Std von Work_Experience
    - Anteil weiblicher Bewerber (%)
    - Anteil internationaler Bewerber (%)

**Aufgabe B: Multi-Level-Gruppierung**

- [ ] Erstelle eine hierarchische Analyse:
    - Gruppiere nach `Gender`, dann nach `International`, dann nach `Decision`
    - Zeige für jede Kombination: Anzahl und durchschnittlichen GPA
    - Welche Kombination hat die höchste Aufnahmequote?

**Aufgabe C: Pivot-Tabellen-Herausforderungen**

Erstelle folgende Pivot-Tabellen:

- [ ] Aufnahmequote (% Admit) nach Gender (Zeilen) und International (Spalten)
- [ ] Durchschnitts-GPA nach Major (Zeilen) und Decision (Spalten)
- [ ] Work_Experience-Statistiken (mean, min, max) nach Gender und Decision

**Aufgabe D: Eigene Aggregationsfunktionen**

Schreibe eigene Funktionen für:

- [ ] `above_threshold(series, threshold)`: Anteil der Werte über einem Schwellenwert
- [ ] `outlier_count(series)`: Anzahl der Ausreißer (außerhalb 1.5*IQR)
- [ ] `top_n_mean(series, n=3)`: Durchschnitt der Top-n Werte
- [ ] Wende diese auf die GPA-Werte pro Decision an.

**Aufgabe E: Dashboard-Daten vorbereiten**

- [ ] Erstelle alle Daten für ein "MBA Admission Dashboard":
    1. Gesamtstatistiken (Anzahl, Quoten)
    2. Vergleich nach Demographics (Gender, International)
    3. Vergleich nach Qualifikation (GPA-Bins, Experience-Bins)
    4. Trend-Daten: Aufnahmequote pro GPA-Dezil
- [ ] Speichere jede Tabelle in einer eigenen Variable.

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **groupby()**: Split-Apply-Combine für Gruppierungen
    - **Aggregatfunktionen**: mean, sum, count, std, min, max, median
    - **agg()**: Mehrere Funktionen auf einmal, pro Spalte unterschiedlich
    - **Named Aggregation**: Übersichtliche Spaltenbenennung
    - **pivot_table()**: Kreuztabellen mit Aggregation
    - **crosstab()**: Speziell für Häufigkeitstabellen

---

??? question "Selbstkontrolle"
    1. Was macht `df.groupby('A')['B'].mean()`?
    2. Wie aggregierst du verschiedene Funktionen auf verschiedene Spalten?
    3. Was ist der Unterschied zwischen `pivot_table` und `crosstab`?
    4. Wie bekommst du die Anzahl pro Gruppe?
    
    ??? success "Antworten"
        1. Gruppiert nach Spalte A und berechnet den Mittelwert von B pro Gruppe
        2. Mit Dictionary: `.agg({'spalte1': 'mean', 'spalte2': ['sum', 'count']})`
        3. `pivot_table` aggregiert beliebige Werte, `crosstab` ist für Häufigkeiten optimiert
        4. `.groupby('A').size()` oder `.groupby('A')['B'].count()` oder `.value_counts()`
