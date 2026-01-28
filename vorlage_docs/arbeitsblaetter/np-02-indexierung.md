# NumPy – Indexierung & Slicing

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Daten aus spezifischen Positionen eines Arrays extrahieren
- 1D- und 2D-Slicing sicher anwenden
- gezielt Teilbereiche großer Datensätze auswählen
- den Unterschied zwischen Views und Copies verstehen

!!! note "Begleitendes Infoblatt"
    Lies **zuerst** das [:material-book-open-variant: Infoblatt NumPy Indexierung](../infoblaetter/numpy-indexierung.md) – dort findest du alle Syntax-Grundlagen zu Slicing, Fancy Indexing und Boolean Indexing.

---

## Einführung

In dieser Lernsituation arbeitest du mit echten NYC Yellow Taxi Trip-Daten. Du lernst, wie du gezielt auf Teile großer Datensätze zugreifst.

**Datenfelder im Datensatz:**

| Spalte | Index | Beschreibung |
|--------|-------|--------------|
| VendorID | 0 | Anbieter-ID |
| lpep_pickup_datetime | 1 | Startzeit |
| lpep_dropoff_datetime | 2 | Endzeit |
| passenger_count | 7 | Anzahl Passagiere |
| trip_distance | 8 | Strecke (Meilen) |
| fare_amount | 9 | Fahrpreis ($) |
| tip_amount | 12 | Trinkgeld ($) |
| total_amount | 16 | Gesamtbetrag ($) |

!!! abstract "Datensatz herunterladen"
    [:material-download: taxi_tripdata.csv](../assets/files/taxi_tripdata.csv){ .md-button }

---

## Aufgaben

### Aufgabe 1 – Daten laden und untersuchen

Bereite den Taxi-Datensatz für die Analyse vor.

- [ ] Importiere NumPy und lade die Datei `taxi_tripdata.csv` mit `np.genfromtxt()`.
   *Nutze `skip_header=1` für die Kopfzeile.*
- [ ] Lass dir die Shape des Arrays ausgeben – wie viele Zeilen und Spalten hat der Datensatz?
- [ ] Bestimme den Datentyp (`dtype`) und die Anzahl der Dimensionen (`ndim`).
- [ ] Prüfe, wie viele NaN-Werte das Array enthält.
   *Tipp: `np.isnan()` und `.sum()` kombinieren.*

!!! question "Reflexionsfrage"
    Warum enthält das Array NaN-Werte? Was passiert mit Text-Spalten beim Laden?

---

### Aufgabe 2 – 1D-Indexierung und Slicing

Arbeite mit der Spalte `trip_distance` (Index 8).

- [ ] Extrahiere die komplette Spalte `trip_distance` in eine eigene Variable.
- [ ] Gib die ersten 30 Einträge dieser Spalte aus.
- [ ] Gib die letzten 10 Einträge aus.
- [ ] Zeige jeden fünften Wert der ersten 100 Einträge.

!!! tip "Hilfe"
    - Spalte extrahieren: `array[:, spalten_index]`
    - Slicing: `array[start:stop:step]`
    - Negative Indizes: `-10:` für die letzten 10

---

### Aufgabe 3 – Mehrere Spalten extrahieren

Extrahiere mehrere Spalten gleichzeitig.

- [ ] Extrahiere die Spalten `fare_amount` (9) und `tip_amount` (12) zusammen in ein neues Array.
- [ ] Erstelle eine Übersicht der ersten 30 Fahrten mit den Spalten: `passenger_count` (7), `trip_distance` (8), `fare_amount` (9), `total_amount` (16).
- [ ] Gib die Übersicht formatiert auf der Konsole aus (z.B. als Tabelle mit Spaltenüberschriften).

!!! tip "Hilfe"
    Mehrere Spalten: `array[:, [index1, index2, index3]]`

---

### Aufgabe 4 – 2D-Slicing

Wähle gezielte Bereiche aus der Matrix.

- [ ] Wähle die Zeilen 1011 bis 1097 und Spalten 6 bis 9 aus. Wie groß ist das Ergebnis-Array?
- [ ] Extrahiere jede zweite Zeile des gesamten Datensatzes. Wie viele Zeilen hat das Ergebnis?
- [ ] Extrahiere die letzte Spalte des Datensatzes.
- [ ] Gib die letzten 10 Zeilen in umgekehrter Reihenfolge aus.

!!! tip "Hilfe"
    - Bereich: `array[start:stop, start:stop]`
    - Jede n-te: `array[::n]`
    - Rückwärts: `array[::-1]` oder negativer Step

---

### Aufgabe 5 – Slicing an kleiner Matrix üben

Erstelle eine kleine Test-Matrix zum Experimentieren.

- [ ] Erstelle mit `np.arange()` und `.reshape()` eine 4×5-Matrix mit den Werten 1-20.
- [ ] Extrahiere:
   - Zeile 1 (zweite Zeile)
   - Spalte 2 (dritte Spalte)
   - Den Bereich Zeilen 1-2, Spalten 2-4
   - Jede zweite Zeile und jede zweite Spalte
- [ ] Erkläre bei jeder Extraktion, was genau ausgewählt wird.

---

### Aufgabe 6 – Views vs. Copies

Ein wichtiges Konzept verstehen: Slicing erstellt Views, keine Kopien!

- [ ] Erstelle ein Array `original = np.array([1, 2, 3, 4, 5])`.
- [ ] Erstelle per Slicing einen Ausschnitt `view = original[1:4]`.
- [ ] Ändere den ersten Wert im View auf 99.
- [ ] Prüfe beide Arrays – was ist passiert? Erkläre das Verhalten.
- [ ] Wiederhole das Experiment, aber erstelle stattdessen eine echte Kopie mit `.copy()`.

!!! warning "Wichtig"
    Bei der Arbeit mit Daten solltest du dir immer bewusst sein, ob du mit einem View oder einer Kopie arbeitest!

---

## Vertiefende Aufgaben

!!! tip "Optionale Aufgaben zur Vertiefung"
    Die folgenden Aufgaben sind **optional** und vertiefen das Gelernte. Sie eignen sich besonders für:
    
    - **Komplexere Datenextraktion** aus dem Taxi-Datensatz
    - **Transponieren und fortgeschrittenes Reshaping**
    - **Eigenständige Analyse ohne Hilfestellung**
    - **Prüfungsvorbereitung** durch eigenständiges Arbeiten

---

### Aufgabe 7 – Praktische Analysen

Wende dein Wissen auf die Taxi-Daten an.

- [ ] Analysiere die ersten 100 Fahrten:
   - Berechne die durchschnittliche Fahrstrecke
   - Berechne den durchschnittlichen Fahrpreis
   *Tipp: Nutze `np.nanmean()` wegen der NaN-Werte.*
- [ ] Vergleiche die ersten 500 mit den letzten 500 Fahrten:
   - Durchschnittliche Strecke
   - Durchschnittlicher Gesamtbetrag
   - Gibt es Unterschiede?
- [ ] Erstelle eine Stichprobe (jede 10. Fahrt) und berechne die durchschnittliche Strecke.
   Vergleiche mit dem Durchschnitt aller Fahrten.

---

### Aufgabe 8 – Bonus: Transponieren und Umformen

- [ ] Wähle 5 Fahrten und 4 Merkmale (z.B. passenger_count, trip_distance, fare_amount, total_amount) aus.
- [ ] Transponiere diesen Ausschnitt mit `.T`.
- [ ] Erkläre, was beim Transponieren passiert und wann das nützlich ist.
- [ ] Probiere `.flatten()` auf dem Ausschnitt – was passiert?

---

### Aufgabe 9 – Eigenständige Analyseaufgaben

!!! warning "Ohne Hilfe lösen"
    Bearbeite diese Aufgaben komplett selbstständig mit den Taxi-Daten.

**Aufgabe A: Datenpartitionierung**

- [ ] Teile den Datensatz in 3 gleich große Teile (erstes Drittel, Mitte, letztes Drittel)
- [ ] Berechne den Durchschnitt der `trip_distance` für jeden Teil
- [ ] Gibt es Unterschiede? Was könnte der Grund sein?

**Aufgabe B: Stichprobenanalyse**

- [ ] Erstelle 5 verschiedene Stichproben: jede 10., 20., 50., 100. Zeile
- [ ] Vergleiche den Durchschnitt der `total_amount` Spalte jeder Stichprobe mit dem Gesamtdurchschnitt
- [ ] Welche Stichprobengröße ist repräsentativ genug?

**Aufgabe C: Datenextraktion und Neukombination**

!!! tip "Nützlich für diese Aufgabe"
    Bei Division können `inf` (Infinity) Werte entstehen. 
    Prüfe mit `np.isinf(arr)` und kombiniere mit `np.isnan(arr)`:
    ```python
    gueltig = ~np.isnan(arr) & ~np.isinf(arr)
    arr_clean = arr[gueltig]
    ```

- [ ] Extrahiere die Spalten `passenger_count`, `trip_distance`, `fare_amount` und `tip_amount`
- [ ] Berechne eine neue Spalte: Trinkgeld pro Meile (tip_amount / trip_distance)
- [ ] Achtung: Wie gehst du mit Division durch 0 um?

**Aufgabe D: View-Problem lösen**

- [ ] Erstelle einen View auf die ersten 100 Zeilen
- [ ] Ändere den ersten Wert im View auf 999
- [ ] Prüfe, ob sich das Original geändert hat
- [ ] Wie verhinderst du solche unbeabsichtigten Änderungen in der Praxis?

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **1D-Slicing**: `arr[start:stop:step]`
    - **2D-Slicing**: `arr[zeilen, spalten]`
    - **Negative Indizes**: `-1` für letztes Element
    - **Mehrere Spalten**: `arr[:, [0, 2, 4]]`
    - **Views vs. Copies**: Slicing erstellt Views, `.copy()` für echte Kopie
    - **Transponieren**: `.T` oder `np.transpose()`

---

??? question "Selbstkontrolle"
    1. Was gibt `daten[5:10, 2:5]` zurück?
    2. Wie extrahierst du die letzte Zeile einer Matrix?
    3. Was ist der Unterschied zwischen `arr[1:4]` und `arr[1:4].copy()`?
    4. Wie würdest du jede dritte Zeile und jede zweite Spalte auswählen?
    
    ??? success "Antworten"
        1. Zeilen 5-9 (5 Zeilen), Spalten 2-4 (3 Spalten) → 5×3 Matrix
        2. `matrix[-1]` oder `matrix[-1, :]`
        3. Das erste ist ein View (Änderungen wirken auf Original), das zweite eine unabhängige Kopie
        4. `arr[::3, ::2]`