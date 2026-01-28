# UL-05: Cluster-Interpretation

!!! abstract "Advance Organizer"
    Cluster zu finden ist nur die halbe Miete – jetzt musst du sie verstehen! Was unterscheidet die Gruppen? Warum sind bestimmte Datenpunkte zusammen? Diese Interpretation ist der wichtigste Schritt für jeden Business Case, denn nur so kannst du Handlungsempfehlungen ableiten.
    
    **Dein Ziel:** Du kannst Cluster-Profile erstellen und inhaltlich interpretieren. Das ist die Basis für jede Präsentation deiner Ergebnisse.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Cluster inhaltlich interpretieren
- [x] Cluster-Profile erstellen und visualisieren
- [x] Ergebnisse für Nicht-Techniker aufbereiten

---

## Aufgabe 1: Daten vorbereiten und clustern

Zuerst führen wir das Clustering durch (Wiederholung):

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

# Daten laden
df = pd.read_csv('Country-data.csv')
numeric_cols = df.select_dtypes(include=[np.number]).columns.tolist()

# Skalieren und clustern
X = df[numeric_cols]
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
df['Cluster'] = kmeans.fit_predict(X_scaled)

# Übersicht
print(df['Cluster'].value_counts().sort_index())
```

**Prüfe dein Ergebnis:**

a) Wie viele Länder sind in jedem Cluster? Sind die Cluster einigermaßen ausgewogen?

b) Was würde passieren, wenn ein Cluster nur 2-3 Länder enthält? Wäre das ein Problem?

---

## Aufgabe 2: Mittelwerte pro Cluster berechnen

Das Cluster-Profil zeigt die typischen Eigenschaften jeder Gruppe:

```python
# Mittelwerte pro Cluster (auf Original-Daten!)
cluster_means = df.groupby('Cluster')[numeric_cols].mean()
print(cluster_means.round(2))
```

**Fragen:**

a) Welches Cluster hat das höchste durchschnittliche Einkommen (`income`)?

b) Welches Cluster hat die höchste Kindersterblichkeit (`child_mort`)?

c) Wie unterscheidet sich die Lebenserwartung (`life_expec`) zwischen den Clustern?

---

## Aufgabe 3: Cluster-Profile als Balkendiagramm

Visualisiere die Unterschiede zwischen den Clustern:

```python
# Für alle Features
fig, axes = plt.subplots(3, 3, figsize=(14, 12))
axes = axes.flatten()

for i, col in enumerate(numeric_cols):
    df.boxplot(column=col, by='Cluster', ax=axes[i])
    axes[i].set_title(col)
    axes[i].set_xlabel('Cluster')

plt.suptitle('Feature-Verteilungen pro Cluster', fontsize=14, y=1.02)
plt.tight_layout()
plt.show()
```

**Analysiere die Boxplots:**

a) Bei welchen Features sind die Unterschiede zwischen den Clustern am größten?

b) Gibt es Features, bei denen sich die Cluster kaum unterscheiden? Was bedeutet das?

c) Welches Cluster hat die größte Streuung (Variabilität) innerhalb der Gruppe?

```python
# Mittelwerte normalisieren für Vergleichbarkeit
cluster_means_norm = cluster_means.apply(lambda x: (x - x.mean()) / x.std())

cluster_means_norm.T.plot(kind='bar', figsize=(12, 6))
plt.xlabel('Feature')
plt.ylabel('Normalisierter Mittelwert (Z-Score)')
plt.title('Cluster-Profile (normalisiert)')
plt.legend(title='Cluster')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

---

## Aufgabe 4: Heatmap der Cluster-Profile

Eine Heatmap zeigt auf einen Blick, welche Features pro Cluster hoch oder niedrig sind:

```python
# Heatmap der normalisierten Mittelwerte
plt.figure(figsize=(12, 8))
sns.heatmap(cluster_means_norm.T, 
            annot=True, 
            cmap='RdYlGn', 
            center=0,
            fmt='.2f',
            linewidths=0.5)
plt.title('Cluster-Profile (Z-Score normalisiert)')
plt.xlabel('Cluster')
plt.ylabel('Feature')
plt.tight_layout()
plt.show()
```

**Interpretiere die Heatmap:**

a) Welches Cluster zeigt durchgehend grüne (positive) Werte? Was bedeutet das inhaltlich?

b) Welches Cluster zeigt durchgehend rote (negative) Werte? 

c) Gibt es ein Cluster mit gemischten Werten (teils grün, teils rot)? Was könnte das bedeuten?

- **Grün/Positive Werte:** Überdurchschnittlich hoch
- **Rot/Negative Werte:** Unterdurchschnittlich niedrig
- **Weiß/Null:** Durchschnittlich

---

## Aufgabe 5: Länder den Clustern zuordnen

Welche Länder sind in welchem Cluster?

```python
# Länder pro Cluster
for cluster_id in sorted(df['Cluster'].unique()):
    countries = df[df['Cluster'] == cluster_id]['country'].tolist()
    print(f"\n=== Cluster {cluster_id} ({len(countries)} Länder) ===")
    print(', '.join(countries[:10]))  # Erste 10
    if len(countries) > 10:
        print(f"... und {len(countries)-10} weitere")
```

**Analyse:**

a) Erkennst du ein Muster? Welche Art von Ländern ist in welchem Cluster?

b) Gibt es Überraschungen? Länder, die du in einem anderen Cluster erwartet hättest?

---

## Aufgabe 6: Cluster benennen

!!! success "Wichtig für die Praxis"
    Cluster-Nummern (0, 1, 2) sind bedeutungslos! Gib den Clustern aussagekräftige Namen.

Basierend auf deiner Analyse, wie würdest du die Cluster benennen?

```python
# Beispiel-Mapping (anpassen nach deiner Analyse!)
cluster_names = {
    0: '___________',  # z.B. "Industrieländer"
    1: '___________',  # z.B. "Entwicklungsländer"  
    2: '___________'   # z.B. "Schwellenländer"
}

df['Cluster_Name'] = df['Cluster'].map(cluster_names)
print(df[['country', 'Cluster', 'Cluster_Name']].head(20))
```

**Tipps für gute Namen:**

- Beschreibe die Hauptmerkmale
- Verwende verständliche Begriffe (keine Fachsprache)
- Vermeide wertende Begriffe wenn möglich

---

## Aufgabe 7: Ergebnisse zusammenfassen

Erstelle eine Zusammenfassung für einen Nicht-Techniker:

```python
# Executive Summary erstellen
print("="*60)
print("CLUSTER-ANALYSE: LÄNDERGRUPPEN")
print("="*60)

for cluster_id in sorted(df['Cluster'].unique()):
    cluster_df = df[df['Cluster'] == cluster_id]
    name = cluster_names.get(cluster_id, f'Cluster {cluster_id}')
    
    print(f"\n{name.upper()} ({len(cluster_df)} Länder)")
    print("-"*40)
    print(f"Typische Merkmale:")
    print(f"  - Einkommen: ${cluster_df['income'].mean():,.0f}")
    print(f"  - Lebenserwartung: {cluster_df['life_expec'].mean():.1f} Jahre")
    print(f"  - Kindersterblichkeit: {cluster_df['child_mort'].mean():.1f} pro 1000")
    print(f"\nBeispielländer: {', '.join(cluster_df['country'].head(5).tolist())}")
```

---

## Aufgabe 8: Reflexion

!!! question "Reflexionsfragen"
    
    1. Wie würdest du die gefundenen Cluster einem Manager erklären (ohne technische Begriffe)?
    
    2. Welche Handlungsempfehlungen könnte man aus den Clustern ableiten?  
       (z.B. für eine Hilfsorganisation: "Fokussiere Ressourcen auf Cluster X wegen...")
    
    3. Sind die Cluster "wahr"? Oder könnten andere Clusteranzahlen genauso sinnvoll sein?
    
    4. Welche zusätzlichen Daten würden die Analyse verbessern?

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **Mittelwerte auf skalierten Daten berechnet**  
    → Die skalierten Werte sind nicht interpretierbar! Nutze Original-Daten:
    ```python
    # Falsch: Mittelwerte der X_scaled
    # Richtig: Mittelwerte der Original-Daten
    df.groupby('Cluster')[numeric_cols].mean()
    ```
    
    ❌ **Cluster-Labels stimmen nicht überein**  
    → Die Label-Nummern sind willkürlich! Cluster 0 heute kann morgen Cluster 2 sein.
    
    ❌ **Bar-Chart unleserlich**  
    → Normalisiere auf Z-Scores oder nutze Heatmap:
    ```python
    sns.heatmap(cluster_means_norm.T, ...)
    ```
    
    ❌ **Keine sinnvolle Interpretation**  
    → Schau dir extreme Werte an: Welches Feature unterscheidet die Cluster am stärksten?
    ```python
    # Varianz zwischen Clustern
    cluster_means.var()
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Cluster-Profile:** Mittelwerte der Features pro Cluster
    - **Heatmaps:** Auf einen Blick sehen, was Cluster unterscheidet
    - **Normalisierung:** Z-Scores für Vergleichbarkeit
    - **Benennung:** Cluster brauchen aussagekräftige Namen!
    - Die inhaltliche Interpretation ist der wichtigste Schritt für Business Value

---

## Nächste Schritte

K-Means erfordert, dass du k vorher festlegst. Im nächsten Arbeitsblatt lernst du hierarchisches Clustering kennen – dort entscheidest du erst nachträglich!

➡️ Weiter zu [UL-06: Hierarchisches Clustering](ul-06-hierarchisch.md)
