# UL-02: Datenvorverarbeitung für Clustering

!!! abstract "Advance Organizer"
    Clustering-Algorithmen messen Abstände zwischen Datenpunkten. Wenn ein Feature in Millionen gemessen wird und ein anderes in Prozent, dominiert das größere Feature die Berechnung – das verfälscht die Ergebnisse! Hier lernst du, wie du Daten so vorbereitest, dass alle Features fair berücksichtigt werden.
    
    **Dein Ziel:** Du verstehst, warum Skalierung notwendig ist, und kannst sie selbstständig anwenden. Dieses Wissen brauchst du für ALLE folgenden Arbeitsblätter.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Die Notwendigkeit der Skalierung verstehen und erklären
- [x] StandardScaler und MinMaxScaler anwenden
- [x] Auswirkungen der Skalierung visualisieren

---

## Aufgabe 1: Warum ist Skalierung wichtig?

Betrachte zwei Features aus dem Country-Datensatz:

```python
import pandas as pd
import numpy as np

df = pd.read_csv('Country-data.csv')

# Zwei Features vergleichen
print(f"Income: Min={df['income'].min():.0f}, Max={df['income'].max():.0f}")
print(f"Child Mort: Min={df['child_mort'].min():.1f}, Max={df['child_mort'].max():.1f}")
```

**Fragen:**

a) Um welchen Faktor ist die Spanne von `income` größer als die von `child_mort`?

b) Wenn ein Clustering-Algorithmus den euklidischen Abstand berechnet, welches Feature dominiert dann?

c) Ist das fair, wenn beide Features gleich wichtig für die Analyse sein sollen?

---

## Aufgabe 2: StandardScaler anwenden

Der StandardScaler transformiert jedes Feature so, dass es Mittelwert 0 und Standardabweichung 1 hat.

```python
from sklearn.preprocessing import StandardScaler

# Nur numerische Spalten auswählen
numeric_cols = df.select_dtypes(include=[np.number]).columns
X = df[numeric_cols]

# StandardScaler anwenden
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# In DataFrame umwandeln
df_scaled = pd.DataFrame(X_scaled, columns=numeric_cols)
```

**Deine Aufgaben:**

a) Prüfe, ob die Transformation korrekt ist:

```python
# Mittelwert und Standardabweichung der skalierten Daten
print("Mittelwerte:")
print(df_scaled.mean().round(2))

print("\nStandardabweichungen:")
print(df_scaled.std().round(2))
```

b) Was sollten die Werte sein? Stimmt das Ergebnis?

---

## Aufgabe 3: Verteilungen vor/nach Skalierung vergleichen

Visualisiere, wie sich die Skalierung auswirkt:

```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Vorher: Income
axes[0,0].hist(df['income'], bins=30, edgecolor='black')
axes[0,0].set_title('Income (Original)')
axes[0,0].set_xlabel('Wert')

# Nachher: Income
axes[0,1].hist(df_scaled['income'], bins=30, edgecolor='black', color='orange')
axes[0,1].set_title('Income (Skaliert)')
axes[0,1].set_xlabel('Z-Score')

# Vorher: Child Mortality
axes[1,0].hist(df['child_mort'], bins=30, edgecolor='black')
axes[1,0].set_title('Child Mortality (Original)')
axes[1,0].set_xlabel('Wert')

# Nachher: Child Mortality
axes[1,1].hist(df_scaled['child_mort'], bins=30, edgecolor='black', color='orange')
axes[1,1].set_title('Child Mortality (Skaliert)')
axes[1,1].set_xlabel('Z-Score')

plt.tight_layout()
plt.show()
```

**Beobachtungen:**

a) Wie verändert sich die Form der Verteilung durch Skalierung?

b) Wie verändert sich der Wertebereich?

c) Was bedeutet ein Z-Score von 2? Was bedeutet -1?

---

## Aufgabe 4: MinMaxScaler als Alternative

Der MinMaxScaler transformiert auf einen Bereich von 0 bis 1:

```python
from sklearn.preprocessing import MinMaxScaler

# Wende den MinMaxScaler analog zum StandardScaler an
minmax_scaler = MinMaxScaler()
X_minmax = minmax_scaler.fit_transform(X)
df_minmax = pd.DataFrame(X_minmax, columns=numeric_cols)

# Wertebereich prüfen
print("Minimum pro Feature:")
print(df_minmax.min().values)

print("\nMaximum pro Feature:")
print(df_minmax.max().values)
```

**Erwartete Ausgabe:** Alle Minima sollten 0.0 sein, alle Maxima 1.0.

**Vergleich StandardScaler vs. MinMaxScaler:**

| Aspekt | StandardScaler | MinMaxScaler |
|--------|----------------|--------------|
| Wertebereich | Unbegrenzt (meist -3 bis +3) | Fest [0, 1] |
| Umgang mit Ausreißern | Robust, Ausreißer beeinflussen weniger | Empfindlich, Ausreißer stauchen Daten |
| Wann verwenden? | K-Means, PCA, die meisten Algorithmen | Neuronale Netze, Bilddaten |

!!! tip "Wann welchen Scaler?"
    - **StandardScaler:** Standard für die meisten Clustering-Algorithmen
    - **MinMaxScaler:** Wenn alle Werte im Bereich [0,1] sein sollen

---

## Aufgabe 5: Korrelationsmatrix erstellen

Korrelationen helfen, redundante Features zu identifizieren:

```python
# Korrelationsmatrix berechnen
correlation = df[numeric_cols].corr()

# Als Heatmap visualisieren
plt.figure(figsize=(10, 8))
sns.heatmap(correlation, annot=True, cmap='coolwarm', center=0, fmt='.2f')
plt.title('Korrelationsmatrix')
plt.tight_layout()
plt.show()
```

**Analysiere:**

a) Welche Features korrelieren stark positiv (r > 0.7)?

b) Welche Features korrelieren stark negativ (r < -0.7)?

c) Sollte man stark korrelierte Features beide behalten? Warum (nicht)?

---

## Aufgabe 6: Daten für Clustering vorbereiten

Erstelle einen vorbereiteten Datensatz für die nächsten Arbeitsblätter:

```python
# 1. Nur numerische Spalten
X = df.select_dtypes(include=[np.number])

# 2. Skalieren
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 3. Ländernamen für spätere Zuordnung
countries = df['country'].values

# Prüfen
print(f"Shape: {X_scaled.shape}")
print(f"Mittelwert ≈ 0: {X_scaled.mean():.10f}")
print(f"Std ≈ 1: {X_scaled.std():.4f}")
```

**Prüfe dein Ergebnis:**

a) Stimmen Mittelwert (≈0) und Standardabweichung (≈1)? Wenn nicht, was könnte falsch sein?

b) Warum speichern wir `countries` separat? Was würde passieren, wenn wir die Ländernamen verlieren?

!!! success "Checkpoint"
    Ab jetzt arbeitest du mit `X_scaled` – den skalierten Daten!

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **`ValueError: could not convert string to float`**  
    → Du hast kategoriale Spalten (z.B. Ländernamen) nicht entfernt!
    ```python
    # Nur numerische Features
    df_numeric = df.select_dtypes(include=[np.number])
    ```
    
    ❌ **Skalierung auf gesamten DataFrame angewendet**  
    → Die country-Spalte kann nicht skaliert werden.
    ```python
    # Falsch:
    scaler.fit_transform(df)  # Fehler!
    
    # Richtig:
    scaler.fit_transform(df[numeric_cols])
    ```
    
    ❌ **Fit und Transform verwechselt**  
    → Erst `fit()` lernt die Parameter, dann `transform()` wendet sie an.
    ```python
    # Erst fit, dann transform (zwei Schritte)
    scaler.fit(X)
    X_scaled = scaler.transform(X)
    
    # Oder in einem Schritt:
    X_scaled = scaler.fit_transform(X)
    ```
    
    ❌ **Originaldaten überschrieben**  
    → Speichere skalierte Daten in neuer Variable!
    ```python
    # Falsch: df = scaler.fit_transform(df)
    # Richtig: X_scaled = scaler.fit_transform(X)
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Ohne Skalierung dominieren Features mit großen Werten das Clustering
    - **StandardScaler** transformiert auf Mittelwert=0, Std=1
    - **MinMaxScaler** transformiert auf den Bereich [0,1]
    - Korrelationsmatrix zeigt redundante Features
    - Immer erst kategoriale Daten entfernen, dann skalieren!

---

## Nächste Schritte

Jetzt sind deine Daten bereit für das erste Clustering! Im nächsten Arbeitsblatt lernst du K-Means kennen.

➡️ Weiter zu [UL-03: K-Means Clustering](ul-03-kmeans.md)
