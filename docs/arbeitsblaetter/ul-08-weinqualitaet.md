# UL-08: Weinqualität-Analyse

!!! abstract "Advance Organizer"
    Mehr Features bedeuten mehr Komplexität: Welche Features sind wichtig? Welche sind redundant? Hier lernst du, mit höherdimensionalen Daten umzugehen und verschiedene Algorithmen systematisch zu vergleichen. Außerdem begegnest du zum ersten Mal dem Silhouette Score als objektive Bewertungsmetrik.
    
    **Dein Ziel:** Du kannst Feature-Korrelationen analysieren, Algorithmen vergleichen und deine Entscheidungen mit Metriken begründen.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Mit mehr Features arbeiten (11 statt 5)
- [x] Feature-Korrelationen verstehen und nutzen
- [x] Verschiedene Algorithmen systematisch vergleichen

---

## Das Szenario

!!! example "Ausgangssituation"
    Ein Weinhändler möchte sein Sortiment besser strukturieren. Statt nach Herkunftsregion oder Preis will er Weine nach ihren chemischen Eigenschaften gruppieren. So kann er ähnliche Weine zusammen vermarkten und Kunden passende Alternativen empfehlen.
    
    **Deine Aufgabe:** Finde Gruppen von Weinen mit ähnlichen Eigenschaften.

---

## Der Datensatz

Der **Wine Quality** Datensatz enthält chemische Analysen von portugiesischem Wein (rot und weiß):

| Feature | Beschreibung |
|---------|--------------|
| fixed acidity | Feste Säuren (g/l) |
| volatile acidity | Flüchtige Säuren (g/l) |
| citric acid | Zitronensäure (g/l) |
| residual sugar | Restzucker (g/l) |
| chlorides | Chloride (g/l) |
| free sulfur dioxide | Freies Schwefeldioxid (mg/l) |
| total sulfur dioxide | Gesamtes Schwefeldioxid (mg/l) |
| density | Dichte (g/ml) |
| pH | pH-Wert |
| sulphates | Sulfate (g/l) |
| alcohol | Alkoholgehalt (%) |
| quality | Qualitätsbewertung (0-10) |

---

## Aufgabe 1: Daten laden und vorbereiten

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler

# Wine Quality Datensätze laden
# Option 1: Nur Rotwein
df_red = pd.read_csv('winequality-red.csv', sep=';')
df_red['type'] = 'red'

# Option 2: Nur Weißwein
df_white = pd.read_csv('winequality-white.csv', sep=';')
df_white['type'] = 'white'

# Option 3: Beide zusammen
df = pd.concat([df_red, df_white], ignore_index=True)

print(f"Rotwein: {len(df_red)} Proben")
print(f"Weißwein: {len(df_white)} Proben")
print(f"Gesamt: {len(df)} Proben")
```

**Deine ersten Beobachtungen:**

a) Wie viele Rot- und wie viele Weißweine gibt es? Ist das Verhältnis ausgeglichen?

b) Prüfe mit `df.info()` und `df.describe()`: Gibt es fehlende Werte? Gibt es Auffälligkeiten in den Wertebereichen?

**Entscheidung:** Analysierst du Rot- und Weißwein zusammen oder getrennt? Begründe!

!!! tip "Tipp"
    Für den Anfang empfehle ich, mit Rotwein zu starten (weniger Daten, schnelleres Experimentieren).

---

## Aufgabe 2: Feature-Korrelationen analysieren

Bei 11 Features ist es wichtig, Redundanzen zu erkennen:

```python
# Korrelationsmatrix
features = df.select_dtypes(include=[np.number]).columns.drop('quality')
corr = df[features].corr()

# Heatmap
plt.figure(figsize=(12, 10))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0, fmt='.2f')
plt.title('Feature-Korrelationen')
plt.tight_layout()
plt.show()

# Stark korrelierte Feature-Paare finden (|r| > 0.7)
high_corr = []
for i in range(len(corr)):
    for j in range(i+1, len(corr)):
        if abs(corr.iloc[i,j]) > 0.7:
            high_corr.append((corr.index[i], corr.columns[j], corr.iloc[i,j]))

print("\nStark korrelierte Features:")
for f1, f2, r in high_corr:
    print(f"  {f1} <-> {f2}: r = {r:.2f}")
```

**Fragen:**

a) Welche Features sind stark korreliert?

b) Sollte man beide behalten oder eines entfernen? Begründe!

---

## Aufgabe 3: Feature-Auswahl treffen

Entscheide, welche Features du für das Clustering verwendest:

```python
# Welche Option wählst du? Begründe deine Entscheidung!

# Option A: Alle Features
# Option B: Hoch korrelierte entfernen 
# Option C: PCA zur Reduktion nutzen

# Trage deine Auswahl ein (Beispiel - passe nach deiner Analyse an!):
selected_features = ['fixed acidity', 'volatile acidity', 'citric acid', 
                     'residual sugar', 'chlorides', 'free sulfur dioxide',
                     'density', 'pH', 'sulphates', 'alcohol']

X = df[selected_features]
print(f"Verwendete Features: {len(selected_features)}")
print(selected_features)

# Skalieren
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

**Begründung deiner Auswahl:** (schreibe 2-3 Sätze)

---

## Aufgabe 4: Clustering mit K-Means

Führe die Elbow- und Silhouette-Analyse selbstständig durch:

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# Erstelle Listen für die Ergebnisse
results = []
K_range = range(2, 10)

# Implementiere die Schleife selbst!
# Für jedes k:
#   1. Erstelle KMeans-Modell
#   2. Trainiere und hole Labels
#   3. Berechne Silhouette Score
#   4. Speichere Ergebnisse

for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels = kmeans.fit_predict(X_scaled)
    sil = silhouette_score(X_scaled, labels)
    results.append({
        'k': k,
        'inertia': kmeans.inertia_,
        'silhouette': sil
    })

# Visualisierung (diesen Teil kannst du so übernehmen)
results_df = pd.DataFrame(results)

fig, axes = plt.subplots(1, 2, figsize=(12, 4))

axes[0].plot(results_df['k'], results_df['inertia'], 'bo-')
axes[0].set_xlabel('Anzahl Cluster')
axes[0].set_ylabel('Inertia')
axes[0].set_title('Elbow-Methode')

axes[1].bar(results_df['k'], results_df['silhouette'], color='steelblue')
axes[1].set_xlabel('Anzahl Cluster')
axes[1].set_ylabel('Silhouette Score')
axes[1].set_title('Silhouette-Analyse')

plt.tight_layout()
plt.show()

print(results_df)
```

**Analysiere die Ergebnisse:**

a) Bei welchem k liegt der "Ellbogen"?

b) Bei welchem k ist der Silhouette Score am höchsten?

c) Welches k wählst du? Begründe!

---

## Aufgabe 5: Hierarchisches Clustering

Vergleiche mit hierarchischem Clustering:

```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage

# Linkage berechnen
Z = linkage(X_scaled, method='ward')

# Dendrogramm (bei vielen Daten truncated)
plt.figure(figsize=(14, 6))
dendrogram(Z, truncate_mode='lastp', p=50)
plt.title('Dendrogramm (Ward-Linkage)')
plt.xlabel('Cluster')
plt.ylabel('Distanz')
plt.show()

# AgglomerativeClustering
agg = AgglomerativeClustering(n_clusters=4, linkage='ward')  # Passe k an!
labels_hc = agg.fit_predict(X_scaled)

print(f"\nHierarchisches Clustering:")
print(pd.Series(labels_hc).value_counts().sort_index())
```

**Analysiere das Dendrogramm:**

a) Bei welcher Distanz würde das Dendrogramm in 3, 4 oder 5 Cluster zerfallen?

b) Gibt es einen "natürlichen" Schnitt, wo die Distanz plötzlich stark ansteigt?

c) Stimmt deine Wahl von k mit dem Dendrogramm überein?

---

## Aufgabe 6: Algorithmen vergleichen

Vergleiche K-Means mit hierarchischem Clustering:

```python
# Dein gewähltes k aus der Analyse:
k_optimal = 4  # Passe nach deiner Analyse an!

# K-Means
kmeans = KMeans(n_clusters=k_optimal, random_state=42, n_init=10)
labels_km = kmeans.fit_predict(X_scaled)

# Hierarchisches Clustering
from sklearn.cluster import AgglomerativeClustering

agg = AgglomerativeClustering(n_clusters=k_optimal, linkage='ward')
labels_hc = agg.fit_predict(X_scaled)

# Silhouette Scores vergleichen
sil_km = silhouette_score(X_scaled, labels_km)
sil_hc = silhouette_score(X_scaled, labels_hc)

print(f"Silhouette Score:")
print(f"  K-Means:      {sil_km:.3f}")
print(f"  Hierarchisch: {sil_hc:.3f}")
```

**Fragen:**

a) Welcher Algorithmus liefert den höheren Silhouette Score?

b) Wie stark stimmen die Ergebnisse überein? (Nutze `adjusted_rand_score`)

---

## Aufgabe 7: Cluster-Profile erstellen

Analysiere, was die Cluster charakterisiert:

```python
# K-Means Labels verwenden
df['Cluster'] = labels_km

# Cluster-Profile
cluster_means = df.groupby('Cluster')[selected_features].mean()

# Heatmap
plt.figure(figsize=(14, 8))
# Normalisieren für bessere Visualisierung
cluster_means_norm = (cluster_means - cluster_means.mean()) / cluster_means.std()
sns.heatmap(cluster_means_norm.T, annot=True, cmap='RdYlGn', center=0, fmt='.2f')
plt.title('Cluster-Profile (Z-Score normalisiert)')
plt.xlabel('Cluster')
plt.tight_layout()
plt.show()
```

**Analyse:**

Welche Eigenschaften zeichnen die verschiedenen Cluster aus?

| Cluster | Charakteristik |
|---------|---------------|
| 0 | _______________ |
| 1 | _______________ |
| 2 | _______________ |
| 3 | _______________ |

---

## Aufgabe 8: Cluster mit Qualität vergleichen

Die Variable `quality` ist ein Label, das wir nicht fürs Clustering genutzt haben. Prüfe, ob es einen Zusammenhang gibt:

```python
# Qualitätsverteilung pro Cluster
plt.figure(figsize=(10, 6))
df.boxplot(column='quality', by='Cluster')
plt.title('Weinqualität pro Cluster')
plt.suptitle('')
plt.xlabel('Cluster')
plt.ylabel('Qualität')
plt.show()

# Durchschnittliche Qualität pro Cluster
print(df.groupby('Cluster')['quality'].agg(['mean', 'std', 'count']))
```

**Fragen:**

a) Gibt es Cluster mit höherer durchschnittlicher Qualität?

b) Kann man das Clustering für Qualitätsvorhersage nutzen?

---

## Zusatzaufgabe: DBSCAN ausprobieren

Findet DBSCAN Ausreißer (z.B. besonders gute oder schlechte Weine)?

```python
from sklearn.cluster import DBSCAN

# DBSCAN – Parameter müssen angepasst werden!
dbscan = DBSCAN(eps=2.0, min_samples=10)
labels_db = dbscan.fit_predict(X_scaled)

n_clusters = len(set(labels_db)) - (1 if -1 in labels_db else 0)
n_outliers = (labels_db == -1).sum()

print(f"Gefundene Cluster: {n_clusters}")
print(f"Ausreißer: {n_outliers}")

# Ausreißer analysieren
if n_outliers > 0:
    outliers = df[labels_db == -1]
    print(f"\nAusreißer-Qualität: {outliers['quality'].mean():.2f} (vs. {df['quality'].mean():.2f} Durchschnitt)")
```

!!! tip "DBSCAN-Parameter"
    - `eps` zu klein → zu viele Ausreißer
    - `eps` zu groß → alles ein Cluster
    - Experimentiere mit verschiedenen Werten!

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **Rot- und Weißwein gemischt ohne Kennzeichnung**  
    → Füge eine `type`-Spalte hinzu oder analysiere getrennt
    
    ❌ **Zu viele korrelierte Features**  
    → Entferne hoch korrelierte Features (> 0.8) oder nutze PCA:
    ```python
    from sklearn.decomposition import PCA
    pca = PCA(n_components=0.95)  # 95% Varianz erhalten
    X_reduced = pca.fit_transform(X_scaled)
    ```
    
    ❌ **Silhouette Score falsch interpretiert**  
    → Höher ist besser! Werte:
    - nahe 1 = sehr gute Trennung
    - nahe 0 = überlappende Cluster
    - negativ = fehlerhafte Zuordnung
    
    ❌ **DBSCAN findet nur 1 Cluster oder nur Ausreißer**  
    → Parameter anpassen. Nutze k-distance Plot:
    ```python
    from sklearn.neighbors import NearestNeighbors
    nn = NearestNeighbors(n_neighbors=10)
    nn.fit(X_scaled)
    distances, _ = nn.kneighbors(X_scaled)
    plt.plot(sorted(distances[:,-1]))
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Feature-Korrelationen analysieren und redundante Features identifizieren
    - Systematischer Vergleich mehrerer Algorithmen
    - Silhouette Score als objektive Metrik
    - DBSCAN zur Ausreißer-Erkennung
    - Clustering-Ergebnisse mit externen Labels validieren

---

## Nächste Schritte

Sehr gut! Du hast jetzt Erfahrung mit verschiedenen Algorithmen und Datensätzen. Im nächsten Arbeitsblatt bereitest du dich auf das Abschlussprojekt vor – mit einem noch größeren Datensatz.

➡️ Weiter zu [UL-09: Musik-Clustering](ul-09-musik-clustering.md)
