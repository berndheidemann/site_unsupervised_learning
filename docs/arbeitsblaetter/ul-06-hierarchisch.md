# UL-06: Hierarchisches Clustering

!!! abstract "Advance Organizer"
    K-Means zwingt dich, die Clusteranzahl vorher festzulegen. Hierarchisches Clustering zeigt dir stattdessen eine Baumstruktur (Dendrogramm), aus der du verschiedene Ebenen ablesen kannst. Das ist besonders nützlich, wenn du die "natürliche" Struktur der Daten entdecken möchtest.
    
    **Dein Ziel:** Du verstehst den Unterschied zwischen K-Means und hierarchischem Clustering und kannst für einen Use Case entscheiden, welcher Algorithmus besser passt.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Hierarchisches Clustering verstehen und erklären
- [x] Dendrogramme erstellen und interpretieren
- [x] Verschiedene Linkage-Methoden vergleichen

---

## Aufgabe 1: Daten vorbereiten

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
from sklearn.cluster import AgglomerativeClustering

# Daten laden und skalieren
df = pd.read_csv('Country-data.csv')
numeric_cols = df.select_dtypes(include=[np.number]).columns.tolist()

X = df[numeric_cols]
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

countries = df['country'].values
```

**Prüfe dein Setup:**

a) Wie viele Länder und wie viele Features hat der Datensatz?

b) Warum ist Skalierung auch für hierarchisches Clustering wichtig?

---

## Aufgabe 2: Linkage-Matrix berechnen

Hierarchisches Clustering baut einen Baum auf, in dem ähnliche Punkte schrittweise zusammengefügt werden:

```python
# Linkage-Matrix berechnen (Ward-Methode)
Z = linkage(X_scaled, method='ward')

# Struktur der Linkage-Matrix
print("Linkage-Matrix Shape:", Z.shape)
print("\nBeispiel der ersten Merge-Schritte:")
print("Cluster 1 | Cluster 2 | Distanz | Anzahl Punkte")
print(Z[:5].round(2))
```

**Analysiere die Linkage-Matrix:**

a) Wie viele Zeilen hat die Matrix? (Tipp: Bei n Datenpunkten gibt es n-1 Merge-Schritte)

b) Schau dir die ersten Merge-Schritte an: Welche Distanzen haben die ersten Zusammenführungen? Sind das ähnliche oder unähnliche Punkte?

c) Wie verändert sich die Distanz in Spalte 2 über die Zeilen hinweg?

**Die Linkage-Matrix erklärt:**

| Spalte | Bedeutung |
|--------|-----------|
| 0 | Index des ersten zusammengeführten Clusters |
| 1 | Index des zweiten zusammengeführten Clusters |
| 2 | Distanz zwischen den Clustern |
| 3 | Anzahl Punkte im neuen Cluster |

---

## Aufgabe 3: Dendrogramm erstellen

Das Dendrogramm visualisiert die hierarchische Struktur:

```python
plt.figure(figsize=(16, 8))

# Dendrogramm mit Ländernamen
dendrogram(Z, 
           labels=countries,
           leaf_rotation=90,
           leaf_font_size=8)

plt.title('Dendrogramm der Länder (Ward-Methode)')
plt.xlabel('Länder')
plt.ylabel('Distanz')
plt.tight_layout()
plt.show()
```

**Analysiere das Dendrogramm:**

a) Bei welcher Distanz (y-Achse) würde das Dendrogramm in 2 Cluster zerfallen? Bei welcher in 3?

b) Gibt es Länder, die besonders früh (unten) zusammengeführt werden? Welche sind das?

c) Gibt es Länder, die erst sehr spät (oben) zusammengeführt werden? Was bedeutet das?

**Wie liest man ein Dendrogramm?**

- **Horizontale Linien:** Zusammenführung zweier Cluster
- **Höhe der Linien:** Distanz (je höher, desto unähnlicher)
- **Vertikale Äste:** Einzelne Datenpunkte oder Cluster

---

## Aufgabe 4: Schnitt auf gewünschter Höhe

Du kannst das Dendrogramm auf verschiedenen Höhen "schneiden":

```python
fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Dendrogramm mit Schnittlinien
for ax, n_clusters, color in [(axes[0], 3, 'red'), (axes[1], 5, 'green')]:
    dendrogram(Z, labels=countries, leaf_rotation=90, leaf_font_size=8, ax=ax)
    
    # Schwellenwert für n Cluster finden
    labels_temp = fcluster(Z, n_clusters, criterion='maxclust')
    threshold = Z[-(n_clusters-1), 2]
    
    ax.axhline(y=threshold, color=color, linestyle='--', linewidth=2, 
               label=f'Schnitt für {n_clusters} Cluster')
    ax.legend()
    ax.set_title(f'{n_clusters} Cluster')

plt.tight_layout()
plt.show()
```

**Vergleiche die beiden Schnitte:**

a) Bei welcher Höhe liegt die rote Linie (3 Cluster)? Bei welcher die grüne (5 Cluster)?

b) Welcher Schnitt erscheint dir "natürlicher" – wo gibt es größere Lücken zwischen den Clustern?

c) Was ist der Vorteil davon, dass du die Clusteranzahl *nachträglich* festlegen kannst?

Extrahiere die Cluster-Labels für eine bestimmte Anzahl:

```python
# 3 Cluster extrahieren
n_clusters = 3
labels_hc = fcluster(Z, n_clusters, criterion='maxclust')

# Achtung: fcluster gibt Labels ab 1 (nicht 0!)
labels_hc = labels_hc - 1  # Anpassen auf 0-indexiert

df['Cluster_HC'] = labels_hc
print(df['Cluster_HC'].value_counts().sort_index())
```

**Alternative mit AgglomerativeClustering:**

```python
agg = AgglomerativeClustering(n_clusters=3, linkage='ward')
labels_agg = agg.fit_predict(X_scaled)

df['Cluster_Agg'] = labels_agg
print(df['Cluster_Agg'].value_counts().sort_index())
```

**Vergleiche die beiden Methoden:**

a) Liefern `fcluster()` und `AgglomerativeClustering` die gleichen Ergebnisse bei gleicher Clusteranzahl?

b) Wann würdest du welche Methode verwenden? (Tipp: `fcluster` braucht die Linkage-Matrix, `AgglomerativeClustering` nicht)

---

## Aufgabe 6: Vergleich K-Means vs. Hierarchisches Clustering

Vergleiche die Ergebnisse beider Methoden:

```python
from sklearn.cluster import KMeans
from sklearn.metrics import adjusted_rand_score

# K-Means
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
labels_km = kmeans.fit_predict(X_scaled)

# Hierarchisch (Ward)
agg = AgglomerativeClustering(n_clusters=3, linkage='ward')
labels_hc = agg.fit_predict(X_scaled)

# Übereinstimmung
ari = adjusted_rand_score(labels_km, labels_hc)
print(f"Adjusted Rand Index: {ari:.3f}")

# Crosstab für detaillierten Vergleich
print("\nVergleich der Zuordnungen:")
print(pd.crosstab(labels_km, labels_hc, 
                  rownames=['K-Means'], 
                  colnames=['Hierarchisch']))
```

**Fragen:**

a) Wie stark stimmen die beiden Methoden überein?

b) Welche Länder werden unterschiedlich zugeordnet?

```python
# Unterschiedlich zugeordnete Länder finden
df_temp = df.copy()
df_temp['KM'] = labels_km
df_temp['HC'] = labels_hc
disagree = df_temp[df_temp['KM'] != df_temp['HC']]
print(f"\n{len(disagree)} Länder mit unterschiedlicher Zuordnung:")
print(disagree[['country', 'KM', 'HC']])
```

---

## Aufgabe 7: Verschiedene Linkage-Methoden vergleichen

Es gibt mehrere Methoden, die Distanz zwischen Clustern zu berechnen:

```python
methods = ['single', 'complete', 'average', 'ward']

fig, axes = plt.subplots(2, 2, figsize=(16, 12))

for ax, method in zip(axes.flatten(), methods):
    Z_method = linkage(X_scaled, method=method)
    dendrogram(Z_method, 
               labels=countries,
               leaf_rotation=90,
               leaf_font_size=6,
               ax=ax,
               truncate_mode='lastp',
               p=30)  # Nur letzte 30 Merges zeigen
    ax.set_title(f'Linkage-Methode: {method}')

plt.tight_layout()
plt.show()
```

**Vergleiche die Linkage-Methoden:**

a) Welche Methode erzeugt die "kettenförmigsten" Cluster (ein langes, dünnes Cluster)? Warum?

b) Welche Methode erzeugt die "ausgewogensten" Cluster (ähnliche Größe)?

c) Für welche Anwendungsfälle könnte `single` trotzdem sinnvoll sein?

**Linkage-Methoden erklärt:**

| Methode | Distanz zwischen Clustern |
|---------|---------------------------|
| **single** | Kürzeste Distanz zwischen Punkten |
| **complete** | Längste Distanz zwischen Punkten |
| **average** | Durchschnittliche Distanz |
| **ward** | Minimiert Varianz im Cluster |

**Frage:** Welche Methode liefert die "ausgewogensten" Cluster?

---

## Aufgabe 8: Vor- und Nachteile diskutieren

Fülle die Tabelle basierend auf deinen Erfahrungen:

| Aspekt | K-Means | Hierarchisches Clustering |
|--------|---------|---------------------------|
| Muss k vorher festgelegt werden? | _________ | _________ |
| Laufzeit bei großen Daten | _________ | _________ |
| Cluster-Form | _________ | _________ |
| Deterministisch? | _________ | _________ |
| Zusätzliche Einblicke | _________ | _________ |

!!! tip "Wann welchen Algorithmus?"
    - **K-Means:** Wenn du schnelle Ergebnisse brauchst und die ungefähre Clusteranzahl kennst
    - **Hierarchisch:** Wenn du die Struktur der Daten erkunden willst oder unsicher über k bist

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **Dendrogramm zu unübersichtlich**  
    → Nutze `truncate_mode='lastp'`:
    ```python
    dendrogram(Z, truncate_mode='lastp', p=30)
    ```
    
    ❌ **Linkage-Objekt nicht verstanden**  
    → `linkage()` gibt eine Matrix zurück, die du an `dendrogram()` oder `fcluster()` übergibst.
    
    ❌ **`fcluster` liefert falsche Anzahl**  
    → Prüfe den Parameter `t` (Schwellenwert) oder nutze `criterion='maxclust'`:
    ```python
    # Nach Distanz-Schwelle:
    fcluster(Z, t=10, criterion='distance')
    
    # Nach Anzahl Cluster:
    fcluster(Z, n_clusters, criterion='maxclust')
    ```
    
    ❌ **Vergleich mit K-Means nicht sinnvoll**  
    → Nutze dieselbe Clusteranzahl für beide Methoden!

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Hierarchisches Clustering baut eine Baumstruktur auf
    - **Dendrogramm:** Visualisiert die hierarchische Struktur
    - **Linkage-Methoden:** single, complete, average, ward
    - Du kannst nachträglich entscheiden, wie viele Cluster du haben möchtest
    - Ward-Linkage liefert oft die "ausgewogensten" Cluster

---

## Nächste Schritte

Du hast jetzt zwei wichtige Clustering-Algorithmen kennengelernt. Im nächsten Arbeitsblatt wendest du alles Gelernte eigenständig auf einen neuen Datensatz an!

➡️ Weiter zu [UL-07: Kundensegmentierung](ul-07-kundensegmentierung.md)
