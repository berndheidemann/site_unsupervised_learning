# NumPy Indexierung & Slicing

## Übersicht

Indexierung und Slicing sind fundamentale Techniken, um auf Teile von Arrays zuzugreifen. NumPy erweitert die Python-Konzepte um mächtige Funktionen für mehrdimensionale Arrays.

---

## 1D-Indexierung

### Grundlagen

```python
import numpy as np

arr = np.array([10, 20, 30, 40, 50, 60, 70, 80, 90])
```

**Visualisierung der Indizes:**

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Wert** | 10 | 20 | 30 | 40 | 50 | 60 | 70 | 80 | 90 |
| **Neg. Index** | -9 | -8 | -7 | -6 | -5 | -4 | -3 | -2 | -1 |

```python
# Einzelne Elemente
print(arr[0])    # 10 (erstes Element)
print(arr[4])    # 50 (fünftes Element)
print(arr[-1])   # 90 (letztes Element)
print(arr[-3])   # 70 (drittletztes Element)
```

### 1D-Slicing

**Syntax:** `arr[start:stop:step]`

- `start`: Startindex (inklusive), Standard: 0
- `stop`: Endindex (exklusive), Standard: Ende
- `step`: Schrittweite, Standard: 1

```python
arr = np.array([10, 20, 30, 40, 50, 60, 70, 80, 90])

print(arr[2:6])     # [30 40 50 60] - Index 2 bis 5
print(arr[:4])      # [10 20 30 40] - Anfang bis Index 3
print(arr[5:])      # [60 70 80 90] - Index 5 bis Ende
print(arr[::2])     # [10 30 50 70 90] - Jedes zweite Element
print(arr[1::2])    # [20 40 60 80] - Jedes zweite ab Index 1
print(arr[::-1])    # [90 80 70 60 50 40 30 20 10] - Umgekehrt
print(arr[-3:])     # [70 80 90] - Letzte 3 Elemente
```

**Slicing-Beispiele visualisiert:**

| Operation | Bedeutung | Ergebnis |
|-----------|-----------|----------|
| `a[1]` | Element an Index 1 | `2` |
| `a[2:4]` | Index 2 bis 3 (Stop exklusiv!) | `[3, 4]` |
| `a[-2:]` | Letzte 2 Elemente | `[4, 5]` |
| `a[::2]` | Jedes 2. Element | `[1, 3, 5]` |
| `a[[1,3,4]]` | Fancy Indexing | `[2, 4, 5]` |

---

## 2D-Indexierung

### Grundlagen

Bei 2D-Arrays: `arr[zeile, spalte]`

```python
matrix = np.array([[1, 2, 3, 4],
                   [5, 6, 7, 8],
                   [9, 10, 11, 12]])
```

**Visualisierung:**

|  | Sp. 0 | Sp. 1 | Sp. 2 | Sp. 3 |
|:---:|:---:|:---:|:---:|:---:|
| **Z. 0** | 1 | 2 | 3 | 4 |
| **Z. 1** | 5 | 6 | 7 | 8 |
| **Z. 2** | 9 | 10 | 11 | 12 |

```python
# Einzelne Elemente
print(matrix[0, 0])   # 1 (oben links)
print(matrix[1, 2])   # 7 (Zeile 1, Spalte 2)
print(matrix[2, -1])  # 12 (letzte Zeile, letzte Spalte)
print(matrix[-1, -1]) # 12 (gleich)
```

### 2D-Slicing

```python
# Ganze Zeilen
print(matrix[0])      # [1 2 3 4] - erste Zeile
print(matrix[1, :])   # [5 6 7 8] - zweite Zeile (explizit)

# Ganze Spalten
print(matrix[:, 0])   # [1 5 9] - erste Spalte
print(matrix[:, -1])  # [4 8 12] - letzte Spalte

# Teilbereiche
print(matrix[0:2, 1:3])
# [[2 3]
#  [6 7]]

# Jede zweite Zeile
print(matrix[::2, :])
# [[ 1  2  3  4]
#  [ 9 10 11 12]]
```

**2D-Slicing Kurzreferenz:**

| Operation | Beschreibung | Ergebnis-Shape |
|-----------|--------------|----------------|
| `matrix[0]` | Erste Zeile | (4,) |
| `matrix[:, 0]` | Erste Spalte | (3,) |
| `matrix[0:2, 1:3]` | Zeilen 0-1, Spalten 1-2 | (2, 2) |
| `matrix[::2, :]` | Jede 2. Zeile | (2, 4) |

---

## Fancy Indexing

Mit Fancy Indexing kannst du mehrere **nicht aufeinanderfolgende** Elemente auswählen.

### Mit Listen

```python
arr = np.array([10, 20, 30, 40, 50, 60, 70, 80, 90])

# Mehrere Indizes gleichzeitig
indices = [0, 2, 5, 8]
print(arr[indices])  # [10 30 60 90]

# Direkt mit Liste
print(arr[[1, 3, 5]])  # [20 40 60]
```

### 2D Fancy Indexing

```python
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])

# Bestimmte Zeilen auswählen
print(matrix[[0, 2]])  # Zeile 0 und 2
# [[1 2 3]
#  [7 8 9]]

# Bestimmte Elemente auswählen
zeilen = [0, 1, 2]
spalten = [0, 1, 2]
print(matrix[zeilen, spalten])  # Diagonale: [1 5 9]
```

---

## Boolean Indexing

Die mächtigste Indexierungsmethode: Auswahl basierend auf Bedingungen.

### Grundprinzip

```python
arr = np.array([10, 20, 30, 40, 50, 60, 70, 80, 90])

# Schritt 1: Bedingung erstellt Boolean-Array
maske = arr > 50
print(maske)  # [False False False False False  True  True  True  True]

# Schritt 2: Boolean-Array als Index nutzen
print(arr[maske])  # [60 70 80 90]

# Kurz: In einer Zeile
print(arr[arr > 50])  # [60 70 80 90]
```

### Mehrere Bedingungen

!!! warning "Wichtig: Klammern und Operatoren"
    Nutze `&` (und), `|` (oder), `~` (nicht) statt `and`, `or`, `not`.
    Jede Bedingung muss in Klammern stehen!

```python
arr = np.array([10, 20, 30, 40, 50, 60, 70, 80, 90])

# UND-Verknüpfung
print(arr[(arr > 30) & (arr < 70)])  # [40 50 60]

# ODER-Verknüpfung
print(arr[(arr < 20) | (arr > 80)])  # [10 90]

# NICHT
print(arr[~(arr > 50)])  # [10 20 30 40 50]
```

### 2D Boolean Indexing

```python
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])

# Alle Werte > 5
print(matrix[matrix > 5])  # [6 7 8 9] - 1D-Array!

# Werte ersetzen
matrix[matrix > 5] = 0
print(matrix)
# [[1 2 3]
#  [4 5 0]
#  [0 0 0]]
```

---

## Views vs. Copies

!!! danger "Wichtiges Konzept"
    Slicing erstellt einen **View** (Ansicht), keine Kopie! Änderungen am View ändern auch das Original.

```python
original = np.array([1, 2, 3, 4, 5])

# Slicing erstellt View
view = original[1:4]
view[0] = 99

print(original)  # [ 1 99  3  4  5] - Original geändert!
print(view)      # [99  3  4]
```

### Explizite Kopie erstellen

```python
original = np.array([1, 2, 3, 4, 5])

# Explizite Kopie
kopie = original[1:4].copy()
kopie[0] = 99

print(original)  # [1 2 3 4 5] - Original unverändert!
print(kopie)     # [99  3  4]
```

!!! danger "Merke: Slicing erstellt einen View!"
    ```python
    a = np.array([1, 2, 3, 4, 5])
    view = a[2:4]      # view zeigt auf a[2] und a[3]
    view[:] = 0        # Ändert auch a!
    print(a)           # [1, 2, 0, 0, 5] ← Original geändert!
    ```

### Wann View, wann Copy?

| Operation | Ergebnis |
|-----------|----------|
| Slicing `arr[1:4]` | View |
| Fancy Indexing `arr[[1,2,3]]` | Copy |
| Boolean Indexing `arr[arr > 5]` | Copy |
| `arr.copy()` | Copy |
| `arr.flatten()` | Copy |
| `arr.ravel()` | View (wenn möglich) |

---

## Praktische Beispiele

### Beispiel 1: Ausreißer finden

```python
messwerte = np.array([22.5, 23.1, 150.0, 22.8, 23.5, 22.9, -10.0, 23.2])

# Plausible Werte: 20-25 Grad
gueltig = messwerte[(messwerte >= 20) & (messwerte <= 25)]
print(f"Gültige Werte: {gueltig}")
# Gültige Werte: [22.5 23.1 22.8 23.5 22.9 23.2]

# Ausreißer
ausreisser = messwerte[(messwerte < 20) | (messwerte > 25)]
print(f"Ausreißer: {ausreisser}")
# Ausreißer: [150.  -10. ]
```

### Beispiel 2: Daten aus Tabelle extrahieren

```python
# Verkaufsdaten: [Produkt-ID, Menge, Preis, Gewinn]
daten = np.array([[101, 50, 29.99, 150.0],
                  [102, 30, 49.99, 200.0],
                  [103, 100, 9.99, 100.0],
                  [104, 25, 99.99, 500.0],
                  [105, 75, 19.99, 225.0]])

# Alle Preise (Spalte 2)
preise = daten[:, 2]
print(f"Preise: {preise}")

# Top-Seller: Menge > 50
top_seller = daten[daten[:, 1] > 50]
print(f"Top-Seller:\n{top_seller}")

# Hochpreisige Produkte (Preis > 30)
hochpreisig = daten[daten[:, 2] > 30]
print(f"Hochpreisig:\n{hochpreisig}")
```

### Beispiel 3: Schachbrettmuster

```python
# 8x8 Schachbrett
schachbrett = np.zeros((8, 8), dtype=int)
schachbrett[::2, 1::2] = 1  # Ungerade Spalten in geraden Zeilen
schachbrett[1::2, ::2] = 1  # Gerade Spalten in ungeraden Zeilen
print(schachbrett)
```

---

## Zusammenfassung

!!! success "Das Wichtigste"
    - **Einfache Indizierung**: `arr[i]` oder `arr[i, j]`
    - **Slicing**: `arr[start:stop:step]` - erzeugt View!
    - **Fancy Indexing**: `arr[[0, 2, 4]]` - mit Listen
    - **Boolean Indexing**: `arr[arr > 5]` - mit Bedingungen
    - **Mehrere Bedingungen**: `&` (und), `|` (oder), `~` (nicht)
    - **Views vs. Copies**: Slicing = View, `.copy()` für echte Kopie

---

??? question "Selbstkontrolle"
    1. Was gibt `arr[2:5]` zurück, wenn `arr = np.array([0, 1, 2, 3, 4, 5, 6])`?
    2. Wie extrahierst du die zweite Spalte einer Matrix?
    3. Wie filterst du alle Werte zwischen 10 und 20 (inklusive)?
    4. Was ist der Unterschied zwischen `&` und `and` bei NumPy-Bedingungen?
    
    ??? success "Antworten"
        1. `[2 3 4]` (Index 2, 3, 4 - Stop ist exklusiv)
        2. `matrix[:, 1]`
        3. `arr[(arr >= 10) & (arr <= 20)]`
        4. `&` ist element-wise für Arrays, `and` funktioniert nicht mit Arrays (nur für einzelne Booleans)
