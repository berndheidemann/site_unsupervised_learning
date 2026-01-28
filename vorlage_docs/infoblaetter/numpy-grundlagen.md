# NumPy Grundlagen

## Was ist NumPy?

**NumPy** (Numerical Python) ist die fundamentale Bibliothek für wissenschaftliches Rechnen in Python. Sie bietet:

- Mehrdimensionale Arrays (ndarray)
- Mathematische Funktionen für Arrays
- Werkzeuge für lineare Algebra
- Zufallszahlengeneratoren

![Numpy Tech Stack](../assets/images/numpy/numpy_techstack.png)

---

## Installation

```bash
pip install numpy
```

Import-Konvention:
```python
import numpy as np
```

---

## Arrays erstellen

### Aus Listen

```python
import numpy as np

# 1D-Array aus Liste
arr1d = np.array([1, 2, 3, 4, 5])
print(arr1d)  # [1 2 3 4 5]

# 2D-Array aus verschachtelter Liste
arr2d = np.array([[1, 2, 3], 
                  [4, 5, 6]])
print(arr2d)
# [[1 2 3]
#  [4 5 6]]
```

### Mit Initialisierungsfunktionen

| Funktion | Beschreibung | Beispiel |
|----------|--------------|----------|
| `np.zeros(shape)` | Array mit Nullen | `np.zeros((3, 4))` |
| `np.ones(shape)` | Array mit Einsen | `np.ones((2, 3))` |
| `np.full(shape, val)` | Array mit Wert | `np.full((2, 2), 7)` |
| `np.empty(shape)` | Nicht initialisiert | `np.empty((3, 3))` |
| `np.eye(n)` | Einheitsmatrix | `np.eye(4)` |

```python
# Beispiele
nullen = np.zeros((3, 4))      # 3x4 Matrix mit Nullen
einsen = np.ones((2, 3))       # 2x3 Matrix mit Einsen
siebener = np.full((2, 2), 7)  # 2x2 Matrix mit 7en
identitaet = np.eye(3)         # 3x3 Einheitsmatrix
```

### Sequenzen erstellen

| Funktion | Beschreibung | Beispiel |
|----------|--------------|----------|
| `np.arange(start, stop, step)` | Wie `range()` | `np.arange(0, 10, 2)` |
| `np.linspace(start, stop, num)` | n gleichmäßige Werte | `np.linspace(0, 1, 5)` |

```python
# arange: Start, Stop (exklusiv), Schrittweite
seq1 = np.arange(0, 10, 2)
print(seq1)  # [0 2 4 6 8]

# linspace: Start, Stop (inklusiv), Anzahl
seq2 = np.linspace(0, 1, 5)
print(seq2)  # [0.   0.25 0.5  0.75 1.  ]
```

### Zufallszahlen

```python
# Gleichverteilte Zufallszahlen zwischen 0 und 1
zufaellig = np.random.rand(3, 4)  # 3x4 Matrix

# Ganzzahlige Zufallszahlen
ganzzahlen = np.random.randint(1, 100, size=(5,))  # 5 Zahlen von 1-99

# Normalverteilte Zufallszahlen (μ=0, σ=1)
normal = np.random.randn(100)

# Reproduzierbare Ergebnisse
np.random.seed(42)
```

---

## Array-Eigenschaften

| Eigenschaft | Beispielwert | Beschreibung |
|-------------|--------------|---------------|
| `shape` | `(3, 4)` | Dimensionen (Zeilen, Spalten) |
| `dtype` | `int64` | Datentyp der Elemente |
| `ndim` | `2` | Anzahl Achsen/Dimensionen |
| `size` | `12` | Gesamtzahl Elemente |
| `itemsize` | `8` | Bytes pro Element |
| `nbytes` | `96` | Gesamtspeicher in Bytes |

```python
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

print(arr.shape)     # (3, 4) - 3 Zeilen, 4 Spalten
print(arr.dtype)     # int64 - Datentyp
print(arr.ndim)      # 2 - Anzahl Dimensionen
print(arr.size)      # 12 - Gesamtzahl Elemente
print(arr.itemsize)  # 8 - Bytes pro Element
print(arr.nbytes)    # 96 - Gesamtspeicher in Bytes
```

---

## Datentypen (dtype)

NumPy unterstützt verschiedene Datentypen für optimale Speichernutzung:

| Typ | Beschreibung | Beispiel |
|-----|--------------|----------|
| `int8, int16, int32, int64` | Ganzzahlen | `np.array([1, 2], dtype=np.int32)` |
| `uint8, uint16, ...` | Positive Ganzzahlen | Bilddaten (0-255) |
| `float16, float32, float64` | Fließkommazahlen | `np.array([1.5], dtype=np.float64)` |
| `bool` | Wahrheitswerte | `np.array([True, False])` |
| `str` | Strings | `np.array(['a', 'b'])` |

```python
# Datentyp beim Erstellen festlegen
arr_float = np.array([1, 2, 3], dtype=np.float64)
print(arr_float)  # [1. 2. 3.]

# Datentyp konvertieren
arr_int = arr_float.astype(np.int32)
print(arr_int)  # [1 2 3]
```

!!! warning "Speicherverbrauch"
    `float64` benötigt 8x so viel Speicher wie `int8`. Bei großen Datensätzen kann die Wahl des richtigen Datentyps erheblich Speicher sparen.

---

## Reshaping (Form ändern)

```python
arr = np.arange(1, 13)  # [1, 2, 3, ..., 12]

# Reshape zu 3x4 Matrix
matrix = arr.reshape(3, 4)
print(matrix)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

# Automatische Dimension mit -1
auto = arr.reshape(4, -1)  # 4 Zeilen, Spalten automatisch
print(auto.shape)  # (4, 3)

# Flatten: Zurück zu 1D
flach = matrix.flatten()
print(flach)  # [ 1  2  3  4  5  6  7  8  9 10 11 12]

# Ravel: Wie flatten, aber View (kein Kopieren)
flach_view = matrix.ravel()
```

!!! tip "Reshape-Regel"
    Die Gesamtzahl der Elemente muss gleich bleiben! Ein 12-Element-Array kann zu (3,4), (4,3), (2,6), (6,2), (12,1), (1,12) umgeformt werden.

---

## Warum NumPy statt Python-Listen?

### Performance-Vergleich

```python
import numpy as np
import time

# Python-Liste
python_liste = list(range(1_000_000))

start = time.time()
ergebnis = [x * 2 for x in python_liste]
print(f"Python-Liste: {time.time() - start:.4f}s")

# NumPy-Array
numpy_array = np.arange(1_000_000)

start = time.time()
ergebnis = numpy_array * 2
print(f"NumPy-Array: {time.time() - start:.4f}s")
```

**Typisches Ergebnis:**
- Python-Liste: ~0.15s
- NumPy-Array: ~0.002s
- **NumPy ist ~75x schneller!**

### Vorteile von NumPy

| Eigenschaft | Python-Liste | NumPy-Array |
|-------------|--------------|-------------|
| Speichereffizienz | Gering | Hoch |
| Rechengeschwindigkeit | Langsam | Sehr schnell |
| Broadcasting | Nein | Ja |
| Vektorisierung | Nein | Ja |
| Einheitlicher Datentyp | Nein | Ja |

---

## Praxisbeispiel: Messwerte analysieren

```python
import numpy as np

# Temperaturen einer Woche (°C)
temperaturen = np.array([22.5, 24.1, 19.8, 23.2, 25.0, 21.7, 20.3])

# Grundlegende Statistiken
print(f"Mittelwert: {temperaturen.mean():.1f}°C")
print(f"Maximum: {temperaturen.max():.1f}°C")
print(f"Minimum: {temperaturen.min():.1f}°C")
print(f"Standardabweichung: {temperaturen.std():.2f}°C")

# Temperatur in Fahrenheit umrechnen (vektorisiert!)
fahrenheit = temperaturen * 9/5 + 32
print(f"In Fahrenheit: {fahrenheit}")
```

---

## Zusammenfassung

!!! success "Das Wichtigste"
    - NumPy ist die Basis für Data Science in Python
    - Arrays mit `np.array()`, `np.zeros()`, `np.arange()` etc. erstellen
    - Eigenschaften: `shape`, `dtype`, `ndim`, `size`
    - `reshape()` ändert die Form, nicht die Daten
    - NumPy ist viel schneller als Python-Listen

---

??? question "Selbstkontrolle"
    1. Wie erstellst du ein 3x3 Array mit Nullen?
    2. Was ist der Unterschied zwischen `np.arange()` und `np.linspace()`?
    3. Was gibt `np.array([1, 2, 3]).shape` zurück?
    4. Wie konvertierst du einen Datentyp von `float64` zu `int32`?
    
    ??? success "Antworten"
        1. `np.zeros((3, 3))`
        2. `arange` nutzt Schrittweite, `linspace` nutzt Anzahl der Werte
        3. `(3,)` - ein Tupel mit einer Dimension
        4. `arr.astype(np.int32)`
