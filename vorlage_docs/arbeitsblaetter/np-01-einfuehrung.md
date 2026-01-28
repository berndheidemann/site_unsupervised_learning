# NumPy – Einführung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- erklären, warum NumPy für Datenanalyse wichtig ist
- NumPy-Arrays erstellen und ihre Eigenschaften inspizieren
- den Unterschied zwischen Python-Listen und NumPy-Arrays verstehen
- grundlegende Operationen auf Arrays durchführen

!!! info "Voraussetzungen"
    Für dieses Arbeitsblatt solltest du folgende Python-Grundlagen beherrschen:
    
    - **Variablen und Datentypen**: `int`, `float`, `str`, `list`
    - **Schleifen**: `for`-Schleifen, `range()`
    - **Bedingungen**: `if`/`else`-Anweisungen
    - **List Comprehensions**: `[x * 2 for x in liste]`
    - **Funktionen**: Eigene Funktionen definieren und aufrufen
    - **Module importieren**: `import modul` bzw. `import modul as alias`

!!! note "Begleitendes Infoblatt"
    [:material-book-open-variant: NumPy Grundlagen](../infoblaetter/numpy-grundlagen.md) – Installation, Arrays, Datentypen, Dimensionen
    
    Lies das Infoblatt **zuerst**, bevor du die Aufgaben bearbeitest. Dort findest du alle Syntax-Beispiele und Erklärungen.

---

## Einführung

Als Data Analyst arbeitest du ständig mit großen Datenmengen. NumPy ist die Grundlage für effiziente numerische Berechnungen in Python.

**Bearbeite alle Aufgaben in einem Jupyter Notebook.**

---

## Aufgaben

### Aufgabe 1 – NumPy einrichten

- [ ] Installiere NumPy mit `pip` (falls noch nicht geschehen)
- [ ] Importiere NumPy mit der üblichen Konvention `np`
- [ ] Gib die installierte Version aus (`np.__version__`)

---

### Aufgabe 2 – Performance-Vergleich

Vergleiche die Geschwindigkeit von Python-Listen und NumPy-Arrays.

- [ ] Erstelle eine Python-Liste und ein NumPy-Array mit jeweils **1 Million** Werten (0 bis 999.999)
- [ ] Messe mit dem `time`-Modul, wie lange das **Verdoppeln aller Werte** dauert:
    - Bei der Liste: mit List Comprehension `[x * 2 for x in liste]`
    - Beim Array: vektorisiert mit `array * 2`
- [ ] Berechne den Speedup-Faktor und dokumentiere dein Ergebnis
- [ ] Experimentiere: Wie ändert sich der Speedup bei 10 Millionen Werten?

!!! question "Reflexionsfrage"
    Warum ist NumPy so viel schneller als Python-Listen? Recherchiere die Begriffe "vektorisierte Operationen" und "C-Implementierung".

---

### Aufgabe 3 – Arrays erstellen

!!! tip "Hilfe"
    Im Infoblatt findest du Tabellen mit allen Erstellungsfunktionen: `np.array()`, `np.zeros()`, `np.ones()`, `np.arange()`, `np.linspace()`, `np.random.randint()`

Erstelle folgende Arrays und gib sie jeweils mit `print()` aus:

- [ ] **Verkaufszahlen:** Ein 1D-Array mit den Werten `[145, 189, 132, 201, 178, 156, 210]`
- [ ] **Preistabelle:** Eine 5×4 Matrix (5 Produkte, 4 Filialen), initial mit Nullen gefüllt
- [ ] **Rabattstufen:** 5 gleichmäßig verteilte Werte von 0.0 bis 0.20 (nutze `linspace`)
- [ ] **Würfelwürfe:** 100 Zufallszahlen zwischen 1 und 6
- [ ] **Gerade Zahlen:** Alle geraden Zahlen von 2 bis 20 (nutze `arange`)

---

### Aufgabe 4 – Array-Eigenschaften

!!! tip "Hilfe"
    Wichtige Eigenschaften: `shape`, `ndim`, `dtype`, `size`, `itemsize`, `nbytes`

- [ ] Erstelle eine 3×4 Matrix mit den Zahlen 1 bis 12
- [ ] Gib alle 6 Eigenschaften (`shape`, `ndim`, `dtype`, `size`, `itemsize`, `nbytes`) aus
- [ ] Erstelle das gleiche Array einmal als `int32` und einmal als `float64`
- [ ] Vergleiche den Speicherverbrauch (`nbytes`) beider Arrays – um welchen Faktor unterscheiden sie sich?

---

### Aufgabe 5 – Reshaping

!!! tip "Hilfe"
    `reshape(zeilen, spalten)` – die Gesamtzahl der Elemente muss gleich bleiben!  
    Mit `-1` wird eine Dimension automatisch berechnet.

- [ ] Erstelle ein 1D-Array mit den Zahlen 1 bis 24
- [ ] Forme es um zu:
    - 4×6 Matrix
    - 6×4 Matrix  
    - 2×3×4 (3D-Array)
    - 8 Zeilen, Spalten automatisch
- [ ] Welche Formen sind **nicht** möglich? Probiere z.B. (5, 5) – was passiert?
- [ ] Mache aus einer Matrix wieder ein 1D-Array (zwei Methoden: `flatten()` und `ravel()`)

!!! info "flatten() vs. ravel()"
    - **`flatten()`** erstellt immer eine **Kopie** – Änderungen wirken sich nicht auf das Original aus
    - **`ravel()`** erstellt wenn möglich einen **View** – Änderungen können das Original verändern!
    
    **Faustregel:** Nimm `flatten()` wenn du sicher sein willst, dass das Original unverändert bleibt. Nimm `ravel()` wenn Performance wichtig ist und du das Original nicht mehr brauchst.

---

### Aufgabe 6 – Arithmetische Operationen

NumPy-Operationen sind **element-weise** – sie werden auf jedes Element einzeln angewendet.

**Gegeben:** Ein Online-Shop hat folgende Nettopreise:  
`[29.99, 49.99, 19.99, 99.99, 14.99]`

Berechne **ohne Schleifen**:

- [ ] Alle Bruttopreise (19% MwSt hinzufügen)
- [ ] Alle Preise mit 15% Rabatt
- [ ] Die Differenz zwischen teuerstem und günstigstem Artikel
- [ ] Die Summe aller Bruttopreise
- [ ] Den Durchschnittspreis
- [ ] **Bonus:** Erstelle ein 3×5 Array (3 Tage × 5 Produkte). Simuliere tägliche Preisänderungen: Tag 1 = -5%, Tag 2 = 0%, Tag 3 = +5%.

---

## Vertiefende Aufgaben

!!! tip "Optionale Aufgaben zur Vertiefung"
    Die folgenden Aufgaben sind **optional** und vertiefen das Gelernte. Sie eignen sich besonders für:
    
    - **Anwendung auf realistische Szenarien** (Temperatursensoren, Noten)
    - **Kombination mehrerer Konzepte** (Würfelsimulation)
    - **Arbeit mit Zufallszahlen und Normalverteilung**
    - **Prüfungsvorbereitung** durch eigenständiges Arbeiten

---

### Aufgabe 7 – Temperatursensor

Ein Sensor liefert 24 Messwerte (stündlich) in **Fahrenheit**. Simuliere diese Daten und analysiere sie.

- [ ] Erzeuge 24 Zufallswerte zwischen 60 und 100 (Fahrenheit)  
   *Tipp: Nutze `np.random.seed(42)` für reproduzierbare Ergebnisse*
- [ ] Konvertiere alle Werte zu Celsius: $C = (F - 32) \times \frac{5}{9}$
- [ ] Berechne: Minimum, Maximum, Durchschnitt, Standardabweichung
- [ ] Finde heraus, um welche Uhrzeit die höchste Temperatur gemessen wurde  
   *Tipp: `np.argmax()` gibt den Index des größten Elements*

---

### Aufgabe 8 – Notenberechnung

Ein Kurs hat **8 Schüler**, die **3 Tests** geschrieben haben.

- [ ] Erstelle ein 8×3 Array mit Zufallspunkten (0-100)
- [ ] Berechne den **Durchschnitt jedes Schülers** (über alle 3 Tests)  
   *Tipp: `mean(axis=1)` für Zeilenmittelwert*
- [ ] Berechne den **Durchschnitt jedes Tests** (über alle 8 Schüler)  
   *Tipp: `mean(axis=0)` für Spaltenmittelwert*
- [ ] Finde den **besten** und **schlechtesten** Schüler (nach Durchschnitt)
- [ ] Wie viel **Prozent** der Schüler haben über 60 Punkte im Schnitt?

---

### Aufgabe 9 – Würfelsimulation

Simuliere **10.000 Würfe** mit zwei Würfeln und analysiere die Ergebnisse.

- [ ] Erstelle zwei Arrays mit je 10.000 Zufallszahlen (1-6) für Würfel 1 und Würfel 2
- [ ] Berechne die **Summe** beider Würfel für jeden Wurf
- [ ] Zähle: Wie oft wurde eine **7** gewürfelt? Wie oft eine **12**?
- [ ] Berechne die **relative Häufigkeit** jeder möglichen Summe (2-12)
- [ ] Vergleiche mit der **theoretischen Wahrscheinlichkeit**:
    - P(7) = 6/36 ≈ 16.67%
    - P(12) = 1/36 ≈ 2.78%

---

### Aufgabe 10 – Körpergrößen-Analyse

Erstelle ein Array mit **1000 simulierten Körpergrößen** (Normalverteilung):

```python
groessen = np.random.normal(170, 10, 1000)  # Mittelwert 170, Std 10
```

Beantworte folgende Fragen:

- [ ] Wie viele Personen sind **größer als 180 cm**?
- [ ] Wie viele sind **zwischen 165 und 175 cm**?
- [ ] Was ist das **90. Perzentil**? (Nutze `np.percentile()`)
- [ ] **Bonus:** Teile die Daten in 10 gleichmäßige Bins von 140 bis 200 cm ein und zähle die Häufigkeiten pro Bin (nutze `np.histogram()`)

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **NumPy importieren**: `import numpy as np`
    - **Arrays erstellen**: `np.array()`, `np.zeros()`, `np.ones()`, `np.arange()`, `np.linspace()`
    - **Eigenschaften**: `shape`, `dtype`, `ndim`, `size`
    - **Reshaping**: `reshape()`, `flatten()`, `ravel()`
    - **Operationen**: Element-weise (+, -, *, /), mathematische Funktionen
    - **Performance**: NumPy ist viel schneller als Python-Listen!

---

??? question "Selbstkontrolle"
    1. Wie erstellst du ein 5×5 Array mit Nullen?
    2. Was ist der Unterschied zwischen `np.arange(0, 10, 2)` und `np.linspace(0, 10, 5)`?
    3. Kann ein Array mit 15 Elementen zu (3, 4) umgeformt werden?
    4. Was gibt `np.array([1, 2, 3]) * 2` zurück?
    
    ??? success "Antworten"
        1. `np.zeros((5, 5))`
        2. `arange` erzeugt `[0, 2, 4, 6, 8]` (Schrittweite 2), `linspace` erzeugt 5 gleichmäßig verteilte Werte von 0 bis 10
        3. Nein, 15 ≠ 3×4=12. Mögliche Formen: (3,5), (5,3), (1,15), (15,1)
        4. `array([2, 4, 6])` – element-weise Multiplikation
