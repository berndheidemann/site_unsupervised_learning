# Datenbereinigung

## Warum Datenbereinigung?

Rohdaten sind selten perfekt. Vor jeder Analyse müssen Daten bereinigt werden:

**Typische Datenprobleme:**

| Problem | Beschreibung | Lösung |
|---------|--------------|--------|
| Fehlende Werte (NaN, None) | Leere Zellen, keine Eingabe | `dropna()`, `fillna()` |
| Duplikate | Doppelte Zeilen | `drop_duplicates()` |
| Inkonsistente Datentypen | Text statt Zahlen | `astype()`, `pd.to_numeric()` |
| Ausreißer | Unplausible Werte | IQR-Methode, Z-Score |
| Inkonsistente Schreibweisen | "Berlin" vs "BERLIN" | `.str.strip()`, `.str.lower()` |

---

## Fehlende Werte (NaN)

### Erkennen

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'Name': ['Max', 'Anna', None, 'Tom'],
    'Alter': [25, np.nan, 30, 28],
    'Stadt': ['Berlin', 'München', 'Hamburg', None]
})

# Fehlende Werte pro Spalte
print(df.isna().sum())
# Name     1
# Alter    1
# Stadt    1

# Prozentual
print(df.isna().mean() * 100)
# Name     25.0
# Alter    25.0
# Stadt    25.0

# Zeilen mit mindestens einem fehlenden Wert
print(df[df.isna().any(axis=1)])
```

### Zusammenfassung fehlender Werte

```python
def missing_summary(df):
    """Übersicht fehlender Werte"""
    missing = df.isna().sum()
    missing_pct = df.isna().mean() * 100
    
    summary = pd.DataFrame({
        'Fehlend': missing,
        'Prozent': missing_pct.round(2)
    })
    return summary[summary['Fehlend'] > 0].sort_values('Fehlend', ascending=False)

print(missing_summary(df))
```

### Entfernen (dropna)

```python
# Zeilen mit IRGENDEINEM fehlenden Wert entfernen
df_clean = df.dropna()

# Zeilen entfernen, wo ALLE Werte fehlen
df_clean = df.dropna(how='all')

# Zeilen entfernen, wo bestimmte Spalten fehlen
df_clean = df.dropna(subset=['Name', 'Alter'])

# Spalten mit fehlenden Werten entfernen
df_clean = df.dropna(axis=1)

# Nur Zeilen behalten mit min. 2 gültigen Werten
df_clean = df.dropna(thresh=2)
```

### Ersetzen (fillna)

```python
# Mit festen Werten
df['Alter'] = df['Alter'].fillna(0)
df['Stadt'] = df['Stadt'].fillna('Unbekannt')

# Mit Statistiken
df['Alter'] = df['Alter'].fillna(df['Alter'].mean())   # Mittelwert
df['Alter'] = df['Alter'].fillna(df['Alter'].median()) # Median

# Vorwärts/Rückwärts füllen (für Zeitreihen)
df['Wert'] = df['Wert'].ffill()  # Forward fill
df['Wert'] = df['Wert'].bfill()  # Backward fill

# Interpolation (für numerische Daten)
df['Wert'] = df['Wert'].interpolate()
```

**Strategien für fehlende Werte:**

| Methode | Code | Wann verwenden? |
|---------|------|----------------|
| Entfernen | `dropna()` | Bei wenigen Fehlern |
| Fester Wert | `fillna('Unbekannt')` | Bei kategorialen Daten |
| Statistik | `fillna(df.mean())` | Bei numerischen Daten |
| Vorwärts/Rückwärts | `ffill()`, `bfill()` | Bei Zeitreihen |
| Interpolation | `interpolate()` | Bei kontinuierlichen Daten |

---

## Duplikate

### Erkennen

```python
df = pd.DataFrame({
    'Name': ['Max', 'Anna', 'Max', 'Tom', 'Anna'],
    'Alter': [25, 30, 25, 28, 30]
})

# Duplikate erkennen
print(df.duplicated())
# 0    False
# 1    False
# 2     True  ← Duplikat von Zeile 0
# 3    False
# 4     True  ← Duplikat von Zeile 1

# Anzahl Duplikate
print(f"Anzahl Duplikate: {df.duplicated().sum()}")

# Duplikate anzeigen
print(df[df.duplicated(keep=False)])  # Alle (inkl. Original)
```

### Entfernen

```python
# Alle Duplikate entfernen (erstes behalten)
df_clean = df.drop_duplicates()

# Letztes behalten
df_clean = df.drop_duplicates(keep='last')

# Alle Duplikate entfernen (keines behalten)
df_clean = df.drop_duplicates(keep=False)

# Nur bestimmte Spalten prüfen
df_clean = df.drop_duplicates(subset=['Name'])
```

---

## Datentypen

### Prüfen und Konvertieren

```python
df = pd.DataFrame({
    'ID': ['001', '002', '003'],
    'Preis': ['10.5', '20.3', '15.0'],
    'Datum': ['2024-01-15', '2024-02-20', '2024-03-25'],
    'Aktiv': ['true', 'false', 'true']
})

print(df.dtypes)
# ID       object
# Preis    object
# Datum    object
# Aktiv    object

# Konvertieren
df['ID'] = df['ID'].astype(int)
df['Preis'] = df['Preis'].astype(float)
df['Datum'] = pd.to_datetime(df['Datum'])
df['Aktiv'] = df['Aktiv'].map({'true': True, 'false': False})

print(df.dtypes)
# ID                int64
# Preis           float64
# Datum    datetime64[ns]
# Aktiv              bool
```

### Fehlerbehandlung bei Konvertierung

```python
# Mit Fehlern umgehen
df['Preis'] = pd.to_numeric(df['Preis'], errors='coerce')  # Fehler → NaN
df['Datum'] = pd.to_datetime(df['Datum'], errors='coerce')  # Fehler → NaT

# errors='ignore' behält ursprünglichen Wert
# errors='raise' wirft Fehler (Standard)
```

---

## Ausreißer

### Erkennen

**Methoden zur Ausreißererkennung:**

| Methode | Formel/Grenze | Wann verwenden? |
|---------|---------------|----------------|
| IQR-Methode | Q1 - 1.5×IQR bis Q3 + 1.5×IQR | Robust, bei beliebiger Verteilung |
| Z-Score | \|z\| > 2-3 Standardabweichungen | Bei Normalverteilung |
| Domain-Wissen | z.B. Alter: 0-120 Jahre | Fachspezifische Grenzen |

#### IQR-Methode (Interquartilsabstand)

```python
def find_outliers_iqr(series):
    """Findet Ausreißer mit IQR-Methode"""
    Q1 = series.quantile(0.25)
    Q3 = series.quantile(0.75)
    IQR = Q3 - Q1
    
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    
    outliers = (series < lower_bound) | (series > upper_bound)
    return outliers

df['Ist_Ausreißer'] = find_outliers_iqr(df['Gehalt'])
print(df[df['Ist_Ausreißer']])
```

#### Z-Score-Methode

```python
def find_outliers_zscore(series, threshold=3):
    """Findet Ausreißer mit Z-Score"""
    z_scores = (series - series.mean()) / series.std()
    return abs(z_scores) > threshold

df['Ist_Ausreißer'] = find_outliers_zscore(df['Gehalt'])
```

### Behandeln

```python
# Entfernen
df_clean = df[~df['Ist_Ausreißer']]

# Ersetzen durch Grenzen (Winsorisierung)
Q1 = df['Gehalt'].quantile(0.25)
Q3 = df['Gehalt'].quantile(0.75)
IQR = Q3 - Q1

df['Gehalt_Clean'] = df['Gehalt'].clip(
    lower=Q1 - 1.5 * IQR,
    upper=Q3 + 1.5 * IQR
)

# Ersetzen durch Median
median = df['Gehalt'].median()
df.loc[df['Ist_Ausreißer'], 'Gehalt'] = median
```

---

## Inkonsistente Schreibweisen

### Text standardisieren

```python
df = pd.DataFrame({
    'Stadt': ['Berlin', 'BERLIN', 'berlin', ' Berlin ', 'Berln']
})

# Whitespace entfernen und einheitliche Schreibweise
df['Stadt_Clean'] = df['Stadt'].str.strip().str.title()
print(df['Stadt_Clean'])
# 0    Berlin
# 1    Berlin
# 2    Berlin
# 3    Berlin
# 4     Berln  ← Tippfehler bleibt!
```

### Kategorien vereinheitlichen

```python
# Mapping für Korrekturen
korrekturen = {
    'Berln': 'Berlin',
    'Belin': 'Berlin',
    'Muenchen': 'München',
    'Koeln': 'Köln'
}

df['Stadt_Clean'] = df['Stadt_Clean'].replace(korrekturen)
```

### Unique-Werte prüfen

```python
# Alle einzigartigen Werte
print(df['Stadt'].unique())

# Anzahl pro Wert
print(df['Stadt'].value_counts())
```

---

## Vollständiges Bereinigungsbeispiel

```python
import pandas as pd
import numpy as np

def bereinige_daten(df):
    """Vollständige Datenbereinigung"""
    df = df.copy()
    
    # 1. Duplikate entfernen
    print(f"Duplikate entfernt: {df.duplicated().sum()}")
    df = df.drop_duplicates()
    
    # 2. Spaltennamen standardisieren
    df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')
    
    # 3. Text-Spalten bereinigen
    for col in df.select_dtypes(include='object').columns:
        df[col] = df[col].str.strip()
    
    # 4. Fehlende Werte behandeln
    # Numerisch: mit Median füllen
    for col in df.select_dtypes(include='number').columns:
        if df[col].isna().any():
            df[col] = df[col].fillna(df[col].median())
    
    # Kategorisch: mit 'Unbekannt' füllen
    for col in df.select_dtypes(include='object').columns:
        if df[col].isna().any():
            df[col] = df[col].fillna('Unbekannt')
    
    # 5. Zusammenfassung
    print(f"Finale Größe: {df.shape}")
    print(f"Fehlende Werte: {df.isna().sum().sum()}")
    
    return df

# Anwendung
df_clean = bereinige_daten(df)
```

---

## Checkliste Datenbereinigung

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

start

:Daten laden;

:1. Erste Inspektion\nshape, info(), head();

:2. Fehlende Werte\nisna().sum();

if (Viele Fehlende?) then (ja)
    :Strategie wählen:\ndropna/fillna;
else (nein)
    :OK;
endif

:3. Duplikate prüfen\nduplicated().sum();

if (Duplikate?) then (ja)
    :drop_duplicates();
else (nein)
    :OK;
endif

:4. Datentypen prüfen\ndtypes;

:5. Ausreißer prüfen;

:6. Konsistenz prüfen\nvalue_counts();

:Bereinigte Daten ✓;

stop
@enduml
```

---

## Zusammenfassung

!!! success "Das Wichtigste"
    - **Fehlende Werte**: `isna()`, `dropna()`, `fillna()`
    - **Duplikate**: `duplicated()`, `drop_duplicates()`
    - **Datentypen**: `astype()`, `pd.to_datetime()`, `pd.to_numeric()`
    - **Ausreißer**: IQR-Methode oder Z-Score
    - **Text**: `.str.strip()`, `.str.lower()`, `.replace()`
    - **Immer prüfen**: `info()`, `describe()`, `value_counts()`

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `dropna()` und `fillna()`?
    2. Wie findest du Ausreißer mit der IQR-Methode?
    3. Was bedeutet `errors='coerce'` bei `pd.to_numeric()`?
    4. Wie entfernst du Duplikate, wobei das letzte Vorkommen behalten wird?
    
    ??? success "Antworten"
        1. `dropna()` entfernt Zeilen/Spalten mit NaN, `fillna()` ersetzt NaN durch Werte
        2. Werte außerhalb von [Q1 - 1.5×IQR, Q3 + 1.5×IQR] sind Ausreißer
        3. Ungültige Werte werden zu NaN konvertiert statt einen Fehler zu werfen
        4. `df.drop_duplicates(keep='last')`
