# UL-OPT-02: Big Data & Clustering *(Optional)*

!!! info "Referenz-Material"
    Dieses Arbeitsblatt enthält vollständigen Code als **Referenz und Nachschlagewerk**. 
    Die Techniken (Sampling, Chunked Processing, Mini-Batch K-Means) sind für reale Big-Data-Szenarien wichtig.
    
    **Empfehlung:** Verstehe die Konzepte, probiere den Code aus, und adaptiere ihn für eigene große Datensätze.

!!! abstract "Advance Organizer"
    In der echten Arbeitswelt sind Datensätze oft zu groß, um sie komplett in den Speicher zu laden. Mit 7,7 Millionen Unfällen lernst du hier Techniken, die du später im Beruf brauchen wirst: Sampling, Chunked Processing und speichereffiziente Algorithmen.
    
    **Dein Ziel:** Du verstehst die Grenzen von Standard-Clustering und kannst Lösungsstrategien für Big-Data-Szenarien entwickeln.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Herausforderungen bei großen Datensätzen verstehen
- [x] Sampling-Strategien anwenden und bewerten
- [x] Mini-Batch K-Means für effizientes Clustering nutzen
- [x] Daten in Chunks verarbeiten

---

## Das Szenario

!!! example "Ausgangssituation"
    Ein Verkehrsministerium möchte Unfallmuster analysieren, um:
    
    1. **Unfallschwerpunkte** zu identifizieren
    2. **Zeitliche Muster** zu erkennen (Tageszeit, Wochentag, Monat)
    3. **Wetterbezogene Zusammenhänge** zu finden
    
    **Problem:** Der Datensatz hat 7,7 Millionen Zeilen – zu groß für Standard-Methoden!
    
    **Deine Aufgabe:** Entwickle eine Strategie für die Analyse!

---

## Der Datensatz

Der **US-Accidents** Datensatz enthält Verkehrsunfälle in den USA von 2016-2023:

| Feature | Beschreibung |
|---------|--------------|
| Start_Time | Zeitpunkt des Unfalls |
| End_Time | Ende der Behinderung |
| Start_Lat, Start_Lng | GPS-Koordinaten |
| Severity | Schweregrad (1-4) |
| Temperature | Temperatur (°F) |
| Humidity | Luftfeuchtigkeit (%) |
| Visibility | Sichtweite (Meilen) |
| Weather_Condition | Wetterbedingung |
| Sunrise_Sunset | Tag/Nacht |
| State, City | Ort |

---

## Aufgabe 1: Datensatz-Größe verstehen

- [ ] Erledigt

Bevor du lädst, prüfe die Größe:

```python
import pandas as pd
import numpy as np
import os

# Dateigröße prüfen (ohne zu laden!)
file_path = 'US_Accidents_March23.csv'
file_size_mb = os.path.getsize(file_path) / 1e6
print(f"Dateigröße: {file_size_mb:.0f} MB")

# Erste Zeilen lesen (ohne alles zu laden)
df_peek = pd.read_csv(file_path, nrows=5)
print(f"\nSpalten: {len(df_peek.columns)}")
print(df_peek.columns.tolist())
```

**Fragen:**

a) Wie groß ist die Datei?

b) Wie viel RAM würde sie benötigen (Faustregel: 2-3x Dateigröße)?

---

## Aufgabe 2: Chunked Loading

- [ ] Erledigt

Lade die Daten in Teilen:

```python
import time

# Variante 1: Nur Anzahl Zeilen zählen
start = time.time()
n_rows = 0
for chunk in pd.read_csv(file_path, chunksize=100000, usecols=['ID']):
    n_rows += len(chunk)
print(f"Anzahl Zeilen: {n_rows:,} (in {time.time()-start:.1f}s gezählt)")

# Variante 2: Nur relevante Spalten laden
relevant_cols = ['Severity', 'Start_Time', 'Start_Lat', 'Start_Lng',
                 'Temperature(F)', 'Humidity(%)', 'Visibility(mi)', 
                 'Weather_Condition', 'State']

df_sample = pd.read_csv(file_path, nrows=100000, usecols=relevant_cols)
print(f"\nSample-Shape: {df_sample.shape}")
print(f"Speicher: {df_sample.memory_usage().sum() / 1e6:.1f} MB")
```

---

## Aufgabe 3: Sampling-Strategien vergleichen

- [ ] Erledigt

Vergleiche verschiedene Sampling-Methoden:

```python
# Strategie 1: Zufälliges Sampling
np.random.seed(42)
sample_random = pd.read_csv(file_path, usecols=relevant_cols,
                             skiprows=lambda x: x > 0 and np.random.random() > 0.01)
print(f"Zufälliges Sample: {len(sample_random):,} Zeilen")

# Strategie 2: Systematisches Sampling (jede 100. Zeile)
sample_systematic = pd.read_csv(file_path, usecols=relevant_cols,
                                 skiprows=lambda x: x > 0 and x % 100 != 0)
print(f"Systematisches Sample: {len(sample_systematic):,} Zeilen")

# Strategie 3: Stratifiziertes Sampling (aus jeder Chunk)
sample_parts = []
for i, chunk in enumerate(pd.read_csv(file_path, usecols=relevant_cols, chunksize=500000)):
    sample_parts.append(chunk.sample(frac=0.02, random_state=42))
    if i >= 4:  # Nur erste 5 Chunks
        break

sample_stratified = pd.concat(sample_parts, ignore_index=True)
print(f"Stratifiziertes Sample: {len(sample_stratified):,} Zeilen")
```

---

## Aufgabe 4: Sample-Repräsentativität prüfen

- [ ] Erledigt

Ist das Sample repräsentativ?

```python
# Vergleiche Verteilungen
def compare_distributions(sample, col_name):
    """Vergleicht Sample mit theoretischer Gesamtverteilung."""
    print(f"\n{col_name}:")
    print(sample[col_name].value_counts(normalize=True).head(5).round(3))

compare_distributions(sample_stratified, 'Severity')
compare_distributions(sample_stratified, 'State')
```

---

## Aufgabe 5: Feature Engineering

- [ ] Erledigt

Extrahiere nützliche Features aus dem Zeitstempel:

```python
# Zeitliche Features
sample_stratified['Start_Time'] = pd.to_datetime(sample_stratified['Start_Time'])

sample_stratified['Hour'] = sample_stratified['Start_Time'].dt.hour
sample_stratified['DayOfWeek'] = sample_stratified['Start_Time'].dt.dayofweek
sample_stratified['Month'] = sample_stratified['Start_Time'].dt.month
sample_stratified['Year'] = sample_stratified['Start_Time'].dt.year

# Visualisierung
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 3, figsize=(14, 4))

sample_stratified['Hour'].value_counts().sort_index().plot(kind='bar', ax=axes[0])
axes[0].set_title('Unfälle nach Stunde')
axes[0].set_xlabel('Stunde')

sample_stratified['DayOfWeek'].value_counts().sort_index().plot(kind='bar', ax=axes[1])
axes[1].set_title('Unfälle nach Wochentag')
axes[1].set_xlabel('Tag (0=Mo)')

sample_stratified['Month'].value_counts().sort_index().plot(kind='bar', ax=axes[2])
axes[2].set_title('Unfälle nach Monat')
axes[2].set_xlabel('Monat')

plt.tight_layout()
plt.show()
```

---

## Aufgabe 6: Features für Clustering auswählen

- [ ] Erledigt

```python
# Numerische Features für Clustering
clustering_features = ['Hour', 'DayOfWeek', 'Temperature(F)', 'Humidity(%)', 'Visibility(mi)']

# Fehlende Werte behandeln
X = sample_stratified[clustering_features].dropna()
print(f"Datenpunkte für Clustering: {len(X):,}")

# Skalieren
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

---

## Aufgabe 7: Mini-Batch K-Means

- [ ] Erledigt

Mini-Batch K-Means ist optimiert für große Datenmengen:

```python
from sklearn.cluster import KMeans, MiniBatchKMeans
from sklearn.metrics import silhouette_score
import time

# Vergleich: Standard vs. Mini-Batch
print("Vergleich K-Means vs. Mini-Batch K-Means:")
print("=" * 50)

for k in [4, 6, 8]:
    # Standard K-Means
    start = time.time()
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels_km = kmeans.fit_predict(X_scaled)
    time_km = time.time() - start
    sil_km = silhouette_score(X_scaled, labels_km)
    
    # Mini-Batch K-Means
    start = time.time()
    mbkmeans = MiniBatchKMeans(n_clusters=k, random_state=42, batch_size=1024)
    labels_mb = mbkmeans.fit_predict(X_scaled)
    time_mb = time.time() - start
    sil_mb = silhouette_score(X_scaled, labels_mb)
    
    print(f"\nk={k}:")
    print(f"  K-Means:       {time_km:.2f}s, Silhouette={sil_km:.3f}")
    print(f"  Mini-Batch:    {time_mb:.2f}s, Silhouette={sil_mb:.3f}")
    print(f"  Speedup:       {time_km/time_mb:.1f}x schneller")
```

---

## Aufgabe 8: Partial Fit für sehr große Daten

- [ ] Erledigt

Bei Daten, die nicht in den Speicher passen:

```python
from sklearn.cluster import MiniBatchKMeans

# Mini-Batch K-Means mit Partial Fit
mbk = MiniBatchKMeans(n_clusters=5, random_state=42, batch_size=1024)

# Trainiere auf Chunks
print("Training auf Chunks:")
chunk_count = 0
for chunk in pd.read_csv(file_path, usecols=relevant_cols, chunksize=100000):
    # Feature Engineering
    chunk['Start_Time'] = pd.to_datetime(chunk['Start_Time'])
    chunk['Hour'] = chunk['Start_Time'].dt.hour
    chunk['DayOfWeek'] = chunk['Start_Time'].dt.dayofweek
    
    # Features extrahieren und skalieren
    X_chunk = chunk[clustering_features].dropna()
    if len(X_chunk) > 0:
        X_chunk_scaled = scaler.transform(X_chunk)  # Nutze bereits gefitteten Scaler!
        mbk.partial_fit(X_chunk_scaled)
        chunk_count += 1
        print(f"  Chunk {chunk_count} verarbeitet ({len(X_chunk):,} Zeilen)")
    
    if chunk_count >= 5:  # Stoppe nach 5 Chunks für Demo
        break

print(f"\nTraining abgeschlossen mit {chunk_count} Chunks")
print(f"Cluster-Zentren Shape: {mbk.cluster_centers_.shape}")
```

---

## Aufgabe 9: Ergebnisse interpretieren

- [ ] Erledigt

Analysiere die gefundenen Cluster:

```python
# Labels für Sample vorhersagen
labels = mbkmeans.predict(X_scaled)
sample_clustered = sample_stratified.copy()
sample_clustered = sample_clustered.loc[X.index]  # Nur Zeilen ohne NaN
sample_clustered['Cluster'] = labels

# Cluster-Profile
cluster_profiles = sample_clustered.groupby('Cluster')[clustering_features].mean()
print("Cluster-Profile:")
print(cluster_profiles.round(2))

# Visualisierung
import seaborn as sns
plt.figure(figsize=(12, 6))
cluster_profiles_norm = (cluster_profiles - cluster_profiles.mean()) / cluster_profiles.std()
sns.heatmap(cluster_profiles_norm.T, annot=True, cmap='RdYlGn', center=0, fmt='.2f')
plt.title('Unfall-Cluster Profile')
plt.xlabel('Cluster')
plt.tight_layout()
plt.show()
```

---

## Aufgabe 10: Speicheroptimierung

- [ ] Erledigt

```python
def optimize_dtypes(df):
    """Reduziere Speicherverbrauch durch optimierte Datentypen."""
    before = df.memory_usage().sum() / 1e6
    
    for col in df.select_dtypes(include=['float64']).columns:
        df[col] = df[col].astype('float32')
    
    for col in df.select_dtypes(include=['int64']).columns:
        if df[col].min() >= 0 and df[col].max() < 255:
            df[col] = df[col].astype('uint8')
        elif df[col].min() >= 0 and df[col].max() < 65535:
            df[col] = df[col].astype('uint16')
    
    after = df.memory_usage().sum() / 1e6
    print(f"Speicher: {before:.1f} MB → {after:.1f} MB ({(1-after/before)*100:.0f}% gespart)")
    return df

sample_optimized = optimize_dtypes(sample_stratified.copy())
```

---

## Aufgabe 11: Reflexion

- [ ] Erledigt

!!! question "Reflexionsfragen"
    
    1. Ab welcher Datengröße werden diese Techniken notwendig?
    
    2. Wann ist Sampling vertretbar? Wann nicht?
    
    3. Was sind die Trade-offs bei Mini-Batch K-Means?
    
    4. Wie würdest du die Analyse in der echten Welt fortsetzen?

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **`MemoryError` beim Laden**  
    → Nutze `usecols` um nur benötigte Spalten zu laden:
    ```python
    pd.read_csv(file, usecols=['col1', 'col2'])
    ```
    
    ❌ **Sample nicht repräsentativ**  
    → Stratifiziertes Sampling nach wichtigen Kategorien:
    ```python
    from sklearn.model_selection import train_test_split
    sample, _ = train_test_split(df, stratify=df['State'], train_size=0.01)
    ```
    
    ❌ **Mini-Batch K-Means mit zu kleiner `batch_size`**  
    → Minimum ~1000, sonst instabile Ergebnisse
    
    ❌ **Datetime nicht geparst**  
    →
    ```python
    pd.read_csv(..., parse_dates=['Start_Time'])
    ```
    
    ❌ **Clustering ohne Feature-Selection**  
    → Der Datensatz hat ~47 Spalten! Wähle 5-10 relevante Features aus

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Sampling:** Zufällig, systematisch, stratifiziert
    - **Chunked Processing:** `read_csv(chunksize=...)` + `partial_fit()`
    - **Mini-Batch K-Means:** 10-100x schneller als Standard
    - **Speicheroptimierung:** float64 → float32, int64 → uint8
    - "Mehr Daten" ≠ "bessere Ergebnisse"

---

## Weiterführende Ressourcen

- [scikit-learn: Scaling Strategies](https://scikit-learn.org/stable/modules/scaling_strategies.html)
- [Pandas: Enhancing Performance](https://pandas.pydata.org/docs/user_guide/enhancingperf.html)
- [Dask für Out-of-Core Computing](https://docs.dask.org/)
