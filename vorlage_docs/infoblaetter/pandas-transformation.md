# Pandas Transformation

## Übersicht

Transformationen ändern oder erweitern Daten: Werte umwandeln, neue Spalten berechnen, Text verarbeiten.

---

## map() – Wertemapping

`map()` ersetzt Werte in einer **Series** basierend auf einem Dictionary oder einer Funktion.

### Mit Dictionary

```python
import pandas as pd

df = pd.DataFrame({
    'Name': ['Max', 'Anna', 'Tom'],
    'Geschlecht': ['M', 'F', 'M'],
    'Abteilung': ['IT', 'HR', 'IT']
})

# Kürzel zu vollständigen Namen
df['Geschlecht_Voll'] = df['Geschlecht'].map({'M': 'Männlich', 'F': 'Weiblich'})
print(df)
#    Name Geschlecht Abteilung Geschlecht_Voll
# 0   Max          M        IT        Männlich
# 1  Anna          F        HR        Weiblich
# 2   Tom          M        IT        Männlich
```

### Mit Funktion

```python
# Funktion anwenden
df['Name_Länge'] = df['Name'].map(len)
print(df['Name_Länge'])
# 0    3
# 1    4
# 2    3

# Lambda-Funktion
df['Name_Upper'] = df['Name'].map(lambda x: x.upper())
```

!!! warning "Nicht gefundene Werte"
    `map()` gibt `NaN` zurück, wenn ein Wert nicht im Dictionary gefunden wird:
    ```python
    df['Neu'] = df['Spalte'].map({'A': 1, 'B': 2})  # 'C' → NaN
    ```

---

## apply() – Flexible Funktionsanwendung

`apply()` ist vielseitiger als `map()` und funktioniert auf **Series und DataFrames**.

### Auf Series

```python
df = pd.DataFrame({
    'Name': ['Max', 'Anna', 'Tom'],
    'Alter': [25, 30, 28],
    'Gehalt': [50000, 65000, 55000]
})

# Einfache Funktion
df['Alter_Kategorie'] = df['Alter'].apply(lambda x: 'Jung' if x < 28 else 'Erfahren')

# Komplexere Funktion
def kategorisiere_gehalt(gehalt):
    if gehalt < 52000:
        return 'Niedrig'
    elif gehalt < 60000:
        return 'Mittel'
    else:
        return 'Hoch'

df['Gehaltsstufe'] = df['Gehalt'].apply(kategorisiere_gehalt)
print(df)
```

### Auf DataFrame (zeilenweise)

```python
# axis=1: Funktion auf jede Zeile anwenden
def beschreibe_person(row):
    return f"{row['Name']} ist {row['Alter']} Jahre alt"

df['Beschreibung'] = df.apply(beschreibe_person, axis=1)
print(df['Beschreibung'])
# 0    Max ist 25 Jahre alt
# 1    Anna ist 30 Jahre alt
# 2    Tom ist 28 Jahre alt
```

### Auf DataFrame (spaltenweise)

```python
# axis=0 (Standard): Funktion auf jede Spalte anwenden
numerische_spalten = df[['Alter', 'Gehalt']]
print(numerische_spalten.apply(lambda x: x.max() - x.min()))
# Alter        5
# Gehalt    15000
```

---

## Vergleich map vs. apply

| Eigenschaft | map() | apply() |
|-------------|-------|---------|
| Anwendbar auf | Nur Series | Series & DataFrame |
| Mit Dictionary | ✅ Ja | ❌ Nein |
| Komplexe Funktionen | Eingeschränkt | ✅ Ja |
| Zugriff auf mehrere Spalten | ❌ Nein | ✅ Ja (axis=1) |
| Performance | Schneller | Langsamer |

---

## String-Methoden (.str Accessor)

Der `.str` Accessor ermöglicht String-Operationen auf Series.

### Grundlegende Operationen

```python
df = pd.DataFrame({
    'Name': ['Max Mustermann', 'Anna Schmidt', 'Tom Müller'],
    'Email': ['max@example.com', 'ANNA@EXAMPLE.COM', 'tom@example.com']
})

# Groß-/Kleinschreibung
df['Name_Upper'] = df['Name'].str.upper()
df['Name_Lower'] = df['Name'].str.lower()
df['Name_Title'] = df['Name'].str.title()

print(df['Name_Upper'])
# 0    MAX MUSTERMANN
# 1     ANNA SCHMIDT
# 2       TOM MÜLLER
```

### Suchen und Prüfen

```python
# Enthält
df['Hat_Mueller'] = df['Name'].str.contains('Müller')
# 0    False
# 1    False
# 2     True

# Beginnt/Endet mit
df['Beginnt_M'] = df['Name'].str.startswith('M')
df['Endet_n'] = df['Name'].str.endswith('n')

# Länge
df['Name_Länge'] = df['Name'].str.len()
```

### Aufteilen und Extrahieren

```python
# Split
df['Vorname'] = df['Name'].str.split(' ').str[0]
df['Nachname'] = df['Name'].str.split(' ').str[-1]

print(df[['Name', 'Vorname', 'Nachname']])
#              Name Vorname   Nachname
# 0  Max Mustermann     Max  Mustermann
# 1    Anna Schmidt    Anna     Schmidt
# 2      Tom Müller     Tom      Müller

# Nur bestimmte Zeichen
df['Initialen'] = df['Name'].str[0] + df['Nachname'].str[0]
```

### Ersetzen

```python
# Einfaches Ersetzen
df['Email_Sauber'] = df['Email'].str.lower()
df['Email_Neu'] = df['Email'].str.replace('@example.com', '@firma.de')

# Whitespace entfernen
df['Name'] = df['Name'].str.strip()    # Beide Seiten
df['Name'] = df['Name'].str.lstrip()   # Links
df['Name'] = df['Name'].str.rstrip()   # Rechts
```

### Regex-Unterstützung

```python
# Mit Regular Expressions
df['Zahlen'] = df['Text'].str.extract(r'(\d+)')  # Erste Zahl extrahieren
df['Enthaelt_Zahl'] = df['Text'].str.contains(r'\d', regex=True)

# Alle Vorkommen finden
df['Alle_Zahlen'] = df['Text'].str.findall(r'\d+')
```

---

## Wichtige String-Methoden

| Methode | Beschreibung | Beispiel |
|---------|--------------|----------|
| `.str.lower()` | Kleinbuchstaben | `'ABC'` → `'abc'` |
| `.str.upper()` | Großbuchstaben | `'abc'` → `'ABC'` |
| `.str.title()` | Titlecase | `'max müller'` → `'Max Müller'` |
| `.str.strip()` | Whitespace entfernen | `' abc '` → `'abc'` |
| `.str.len()` | Länge | `'abc'` → `3` |
| `.str.contains()` | Enthält | `'abc'.contains('b')` → `True` |
| `.str.startswith()` | Beginnt mit | |
| `.str.endswith()` | Endet mit | |
| `.str.split()` | Aufteilen | `'a,b'.split(',')` → `['a', 'b']` |
| `.str.replace()` | Ersetzen | |
| `.str.extract()` | Regex-Extraktion | |
| `.str.get()` | Element aus Liste | `str.get(0)` |
| `.str.slice()` | Teilstring | `str.slice(0, 3)` |

---

## Bedingte Transformationen

### np.where() / np.select()

```python
import numpy as np

df = pd.DataFrame({
    'Punkte': [45, 75, 88, 52, 95]
})

# Einfache Bedingung
df['Bestanden'] = np.where(df['Punkte'] >= 50, 'Ja', 'Nein')

# Mehrere Bedingungen
bedingungen = [
    df['Punkte'] >= 90,
    df['Punkte'] >= 75,
    df['Punkte'] >= 50
]
kategorien = ['Sehr Gut', 'Gut', 'Bestanden']

df['Note'] = np.select(bedingungen, kategorien, default='Nicht Bestanden')
print(df)
```

### pd.cut() – Numerische Kategorisierung

```python
# Gleichmäßige Bins
df['Kategorie'] = pd.cut(df['Punkte'], bins=3, labels=['Niedrig', 'Mittel', 'Hoch'])

# Eigene Grenzen
df['Note'] = pd.cut(
    df['Punkte'],
    bins=[0, 50, 75, 90, 100],
    labels=['Nicht Bestanden', 'Bestanden', 'Gut', 'Sehr Gut']
)
```

### pd.qcut() – Quantil-basierte Kategorisierung

```python
# Gleichgroße Gruppen (nach Quantilen)
df['Quartil'] = pd.qcut(df['Punkte'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])
```

---

## Praktische Beispiele

### Beispiel 1: Datenbereinigung

```python
df = pd.DataFrame({
    'Email': ['  MAX@EXAMPLE.COM  ', 'anna@example.com', 'TOM@Example.Com']
})

# Bereinigen: Strip + Lowercase
df['Email_Clean'] = df['Email'].str.strip().str.lower()
print(df['Email_Clean'])
# 0    max@example.com
# 1    anna@example.com
# 2    tom@example.com
```

### Beispiel 2: Kategorisierung

```python
df = pd.DataFrame({
    'Umsatz': [5000, 15000, 25000, 8000, 50000]
})

def umsatz_kategorie(umsatz):
    if umsatz < 10000:
        return 'Klein'
    elif umsatz < 30000:
        return 'Mittel'
    else:
        return 'Groß'

df['Kategorie'] = df['Umsatz'].apply(umsatz_kategorie)
```

### Beispiel 3: Mehrere Spalten kombinieren

```python
df = pd.DataFrame({
    'Vorname': ['Max', 'Anna'],
    'Nachname': ['Müller', 'Schmidt'],
    'Geburtsjahr': [1995, 1990]
})

df['Vollständiger_Name'] = df['Vorname'] + ' ' + df['Nachname']
df['Alter_2024'] = 2024 - df['Geburtsjahr']
```

---

## Performance-Tipps

**Performance-Ranking (schnell → langsam):**

| Rang | Methode | Beispiel | Geschwindigkeit |
|------|---------|----------|----------------|
| 1 | Vektorisiert | `df['a'] + df['b']` | ⚡ Sehr schnell |
| 2 | `map()` | `df['col'].map(dict)` | 🚀 Schnell |
| 3 | `apply()` | `df.apply(lambda x: ...)` | 🐢 Langsam |
| 4 | for-Schleife | `for i in range(len(df))` | 🐌 **VERMEIDEN!** |

!!! tip "Performance-Regeln"
    1. **Vektorisierte Operationen** bevorzugen
    2. **map()** für einfache Werteersetzung
    3. **apply()** nur wenn nötig
    4. **for-Schleifen vermeiden**

```python
# LANGSAM (for-Schleife)
for i in range(len(df)):
    df.loc[i, 'Neu'] = df.loc[i, 'Alt'] * 2

# SCHNELL (vektorisiert)
df['Neu'] = df['Alt'] * 2
```

---

## Zusammenfassung

!!! success "Das Wichtigste"
    - **map()**: Wertemapping auf Series (Dictionary oder Funktion)
    - **apply()**: Flexible Funktionsanwendung (Series/DataFrame)
    - **axis=1**: Zeilenweise auf DataFrame anwenden
    - **.str Accessor**: String-Operationen auf Series
    - **pd.cut()**: Numerische Werte in Kategorien
    - **np.where()**: Bedingte Wertzuweisung
    - Vektorisierte Operationen sind am schnellsten!

---

??? question "Selbstkontrolle"
    1. Wann verwendet man `map()` statt `apply()`?
    2. Wie greifst du auf das erste Wort eines Strings in einer Series zu?
    3. Was ist der Unterschied zwischen `pd.cut()` und `pd.qcut()`?
    4. Wie wandelst du Ja/Nein-Werte in 1/0 um?
    
    ??? success "Antworten"
        1. `map()` für einfaches Dictionary-Mapping, `apply()` für komplexere Funktionen
        2. `df['Spalte'].str.split().str[0]` oder `df['Spalte'].str.split().str.get(0)`
        3. `cut()` verwendet feste Grenzen, `qcut()` teilt nach Quantilen (gleichgroße Gruppen)
        4. `df['Spalte'].map({'Ja': 1, 'Nein': 0})` oder `df['Spalte'].apply(lambda x: 1 if x == 'Ja' else 0)`
