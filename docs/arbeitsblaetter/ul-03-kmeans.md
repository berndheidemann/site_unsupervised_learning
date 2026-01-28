# UL-03: K-Means Clustering

!!! abstract "Advance Organizer"
    K-Means ist DER Standard-Algorithmus für Clustering – einfach zu verstehen, schnell zu berechnen, und in 90% der Fälle ein guter Startpunkt. Die Herausforderung: Du musst vorher wissen, wie viele Cluster du suchst! Die Elbow-Methode hilft dir dabei.
    
    **Dein Ziel:** Du kannst K-Means anwenden und die optimale Clusteranzahl begründet wählen. Das ist die Kernkompetenz für alle weiteren Clustering-Aufgaben.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Den K-Means Algorithmus verstehen und erklären
- [x] Die Elbow-Methode zur Bestimmung der Clusteranzahl anwenden
- [x] K-Means Clustering mit scikit-learn durchführen

---

## Aufgabe 1: K-Means "per Hand" verstehen

- [ ] Erledigt

Bevor wir K-Means anwenden, verstehen wir den Algorithmus am **Iris-Dataset** – einem einfachen Datensatz mit nur 4 Features und 3 bekannten Klassen.

```python
from sklearn.datasets import load_iris
import pandas as pd
import numpy as np

# Iris laden
iris = load_iris()
X_iris = iris.data[:, :2]  # Nur 2 Features für Visualisierung

print(f"Shape: {X_iris.shape}")
print(f"Bekannte Klassen: {iris.target_names}")
```

**K-Means Algorithmus Schritt für Schritt:**

```python
import matplotlib.pyplot as plt

# Schritt 1: Zufällige Startzentren wählen
k = 3
centers = X_iris[np.random.choice(len(X_iris), k, replace=False)]

plt.figure(figsize=(10, 4))

# Iteration 0: Startzustand
plt.subplot(1, 3, 1)
plt.scatter(X_iris[:,0], X_iris[:,1], c='gray', alpha=0.5)
plt.scatter(centers[:,0], centers[:,1], c=['red','green','blue'], marker='X', s=200)
plt.title('Schritt 1: Zufällige Zentren')

# Schritt 2: Punkte zum nächsten Zentrum zuweisen
from scipy.spatial.distance import cdist
distances = cdist(X_iris, centers)
labels = distances.argmin(axis=1)

plt.subplot(1, 3, 2)
plt.scatter(X_iris[:,0], X_iris[:,1], c=labels, cmap='viridis', alpha=0.5)
plt.scatter(centers[:,0], centers[:,1], c=['red','green','blue'], marker='X', s=200)
plt.title('Schritt 2: Punkte zuweisen')

# Schritt 3: Zentren neu berechnen
new_centers = np.array([X_iris[labels==i].mean(axis=0) for i in range(k)])

plt.subplot(1, 3, 3)
plt.scatter(X_iris[:,0], X_iris[:,1], c=labels, cmap='viridis', alpha=0.5)
plt.scatter(new_centers[:,0], new_centers[:,1], c=['red','green','blue'], marker='X', s=200)
plt.title('Schritt 3: Zentren aktualisieren')

plt.tight_layout()
plt.show()
```

!!! tip "Interaktive Visualisierung"
    Probiere den Algorithmus interaktiv aus und beobachte, wie die Zentren wandern: [K-Means Visualisierung von Naftali Harris](https://www.naftaliharris.com/blog/visualizing-k-means-clustering/){ target="_blank" }

**Fragen:**

a) Was passiert, wenn wir Schritt 2 und 3 wiederholen?

b) Wann stoppt der Algorithmus?

c) Experimentiere mit der interaktiven Visualisierung: Was passiert bei unterschiedlichen Startpositionen der Zentren?

c) Warum ist die Initialisierung (Startzentren) wichtig?

---

## Aufgabe 2: Elbow-Methode für Country-Daten

- [ ] Erledigt

!!! info "Datensatz-Wechsel"
    In Aufgabe 1 hast du den K-Means Algorithmus am Iris-Dataset verstanden. Jetzt wenden wir das Gelernte auf den **Country-Datensatz** an – denselben, den du in UL-01 und UL-02 bereits exploriert und skaliert hast.

Wie finden wir die optimale Anzahl an Clustern? Die Elbow-Methode hilft!

```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
import pandas as pd

# Country-Daten laden und skalieren
df = pd.read_csv('Country-data.csv')
X = df.select_dtypes(include=[np.number])
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Elbow-Methode
inertias = []
K_range = range(1, 11)

for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans.fit(X_scaled)
    inertias.append(kmeans.inertia_)

# Visualisierung
plt.figure(figsize=(8, 5))
plt.plot(K_range, inertias, 'bo-', linewidth=2, markersize=8)
plt.xlabel('Anzahl Cluster (k)')
plt.ylabel('Inertia (Within-Cluster Sum of Squares)')
plt.title('Elbow-Methode')
plt.xticks(K_range)
plt.grid(True, alpha=0.3)
plt.show()
```

**Analysiere:**

a) Wo ist der "Ellbogen" (Knick) in der Kurve?

b) Warum sinkt die Inertia immer weiter, wenn k steigt?

c) Welches k würdest du wählen? Begründe!

---

## Aufgabe 3: K-Means durchführen

- [ ] Erledigt

Jetzt wenden wir K-Means mit der gewählten Clusteranzahl an:

```python
# K-Means mit optimalem k
k = 3  # Passe diesen Wert nach deiner Elbow-Analyse an!

kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
labels = kmeans.fit_predict(X_scaled)

# Cluster-Labels zum DataFrame hinzufügen
df['Cluster'] = labels

# Übersicht
print(df['Cluster'].value_counts())
```

**Deine Aufgaben:**

a) Ändere den Wert von `k` auf deinen Wert aus der Elbow-Analyse. Wie viele Länder sind in jedem Cluster?

b) Welche Cluster-Zentren hat der Algorithmus gefunden?

```python
# Cluster-Zentren (skaliert)
print("Cluster-Zentren (skaliert):")
print(kmeans.cluster_centers_)

# Zentren zurück-transformieren
centers_original = scaler.inverse_transform(kmeans.cluster_centers_)
print("\nCluster-Zentren (Original-Skala):")
print(pd.DataFrame(centers_original, columns=X.columns))
```

---

## Aufgabe 4: Silhouette Score berechnen

- [ ] Erledigt

Der Silhouette Score misst, wie gut die Cluster getrennt sind:

```python
from sklearn.metrics import silhouette_score

score = silhouette_score(X_scaled, labels)
print(f"Silhouette Score: {score:.3f}")
```

**Interpretation:**

| Wert | Bedeutung |
|------|-----------|
| 0.7 - 1.0 | Starke Cluster-Struktur |
| 0.5 - 0.7 | Vernünftige Struktur |
| 0.25 - 0.5 | Schwache Struktur |
| < 0.25 | Keine klare Struktur |

**Frage:** Wie gut ist dein Clustering laut Silhouette Score?

---

## Aufgabe 5: Verschiedene k vergleichen

- [ ] Erledigt

Berechne den Silhouette Score für verschiedene k:

```python
silhouettes = []
K_range = range(2, 10)  # min. 2 Cluster für Silhouette

for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels = kmeans.fit_predict(X_scaled)
    score = silhouette_score(X_scaled, labels)
    silhouettes.append(score)
    print(f"k={k}: Silhouette Score = {score:.3f}")

# Visualisierung
plt.figure(figsize=(8, 5))
plt.bar(K_range, silhouettes, color='steelblue', edgecolor='black')
plt.xlabel('Anzahl Cluster (k)')
plt.ylabel('Silhouette Score')
plt.title('Silhouette Score für verschiedene k')
plt.xticks(K_range)
plt.show()
```

**Fragen:**

a) Welches k hat den höchsten Silhouette Score?

b) Stimmt das mit der Elbow-Methode überein?

c) Was wählst du als finales k?

---

## Aufgabe 6: Reflexion – Warum funktioniert K-Means bei Iris so gut?

- [ ] Erledigt

Lade das Iris-Dataset und vergleiche die echten Labels mit den K-Means Clustern:

```python
from sklearn.datasets import load_iris

iris = load_iris()
X_iris = iris.data
y_true = iris.target

# K-Means mit k=3
kmeans = KMeans(n_clusters=3, random_state=42)
labels_pred = kmeans.fit_predict(X_iris)

# Vergleich
from sklearn.metrics import adjusted_rand_score
ari = adjusted_rand_score(y_true, labels_pred)
print(f"Adjusted Rand Index: {ari:.3f}")  # 1.0 = perfekte Übereinstimmung
```

**Reflexion:**

Warum funktioniert K-Means bei Iris so gut? (Tipp: Visualisiere die echten Klassen und die gefundenen Cluster in 2D mit PCA)

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **Elbow-Kurve zeigt keinen klaren Knick**  
    → Das ist normal! Wähle den Punkt, ab dem die Verbesserung deutlich abnimmt.
    
    ❌ **Unterschiedliche Ergebnisse bei jedem Durchlauf**  
    → Setze `random_state=42` für Reproduzierbarkeit:
    ```python
    KMeans(n_clusters=3, random_state=42)
    ```
    
    ❌ **Daten nicht skaliert**  
    → K-Means braucht skalierte Daten! Zurück zu UL-02.
    
    ❌ **`labels_` ist leer oder AttributeError**  
    → Du musst zuerst `fit()` oder `fit_predict()` aufrufen!
    ```python
    # Falsch:
    kmeans = KMeans(n_clusters=3)
    print(kmeans.labels_)  # Fehler!
    
    # Richtig:
    kmeans = KMeans(n_clusters=3)
    kmeans.fit(X_scaled)
    print(kmeans.labels_)  # Jetzt funktioniert's!
    ```
    
    ❌ **`n_init` Warnung in neueren scikit-learn Versionen**  
    → Setze explizit `n_init=10` oder `n_init='auto'`

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - K-Means iteriert: Punkte zuweisen → Zentren aktualisieren → wiederholen
    - **Elbow-Methode:** Suche den Knick in der Inertia-Kurve
    - **Silhouette Score:** Misst Cluster-Qualität (höher = besser)
    - Immer `random_state` setzen für reproduzierbare Ergebnisse
    - K-Means funktioniert gut bei kompakten, kugelförmigen Clustern

---

## Nächste Schritte

Wie visualisierst du 9 Dimensionen? Im nächsten Arbeitsblatt lernst du PCA zur Dimensionsreduktion.

➡️ Weiter zu [UL-04: Dimensionsreduktion mit PCA](ul-04-pca.md)
