# UL-04: Dimensionsreduktion mit PCA

!!! abstract "Advance Organizer"
    Wie visualisiert man 9 Features gleichzeitig? Gar nicht – unser Gehirn kann maximal 3 Dimensionen verarbeiten. PCA (Principal Component Analysis) komprimiert viele Features auf wenige "Hauptkomponenten", ohne zu viel Information zu verlieren. So kannst du Cluster endlich sehen!
    
    **Dein Ziel:** Du kannst PCA anwenden und verstehst, was die Hauptkomponenten bedeuten. Diese Technik wirst du im Abschlussprojekt brauchen.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Das PCA-Prinzip verstehen und erklären
- [x] Dimensionsreduktion durchführen
- [x] Cluster in 2D visualisieren

---

## Aufgabe 1: PCA am Iris-Dataset

Das Iris-Dataset hat 4 Features – perfekt, um PCA zu verstehen:

```python
from sklearn.datasets import load_iris
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
import numpy as np

# Daten laden und skalieren
iris = load_iris()
X = iris.data
y = iris.target

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# PCA: 4D → 2D
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

print(f"Original Shape: {X_scaled.shape}")
print(f"Nach PCA Shape: {X_pca.shape}")
```

**Frage:** Von wie vielen auf wie viele Dimensionen wurde reduziert?

---

## Aufgabe 2: Erklärte Varianz analysieren

Wie viel Information haben wir durch die Reduktion verloren?

```python
# Erklärte Varianz
print("Erklärte Varianz pro Komponente:")
for i, var in enumerate(pca.explained_variance_ratio_):
    print(f"  PC{i+1}: {var:.2%}")

print(f"\nGesamte erklärte Varianz: {pca.explained_variance_ratio_.sum():.2%}")

# Visualisierung
plt.figure(figsize=(8, 4))
plt.bar(range(1, 3), pca.explained_variance_ratio_, color='steelblue', edgecolor='black')
plt.xlabel('Hauptkomponente')
plt.ylabel('Erklärte Varianz')
plt.title('Erklärte Varianz pro Hauptkomponente')
plt.xticks([1, 2])
plt.show()
```

**Analyse:**

a) Wie viel Prozent der ursprünglichen Information ist in den 2 Hauptkomponenten erhalten?

b) Ist das genug? (Faustregel: >70% ist akzeptabel, >85% ist gut)

---

## Aufgabe 3: Cluster in 2D visualisieren

Jetzt können wir die echten Iris-Klassen in 2D sehen:

```python
plt.figure(figsize=(10, 8))

# Scatterplot mit echten Labels
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], 
                       c=y, cmap='viridis', alpha=0.8, edgecolors='black')

plt.xlabel('Hauptkomponente 1')
plt.ylabel('Hauptkomponente 2')
plt.title('Iris-Dataset nach PCA (echte Klassen)')
plt.colorbar(scatter, label='Klasse')
plt.legend(*scatter.legend_elements(), title='Spezies')
plt.show()
```

**Beobachtungen:**

a) Sind die drei Spezies klar voneinander getrennt?

b) Welche zwei Spezies überlappen am meisten?

---

## Aufgabe 4: K-Means auf PCA-Daten

Vergleiche K-Means auf Original-Daten vs. PCA-reduzierte Daten:

```python
from sklearn.cluster import KMeans
from sklearn.metrics import adjusted_rand_score

# K-Means auf Original-Daten (4 Features)
kmeans_orig = KMeans(n_clusters=3, random_state=42, n_init=10)
labels_orig = kmeans_orig.fit_predict(X_scaled)

# K-Means auf PCA-Daten (2 Features)
kmeans_pca = KMeans(n_clusters=3, random_state=42, n_init=10)
labels_pca = kmeans_pca.fit_predict(X_pca)

# Vergleich mit echten Labels
print(f"ARI (Original): {adjusted_rand_score(y, labels_orig):.3f}")
print(f"ARI (PCA):      {adjusted_rand_score(y, labels_pca):.3f}")
```

**Frage:** Macht PCA die Cluster besser oder schlechter erkennbar?

---

## Aufgabe 5: Country-Daten mit PCA visualisieren

Jetzt wenden wir PCA auf die Country-Daten an (9 Features → 2):

```python
import pandas as pd

# Daten laden und vorbereiten
df = pd.read_csv('Country-data.csv')
X = df.select_dtypes(include=[np.number])
countries = df['country'].values

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# K-Means durchführen
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)  # Experimentiere mit k!
labels = kmeans.fit_predict(X_scaled)

# PCA für Visualisierung (auf 2 Dimensionen reduzieren)
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

print(f"Erklärte Varianz: {pca.explained_variance_ratio_.sum():.2%}")
```

**Deine Aufgaben:**

a) Führe den Code aus. Wie viel Prozent der Varianz wird durch 2 Komponenten erklärt? Ist das akzeptabel?

b) Ändere `n_clusters` auf 4 oder 5 – was passiert mit der Visualisierung? Welches k passt am besten zu den sichtbaren Gruppen?

c) Vergleiche die Visualisierung mit deinem Elbow-Ergebnis aus UL-03 – bestätigt der Plot deine k-Wahl?

**Visualisierung:**

```python
plt.figure(figsize=(12, 8))
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], 
                       c=labels, cmap='viridis', alpha=0.7, edgecolors='black')

# Einige Ländernamen annotieren
for i, country in enumerate(countries):
    if i % 10 == 0:  # Jeden 10. Namen zeigen
        plt.annotate(country, (X_pca[i, 0], X_pca[i, 1]), fontsize=8, alpha=0.7)

plt.xlabel(f'PC1 ({pca.explained_variance_ratio_[0]:.1%} Varianz)')
plt.ylabel(f'PC2 ({pca.explained_variance_ratio_[1]:.1%} Varianz)')
plt.title('Länder-Clustering nach PCA')
plt.colorbar(scatter, label='Cluster')
plt.show()
```

---

## Aufgabe 6: Was bedeuten die Hauptkomponenten?

Welche Original-Features stecken in den Hauptkomponenten?

```python
# Loadings: Gewichte der Original-Features
loadings = pd.DataFrame(
    pca.components_.T,
    index=X.columns,
    columns=['PC1', 'PC2']
)
print("Feature-Loadings:")
print(loadings.round(3))

# Als Heatmap
plt.figure(figsize=(8, 6))
import seaborn as sns
sns.heatmap(loadings, annot=True, cmap='coolwarm', center=0)
plt.title('Feature-Loadings der Hauptkomponenten')
plt.tight_layout()
plt.show()
```

**Interpretiere:**

a) Welche Features haben den größten Einfluss auf PC1?

b) Welche Features haben den größten Einfluss auf PC2?

c) Was "repräsentiert" PC1 inhaltlich? (z.B. "Wohlstand" wenn income, gdpp, life_expec hoch laden)

---

## Aufgabe 7: Scree Plot – Wie viele Komponenten?

Für die Country-Daten: Wie viele Komponenten brauchen wir?

```python
# PCA mit allen Komponenten
pca_full = PCA()
pca_full.fit(X_scaled)

# Scree Plot
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Erklärte Varianz pro Komponente
axes[0].bar(range(1, len(pca_full.explained_variance_ratio_)+1), 
            pca_full.explained_variance_ratio_, 
            color='steelblue', edgecolor='black')
axes[0].set_xlabel('Hauptkomponente')
axes[0].set_ylabel('Erklärte Varianz')
axes[0].set_title('Scree Plot')

# Kumulierte Varianz
cumvar = np.cumsum(pca_full.explained_variance_ratio_)
axes[1].plot(range(1, len(cumvar)+1), cumvar, 'bo-', linewidth=2)
axes[1].axhline(y=0.9, color='r', linestyle='--', label='90% Schwelle')
axes[1].axhline(y=0.95, color='orange', linestyle='--', label='95% Schwelle')
axes[1].set_xlabel('Anzahl Komponenten')
axes[1].set_ylabel('Kumulierte erklärte Varianz')
axes[1].set_title('Kumulierte erklärte Varianz')
axes[1].legend()

plt.tight_layout()
plt.show()

print("\nKumulierte Varianz:")
for i, var in enumerate(cumvar):
    print(f"  {i+1} Komponenten: {var:.1%}")
```

**Fragen:**

a) Wie viele Komponenten braucht man für 90% der Varianz?

b) Wie viele für 95%?

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **Daten nicht skaliert vor PCA**  
    → PCA ist extrem empfindlich gegenüber unterschiedlichen Skalen!
    ```python
    # IMMER vorher skalieren:
    scaler = StandardScaler()
    X_scaled = scaler.fit_transform(X)
    pca.fit(X_scaled)
    ```
    
    ❌ **Zu wenig Varianz erklärt (<70%)**  
    → Verwende mehr Komponenten:
    ```python
    PCA(n_components=3)  # oder mehr
    ```
    
    ❌ **`explained_variance_ratio_` nicht verstanden**  
    → Die Summe zeigt, wie viel Information erhalten bleibt:
    ```python
    # z.B. 0.85 = 85% der Original-Information ist erhalten
    print(pca.explained_variance_ratio_.sum())
    ```
    
    ❌ **Scatterplot ohne Farben**  
    → Färbe nach Cluster-Label:
    ```python
    plt.scatter(X_pca[:,0], X_pca[:,1], c=labels, cmap='viridis')
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - PCA reduziert Dimensionen bei minimalem Informationsverlust
    - **Explained Variance Ratio:** Zeigt, wie viel Information erhalten bleibt
    - **Scree Plot:** Hilft bei der Wahl der Komponentenzahl
    - **Loadings:** Zeigen, welche Original-Features in den Komponenten stecken
    - Für Visualisierung meist 2-3 Komponenten, für Analyse oft mehr nötig

---

## Nächste Schritte

Du kannst jetzt Cluster finden und visualisieren. Aber was bedeuten sie? Im nächsten Arbeitsblatt lernst du, Cluster zu interpretieren.

➡️ Weiter zu [UL-05: Cluster-Interpretation](ul-05-interpretation.md)
