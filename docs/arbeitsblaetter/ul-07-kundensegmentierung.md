# UL-07: Kundensegmentierung

!!! abstract "Advance Organizer"
    Jetzt wird es ernst: Du bekommst einen neuen Datensatz ohne Anleitung und musst selbst entscheiden, welche Schritte nötig sind. Das entspricht einer echten Aufgabe als Datenanalyst. Kundensegmentierung ist einer der häufigsten Anwendungsfälle für Clustering in der Wirtschaft!
    
    **Dein Ziel:** Du führst eine vollständige Clustering-Analyse durch und leitest konkrete Marketing-Empfehlungen ab. Das ist deine Generalprobe für das Abschlussprojekt.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Gelerntes eigenständig auf einen neuen Datensatz anwenden
- [x] Eine geschäftliche Fragestellung mit Clustering beantworten
- [x] Marketing-Empfehlungen ableiten

---

## Das Szenario

!!! example "Ausgangssituation"
    Ein Einkaufszentrum möchte seine Kunden besser verstehen, um gezieltes Marketing zu betreiben. Die Marketing-Abteilung hat dir die Aufgabe gegeben, verschiedene Kundengruppen zu identifizieren.
    
    **Deine Aufgabe:** Analysiere das Kundenverhalten und identifiziere verschiedene Kundengruppen. Leite daraus konkrete Marketing-Maßnahmen ab.

---

## Der Datensatz

Der **Mall Customers** Datensatz enthält Informationen über 200 Kunden:

| Feature | Beschreibung |
|---------|--------------|
| CustomerID | Eindeutige Kundennummer |
| Gender | Geschlecht (Male/Female) |
| Age | Alter in Jahren |
| Annual Income (k$) | Jahreseinkommen in Tausend Dollar |
| Spending Score (1-100) | Bewertung des Kaufverhaltens (1=wenig, 100=viel) |

---

## Aufgabe 1: Daten laden und explorieren

- [ ] Erledigt

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# Daten laden
df = pd.read_csv('Mall_Customers.csv')

# Deine Aufgaben:
# a) Zeige die ersten Zeilen
# b) Prüfe die Datentypen
# c) Gibt es fehlende Werte?
# d) Erstelle deskriptive Statistik

```

---

## Aufgabe 2: Features auswählen

- [ ] Erledigt

!!! warning "Wichtige Entscheidung"
    Nicht alle Features sind für Clustering geeignet!

**Überlege:**

a) Sollte `CustomerID` als Feature verwendet werden? Warum (nicht)?

b) Wie gehst du mit `Gender` um? (Optionen: ignorieren, One-Hot-Encoding, separate Analyse)

c) Welche Features sind für Marketing-Segmentierung am relevantesten?

```python
# Deine Feature-Auswahl
features = ['___', '___', '___']  # Trage deine Auswahl ein

X = df[features]
print(X.describe())
```

---

## Aufgabe 3: Daten visualisieren

- [ ] Erledigt

Erstelle Visualisierungen, um die Datenstruktur zu verstehen:

```python
# Pairplot oder Scatterplot erstellen
# Tipp: Bei nur 2-3 Features geht auch ein einfacher 2D-Scatterplot

```

**Fragen:**

a) Erkennst du bereits Gruppen in den Daten?

b) Wie viele Cluster vermutest du?

---

## Aufgabe 4: Daten skalieren

- [ ] Erledigt

```python
# Daten skalieren
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Prüfe das Ergebnis

```

---

## Aufgabe 5: Optimale Clusteranzahl bestimmen

- [ ] Erledigt

Wende die Elbow-Methode und/oder Silhouette-Analyse an:

```python
# Elbow-Methode



# Silhouette-Scores



# Deine Entscheidung: k = ___
```

---

## Aufgabe 6: K-Means Clustering durchführen

- [ ] Erledigt

```python
# K-Means mit optimalem k



# Cluster dem DataFrame hinzufügen
df['Cluster'] = ___

# Übersicht
print(df['Cluster'].value_counts())
```

---

## Aufgabe 7: Cluster visualisieren

- [ ] Erledigt

Erstelle aussagekräftige Visualisierungen:

```python
# 2D-Scatterplot mit Cluster-Farben



# Boxplots der Features pro Cluster



# Heatmap der Cluster-Mittelwerte


```

---

## Aufgabe 8: Cluster interpretieren und benennen

- [ ] Erledigt

Analysiere die Cluster und gib ihnen aussagekräftige Namen:

```python
# Cluster-Profile berechnen
cluster_profiles = df.groupby('Cluster')[features].mean()
print(cluster_profiles)

# Cluster benennen
cluster_names = {
    0: '___________',
    1: '___________',
    2: '___________',
    # ...
}

df['Segment'] = df['Cluster'].map(cluster_names)
```

**Typische Kundengruppen könnten sein:**

- "Sparsame Senioren"
- "Junge High-Spender"  
- "Durchschnitts-Shopper"
- "Premium-Kunden"
- etc.

---

## Aufgabe 9: Marketing-Empfehlungen ableiten

- [ ] Erledigt

!!! success "Das wichtigste Ergebnis!"
    Jetzt kommt der Business Value: Was soll das Marketing-Team mit deinen Erkenntnissen tun?

Für jede Kundengruppe:

| Segment | Charakteristik | Marketing-Empfehlung |
|---------|---------------|---------------------|
| ___ | ___ | ___ |
| ___ | ___ | ___ |
| ___ | ___ | ___ |

**Beispiele für Marketing-Maßnahmen:**

- Newsletter-Inhalte anpassen
- Spezielle Rabattaktionen
- Premium-Services anbieten
- Loyalty-Programme
- Events im Einkaufszentrum
- Personalisierte Werbung

---

## Zusatzaufgabe für Schnelle

Vergleiche das K-Means Ergebnis mit hierarchischem Clustering:

```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage

# Hierarchisches Clustering



# Vergleich mit K-Means


```

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **CustomerID als Feature verwendet**  
    → IDs sind keine sinnvollen Features! Sie enthalten keine Information über Kundenverhalten.
    
    ❌ **Geschlecht nicht berücksichtigt**  
    → Optionen:
    - One-Hot-Encoding: `pd.get_dummies(df, columns=['Gender'])`
    - Separate Analyse für Männer und Frauen
    - Ignorieren und später bei Interpretation berücksichtigen
    
    ❌ **Nur 2D-Visualisierung**  
    → Mit nur 3-4 Features geht auch 3D:
    ```python
    from mpl_toolkits.mplot3d import Axes3D
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    ax.scatter(X[:,0], X[:,1], X[:,2], c=labels)
    ```
    
    ❌ **Marketing-Empfehlungen zu vage**  
    → Sei konkret! 
    ```
    # Statt: "Marketing für Cluster 2 anpassen"
    # Besser: "Cluster 2 sind junge High-Spender (20-35 Jahre, hohes Einkommen, 
    #          Spending Score >70) → Premium-Events und exklusive Angebote"
    ```

---

## Checkliste für deine Analyse

- [ ] Daten geladen und exploriert
- [ ] Features sinnvoll ausgewählt
- [ ] Daten skaliert
- [ ] Optimale Clusteranzahl bestimmt (begründet!)
- [ ] Clustering durchgeführt
- [ ] Visualisierungen erstellt
- [ ] Cluster inhaltlich interpretiert
- [ ] Aussagekräftige Namen vergeben
- [ ] Marketing-Empfehlungen abgeleitet

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Eigenständige Anwendung des kompletten Clustering-Workflows
    - Feature-Auswahl für Business-Fragestellungen
    - Von technischen Ergebnissen zu Business-Empfehlungen
    - Kundensegmentierung als wichtiger Use Case

---

## Nächste Schritte

Sehr gut! Du hast eine eigenständige Analyse durchgeführt. Im nächsten Arbeitsblatt arbeitest du mit mehr Features und vergleichst verschiedene Algorithmen systematisch.

➡️ Weiter zu [UL-08: Weinqualität-Analyse](ul-08-weinqualitaet.md)
