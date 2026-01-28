# UL-OPT-01: Kreditkartenanalyse *(Optional)*

!!! info "Referenz-Material"
    Dieses Arbeitsblatt enthält vollständigen Code als **Referenz und Nachschlagewerk**. 
    Der Fokus liegt auf dem Verständnis fortgeschrittener Techniken (DBSCAN, GMM), nicht auf der eigenständigen Implementierung.
    
    **Empfehlung:** Arbeite den Code durch, experimentiere mit Parametern, und nutze ihn als Vorlage für eigene Projekte.

!!! abstract "Advance Organizer"
    Dieser Datensatz ist der komplexeste bisher: 17 Features, viele Korrelationen, und die Frage nach Risikokunden erfordert besondere Aufmerksamkeit für Ausreißer. Hier lernst du DBSCAN und GMM praktisch anzuwenden – Algorithmen, die bei K-Means nicht gut funktionierenden Datenstrukturen glänzen können.
    
    **Dein Ziel:** Du beherrschst den Umgang mit hochdimensionalen Daten und kannst verschiedene Algorithmen gezielt einsetzen.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Komplexere Datensätze mit vielen Features analysieren
- [x] Feature-Engineering und -Selection anwenden
- [x] DBSCAN und Gaussian Mixture Models praktisch nutzen

---

## Das Szenario

!!! example "Ausgangssituation"
    Eine Bank möchte ihre Kreditkartenkunden segmentieren, um:
    
    1. **Personalisierte Angebote** zu entwickeln
    2. **Risikokunden** zu identifizieren (hohe Balance, wenig Zahlungen)
    3. **Cross-Selling-Potenziale** zu erkennen
    
    **Deine Aufgabe:** Analysiere das Kundenverhalten und erstelle eine Segmentierung.

---

## Der Datensatz

Der **CustomerData** Datensatz enthält Kreditkartenverhalten von ~900 Kunden:

| Feature | Beschreibung |
|---------|--------------|
| CUST_ID | Kundennummer |
| BALANCE | Kontostand |
| BALANCE_FREQUENCY | Häufigkeit der Kontostand-Updates |
| PURCHASES | Gesamtkäufe |
| ONEOFF_PURCHASES | Einmalkäufe |
| INSTALLMENTS_PURCHASES | Ratenkäufe |
| CASH_ADVANCE | Bargeldabhebungen |
| PURCHASES_FREQUENCY | Kaufhäufigkeit |
| ONEOFF_PURCHASES_FREQUENCY | Einmalkauf-Häufigkeit |
| PURCHASES_INSTALLMENTS_FREQUENCY | Ratenkauf-Häufigkeit |
| CASH_ADVANCE_FREQUENCY | Bargeldabhebungs-Häufigkeit |
| CASH_ADVANCE_TRX | Anzahl Bargeldabhebungen |
| PURCHASES_TRX | Anzahl Käufe |
| CREDIT_LIMIT | Kreditlimit |
| PAYMENTS | Zahlungen |
| MINIMUM_PAYMENTS | Mindestzahlungen |
| PRC_FULL_PAYMENT | Anteil vollständiger Zahlungen |
| TENURE | Kundenalter (Monate) |

---

## Aufgabe 1: Daten laden und explorieren

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler

# Daten laden
df = pd.read_csv('CustomerData.csv')

print(f"Anzahl Kunden: {len(df)}")
print(f"Anzahl Features: {len(df.columns)}")
print(f"\nFehlende Werte:")
print(df.isnull().sum()[df.isnull().sum() > 0])

# Deskriptive Statistik
df.describe()
```

**Aufgaben:**

a) Wie viele fehlende Werte gibt es? Welche Strategie wählst du?

b) Welche Features haben besonders hohe Varianz?

---

## Aufgabe 2: Feature-Korrelationen analysieren

Bei 17 Features sind Korrelationen wahrscheinlich:

```python
# ID entfernen, numerische Features
numeric_cols = df.select_dtypes(include=[np.number]).columns.drop('CUST_ID', errors='ignore')

# Korrelationsmatrix
corr = df[numeric_cols].corr()

plt.figure(figsize=(14, 12))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0, fmt='.1f', annot_kws={'size': 8})
plt.title('Feature-Korrelationen')
plt.tight_layout()
plt.show()

# Stark korrelierte Feature-Paare (> 0.8)
high_corr = []
for i in range(len(corr)):
    for j in range(i+1, len(corr)):
        if abs(corr.iloc[i,j]) > 0.8:
            high_corr.append((corr.index[i], corr.columns[j], corr.iloc[i,j]))

print("\nStark korrelierte Features (|r| > 0.8):")
for f1, f2, r in sorted(high_corr, key=lambda x: abs(x[2]), reverse=True):
    print(f"  {f1} <-> {f2}: {r:.2f}")
```

**Frage:** Welche Features würdest du entfernen oder zusammenfassen?

---

## Aufgabe 3: Dimensionsreduktion vor dem Clustering

Bei vielen Features ist PCA sinnvoll:

```python
from sklearn.decomposition import PCA

# Daten vorbereiten
X = df[numeric_cols].dropna()
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# PCA mit allen Komponenten
pca_full = PCA()
pca_full.fit(X_scaled)

# Scree Plot
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

axes[0].bar(range(1, len(pca_full.explained_variance_ratio_)+1), 
            pca_full.explained_variance_ratio_)
axes[0].set_xlabel('Komponente')
axes[0].set_ylabel('Erklärte Varianz')
axes[0].set_title('Scree Plot')

cumvar = np.cumsum(pca_full.explained_variance_ratio_)
axes[1].plot(range(1, len(cumvar)+1), cumvar, 'bo-')
axes[1].axhline(y=0.9, color='r', linestyle='--', label='90%')
axes[1].set_xlabel('Anzahl Komponenten')
axes[1].set_ylabel('Kumulierte Varianz')
axes[1].set_title('Kumulierte erklärte Varianz')
axes[1].legend()

plt.tight_layout()
plt.show()

# Wie viele Komponenten für 90%?
n_components = np.argmax(cumvar >= 0.9) + 1
print(f"\n{n_components} Komponenten erklären 90% der Varianz")
```

---

## Aufgabe 4: DBSCAN anwenden

DBSCAN kann Ausreißer (potenzielle Risikokunden) identifizieren:

```python
from sklearn.cluster import DBSCAN
from sklearn.neighbors import NearestNeighbors

# PCA mit gewählter Komponentenzahl
pca = PCA(n_components=5)  # Anpassen!
X_pca = pca.fit_transform(X_scaled)

# k-distance Plot für eps-Bestimmung
nn = NearestNeighbors(n_neighbors=10)
nn.fit(X_pca)
distances, _ = nn.kneighbors(X_pca)
k_distances = np.sort(distances[:, -1])

plt.figure(figsize=(10, 5))
plt.plot(k_distances)
plt.xlabel('Punkte (sortiert)')
plt.ylabel('Distanz zum 10. Nachbarn')
plt.title('k-Distance Plot für eps-Bestimmung')
plt.grid(True)
plt.show()
```

```python
# DBSCAN mit gewählten Parametern
eps_value = 2.0  # Aus k-distance Plot ablesen!
dbscan = DBSCAN(eps=eps_value, min_samples=10)
labels_db = dbscan.fit_predict(X_pca)

n_clusters = len(set(labels_db)) - (1 if -1 in labels_db else 0)
n_outliers = (labels_db == -1).sum()

print(f"Gefundene Cluster: {n_clusters}")
print(f"Ausreißer: {n_outliers} ({n_outliers/len(labels_db)*100:.1f}%)")
```

---

## Aufgabe 5: Ausreißer analysieren

Sind die Ausreißer wirklich "Risikokunden"?

```python
# Ausreißer vs. Normale
df_analysis = df.copy()
df_analysis['is_outlier'] = labels_db == -1

# Vergleich
comparison = df_analysis.groupby('is_outlier')[numeric_cols].mean().T
comparison.columns = ['Normale', 'Ausreißer']
comparison['Differenz %'] = (comparison['Ausreißer'] - comparison['Normale']) / comparison['Normale'] * 100

print("Vergleich Ausreißer vs. Normale Kunden:")
print(comparison.sort_values('Differenz %', ascending=False).round(1))
```

**Analyse:**

a) Was zeichnet die Ausreißer aus?

b) Sind das wirklich Risikokunden?

---

## Aufgabe 6: Gaussian Mixture Models

GMM kann "weiche" Cluster finden (Wahrscheinlichkeiten statt harte Zuordnung):

```python
from sklearn.mixture import GaussianMixture

# Optimale Anzahl Komponenten mit BIC
bics = []
for n in range(2, 10):
    gmm = GaussianMixture(n_components=n, random_state=42)
    gmm.fit(X_pca)
    bics.append((n, gmm.bic(X_pca)))

bic_df = pd.DataFrame(bics, columns=['n_components', 'BIC'])
plt.figure(figsize=(8, 4))
plt.plot(bic_df['n_components'], bic_df['BIC'], 'bo-')
plt.xlabel('Anzahl Komponenten')
plt.ylabel('BIC (niedriger = besser)')
plt.title('GMM: Modellauswahl mit BIC')
plt.show()
```

```python
# GMM mit optimalem n
n_opt = 4  # Anpassen basierend auf BIC!
gmm = GaussianMixture(n_components=n_opt, random_state=42)
labels_gmm = gmm.fit_predict(X_pca)

# Wahrscheinlichkeiten
probs = gmm.predict_proba(X_pca)
print(f"\nWahrscheinlichkeits-Matrix Shape: {probs.shape}")
print("\nBeispiel - Erste 5 Kunden:")
print(pd.DataFrame(probs[:5], columns=[f'Cluster_{i}' for i in range(n_opt)]).round(3))
```

---

## Aufgabe 7: Algorithmen vergleichen

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# K-Means zum Vergleich
kmeans = KMeans(n_clusters=n_opt, random_state=42, n_init=10)
labels_km = kmeans.fit_predict(X_pca)

# Silhouette Scores
print("Silhouette Scores:")
print(f"  K-Means: {silhouette_score(X_pca, labels_km):.3f}")
print(f"  GMM:     {silhouette_score(X_pca, labels_gmm):.3f}")

# DBSCAN nur für Nicht-Ausreißer
if n_outliers < len(labels_db):
    mask = labels_db != -1
    print(f"  DBSCAN:  {silhouette_score(X_pca[mask], labels_db[mask]):.3f} (ohne Ausreißer)")
```

---

## Aufgabe 8: Kundenprofile erstellen

Erstelle aussagekräftige Kundenprofile:

```python
# K-Means oder GMM Labels verwenden
df_final = df.copy()
df_final['Cluster'] = labels_km

# Cluster-Profile
profiles = df_final.groupby('Cluster')[numeric_cols].mean()

# Heatmap
plt.figure(figsize=(14, 10))
profiles_norm = (profiles - profiles.mean()) / profiles.std()
sns.heatmap(profiles_norm.T, annot=True, cmap='RdYlGn', center=0, fmt='.2f')
plt.title('Kundenprofile (Z-Score normalisiert)')
plt.xlabel('Cluster')
plt.tight_layout()
plt.show()
```

---

## Aufgabe 9: Cluster interpretieren und benennen

Gib den Clustern aussagekräftige Namen:

```python
# Beispiel-Interpretation (anpassen!)
cluster_names = {
    0: '___________',  # z.B. "Premium-Kunden"
    1: '___________',  # z.B. "Gelegenheitskäufer"
    2: '___________',  # z.B. "Bargeld-Nutzer"
    3: '___________'   # z.B. "Risikokunden"
}

df_final['Segment'] = df_final['Cluster'].map(cluster_names)

# Zusammenfassung
print("Kundensegmente:")
for cluster_id, name in cluster_names.items():
    count = (df_final['Cluster'] == cluster_id).sum()
    print(f"\n{name} ({count} Kunden):")
    profile = profiles.loc[cluster_id]
    print(f"  - Balance: ${profile['BALANCE']:,.0f}")
    print(f"  - Käufe: ${profile['PURCHASES']:,.0f}")
    print(f"  - Zahlungen: ${profile['PAYMENTS']:,.0f}")
```

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **17 Features auf einmal clustern**  
    → Starte mit PCA oder Feature-Selection (5-8 wichtigste Features)
    
    ❌ **DBSCAN-Parameter blind gewählt**  
    → Nutze k-distance Plot zur eps-Bestimmung:
    ```python
    from sklearn.neighbors import NearestNeighbors
    nn = NearestNeighbors(n_neighbors=10)
    distances, _ = nn.kneighbors(X)
    plt.plot(sorted(distances[:,-1]))
    ```
    
    ❌ **GMM ohne Kovarianz-Typ getestet**  
    → Probiere verschiedene:
    ```python
    GaussianMixture(covariance_type='full')   # Flexibelste
    GaussianMixture(covariance_type='diag')   # Schneller
    GaussianMixture(covariance_type='spherical')  # Wie K-Means
    ```
    
    ❌ **Ausreißer = Risikokunden?**  
    → Nicht automatisch! Prüfe, ob Ausreißer wirklich problematisch sind.

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Umgang mit hochdimensionalen Daten (17 Features)
    - PCA zur Dimensionsreduktion vor Clustering
    - DBSCAN für Ausreißer-Erkennung
    - GMM für "weiche" Cluster mit Wahrscheinlichkeiten
    - Systematischer Algorithmen-Vergleich

---

## Weiter zu

➡️ [UL-OPT-02: Big Data & Clustering](ul-opt-02-big-data.md)
