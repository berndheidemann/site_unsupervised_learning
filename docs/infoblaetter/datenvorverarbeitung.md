# Datenvorverarbeitung für Clustering

## Warum Vorverarbeitung?

Clustering-Algorithmen berechnen **Abstände** zwischen Datenpunkten. Ohne Vorverarbeitung können folgende Probleme auftreten:

- Features mit großen Werten dominieren
- Kategoriale Daten können nicht direkt verwendet werden
- Redundante Features verfälschen Ergebnisse

---

## Skalierung

### Das Problem ohne Skalierung

```python
# Beispiel: Gehalt (€) vs. Alter (Jahre)
gehalt = [50000, 75000, 40000]  # Werte: 40.000 - 75.000
alter = [25, 45, 30]            # Werte: 25 - 45

# Ohne Skalierung: Gehalt dominiert die Distanz!
```

!!! warning "Wichtig"
    K-Means und hierarchisches Clustering sind **distanzbasiert**. Ein Feature mit Werten von 0-100.000 hat mehr Einfluss als eines mit Werten von 0-10.

### StandardScaler (Z-Score Normalisierung)

Transformiert Daten zu **Mittelwert = 0** und **Standardabweichung = 1**.

$$z = \frac{x - \mu}{\sigma}$$

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Zurück-transformieren (falls nötig)
X_original = scaler.inverse_transform(X_scaled)
```

**Wann verwenden:** Standard für die meisten Clustering-Aufgaben.

### MinMaxScaler

Skaliert Daten auf einen **festen Bereich** (meist 0-1).

$$x_{scaled} = \frac{x - x_{min}}{x_{max} - x_{min}}$$

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()  # Default: 0-1
X_scaled = scaler.fit_transform(X)

# Anderer Bereich
scaler = MinMaxScaler(feature_range=(-1, 1))
```

**Wann verwenden:** Wenn Werte in bestimmtem Bereich sein müssen (z.B. Neuronale Netze).

### Vergleich der Scaler

| Scaler | Mittelwert | Bereich | Ausreißer-Empfindlich |
|--------|------------|---------|----------------------|
| StandardScaler | 0 | unbegrenzt | Ja |
| MinMaxScaler | variabel | 0-1 | Sehr |
| RobustScaler | Median | unbegrenzt | Nein |

```python
from sklearn.preprocessing import RobustScaler

# Für Daten mit Ausreißern
scaler = RobustScaler()  # Nutzt Median und IQR
X_scaled = scaler.fit_transform(X)
```

---

## Umgang mit kategorialen Daten

Clustering-Algorithmen brauchen **numerische** Eingaben.

### One-Hot Encoding

Wandelt kategoriale in binäre Spalten um:

```python
import pandas as pd

df = pd.DataFrame({'Farbe': ['rot', 'blau', 'grün', 'rot']})

# Mit pandas
df_encoded = pd.get_dummies(df, columns=['Farbe'])
print(df_encoded)
#    Farbe_blau  Farbe_grün  Farbe_rot
# 0           0           0          1
# 1           1           0          0
# 2           0           1          0
# 3           0           0          1
```

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(sparse_output=False)
X_encoded = encoder.fit_transform(df[['Farbe']])
```

!!! tip "Wann One-Hot Encoding?"
    - Wenige Kategorien (< 10)
    - Keine Ordnung zwischen Kategorien
    - Achtung: Viele Kategorien → viele neue Spalten!

### Label Encoding

Für **ordinale** Daten (mit Reihenfolge):

```python
from sklearn.preprocessing import LabelEncoder

# Für ordinale Daten
groesse = ['klein', 'mittel', 'groß', 'klein']
encoder = LabelEncoder()
groesse_encoded = encoder.fit_transform(groesse)
# [0, 2, 1, 0]  # Achtung: Alphabetisch!

# Besser: Manuelle Zuordnung
mapping = {'klein': 0, 'mittel': 1, 'groß': 2}
df['groesse_num'] = df['groesse'].map(mapping)
```

---

## Feature Selection

### Warum Features reduzieren?

- **Fluch der Dimensionalität**: Bei vielen Features werden Abstände bedeutungslos
- **Rechenzeit**: Weniger Features = schnellere Berechnung
- **Interpretierbarkeit**: Cluster mit 50 Features schwer zu verstehen

### Korrelationsanalyse

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Korrelationsmatrix
corr = df.corr()

# Heatmap visualisieren
plt.figure(figsize=(10, 8))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0)
plt.title('Korrelationsmatrix')
plt.tight_layout()
plt.show()

# Hoch korrelierte Features finden (>0.8)
high_corr = np.where(np.abs(corr) > 0.8)
for i, j in zip(*high_corr):
    if i != j:
        print(f"{corr.columns[i]} <-> {corr.columns[j]}: {corr.iloc[i, j]:.2f}")
```

!!! tip "Faustregel"
    Bei Korrelation > 0.8: Eines der Features entfernen oder PCA nutzen.

### Varianz-basierte Selektion

Features mit sehr geringer Varianz enthalten wenig Information:

```python
from sklearn.feature_selection import VarianceThreshold

# Features mit Varianz < 0.1 entfernen
selector = VarianceThreshold(threshold=0.1)
X_selected = selector.fit_transform(X_scaled)

print(f"Vorher: {X_scaled.shape[1]} Features")
print(f"Nachher: {X_selected.shape[1]} Features")
```

---

## Der komplette Vorverarbeitungs-Workflow

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler

# 1. Daten laden
df = pd.read_csv('data.csv')

# 2. Fehlende Werte prüfen
print(df.isnull().sum())

# 3. Nur numerische Features (oder encodieren)
X = df.select_dtypes(include=[np.number])

# 4. Korrelationen prüfen (optional)
corr_matrix = X.corr()

# 5. Skalieren
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Jetzt bereit für Clustering!
```

---

## Häufige Fehler

| Fehler | Problem | Lösung |
|--------|---------|--------|
| Nicht-numerische Spalten | `ValueError: could not convert string` | `select_dtypes(include=[np.number])` |
| Fehlende Werte | `NaN` beschädigt Ergebnisse | `df.dropna()` oder `df.fillna()` |
| ID-Spalten mit skaliert | IDs verfälschen Clustering | IDs vorher entfernen |
| Fit auf Testdaten | Data Leakage | Nur auf Trainingsdaten fitten |

---

## Zusammenfassung

!!! success "Das Wichtigste"
    - **Immer skalieren** bei distanzbasierten Methoden
    - StandardScaler ist der Standard für Clustering
    - Kategoriale Daten mit One-Hot Encoding umwandeln
    - Hoch korrelierte Features prüfen und ggf. entfernen
    - Fehlende Werte VOR dem Skalieren behandeln

---

??? question "Selbstkontrolle"
    1. Warum ist Skalierung für K-Means wichtig?
    2. Was ist der Unterschied zwischen StandardScaler und MinMaxScaler?
    3. Wie behandelst du kategoriale Daten für Clustering?
    4. Was tust du bei hoch korrelierten Features?
    
    ??? success "Antworten"
        1. K-Means ist distanzbasiert – ohne Skalierung dominieren Features mit großen Werten
        2. StandardScaler: Mittelwert=0, Std=1; MinMaxScaler: Werte in festem Bereich (0-1)
        3. One-Hot Encoding oder Label Encoding (bei ordinalen Daten)
        4. Eines der Features entfernen oder PCA zur Dimensionsreduktion nutzen
