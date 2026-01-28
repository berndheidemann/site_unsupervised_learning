# UL-01: Einführung in Unsupervised Learning

!!! abstract "Advance Organizer"
    In diesem Arbeitsblatt lernst du die Grundidee von Unsupervised Learning kennen. Anders als bei Supervised Learning gibt es hier keine "richtigen Antworten" – der Algorithmus findet selbst Muster in den Daten. Diese Technik wird in der Praxis häufig für Kundensegmentierung, Anomalie-Erkennung und Datenexploration eingesetzt.
    
    **Dein Ziel:** Am Ende kannst du erklären, wann Clustering sinnvoll ist, und hast erste Hypothesen über mögliche Gruppen im Datensatz formuliert.

---

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [x] Den Unterschied zwischen Supervised und Unsupervised Learning erklären
- [x] Anwendungsfälle für Clustering nennen
- [x] Einen ersten Datensatz laden und explorieren

---

## Aufgabe 1: Theorie – Was ist Unsupervised Learning?

Lies das [Infoblatt Einführung Unsupervised Learning](../infoblaetter/einfuehrung-unsupervised.md) und beantworte folgende Fragen:

1. Was ist der Hauptunterschied zwischen Supervised und Unsupervised Learning?
2. Nenne drei Anwendungsfälle für Clustering aus der Praxis.
3. Was versteht man unter "Cluster"?

---

## Aufgabe 2: Country-Datensatz laden

Lade den Country-Datensatz und verschaffe dir einen ersten Überblick.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

## Datensatz laden
df = pd.read_csv('Country-data.csv')

# Erste Zeilen anzeigen
df.head()
```

**Deine Aufgaben:**

a) Wie viele Länder enthält der Datensatz?

b) Welche Features (Spalten) gibt es?

c) Welcher Datentyp hat jede Spalte?

```python
# Anzahl der Zeilen und Spalten


# Spaltenübersicht mit Datentypen

```

---

## Aufgabe 3: Data Dictionary verstehen

Lies die Datei `data-dictionary.csv` oder die folgende Tabelle, um die Features zu verstehen:

| Feature | Beschreibung |
|---------|--------------|
| country | Name des Landes |
| child_mort | Kindersterblichkeit (pro 1000 Geburten) |
| exports | Exporte (% des BIP) |
| health | Gesundheitsausgaben (% des BIP) |
| imports | Importe (% des BIP) |
| income | Pro-Kopf-Einkommen |
| inflation | Inflationsrate (%) |
| life_expec | Lebenserwartung (Jahre) |
| total_fer | Geburtenrate (Kinder pro Frau) |
| gdpp | BIP pro Kopf |

**Frage:** Welche Features könnten zusammenhängen? Notiere deine Vermutungen!

---

## Aufgabe 4: Deskriptive Statistik

Erstelle eine Übersicht der wichtigsten statistischen Kennzahlen.

```python
# Deskriptive Statistik für alle numerischen Features
df.describe()
```

**Analysiere:**

a) Welches Feature hat die größte Streuung (Standardabweichung)?

b) Gibt es Features mit sehr verschiedenen Skalen? (Hinweis: Vergleiche min/max-Werte)

c) Für welches Feature ist der Unterschied zwischen Median und Mittelwert besonders groß? Was bedeutet das?

---

## Aufgabe 5: Pairplot erstellen

Ein Pairplot zeigt alle paarweisen Zusammenhänge. Da wir 9 numerische Features haben, wähle zunächst nur vier aus:

```python
# Pairplot für ausgewählte Features
selected_features = ['child_mort', 'income', 'life_expec', 'gdpp']

sns.pairplot(df[selected_features])
plt.tight_layout()
plt.savefig('pairplot_countries.png', dpi=150)
plt.show()
```

**Beobachtungen:**

a) Welche Features korrelieren positiv miteinander?

b) Welche Features korrelieren negativ?

c) Erkennst du bereits "Gruppen" von Ländern in den Scatterplots?

---

## Aufgabe 6: Reflexion – Mögliche Ländergruppen

!!! question "Reflexion"
    Basierend auf deiner bisherigen Exploration:
    
    1. Welche Gruppen von Ländern könnte es geben?
    2. Anhand welcher Features würdest du die Gruppen unterscheiden?
    3. Wie viele Gruppen vermutest du?
    
    **Notiere deine Hypothesen!** Du wirst sie später mit den Clustering-Ergebnissen vergleichen.

---

## Häufige Fehler

!!! failure "Typische Probleme und Lösungen"
    
    ❌ **`FileNotFoundError: No such file or directory`**  
    → Prüfe den Dateipfad! Liegt die CSV im richtigen Ordner?
    ```python
    # Aktuellen Pfad prüfen
    import os
    print(os.getcwd())
    print(os.listdir())
    ```
    
    ❌ **Pairplot zu langsam**  
    → Wähle nur 3-4 Features aus:
    ```python
    sns.pairplot(df[['feature1', 'feature2', 'feature3']])
    ```
    
    ❌ **Daten nicht verstanden**  
    → Lies zuerst das Data Dictionary! Ohne Verständnis der Features ist keine sinnvolle Analyse möglich.
    
    ❌ **`TypeError` bei describe()**  
    → Prüfe, ob alle Spalten den erwarteten Typ haben:
    ```python
    df.dtypes
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - Unsupervised Learning findet Muster ohne vorgegebene Labels
    - Der Country-Datensatz enthält Wirtschafts- und Gesundheitsdaten von 167 Ländern
    - Erste visuelle Exploration zeigt bereits Hinweise auf mögliche Cluster
    - Features haben sehr unterschiedliche Skalen – das wird wichtig für das nächste Arbeitsblatt!

---

## Nächste Schritte

Im nächsten Arbeitsblatt lernst du, warum die unterschiedlichen Skalen ein Problem sind und wie du das mit **Skalierung** löst.

➡️ Weiter zu [UL-02: Datenvorverarbeitung](ul-02-vorverarbeitung.md)
