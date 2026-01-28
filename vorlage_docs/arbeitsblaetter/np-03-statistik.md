# NumPy – Statistik & Aggregation

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- statistische Kennzahlen mit NumPy berechnen
- Aggregationsfunktionen auf Arrays anwenden
- den `axis`-Parameter für zeilen-/spaltenweise Berechnungen nutzen
- mit NaN-Werten in Statistiken umgehen

!!! note "Begleitende Infoblätter"
    [:material-book-open-variant: Statistische Grundbegriffe](../infoblaetter/statistik-grundlagen.md) – Was bedeuten Mittelwert, Median, Standardabweichung? Wann welches Maß verwenden?
    
    [:material-book-open-variant: NumPy Funktionen](../infoblaetter/numpy-funktionen.md) – Syntax und Code-Beispiele für statistische Funktionen
    
    Lies die Infoblätter **zuerst**, bevor du die Aufgaben bearbeitest.

---

## Einführung

Statistische Analysen sind das Kerngeschäft eines Data Analysts. NumPy bietet optimierte Funktionen für alle gängigen Statistiken.

**Bearbeite alle Aufgaben in einem Jupyter Notebook.**

**Spaltenübersicht für den Taxi-Datensatz:**

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
| payment_type | 17 | Zahlungsart (1=Kreditkarte, 2=Bar) |

!!! abstract "Datensatz herunterladen"
    [:material-download: taxi_tripdata.csv](../assets/files/taxi_tripdata.csv){ .md-button }

---

## Aufgaben

### Aufgabe 1 – Daten laden und vorbereiten

Lade die Taxi-Daten (`taxi_tripdata.csv`) und extrahiere die relevanten Spalten für die Analyse.

- [ ] Lade die Datei mit `np.genfromtxt()` und nutze `skip_header=1`
- [ ] Gib die Shape des Datensatzes aus
- [ ] Extrahiere die Spalten `passenger_count`, `trip_distance`, `fare_amount`, `tip_amount` und `total_amount` in separate Variablen

!!! tip "Hilfe"
    - Datei laden: `np.genfromtxt(pfad, delimiter=',', skip_header=1)`
    - Spalte extrahieren: `array[:, spalten_index]`

---

### Aufgabe 2 – Lagemaße berechnen

Berechne Mittelwert, Median und Extremwerte für die Taxi-Daten.

- [ ] Berechne den **Durchschnitt** und **Median** der Fahrstrecke (`trip_distance`)
- [ ] Berechne **Minimum** und **Maximum** für Strecke und Fahrpreis
- [ ] Vergleiche `np.mean()` mit `np.nanmean()` – was passiert bei NaN-Werten?

!!! tip "Hilfe"
    - Lagemaße: `np.mean()`, `np.median()`, `np.min()`, `np.max()`
    - NaN-sichere Varianten: `np.nanmean()`, `np.nanmedian()`, `np.nanmin()`, `np.nanmax()`

!!! question "Reflexionsfrage"
    Warum unterscheiden sich Median und Mittelwert beim Fahrpreis? Was sagt das über die Verteilung aus?

---

### Aufgabe 3 – Streuungsmaße

Berechne Standardabweichung, Varianz und Variationskoeffizient.

- [ ] Berechne **Standardabweichung** und **Varianz** der Fahrpreise
- [ ] Berechne den Bereich, in dem 68% bzw. 95% der Fahrpreise liegen (Mittelwert ± 1 bzw. 2 Standardabweichungen)
- [ ] Berechne den **Variationskoeffizienten** (CV = std / mean) für Strecke und Fahrpreis – welche Größe streut relativ stärker?

!!! tip "Hilfe"
    - Streuungsmaße: `np.nanstd()` für Standardabweichung, `np.nanvar()` für Varianz
    - Variationskoeffizient: CV = Standardabweichung / Mittelwert

---

### Aufgabe 4 – Aggregationsfunktionen

Berechne Summen und zähle gültige Werte.

- [ ] Berechne den **Gesamtumsatz** (Summe aller `total_amount`) und das **gesamte Trinkgeld**
- [ ] Berechne den **Trinkgeld-Anteil** am Gesamtumsatz in Prozent
- [ ] Bestimme, wie viele **gültige Werte** (nicht NaN) die Spalte `trip_distance` hat

!!! tip "Hilfe"
    - Summe: `np.nansum()`
    - NaN zählen: `np.isnan(arr).sum()` oder `np.sum(~np.isnan(arr))` für gültige Werte

---

### Aufgabe 5 – Der axis-Parameter

Verstehe die Unterscheidung zwischen zeilen- und spaltenweiser Aggregation.

1. **Erstelle eine Test-Matrix:** 4 Produkte (Zeilen) × 3 Monate (Spalten) mit festen Verkaufszahlen
2. Berechne:
   - Die **Gesamtsumme** aller Werte (ohne axis)
   - Die **Summe pro Monat** (axis=0 → Ergebnis: 3 Werte)
   - Die **Summe pro Produkt** (axis=1 → Ergebnis: 4 Werte)
3. Wende `np.nanmean(daten, axis=0)` auf die Taxi-Daten an und ermittle den **Durchschnitt pro Spalte**

!!! tip "Hilfe"
    - `axis=0`: Aggregation entlang der Zeilen → Ergebnis: ein Wert pro Spalte
    - `axis=1`: Aggregation entlang der Spalten → Ergebnis: ein Wert pro Zeile
    - `axis=None` (Standard): Aggregation über alle Werte

---

### Aufgabe 6 – Extremwerte finden

Finde die Position von Minimum und Maximum.

- [ ] Finde die Fahrt mit dem **höchsten Trinkgeld** – gib Index, Trinkgeld-Betrag, Gesamtbetrag und Strecke aus
- [ ] Finde die **längste** und **kürzeste** Fahrt (ignoriere Fahrten mit 0 Meilen)
- [ ] Finde die **Top 5 Trinkgelder** – nutze `np.argsort()` und kehre die Reihenfolge um

!!! tip "Hilfe"
    - Position des Maximums: `np.nanargmax()`
    - Sortierte Indizes: `np.argsort(arr)[::-1]` für absteigende Reihenfolge

---

### Aufgabe 7 – Perzentile und Quartile

Analysiere die Verteilung mit Perzentilen.

- [ ] Berechne die **Quartile** (Q1, Q2, Q3) und den **Interquartilsabstand** (IQR = Q3 - Q1) für den Fahrpreis
- [ ] Berechne die Perzentile 10, 25, 50, 75, 90, 95 und 99 in einem Aufruf
- [ ] Interpretiere: Was bedeutet das 99. Perzentil für den Fahrpreis?

!!! tip "Hilfe"
    - Einzelnes Perzentil: `np.nanpercentile(arr, 25)` für Q1
    - Mehrere Perzentile: `np.nanpercentile(arr, [10, 25, 50, 75, 90])`

---

## Vertiefende Aufgaben

!!! tip "Optionale Aufgaben zur Vertiefung"
    Die folgenden Aufgaben sind **optional** und vertiefen das Gelernte. Sie eignen sich besonders für:
    
    - **Eigene Statistik-Funktionen entwickeln**
    - **Komplexe Analysen selbstständig durchführen**
    - **Ausreißer-Erkennung und fortgeschrittene Kennzahlen**
    - **Prüfungsvorbereitung** durch eigenständiges Arbeiten

---

### Aufgabe 8 – Statistik-Funktion erstellen

Erstelle eine wiederverwendbare Funktion für statistische Zusammenfassungen.

- [ ] Schreibe eine Funktion `statistik_zusammenfassung(arr, name)`, die ausgibt:
    - Anzahl gültiger Werte und fehlende Werte
    - Minimum, Maximum, Spannweite (`np.ptp()`)
    - Mittelwert, Median, Standardabweichung
    - 25. und 75. Perzentil
- [ ] Wende die Funktion auf `trip_distance`, `fare_amount` und `tip_amount` an

!!! tip "Hilfe"
    - Spannweite: `np.ptp(arr)` (peak-to-peak) = Maximum - Minimum
    - NaN-Werte filtern: `arr[~np.isnan(arr)]`

---

### Aufgabe 9 – Eigenständige Analysen

!!! warning "Ohne Hilfe lösen"
    Bearbeite diese komplexen Analysen selbstständig.

**Aufgabe A: Preisstruktur verstehen**

- [ ] Berechne das 10., 25., 50., 75., 90. und 99. Perzentil des Fahrpreises
- [ ] Wie viel Prozent des Gesamtumsatzes machen die teuersten 10% der Fahrten aus?
- [ ] Erstelle eine "5-Zahlen-Zusammenfassung" (Min, Q1, Median, Q3, Max) für Strecke, Preis und Trinkgeld

**Aufgabe B: Vergleichsanalyse**

- [ ] Teile die Fahrten in zwei Gruppen: mit Trinkgeld (>0) und ohne Trinkgeld (=0 oder NaN)
- [ ] Vergleiche für beide Gruppen: Durchschnittliche Fahrstrecke, Fahrpreis und Passagierzahl
- [ ] Stelle eine Hypothese auf: Wann wird eher Trinkgeld gegeben?

**Aufgabe C: Effizienz-Kennzahlen**

- [ ] Berechne für jede Fahrt: "Umsatz pro Meile" (total_amount / trip_distance)
- [ ] Entferne ungültige Werte (NaN, Infinity)
- [ ] Was ist der Durchschnitt und die Standardabweichung dieser Kennzahl?
- [ ] Finde die 10 effizientesten und 10 ineffizientesten Fahrten

**Aufgabe D: Streuungsanalyse**

- [ ] Berechne den Variationskoeffizienten für alle vier Spalten
- [ ] Welche Größe streut relativ am stärksten? Was bedeutet das inhaltlich?

**Aufgabe E: Eigene Statistik-Funktion**

- [ ] Erweitere deine Funktion aus Aufgabe 8 um die Anzahl der Ausreißer (Werte außerhalb von Q1-1.5×IQR und Q3+1.5×IQR).

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Lagemaße**: `mean()`, `median()`, `min()`, `max()`
    - **Streuungsmaße**: `std()`, `var()`, `percentile()`
    - **Aggregation**: `sum()`, `cumsum()`, `argmin()`, `argmax()`
    - **axis-Parameter**: `axis=0` (spaltenweise), `axis=1` (zeilenweise)
    - **NaN-sichere Funktionen**: `nanmean()`, `nansum()`, etc.

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `np.mean()` und `np.nanmean()`?
    2. Was gibt `np.argmax([3, 1, 4, 1, 5])` zurück?
    3. Bei einer 5×3 Matrix: Welche Shape hat `np.sum(matrix, axis=0)`?
    4. Wie berechnest du den Interquartilsabstand (IQR)?
    
    ??? success "Antworten"
        1. `mean()` gibt NaN zurück wenn NaN-Werte vorhanden, `nanmean()` ignoriert sie
        2. `4` (Index des Maximums 5)
        3. `(3,)` – ein Wert pro Spalte
        4. `IQR = np.percentile(arr, 75) - np.percentile(arr, 25)`
