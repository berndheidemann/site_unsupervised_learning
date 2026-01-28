# UL-09: Musik-Clustering – Projektvorbereitung

!!! abstract "Advance Organizer"
    Das Abschlussprojekt steht vor der Tür! In diesem Arbeitsblatt erkundest du den Spotify-Datensatz und entwickelst deine Strategie. Mit ~100.000 Tracks ist das der größte Datensatz bisher – du musst also auch über Effizienz nachdenken.
    
    **Dein Ziel:** Du hast den Datensatz verstanden, eine Fragestellung formuliert und erste Clustering-Ergebnisse erzielt. Damit bist du für das Abschlussprojekt vorbereitet.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Einen größeren Datensatz eigenständig analysieren
- [x] Feature-Auswahl und -Engineering durchführen
- [x] Eine sinnvolle Clustering-Strategie entwickeln

---

## Der Datensatz

Der **Spotify Tracks** Datensatz enthält Audio-Features von Songs:

| Feature | Beschreibung | Wertebereich |
|---------|-------------|--------------|
| danceability | Tanzbarkeit | 0.0 - 1.0 |
| energy | Energie/Intensität | 0.0 - 1.0 |
| key | Tonart | 0-11 (C, C#, D, ...) |
| loudness | Lautstärke | -60 bis 0 dB |
| mode | Dur (1) oder Moll (0) | 0, 1 |
| speechiness | Sprachanteil | 0.0 - 1.0 |
| acousticness | Akustische Eigenschaften | 0.0 - 1.0 |
| instrumentalness | Instrumental (keine Vocals) | 0.0 - 1.0 |
| liveness | Live-Aufnahme wahrscheinlich | 0.0 - 1.0 |
| valence | Stimmung (fröhlich/traurig) | 0.0 - 1.0 |
| tempo | Geschwindigkeit | 0 - 250 BPM |
| duration_ms | Länge in Millisekunden | variabel |

---

## Aufgabe 1: Daten laden und Überblick verschaffen

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Spotify-Datensatz laden
df = pd.read_csv('spotify_tracks.csv')

print(f"Anzahl Tracks: {len(df):,}")
print(f"Anzahl Features: {len(df.columns)}")
print(f"\nSpeicherverbrauch: {df.memory_usage().sum() / 1e6:.1f} MB")

# Spaltenübersicht
print("\nSpalten:")
print(df.columns.tolist())

# Erste Zeilen
df.head()
```

**Verschaffe dir einen Überblick:**

a) Wie viele Tracks und wie viele Features hat der Datensatz?

b) Welche Spalten sind numerisch, welche kategorial (z.B. Artist, Genre)?

c) Gibt es fehlende Werte? Prüfe mit `df.isnull().sum()`

---

## Aufgabe 2: Audio-Features verstehen

Visualisiere die Verteilungen der Audio-Features:

```python
audio_features = ['danceability', 'energy', 'loudness', 'speechiness', 
                  'acousticness', 'instrumentalness', 'liveness', 
                  'valence', 'tempo']

fig, axes = plt.subplots(3, 3, figsize=(14, 10))
axes = axes.flatten()

for i, feature in enumerate(audio_features):
    df[feature].hist(bins=50, ax=axes[i], edgecolor='black', alpha=0.7)
    axes[i].set_title(feature)
    axes[i].set_xlabel(feature)

plt.tight_layout()
plt.show()
```

**Analysiere:**

a) Welche Features sind normalverteilt?

b) Welche Features haben eine schiefe Verteilung?

c) Gibt es Features mit vielen Nullwerten (z.B. instrumentalness)?

---

## Aufgabe 3: Sampling-Strategie entwickeln

Bei ~100.000 Tracks wird Clustering langsam. Entwickle eine Sampling-Strategie:

```python
# Zufälliges Sample für Experimente
np.random.seed(42)
sample_size = 20000  # Starte mit 20k für schnelles Experimentieren

df_sample = df.sample(n=sample_size, random_state=42)
print(f"Sample-Größe: {len(df_sample):,} Tracks")

# Prüfe: Ist das Sample repräsentativ?
# Vergleiche Mittelwerte
comparison = pd.DataFrame({
    'Gesamt': df[audio_features].mean(),
    'Sample': df_sample[audio_features].mean()
})
comparison['Differenz %'] = abs(comparison['Gesamt'] - comparison['Sample']) / comparison['Gesamt'] * 100
print(comparison.round(3))
```

!!! tip "Sample-Größe"
    - **Exploration:** 10.000 - 20.000 (schnell)
    - **Finale Analyse:** 50.000+ (genauer)
    - **Vollständig:** Nur wenn nötig (langsam)

---

## Aufgabe 4: Feature-Korrelationen analysieren

```python
# Korrelationsmatrix
corr = df_sample[audio_features].corr()

plt.figure(figsize=(10, 8))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0, fmt='.2f')
plt.title('Feature-Korrelationen')
plt.tight_layout()
plt.show()

# Stark korrelierte Features
print("Stark korrelierte Features (|r| > 0.5):")
for i in range(len(corr)):
    for j in range(i+1, len(corr)):
        if abs(corr.iloc[i,j]) > 0.5:
            print(f"  {corr.index[i]} <-> {corr.columns[j]}: {corr.iloc[i,j]:.2f}")
```

**Analysiere die Korrelationen:**

a) Welche Feature-Paare sind stark korreliert? Könnten diese redundant sein?

b) Gibt es Features, die mit fast keinem anderen Feature korrelieren? Was könnte das bedeuten?

c) Welche Features würdest du basierend auf der Korrelationsanalyse ausschließen?

---

## Aufgabe 5: Feature-Auswahl treffen

Wähle die Features für dein Clustering:

```python
# Option 1: Alle Audio-Features
selected_features = audio_features.copy()

# Option 2: Subset ohne stark korrelierte
# selected_features = ['danceability', 'energy', 'speechiness', 
#                      'acousticness', 'instrumentalness', 'valence', 'tempo']

# Option 3: Nur die "musikalischen" Features
# selected_features = ['danceability', 'energy', 'valence', 'acousticness']

print(f"Ausgewählte Features: {selected_features}")
```

**Deine Entscheidung:**

a) Welche Option wählst du und warum?

b) Wie viele Features bleiben übrig?

c) Welche Features hast du bewusst ausgeschlossen und warum?

---

## Aufgabe 6: Erste Clustering-Versuche

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# Daten vorbereiten
X = df_sample[selected_features].dropna()
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Elbow + Silhouette
results = []
for k in range(2, 12):
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels = kmeans.fit_predict(X_scaled)
    results.append({
        'k': k,
        'inertia': kmeans.inertia_,
        'silhouette': silhouette_score(X_scaled, labels)
    })

results_df = pd.DataFrame(results)

fig, axes = plt.subplots(1, 2, figsize=(12, 4))
axes[0].plot(results_df['k'], results_df['inertia'], 'bo-')
axes[0].set_xlabel('k')
axes[0].set_ylabel('Inertia')
axes[0].set_title('Elbow-Methode')

axes[1].bar(results_df['k'], results_df['silhouette'])
axes[1].set_xlabel('k')
axes[1].set_ylabel('Silhouette')
axes[1].set_title('Silhouette Score')

plt.tight_layout()
plt.show()
```

**Analysiere die Plots:**

a) Wo liegt der "Ellbogen" in der Elbow-Kurve?

b) Bei welchem k ist der Silhouette Score am höchsten?

c) Stimmen beide Methoden überein? Wenn nicht, welchem Ergebnis vertraust du mehr?

Nutze PCA für die Visualisierung:

```python
from sklearn.decomposition import PCA

# K-Means mit gewähltem k
k = 5  # Anpassen nach deiner Analyse!
kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
labels = kmeans.fit_predict(X_scaled)

# PCA für 2D-Visualisierung
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

plt.figure(figsize=(12, 8))
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], c=labels, cmap='viridis', alpha=0.5)
plt.xlabel(f'PC1 ({pca.explained_variance_ratio_[0]:.1%})')
plt.ylabel(f'PC2 ({pca.explained_variance_ratio_[1]:.1%})')
plt.title('Spotify Tracks Clustering')
plt.colorbar(scatter, label='Cluster')
plt.show()

print(f"Erklärte Varianz (2 Komponenten): {pca.explained_variance_ratio_.sum():.1%}")
```

**Betrachte die Visualisierung:**

a) Sind die Cluster visuell klar getrennt oder überlappen sie stark?

b) Wie viel Varianz erklären die 2 PCA-Komponenten? Ist das ausreichend für eine zuverlässige 2D-Darstellung?

c) Gibt es Ausreißer, die zu keinem Cluster zu gehören scheinen?

---

## Aufgabe 8: Cluster interpretieren

Was charakterisiert die verschiedenen Cluster?

```python
# Cluster-Profile
df_sample_clustered = df_sample.copy()
df_sample_clustered['Cluster'] = labels

cluster_profiles = df_sample_clustered.groupby('Cluster')[selected_features].mean()

# Heatmap
plt.figure(figsize=(14, 8))
cluster_profiles_norm = (cluster_profiles - cluster_profiles.mean()) / cluster_profiles.std()
sns.heatmap(cluster_profiles_norm.T, annot=True, cmap='RdYlGn', center=0, fmt='.2f')
plt.title('Cluster-Profile (normalisiert)')
plt.xlabel('Cluster')
plt.tight_layout()
plt.show()

# Beschreibungen notieren
print("\nCluster-Beschreibungen:")
for c in range(k):
    profile = cluster_profiles.loc[c]
    print(f"\nCluster {c}:")
    print(f"  - Danceability: {profile['danceability']:.2f}")
    print(f"  - Energy: {profile['energy']:.2f}")
    print(f"  - Valence: {profile['valence']:.2f}")
```

**Interpretiere die Cluster:**

a) Welches Cluster könnte "Party-Musik" sein? (hohe Energy, hohe Danceability)

b) Welches Cluster könnte "Entspannungsmusik" sein? (niedrige Energy, hohe Acousticness)

c) Gibt es ein Cluster, das schwer zu interpretieren ist? Warum?

d) Gib jedem Cluster einen aussagekräftigen Namen:

---

## Aufgabe 9: Fragestellung für Abschlussprojekt formulieren

!!! question "Mögliche Fragestellungen"
    
    Wähle eine Fragestellung für dein Abschlussprojekt:
    
    1. **Musik-Typen:** Welche "Musik-Typen" gibt es? (z.B. Party, Entspannung, Workout)
    
    2. **Genre-Entdeckung:** Kann man Genres durch Clustering "entdecken"?  
       (Vergleiche mit dem `genre`-Feld, falls vorhanden)
    
    3. **Playlist-Empfehlungen:** Wie könnte Spotify diese Cluster für Empfehlungen nutzen?
    
    4. **Zeitliche Entwicklung:** Unterscheiden sich Cluster für verschiedene Jahrzehnte?
       (Falls `year` oder `release_date` vorhanden)
    
    5. **Eigene Idee:** ___________________________

**Deine gewählte Fragestellung:**

> _______________________________________________________

---

## Aufgabe 10: Nächste Schritte planen

Plane dein Abschlussprojekt:

- [ ] Sample-Größe für finale Analyse festlegen
- [ ] Feature-Auswahl finalisieren
- [ ] Optimales k bestimmen
- [ ] Algorithmus(en) wählen
- [ ] Visualisierungen planen
- [ ] Interpretation/Story vorbereiten

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **`MemoryError` bei 100k Tracks**  
    → Arbeite mit Sample:
    ```python
    df_sample = df.sample(n=20000, random_state=42)
    ```
    
    ❌ **Kategoriale Features (Genre, Artist) ignoriert**  
    → Diese können später zur Validierung dienen!
    ```python
    # Prüfe Genre-Verteilung pro Cluster
    df_sample_clustered.groupby('Cluster')['genre'].value_counts()
    ```
    
    ❌ **Tempo extrem unterschiedlich (60-200 BPM)**  
    → Skalierung ist hier besonders wichtig! Prüfe die Verteilung.
    
    ❌ **Keine klare Fragestellung**  
    → Formuliere eine konkrete Frage, bevor du loslegst!
    
    ❌ **Zu viele Cluster gewählt**  
    → 5-8 Cluster sind meist interpretierbar, mehr wird unübersichtlich.

---

## Checkliste für die Vorbereitung

- [ ] Datensatz geladen und verstanden
- [ ] Audio-Features exploriert
- [ ] Sampling-Strategie entwickelt
- [ ] Feature-Korrelationen analysiert
- [ ] Feature-Auswahl getroffen
- [ ] Erste Clustering-Versuche durchgeführt
- [ ] Cluster visualisiert
- [ ] Erste Interpretationen notiert
- [ ] Fragestellung formuliert

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Umgang mit größeren Datensätzen durch Sampling
    - Systematische Feature-Analyse für Audio-Daten
    - Entwicklung einer eigenen Analysestrategie
    - Vorbereitung eines eigenständigen Projekts

---

## Nächste Schritte

Du bist jetzt bereit für das Abschlussprojekt! Dort führst du eine vollständige Analyse durch und präsentierst deine Ergebnisse.

➡️ Weiter zu [UL-10: Abschlussprojekt](ul-10-abschlussprojekt.md)
