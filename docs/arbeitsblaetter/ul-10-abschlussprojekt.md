# UL-10: Abschlussprojekt – Musik-Clustering

!!! abstract "Advance Organizer"
    Dies ist deine Gelegenheit, alles Gelernte zusammenzuführen: Exploration, Vorverarbeitung, Clustering, Visualisierung und Interpretation. Du arbeitest eigenständig und präsentierst deine Ergebnisse – genau wie im echten Berufsleben als Datenanalyst.
    
    **Dein Ziel:** Eine vollständige, dokumentierte Analyse mit konkreten Handlungsempfehlungen. Qualität geht vor Quantität!

---

## Lernziele

Nach Bearbeitung dieses Projekts kannst du:

- [x] Eine vollständige Clustering-Analyse eigenständig durchführen
- [x] Ergebnisse professionell präsentieren und dokumentieren
- [x] Geschäftliche Empfehlungen aus technischen Analysen ableiten

---

## Aufgabenstellung

!!! example "Projektauftrag"
    
    **Szenario:** Du arbeitest als Data Analyst für einen Musik-Streaming-Dienst. Das Produktteam möchte verstehen, welche "Musik-Typen" es gibt, um bessere Playlists und Empfehlungen zu entwickeln.
    
    **Dein Auftrag:** Analysiere den Spotify Tracks Datensatz und finde Muster in der Musik.

---

## Anforderungen

Deine Analyse muss folgende Elemente enthalten:

### 1. Datenexploration und Vorverarbeitung
- [ ] Daten laden und verstehen
- [ ] Fehlende Werte behandeln
- [ ] Feature-Auswahl begründen
- [ ] Skalierung durchführen

### 2. Clustering-Analyse
- [ ] Optimale Clusteranzahl bestimmen (mit Begründung!)
- [ ] Mindestens **zwei verschiedene Algorithmen** anwenden
- [ ] Ergebnisse vergleichen

### 3. Visualisierung
- [ ] PCA für 2D-Darstellung
- [ ] Cluster-Profile (Heatmap oder Balkendiagramm)
- [ ] Mindestens **3 aussagekräftige Grafiken**

### 4. Interpretation
- [ ] Was bedeuten die gefundenen Cluster?
- [ ] Aussagekräftige Namen für jedes Cluster
- [ ] Beispiel-Songs pro Cluster (falls verfügbar)

### 5. Anwendungsvorschlag
- [ ] Wie könnte Spotify diese Erkenntnisse nutzen?
- [ ] Konkrete Vorschläge (z.B. Playlist-Ideen)

---

## Abgabeleistungen

| Leistung | Beschreibung |
|----------|--------------|
| **Jupyter Notebook** | Dokumentierte Analyse mit Code und Erklärungen |
| **Präsentation** | 5-10 Minuten, wichtigste Erkenntnisse |
| **Handout** | 1-2 Seiten Zusammenfassung |

---

## Bewertungskriterien

| Kriterium | Gewichtung | Beschreibung |
|-----------|------------|--------------|
| **Methoden** | 30% | Korrekte Anwendung von Skalierung, Clustering, PCA |
| **Visualisierungen** | 20% | Aussagekräftige, beschriftete Grafiken |
| **Interpretation** | 30% | Inhaltliche Bedeutung der Cluster, Business-Relevanz |
| **Dokumentation** | 20% | Struktur, Lesbarkeit, Nachvollziehbarkeit |

---

## Hinweise zur Durchführung

### Struktur deines Notebooks

```markdown
# Musik-Clustering mit Spotify-Daten

## 1. Einleitung
- Fragestellung
- Datensatz-Beschreibung

## 2. Datenexploration
- Laden und Überblick
- Deskriptive Statistik
- Visualisierungen

## 3. Datenvorverarbeitung
- Feature-Auswahl (mit Begründung!)
- Skalierung

## 4. Clustering
- Optimale Clusteranzahl
- Algorithmus 1: K-Means
- Algorithmus 2: [deine Wahl]
- Vergleich

## 5. Visualisierung
- PCA
- Cluster-Profile

## 6. Interpretation
- Was bedeuten die Cluster?
- Cluster-Namen
- Beispiel-Songs

## 7. Fazit und Empfehlungen
- Zusammenfassung
- Vorschläge für Spotify
```

### Tipps für die Präsentation

!!! tip "Präsentations-Tipps"
    
    **DO:**
    - Starte mit der Fragestellung
    - Zeige die wichtigsten 3-4 Grafiken
    - Erkläre die Cluster in verständlicher Sprache
    - Ende mit konkreten Empfehlungen
    
    **DON'T:**
    - Notebook vorlesen
    - Jeden Code-Block erklären
    - Zu technisch werden
    - Mehr als 10 Minuten

---

## Beispiel-Cluster (zur Inspiration)

Typische Musik-Cluster könnten sein:

| Cluster | Merkmale | Möglicher Name |
|---------|----------|----------------|
| A | Hohe Energy, hohe Danceability, mittlere Valence | "Party Mix" |
| B | Niedrige Energy, hohe Acousticness, geringe Instrumentalness | "Akustik & Singer-Songwriter" |
| C | Hohe Instrumentalness, niedrige Speechiness | "Instrumentale Musik" |
| D | Hohe Valence, mittlere Energy | "Feel-Good Hits" |
| E | Niedrige Valence, niedrige Energy | "Melancholische Balladen" |

---

## Häufige Fehler

!!! failure "Vermeide diese Fehler!"
    
    ❌ **Zu viele Cluster gewählt**  
    → 5-8 Cluster sind meist interpretierbar, mehr wird unübersichtlich
    
    ❌ **Notebook schlecht dokumentiert**  
    → Markdown-Zellen nutzen! Jeder Schritt braucht eine Erklärung
    
    ❌ **Visualisierungen ohne Titel/Beschriftung**  
    → Jedes Diagramm braucht:
    - Aussagekräftigen Titel
    - Achsenbeschriftungen
    - Legende (falls nötig)
    
    ❌ **Interpretation zu oberflächlich**  
    → Was bedeutet "hohe Energy"? Nenne konkrete Songs/Genres als Beispiele!
    
    ❌ **Präsentation = Notebook vorlesen**  
    → Extrahiere die Kernaussagen, zeige nur die wichtigsten Grafiken
    
    ❌ **Keine Business-Empfehlungen**  
    → Ohne Handlungsempfehlungen hat die Analyse keinen Wert für das Unternehmen

---

## Zeitplan-Vorschlag

| Phase | Dauer | Aktivitäten |
|-------|-------|-------------|
| **Exploration** | 45 min | Daten laden, verstehen, visualisieren |
| **Clustering** | 60 min | k wählen, Algorithmen anwenden, vergleichen |
| **Interpretation** | 30 min | Cluster analysieren, Namen vergeben |
| **Dokumentation** | 30 min | Notebook aufräumen, Texte schreiben |
| **Präsentation** | 15 min | Folien erstellen, Handout schreiben |

---

## Ressourcen

### Infoblätter zur Wiederholung
- [Einführung Unsupervised Learning](../infoblaetter/einfuehrung-unsupervised.md)
- [K-Means Clustering](../infoblaetter/kmeans-clustering.md)
- [PCA Dimensionsreduktion](../infoblaetter/pca-dimensionsreduktion.md)
- [Cluster-Evaluation](../infoblaetter/cluster-evaluation.md)

### Code-Snippets

```python
# Quick-Reference: Die wichtigsten Imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans, AgglomerativeClustering, DBSCAN
from sklearn.decomposition import PCA
from sklearn.metrics import silhouette_score

# Daten laden
df = pd.read_csv('spotify_tracks.csv')

# Sample für schnelles Arbeiten
df_sample = df.sample(n=20000, random_state=42)

# Skalieren
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# K-Means
kmeans = KMeans(n_clusters=5, random_state=42, n_init=10)
labels = kmeans.fit_predict(X_scaled)

# Silhouette Score
score = silhouette_score(X_scaled, labels)

# PCA für Visualisierung
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
```

---

## Viel Erfolg! 🎵

!!! success "Du schaffst das!"
    Du hast alle Werkzeuge gelernt. Jetzt bring sie zusammen und zeig, was du kannst!
    
    Denk daran:
    - Qualität vor Quantität
    - Interpretation ist wichtiger als fancy Grafiken
    - Ein gutes Cluster-Modell erzählt eine Geschichte

---

## Nach dem Projekt

Nach Abgabe und Präsentation:

- [ ] Feedback einholen
- [ ] Was hat gut funktioniert?
- [ ] Was würdest du beim nächsten Mal anders machen?

---

## Optionale Erweiterungen

Falls du früher fertig bist oder das Projekt vertiefen möchtest:

1. **Hierarchisches Clustering** als dritten Algorithmus
2. **DBSCAN** zur Ausreißer-Erkennung (Songs, die zu keinem Cluster passen)
3. **Zeitliche Analyse:** Unterscheiden sich Cluster nach Jahrzehnten?
4. **Genre-Validierung:** Stimmen deine Cluster mit echten Genres überein?

➡️ Optionale Arbeitsblätter: 
- [UL-OPT-01: Kreditkartenanalyse](ul-opt-01-kreditkarten.md)
- [UL-OPT-02: Big Data & Clustering](ul-opt-02-big-data.md)
