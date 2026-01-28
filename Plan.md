# Plan: Lernsituation Unsupervised Learning

## Übersicht

Dieses Dokument beschreibt den Plan für eine strukturierte Lernsituation zum Thema **Unsupervised Learning** für Fachinformatiker für Daten- und Prozessanalyse.

---

## Analyse des vorhandenen Materials

### Bisheriger Verlauf

Laut `old/bisheriger Verlauf.txt`:
- Das **Country-Dataset** wurde als Einführungsbeispiel verwendet (K-Means, PCA, Hierarchisches Clustering, GMM)
- Schüler haben anschließend die **Creditcard-Aufgabe** bearbeitet
- **Wunsch:** Strukturierterer und umfassenderer Aufbau

### Verfügbare Datensätze (aktualisiert)

| Datensatz | Beschreibung | Umfang | Komplexität | Eignung | Link |
|-----------|--------------|--------|-------------|---------|------|
| **Iris** | Blumenklassifikation (scikit-learn) | 150 Zeilen, 4 Features | Sehr niedrig | Mini-Beispiele, K-Means per Hand | Built-in |
| **Country-data** | Länderdaten (Wirtschaft, Gesundheit) | 167 Länder, 9 Features | Niedrig | Einführung/Demo | [Kaggle](https://www.kaggle.com/datasets/rohan0301/unsupervised-learning-on-country-data) |
| **Mall Customers** | Kundensegmentierung | 200 Kunden, 5 Features | Mittel | Erste eigenständige Analyse | [Kaggle](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) |
| **Wine Quality** | Weinqualität (rot/weiß) | ~6.500 Zeilen, 11 Features | Mittel-Hoch | Brücke zu komplexeren Daten | [UCI](https://archive.ics.uci.edu/ml/datasets/wine+quality) |
| **CustomerData (Creditcard)** | Kreditkartennutzung | ~900 Kunden, 17 Features | Hoch | Optionale Vertiefung | [Kaggle](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata) |
| **Spotify Tracks** | Audio-Features von Songs | ~100k Tracks, 10-15 Features | Hoch | **Abschlussprojekt** | [Kaggle](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset) |
| **US-Accidents** | Verkehrsunfälle USA | 7,7 Mio. Datensätze | Sehr hoch | **Optionales AB: Big Data** | [Kaggle](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents) |

### Im Notebook gezeigte Algorithmen (Country-Dataset)

1. **Datenexploration** - Pairplot mit Seaborn
2. **K-Means Clustering** inkl. Elbow-Methode
3. **PCA (Dimensionsreduktion)** für Visualisierung
4. **Hierarchisches Clustering** mit Dendrogramm
5. **Gaussian Mixture Models (GMM)**
6. **Cluster-Analyse/-Interpretation** (Normalisierung, Bar-Charts)

---

## Geplante Struktur der Lernsituation

### Infoblätter (Nachschlagewerke)

Die Infoblätter erklären die theoretischen Grundlagen und Syntax.

| Nr. | Infoblatt | Inhalte |
|-----|-----------|---------|
| **I-01** | **Einführung Unsupervised Learning** | Unterschied zu Supervised Learning, Anwendungsfälle, Überblick der Algorithmen |
| **I-02** | **Datenvorverarbeitung** | Skalierung (StandardScaler, MinMaxScaler), Umgang mit kategorialen Daten, Feature Selection |
| **I-03** | **K-Means Clustering** | Algorithmus, Initialisierung, Elbow-Methode, Silhouette Score, Vor-/Nachteile |
| **I-04** | **Hierarchisches Clustering** | Agglomeratives vs. Divisives Clustering, Linkage-Methoden, Dendrogramme |
| **I-05** | **Dimensionsreduktion (PCA)** | Prinzip der Hauptkomponentenanalyse, Varianz, Visualisierung, t-SNE/UMAP kurz erwähnt |
| **I-06** | **Weitere Clustering-Algorithmen** | DBSCAN, Gaussian Mixture Models, Vergleich der Algorithmen |
| **I-07** | **Cluster-Evaluation & Interpretation** | Metriken (Silhouette, Davies-Bouldin), Interpretation, Visualisierung, Wann Clustering NICHT nutzen |
| **I-08** | **Umgang mit großen Datenmengen** | Sampling-Strategien, Mini-Batch K-Means, Speicheroptimierung, Chunked Processing |

---

### Arbeitsblätter (Übungen)

Die Arbeitsblätter enthalten praktische Aufgaben mit steigendem Schwierigkeitsgrad.

| Nr. | Arbeitsblatt | Thema | Datensatz | Schwierigkeit |
|-----|--------------|-------|-----------|---------------|
| **UL-01** | Einführung | Was ist Clustering? Erste Visualisierung | Country-data | ⭐ |
| **UL-02** | Datenvorverarbeitung | Skalierung, Exploration | Country-data | ⭐ |
| **UL-03** | K-Means Grundlagen | Algorithmus verstehen, Elbow-Methode | Iris + Country-data | ⭐⭐ |
| **UL-04** | Dimensionsreduktion | PCA anwenden, Cluster visualisieren | Iris (4→2D perfekt) | ⭐⭐ |
| **UL-05** | Cluster-Interpretation | Analyse der Cluster-Eigenschaften | Country-data | ⭐⭐ |
| **UL-06** | Hierarchisches Clustering | Dendrogramme, Vergleich mit K-Means | Country-data | ⭐⭐ |
| **UL-07** | Kundensegmentierung | Erste eigenständige Analyse | Mall Customers | ⭐⭐⭐ |
| **UL-08** | Weinqualität-Analyse | Mehr Features, Algorithmenvergleich | Wine Quality | ⭐⭐⭐ |
| **UL-09** | Musik-Clustering | Abschlussprojekt vorbereiten | Spotify Tracks | ⭐⭐⭐⭐ |
| **UL-10** | Abschlussprojekt | Vollständige Analyse präsentieren | Spotify Tracks | ⭐⭐⭐⭐⭐ |
| | | | | |
| **UL-OPT-01** | *(Optional)* Kreditkartenanalyse | Komplexe Feature-Analyse, DBSCAN, GMM | CustomerData | ⭐⭐⭐⭐ |
| **UL-OPT-02** | *(Optional)* Big Data & Clustering | Umgang mit großen Datenmengen | US-Accidents | ⭐⭐⭐⭐⭐ |

---

## Detailplanung der Arbeitsblätter

### Phase 1: Einführung (Country-Dataset + Iris)

#### UL-01: Einführung in Unsupervised Learning

**Advance Organizer:**
> In diesem Arbeitsblatt lernst du die Grundidee von Unsupervised Learning kennen. Anders als bei Supervised Learning gibt es hier keine "richtigen Antworten" – der Algorithmus findet selbst Muster in den Daten. Diese Technik wird in der Praxis häufig für Kundensegmentierung, Anomalie-Erkennung und Datenexploration eingesetzt. 
>
> **Dein Ziel:** Am Ende kannst du erklären, wann Clustering sinnvoll ist, und hast erste Hypothesen über mögliche Gruppen im Datensatz formuliert.

**Lernziele:**
- Unterschied zwischen Supervised und Unsupervised Learning erklären
- Anwendungsfälle für Clustering nennen
- Ersten Datensatz laden und explorieren

**Aufgaben:**
- [ ] Theorie: Was ist Unsupervised Learning?
- [ ] Country-Datensatz laden und inspizieren
- [ ] Deskriptive Statistik erstellen
- [ ] Pairplot für ausgewählte Features
- [ ] Reflexion: Welche Gruppen von Ländern könnte es geben?

**Häufige Fehler:**
- ❌ `FileNotFoundError` → Prüfe den Dateipfad! Liegt die CSV im richtigen Ordner?
- ❌ Pairplot zu langsam → Wähle nur 3-4 Features aus: `sns.pairplot(df[['feature1', 'feature2', 'feature3']])`
- ❌ Daten nicht verstanden → Lies zuerst das Data Dictionary!

---

#### UL-02: Datenvorverarbeitung für Clustering

**Advance Organizer:**
> Clustering-Algorithmen messen Abstände zwischen Datenpunkten. Wenn ein Feature in Millionen gemessen wird und ein anderes in Prozent, dominiert das größere Feature die Berechnung – das verfälscht die Ergebnisse! Hier lernst du, wie du Daten so vorbereitest, dass alle Features fair berücksichtigt werden.
>
> **Dein Ziel:** Du verstehst, warum Skalierung notwendig ist, und kannst sie selbstständig anwenden. Dieses Wissen brauchst du für ALLE folgenden Arbeitsblätter.

**Lernziele:**
- Notwendigkeit der Skalierung verstehen
- StandardScaler und MinMaxScaler anwenden
- Auswirkungen der Skalierung visualisieren

**Aufgaben:**
- [ ] Warum ist Skalierung wichtig? (Beispiel mit unterschiedlichen Einheiten)
- [ ] StandardScaler auf Country-Daten anwenden
- [ ] Verteilungen vor/nach Skalierung vergleichen
- [ ] Korrelationsmatrix erstellen

**Häufige Fehler:**
- ❌ `ValueError: could not convert string to float` → Du hast kategoriale Spalten (z.B. Ländernamen) nicht entfernt!
- ❌ Skalierung auf gesamten DataFrame angewendet → Nur numerische Features skalieren: `df_numeric = df.select_dtypes(include=[np.number])`
- ❌ Fit und Transform verwechselt → Erst `fit()`, dann `transform()`, oder direkt `fit_transform()`

---

#### UL-03: K-Means Clustering

**Advance Organizer:**
> K-Means ist DER Standard-Algorithmus für Clustering – einfach zu verstehen, schnell zu berechnen, und in 90% der Fälle ein guter Startpunkt. Die Herausforderung: Du musst vorher wissen, wie viele Cluster du suchst! Die Elbow-Methode hilft dir dabei.
>
> **Dein Ziel:** Du kannst K-Means anwenden und die optimale Clusteranzahl begründet wählen. Das ist die Kernkompetenz für alle weiteren Clustering-Aufgaben.

**Lernziele:**
- K-Means Algorithmus verstehen
- Elbow-Methode zur Bestimmung der Clusteranzahl anwenden
- Erstes Clustering durchführen

**Aufgaben:**
- [ ] K-Means "per Hand" nachvollziehen am **Iris-Dataset** (nur 4 Features, 3 bekannte Cluster)
- [ ] Elbow-Kurve für Country-Daten erstellen
- [ ] K-Means mit optimaler Clusteranzahl anwenden
- [ ] Cluster-Labels dem Datensatz hinzufügen
- [ ] **Reflexion:** Warum funktioniert K-Means bei Iris so gut? (kompakte, getrennte Cluster)

**Häufige Fehler:**
- ❌ Elbow-Kurve zeigt keinen klaren Knick → Das ist normal! Wähle den Punkt, ab dem die Verbesserung deutlich abnimmt.
- ❌ Unterschiedliche Ergebnisse bei jedem Durchlauf → Setze `random_state=42` für Reproduzierbarkeit: `KMeans(n_clusters=3, random_state=42)`
- ❌ Daten nicht skaliert → K-Means braucht skalierte Daten! Zurück zu UL-02.
- ❌ `labels_` ist leer → Du musst zuerst `fit()` oder `fit_predict()` aufrufen!

---

#### UL-04: Dimensionsreduktion mit PCA

**Advance Organizer:**
> Wie visualisiert man 9 Features gleichzeitig? Gar nicht – unser Gehirn kann maximal 3 Dimensionen verarbeiten. PCA (Principal Component Analysis) komprimiert viele Features auf wenige "Hauptkomponenten", ohne zu viel Information zu verlieren. So kannst du Cluster endlich sehen!
>
> **Dein Ziel:** Du kannst PCA anwenden und verstehst, was die Hauptkomponenten bedeuten. Diese Technik wirst du im Abschlussprojekt brauchen.

**Lernziele:**
- PCA-Prinzip verstehen
- Dimensionsreduktion durchführen
- Cluster in 2D visualisieren

**Aufgaben:**
- [ ] PCA am **Iris-Dataset** anwenden (4D → 2D perfekt visualisierbar)
- [ ] Erklärte Varianz analysieren
- [ ] Scatterplot mit ersten zwei Hauptkomponenten erstellen
- [ ] Country-Daten mit PCA visualisieren
- [ ] Korrelation der Hauptkomponenten mit Original-Features

**Häufige Fehler:**
- ❌ Daten nicht skaliert vor PCA → PCA ist extrem empfindlich gegenüber unterschiedlichen Skalen!
- ❌ Zu wenig Varianz erklärt (<70%) → Verwende mehr Komponenten: `PCA(n_components=3)`
- ❌ `explained_variance_ratio_` nicht verstanden → Die Summe zeigt, wie viel Information erhalten bleibt (z.B. 0.85 = 85%)
- ❌ Scatterplot ohne Farben → Färbe nach Cluster-Label: `plt.scatter(X_pca[:,0], X_pca[:,1], c=labels)`

---

#### UL-05: Cluster-Interpretation

**Advance Organizer:**
> Cluster zu finden ist nur die halbe Miete – jetzt musst du sie verstehen! Was unterscheidet die Gruppen? Warum sind bestimmte Datenpunkte zusammen? Diese Interpretation ist der wichtigste Schritt für jeden Business Case, denn nur so kannst du Handlungsempfehlungen ableiten.
>
> **Dein Ziel:** Du kannst Cluster-Profile erstellen und inhaltlich interpretieren. Das ist die Basis für jede Präsentation deiner Ergebnisse.

**Lernziele:**
- Cluster inhaltlich interpretieren
- Cluster-Profile erstellen
- Ergebnisse visualisieren

**Aufgaben:**
- [ ] Mittelwerte pro Cluster berechnen
- [ ] Cluster-Profile als Balkendiagramm
- [ ] Länder den Clustern zuordnen
- [ ] Interpretation: Was charakterisiert jedes Cluster?
- [ ] **Reflexion:** Wie würdest du die Cluster benennen?

**Häufige Fehler:**
- ❌ Mittelwerte auf skalierten Daten berechnet → Zurück-transformieren oder auf Original-Daten gruppieren!
- ❌ Cluster-Labels stimmen nicht überein → Die Label-Nummern sind willkürlich! Cluster 0 heute kann morgen Cluster 2 sein.
- ❌ Bar-Chart unleserlich → Normalisiere auf Prozent oder nutze Heatmap: `sns.heatmap(cluster_means)`
- ❌ Keine sinnvolle Interpretation → Schau dir extreme Werte an: Welches Feature unterscheidet die Cluster am stärksten?

---

#### UL-06: Hierarchisches Clustering

**Advance Organizer:**
> K-Means zwingt dich, die Clusteranzahl vorher festzulegen. Hierarchisches Clustering zeigt dir stattdessen eine Baumstruktur (Dendrogramm), aus der du verschiedene Ebenen ablesen kannst. Das ist besonders nützlich, wenn du die "natürliche" Struktur der Daten entdecken möchtest.
>
> **Dein Ziel:** Du verstehst den Unterschied zwischen K-Means und hierarchischem Clustering und kannst für einen Use Case entscheiden, welcher Algorithmus besser passt.

**Lernziele:**
- Hierarchisches Clustering verstehen
- Dendrogramme erstellen und interpretieren
- Verschiedene Linkage-Methoden vergleichen

**Aufgaben:**
- [ ] Dendrogramm für Country-Daten erstellen
- [ ] Schnitt auf gewünschter Höhe durchführen
- [ ] Vergleich K-Means vs. Hierarchisches Clustering
- [ ] Vor-/Nachteile beider Methoden diskutieren
- [ ] **Zusatzaufgabe:** Welche Linkage-Methode liefert die "besten" Cluster?

**Häufige Fehler:**
- ❌ Dendrogramm zu unübersichtlich → Nutze `truncate_mode='lastp'`: `dendrogram(Z, truncate_mode='lastp', p=30)`
- ❌ Linkage-Objekt nicht verstanden → `linkage()` gibt eine Matrix zurück, die du an `dendrogram()` übergibst
- ❌ `fcluster` liefert falsche Anzahl → Prüfe den Parameter `t` (Schwellenwert) oder nutze `criterion='maxclust'`
- ❌ Vergleich mit K-Means nicht sinnvoll → Nutze dieselbe Clusteranzahl für beide Methoden!

---

### Phase 2: Anwendung (Mall Customer Segmentation)

#### UL-07: Kundensegmentierung

**Advance Organizer:**
> Jetzt wird es ernst: Du bekommst einen neuen Datensatz ohne Anleitung und musst selbst entscheiden, welche Schritte nötig sind. Das entspricht einer echten Aufgabe als Datenanalyst. Kundensegmentierung ist einer der häufigsten Anwendungsfälle für Clustering in der Wirtschaft!
>
> **Dein Ziel:** Du führst eine vollständige Clustering-Analyse durch und leitest konkrete Marketing-Empfehlungen ab. Das ist deine Generalprobe für das Abschlussprojekt.

**Lernziele:**
- Gelerntes eigenständig auf neuen Datensatz anwenden
- Geschäftliche Fragestellung mit Clustering beantworten

**Aufgaben:**
- [ ] Mall Customer Datensatz laden und explorieren
- [ ] Relevante Features auswählen und skalieren
- [ ] Optimale Clusteranzahl bestimmen
- [ ] K-Means Clustering durchführen
- [ ] Cluster visualisieren und interpretieren
- [ ] Marketing-Empfehlungen ableiten

**Szenario:**
> Ein Einkaufszentrum möchte seine Kunden besser verstehen, um gezieltes Marketing zu betreiben. Analysiere das Kundenverhalten und identifiziere verschiedene Kundengruppen.

**Zusatzaufgabe für Schnelle:**
- [ ] Vergleiche das Ergebnis mit hierarchischem Clustering

**Häufige Fehler:**
- ❌ CustomerID als Feature verwendet → IDs sind keine sinnvollen Features!
- ❌ Geschlecht nicht berücksichtigt → Kategoriale Features müssen encodiert werden (oder separat analysieren)
- ❌ Nur 2D-Visualisierung → Mit nur 3-4 Features geht auch 3D: `from mpl_toolkits.mplot3d import Axes3D`
- ❌ Marketing-Empfehlungen zu vage → Sei konkret! "Cluster 2 sind junge High-Spender → Luxus-Events anbieten"

---

### Phase 3: Vertiefung (Wine Quality Dataset)

#### UL-08: Weinqualität-Analyse

**Advance Organizer:**
> Mehr Features bedeuten mehr Komplexität: Welche Features sind wichtig? Welche sind redundant? Hier lernst du, mit höherdimensionalen Daten umzugehen und verschiedene Algorithmen systematisch zu vergleichen. Außerdem begegnest du zum ersten Mal dem Silhouette Score als objektive Bewertungsmetrik.
>
> **Dein Ziel:** Du kannst Feature-Korrelationen analysieren, Algorithmen vergleichen und deine Entscheidungen mit Metriken begründen.

**Lernziele:**
- Mit mehr Features arbeiten (11 statt 5)
- Feature-Korrelationen verstehen und nutzen
- Verschiedene Algorithmen vergleichen

**Aufgaben:**
- [ ] Wine Quality Datensatz laden (rot und weiß getrennt oder zusammen?)
- [ ] Feature-Korrelationen analysieren – welche Features sind redundant?
- [ ] K-Means und Hierarchisches Clustering anwenden
- [ ] Ergebnisse vergleichen: Welcher Algorithmus liefert "bessere" Cluster?
- [ ] Silhouette Score berechnen und interpretieren
- [ ] Cluster-Profile erstellen: Welche Weintypen gibt es?

**Szenario:**
> Ein Weinhändler möchte sein Sortiment besser strukturieren. Finde Gruppen von Weinen mit ähnlichen Eigenschaften.

**Zusatzaufgabe für Schnelle:**
- [ ] Probiere DBSCAN aus – findet er Ausreißer (schlechte Weine)?

**Häufige Fehler:**
- ❌ Rot- und Weißwein gemischt ohne Kennzeichnung → Füge eine Spalte `type` hinzu oder analysiere getrennt
- ❌ Zu viele korrelierte Features → Entferne hoch korrelierte Features (>0.8) oder nutze PCA zur Reduktion
- ❌ Silhouette Score falsch interpretiert → Höher ist besser! Werte nahe 1 = gute Trennung, nahe 0 = überlappend
- ❌ DBSCAN findet nur 1 Cluster → Parameter `eps` und `min_samples` anpassen (Grid-Search!)

---

### Phase 4: Projekt (Spotify Tracks)

#### UL-09: Musik-Clustering – Projektvorbereitung

**Advance Organizer:**
> Das Abschlussprojekt steht vor der Tür! In diesem Arbeitsblatt erkundest du den Spotify-Datensatz und entwickelst deine Strategie. Mit ~100.000 Tracks ist das der größte Datensatz bisher – du musst also auch über Effizienz nachdenken.
>
> **Dein Ziel:** Du hast den Datensatz verstanden, eine Fragestellung formuliert und erste Clustering-Ergebnisse erzielt. Damit bist du für das Abschlussprojekt vorbereitet.

**Lernziele:**
- Größeren Datensatz eigenständig analysieren
- Feature-Auswahl und -Engineering
- Sinnvolle Clustering-Strategie entwickeln

**Aufgaben:**
- [ ] Spotify Tracks Datensatz laden und explorieren
- [ ] Audio-Features verstehen (danceability, energy, tempo, valence, acousticness, ...)
- [ ] Feature-Korrelationen analysieren
- [ ] Sampling-Strategie für effizientes Arbeiten entwickeln (~10.000-50.000 Tracks)
- [ ] Erste Clustering-Versuche durchführen
- [ ] Fragestellung für Abschlussprojekt formulieren

**Mögliche Fragestellungen:**
- Welche "Musik-Typen" gibt es? (Party, Entspannung, Workout, ...)
- Kann man Genres durch Clustering "entdecken"?
- Wie unterscheiden sich die Cluster verschiedener Jahrzehnte?

**Häufige Fehler:**
- ❌ `MemoryError` bei 100k Tracks → Arbeite mit Sample: `df_sample = df.sample(n=20000, random_state=42)`
- ❌ Kategoriale Features (Genre, Artist) ignoriert → Diese können später zur Validierung dienen!
- ❌ Tempo extrem unterschiedlich (60-200 BPM) → Skalierung ist hier besonders wichtig
- ❌ Keine klare Fragestellung → Formuliere eine konkrete Frage, bevor du loslegst!

---

#### UL-10: Abschlussprojekt – Musik-Clustering

**Advance Organizer:**
> Dies ist deine Gelegenheit, alles Gelernte zusammenzuführen: Exploration, Vorverarbeitung, Clustering, Visualisierung und Interpretation. Du arbeitest eigenständig und präsentierst deine Ergebnisse – genau wie im echten Berufsleben als Datenanalyst.
>
> **Dein Ziel:** Eine vollständige, dokumentierte Analyse mit konkreten Handlungsempfehlungen. Qualität geht vor Quantität!

**Lernziele:**
- Vollständige Clustering-Analyse eigenständig durchführen
- Ergebnisse präsentieren und dokumentieren
- Geschäftliche Empfehlungen ableiten

**Aufgabenstellung:**
> Analysiere den Spotify Tracks Datensatz und finde Muster in der Musik.
> 
> Erstelle eine vollständige Analyse mit:
> - Datenexploration und Vorverarbeitung
> - Anwendung mindestens zweier Clustering-Algorithmen
> - Visualisierung der Ergebnisse (PCA, Cluster-Profile)
> - Interpretation: Was bedeuten die gefundenen Cluster?
> - Anwendungsvorschlag: Wie könnte Spotify diese Erkenntnisse nutzen?

**Abgabeleistungen:**
- [ ] Jupyter Notebook mit dokumentierter Analyse
- [ ] Kurze Präsentation (5-10 Min)
- [ ] Handout mit den wichtigsten Erkenntnissen

**Benotungskriterien:**
| Kriterium | Gewichtung |
|-----------|------------|
| Korrekte Anwendung der Methoden | 30% |
| Qualität der Visualisierungen | 20% |
| Interpretation und Begründung | 30% |
| Präsentation und Dokumentation | 20% |

**Häufige Fehler:**
- ❌ Zu viele Cluster gewählt → 5-8 Cluster sind meist interpretierbar, mehr wird unübersichtlich
- ❌ Notebook schlecht dokumentiert → Markdown-Zellen nutzen! Jeder Schritt braucht eine Erklärung
- ❌ Visualisierungen ohne Titel/Beschriftung → Jedes Diagramm braucht Titel, Achsenbeschriftung, Legende
- ❌ Interpretation zu oberflächlich → Was bedeutet "hohe Energy"? Nenne konkrete Songs/Genres als Beispiele!
- ❌ Präsentation = Notebook vorlesen → Extrahiere die Kernaussagen, zeige nur die wichtigsten Grafiken

---

### Optionale Arbeitsblätter (für schnelle Schüler)

#### UL-OPT-01: Kreditkartenanalyse *(Optional)*

**Advance Organizer:**
> Dieser Datensatz ist der komplexeste bisher: 17 Features, viele Korrelationen, und die Frage nach Risikokunden erfordert besondere Aufmerksamkeit für Ausreißer. Hier lernst du DBSCAN und GMM praktisch anzuwenden – Algorithmen, die bei K-Means nicht gut funktionierenden Datenstrukturen glänzen können.
>
> **Dein Ziel:** Du beherrschst den Umgang mit hochdimensionalen Daten und kannst verschiedene Algorithmen gezielt einsetzen.

**Lernziele:**
- Komplexere Datensätze mit vielen Features analysieren
- Feature-Engineering und -Selection anwenden
- DBSCAN und GMM praktisch anwenden

**Aufgaben:**
- [ ] CustomerData Datensatz explorieren (17 Features verstehen)
- [ ] Feature-Korrelationen analysieren – dimensionsreduzierte Feature-Sets erstellen
- [ ] DBSCAN als Alternative zu K-Means ausprobieren (Ausreißer = Risikokunden?)
- [ ] Gaussian Mixture Models anwenden
- [ ] Algorithmen-Vergleich durchführen
- [ ] Kundenprofile erstellen und interpretieren

**Szenario:**
> Eine Bank möchte Kreditkartenkunden segmentieren, um personalisierte Angebote zu entwickeln und Risikokunden zu identifizieren.

**Häufige Fehler:**
- ❌ 17 Features auf einmal clustern → Starte mit PCA oder Feature-Selection (5-8 wichtigste Features)
- ❌ DBSCAN-Parameter blind gewählt → Nutze k-distance Plot zur `eps`-Bestimmung
- ❌ GMM ohne Kovarianz-Typ getestet → Probiere `covariance_type='full'` vs. `'diag'`
- ❌ Ausreißer = Risikokunden? → Nicht automatisch! Prüfe, ob Ausreißer wirklich problematisch sind (hohe Balance + keine Zahlungen)

---

#### UL-OPT-02: Big Data & Clustering – Umgang mit großen Datenmengen *(Optional)*

**Advance Organizer:**
> In der echten Arbeitswelt sind Datensätze oft zu groß, um sie komplett in den Speicher zu laden. Mit 7,7 Millionen Unfällen lernst du hier Techniken, die du später im Beruf brauchen wirst: Sampling, Chunked Processing und speichereffiziente Algorithmen.
>
> **Dein Ziel:** Du verstehst die Grenzen von Standard-Clustering und kannst Lösungsstrategien für Big-Data-Szenarien entwickeln.

**Lernziele:**
- Herausforderungen bei großen Datensätzen verstehen
- Sampling-Strategien anwenden
- Speicher- und Laufzeitoptimierung
- Mini-Batch K-Means kennenlernen

**Aufgaben:**
- [ ] US-Accidents Datensatz laden – Speicherverbrauch und Ladezeit messen
- [ ] **Chunked Loading:** Datei in Teilen einlesen mit `pd.read_csv(..., chunksize=...)`
- [ ] **Sampling-Strategien vergleichen:**
    - Zufälliges Sampling
    - Stratifiziertes Sampling (nach State, Severity, ...)
    - Systematisches Sampling
- [ ] **Mini-Batch K-Means** vs. Standard K-Means: Laufzeit und Qualität vergleichen
- [ ] **Feature-Engineering:** Zeitliche Features extrahieren (Stunde, Wochentag, Monat)
- [ ] Clustering auf Sample durchführen und Ergebnisse auf Gesamtdatensatz übertragen
- [ ] **Reflexion:** Ab welcher Datengröße werden diese Techniken notwendig?

**Szenario:**
> Ein Verkehrsministerium möchte Unfallmuster analysieren. Der Datensatz ist zu groß für direktes Clustering – entwickle eine Strategie!

**Technische Inhalte:**
```python
# Chunked Loading
for chunk in pd.read_csv('US_Accidents.csv', chunksize=100000):
    process(chunk)

# Mini-Batch K-Means
from sklearn.cluster import MiniBatchKMeans
mbk = MiniBatchKMeans(n_clusters=5, batch_size=1000)
mbk.partial_fit(sample)

# Speicheroptimierung
df['severity'] = df['severity'].astype('int8')  # statt int64
```

**Lerneffekt:**
- Verstehen, dass "mehr Daten" nicht immer "bessere Ergebnisse" bedeutet
- Praktische Techniken für die Arbeitswelt (dort sind Datensätze oft groß)
- Kritisches Denken: Wann ist Sampling vertretbar?

**Häufige Fehler:**
- ❌ `MemoryError` beim Laden → Nutze `usecols` um nur benötigte Spalten zu laden
- ❌ Sample nicht repräsentativ → Stratifiziertes Sampling nach wichtigen Kategorien (State, Severity)
- ❌ Mini-Batch K-Means mit zu kleiner `batch_size` → Minimum ~1000, sonst instabile Ergebnisse
- ❌ Datetime nicht geparst → `pd.read_csv(..., parse_dates=['Start_Time'])`
- ❌ Clustering ohne Feature-Selection → Der Datensatz hat ~47 Spalten! Wähle 5-10 relevante Features aus

---

## Lernziele der Lernsituation (Gesamtübersicht)

Nach Bearbeitung der Materialien können die Schüler:

### Wissen (Kognitiv)
- [x] Den Unterschied zwischen Supervised und Unsupervised Learning erklären
- [x] Die Funktionsweise von K-Means, Hierarchischem Clustering und DBSCAN beschreiben
- [x] PCA als Dimensionsreduktionsmethode erklären
- [x] Metriken zur Cluster-Evaluation benennen und interpretieren
- [x] Strategien für den Umgang mit großen Datenmengen kennen *(optional)*

### Können (Methodisch)
- [x] Daten für Clustering vorbereiten (Skalierung, Feature Selection)
- [x] Die optimale Clusteranzahl bestimmen (Elbow, Silhouette)
- [x] Verschiedene Clustering-Algorithmen mit scikit-learn anwenden
- [x] Cluster-Ergebnisse visualisieren und interpretieren
- [x] Sampling-Strategien für große Datensätze anwenden *(optional)*

### Bewerten (Analytisch)
- [x] Algorithmen für verschiedene Anwendungsfälle auswählen
- [x] Clustering-Ergebnisse kritisch bewerten
- [x] Geschäftliche Empfehlungen aus Analysen ableiten
- [x] Grenzen von Clustering-Methoden erkennen

---

## Zeitplanung (Vorschlag)

| Phase | Arbeitsblätter | Geschätzter Zeitaufwand |
|-------|---------------|------------------------|
| **Phase 1: Einführung** | UL-01 bis UL-06 | 6-8 Unterrichtsstunden |
| **Phase 2: Anwendung** | UL-07 | 2-3 Unterrichtsstunden |
| **Phase 3: Vertiefung** | UL-08 | 2-3 Unterrichtsstunden |
| **Phase 4: Projekt** | UL-09 bis UL-10 | 4-6 Unterrichtsstunden |
| **Gesamt (Pflicht)** | | **14-20 Unterrichtsstunden** |
| | | |
| *Optional: Kreditkarten* | UL-OPT-01 | 2-3 Unterrichtsstunden |
| *Optional: Big Data* | UL-OPT-02 | 2-3 Unterrichtsstunden |

---

## Technische Voraussetzungen

### Python-Pakete
```python
# Installation
pip install numpy pandas matplotlib seaborn scikit-learn scipy

# Imports für die Lernsituation
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_iris, load_wine
from sklearn.preprocessing import StandardScaler, MinMaxScaler
from sklearn.cluster import KMeans, AgglomerativeClustering, DBSCAN, MiniBatchKMeans
from sklearn.mixture import GaussianMixture
from sklearn.decomposition import PCA
from sklearn.metrics import silhouette_score
from scipy.cluster.hierarchy import dendrogram, linkage
```

### Datensätze

| Datensatz | Quelle | Link | Hinweis |
|-----------|--------|------|---------|
| Iris | scikit-learn | `sklearn.datasets.load_iris()` | Built-in, kein Download |
| Country-data | Kaggle | [Link](https://www.kaggle.com/datasets/rohan0301/unsupervised-learning-on-country-data) | Bereits auf IBM-Server |
| Mall Customers | Kaggle | [Link](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) | Bereits auf IBM-Server |
| Wine Quality | UCI ML Repository | [Link](https://archive.ics.uci.edu/ml/datasets/wine+quality) | Download erforderlich |
| CustomerData | Kaggle | [Link](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata) | Bereits auf IBM-Server |
| Spotify Tracks | Kaggle | [Link](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset) | Download erforderlich |
| US-Accidents | Kaggle | [Link](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents) | ~1GB, Download erforderlich |

---

## Ordnerstruktur (Ziel)

```
docs/
├── index.md                          # Startseite
├── arbeitsblaetter/
│   ├── ul-01-einfuehrung.md
│   ├── ul-02-vorverarbeitung.md
│   ├── ul-03-kmeans.md
│   ├── ul-04-pca.md
│   ├── ul-05-interpretation.md
│   ├── ul-06-hierarchisch.md
│   ├── ul-07-kundensegmentierung.md
│   ├── ul-08-weinqualitaet.md
│   ├── ul-09-musik-clustering.md
│   ├── ul-10-abschlussprojekt.md
│   ├── ul-opt-01-kreditkarten.md
│   └── ul-opt-02-big-data.md
├── infoblaetter/
│   ├── einfuehrung-unsupervised.md
│   ├── datenvorverarbeitung.md
│   ├── kmeans-clustering.md
│   ├── hierarchisches-clustering.md
│   ├── pca-dimensionsreduktion.md
│   ├── weitere-algorithmen.md
│   ├── cluster-evaluation.md
│   └── grosse-datenmengen.md
└── assets/
    ├── files/
    │   ├── Country-data.csv
    │   ├── data-dictionary.csv
    │   ├── Mall_Customers.csv
    │   ├── CustomerData.csv
    │   ├── winequality-red.csv
    │   ├── winequality-white.csv
    │   └── spotify_tracks.csv
    └── images/
        └── unsupervised/
            ├── elbow-method.png
            ├── dendrogramm.png
            ├── pca-visualization.png
            └── sampling-strategies.png
```

---

## Nächste Schritte

1. [ ] **docs/index.md** erstellen (Startseite)
2. [ ] **Infoblätter** erstellen (I-01 bis I-08)
3. [ ] **Arbeitsblätter** erstellen (UL-01 bis UL-10 + optionale)
4. [ ] **mkdocs.yml** anpassen (Navigation)
5. [ ] **Datensätze** beschaffen:
   - [ ] Wine Quality von UCI
   - [ ] Spotify Tracks von Kaggle
   - [ ] US-Accidents auf IBM-Server prüfen
6. [ ] **Grafiken** erstellen und einbinden
7. [ ] Lokale Tests durchführen
8. [ ] GitHub Pages Deployment prüfen

---

## Entscheidungen

- [x] Lösungen sollen nicht angeboten werden.
- [x] Die Aufgaben sollen auf dem IBM-Server ausgeführt werden. Die Datasets liegen dort vor.
- [x] Optionale Zusatzaufgaben für schnellere Schüler → UL-OPT-01, UL-OPT-02
- [x] Keine JupyterLite-Integration (IBM-Server reicht)
- [x] Spotify Tracks als Abschlussprojekt (statt US-Accidents)
- [x] US-Accidents als optionales Big-Data-Arbeitsblatt

---

## Änderungshistorie

| Datum | Änderung |
|-------|---------|
| 28.01.2026 | Initiale Version erstellt |
| 28.01.2026 | Überarbeitung: Neue Datensätze (Iris, Wine, Spotify), bessere Progression, optionales Big-Data-AB mit US-Accidents |

---

*Erstellt am: 28. Januar 2026*
*Letzte Aktualisierung: 28. Januar 2026*
