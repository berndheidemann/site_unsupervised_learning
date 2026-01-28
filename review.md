# Didaktische Analyse: Code-Vollständigkeit vs. Lernziele

## Fragestellung

**Fördert vollständiger Code das Lernen oder nur Copy&Paste?**

Diese Analyse überprüft für jedes Arbeitsblatt, ob die Code-Vollständigkeit zu den formulierten Lernzielen passt.

---

## Methodik

Für jedes Arbeitsblatt wurde geprüft:
1. **Lernziele** – Welche Bloom-Stufe wird angestrebt?
2. **Code-Vollständigkeit** – Komplett, Gerüst mit Lücken, oder nur Struktur?
3. **Passung** – ✅ Angemessen, ⚠️ Fragwürdig, ❌ Mismatch

### Bloom-Taxonomie Referenz

| Stufe | Verb-Beispiele | Code-Erwartung |
|-------|----------------|----------------|
| **Verstehen** | erklären, interpretieren, verstehen | Vollständiger Code akzeptabel |
| **Anwenden** | anwenden, durchführen, berechnen | Lücken oder Aufgaben zum Coden |
| **Analysieren** | vergleichen, untersuchen, unterscheiden | Eigenständige Analyse gefordert |
| **Bewerten** | beurteilen, entscheiden, begründen | Reflexionsfragen, keine Code-Lösung |

---

## Analyse der Arbeitsblätter

### Phase 1: Grundlagen (UL-01 bis UL-05)

#### UL-01: Einführung Unsupervised Learning

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Supervised und Unsupervised Learning unterscheiden" | Verstehen | Minimaler Code (nur Laden) | ✅ |
| "Country-Datensatz explorieren" | Anwenden | Fast kein Code, nur Fragen | ✅ |

**Bewertung:** ✅ **Gut** – Wenig Code, viele offene Fragen. Passt zum explorativen Einstieg.

---

#### UL-02: Datenvorverarbeitung (Skalierung)

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Notwendigkeit von Skalierung verstehen" | Verstehen | Vergleichs-Code komplett | ✅ |
| "StandardScaler und MinMaxScaler anwenden" | Anwenden | Komplett vorgegeben | ⚠️ |
| "Feature-Korrelationen berechnen" | Anwenden | Komplett vorgegeben | ⚠️ |

**Bewertung:** ⚠️ **Verbesserungswürdig** – "Anwenden" impliziert eigenes Tun, aber Code ist komplett.

**Empfehlung:**
- Lernziele anpassen zu "verstehen, wie..." ODER
- Code-Lücken einfügen für `fit_transform()` Aufrufe

---

#### UL-03: K-Means Clustering

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "K-Means Algorithmus verstehen und erklären" | Verstehen | Schritt-für-Schritt Code | ✅ |
| "Elbow-Methode anwenden" | Anwenden | Komplett vorgegeben | ⚠️ |
| "K-Means mit scikit-learn durchführen" | Anwenden | Fast komplett | ⚠️ |

**Bewertung:** ⚠️ **Verbesserungswürdig** – Verstehen funktioniert, "Anwenden" fraglich.

**Empfehlung:**
- Elbow-Schleife als Lücke: `for k in range(1, 11):` vorgeben, aber `kmeans = ___` ausfüllen lassen
- Silhouette-Berechnung selbst ergänzen lassen

---

#### UL-04: PCA Dimensionsreduktion

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "PCA-Konzept verstehen" | Verstehen | Visualisierungs-Code komplett | ✅ |
| "PCA auf Datensätze anwenden" | Anwenden | Komplett vorgegeben | ⚠️ |
| "Scree-Plot interpretieren" | Analysieren | Code komplett, Interpretation offen | ✅ |

**Bewertung:** ⚠️ **Teils gut** – Interpretation durch Fragen gefordert, aber Anwendung zu komplett.

---

#### UL-05: Cluster-Interpretation

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Cluster inhaltlich interpretieren" | Analysieren | Code komplett, aber **Lücken für Namen** | ✅ |
| "Aussagekräftige Cluster-Namen vergeben" | Bewerten | `cluster_names = {0: '___', ...}` | ✅ |

**Bewertung:** ✅ **Gut** – Sinnvolle Mischung: Code liefert Daten, Interpretation bleibt beim Lernenden.

---

### Phase 2: Eigenständige Anwendung (UL-06 bis UL-10)

#### UL-06: Hierarchisches Clustering

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Hierarchisches Clustering verstehen und erklären" | Verstehen | Code komplett | ✅ |
| "Dendrogramme interpretieren" | Analysieren | Code komplett, Fragen offen | ✅ |
| "Linkage-Methoden vergleichen" | Analysieren | Code komplett, **Tabelle mit Lücken** | ✅ |

**Bewertung:** ✅ **Gut** – Code zeigt Technik, Tabellen und Fragen fordern Analyse.

---

#### UL-07: Kundensegmentierung ⭐

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Gelerntes eigenständig anwenden" | Anwenden | **Code-Gerüste mit Lücken** | ✅ |
| "Geschäftliche Fragestellung beantworten" | Bewerten | Nur Struktur, keine Lösung | ✅ |
| "Marketing-Empfehlungen ableiten" | Bewerten | Tabellen-Template, kein Code | ✅ |

**Bewertung:** ✅ **Sehr gut** – Hier wird es ernst: 
- `features = ['___', '___', '___']`
- `# Elbow-Methode` (leerer Codeblock)
- `df['Cluster'] = ___`

**Das ist die richtige Abstufung!**

---

#### UL-08: Weinqualität-Analyse

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Mit mehr Features arbeiten" | Anwenden | Korrelations-Code komplett | ⚠️ |
| "Algorithmen systematisch vergleichen" | Analysieren | Vergleichs-Code komplett | ⚠️ |

**Bewertung:** ⚠️ **Verbesserungswürdig** – Für ein Arbeitsblatt in Phase 2 zu viel vorgegebener Code.

**Empfehlung:** 
- Feature-Auswahl selbst treffen lassen
- Nur Struktur für Silhouette-Vergleich vorgeben

---

#### UL-09: Musik-Clustering (Projektvorbereitung)

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Größeren Datensatz analysieren" | Anwenden | Sampling-Code komplett | ✅* |
| "Clustering-Strategie entwickeln" | Bewerten | Code als Vorlage, Entscheidungen offen | ✅ |

**Bewertung:** ✅ **Akzeptabel** – Fokus auf Strategie-Entwicklung, Code dient als Orientierung.

*Hinweis: Der vollständige Code ist hier vertretbar, da es um Lerngerüste für das Abschlussprojekt geht.

---

#### UL-10: Abschlussprojekt

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Vollständige Analyse eigenständig durchführen" | Anwenden + Analysieren | **Nur Struktur + Quick-Reference** | ✅ |
| "Ergebnisse präsentieren" | Bewerten | Keine Code-Lösung | ✅ |

**Bewertung:** ✅ **Sehr gut** – Genau richtig: Struktur vorgeben, aber keine Lösung.

---

### Optionale Arbeitsblätter

#### UL-OPT-01: Kreditkartenanalyse

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Komplexere Datensätze analysieren" | Analysieren | Code komplett | ⚠️ |
| "DBSCAN und GMM praktisch nutzen" | Anwenden | Code komplett | ⚠️ |

**Bewertung:** ⚠️ **Verbesserungswürdig** – Bei optionalem, fortgeschrittenem Material mehr Eigenleistung erwartbar.

---

#### UL-OPT-02: Big Data & Clustering

| Lernziel | Bloom-Stufe | Code-Vollständigkeit | Passung |
|----------|-------------|---------------------|---------|
| "Sampling-Strategien anwenden" | Anwenden | Code komplett | ⚠️ |
| "Mini-Batch K-Means nutzen" | Anwenden | Code komplett | ⚠️ |

**Bewertung:** ⚠️ **Verbesserungswürdig** – Techniken werden demonstriert, nicht erarbeitet.

---

## Zusammenfassung

### Übersichtstabelle

| Arbeitsblatt | Phase | Lernziel-Stufe | Code-Vollständigkeit | Bewertung |
|--------------|-------|----------------|---------------------|-----------|
| UL-01 | Grundlagen | Verstehen | Minimal | ✅ |
| UL-02 | Grundlagen | Verstehen/Anwenden | Komplett | ⚠️ |
| UL-03 | Grundlagen | Verstehen/Anwenden | Komplett | ⚠️ |
| UL-04 | Grundlagen | Verstehen/Anwenden | Komplett | ⚠️ |
| UL-05 | Grundlagen | Analysieren | Komplett + Lücken | ✅ |
| UL-06 | Vertiefung | Verstehen/Analysieren | Komplett + Lücken | ✅ |
| **UL-07** | **Anwendung** | **Anwenden/Bewerten** | **Gerüste + Lücken** | ✅ |
| UL-08 | Anwendung | Anwenden/Analysieren | Komplett | ⚠️ |
| UL-09 | Vorbereitung | Anwenden/Bewerten | Komplett (als Vorlage) | ✅ |
| **UL-10** | **Projekt** | **Alle Stufen** | **Nur Struktur** | ✅ |
| UL-OPT-01 | Optional | Anwenden | Komplett | ⚠️ |
| UL-OPT-02 | Optional | Anwenden | Komplett | ⚠️ |

### Statistik

- ✅ Gut: **6 von 12** (50%)
- ⚠️ Verbesserungswürdig: **6 von 12** (50%)
- ❌ Kritisch: **0 von 12** (0%)

---

## Handlungsempfehlungen

### Option A: Lernziele anpassen (minimaler Aufwand)

Bei Arbeitsblättern mit "Anwenden"-Zielen und vollständigem Code die Lernziele zu "Verstehen" umformulieren:

| Vorher | Nachher |
|--------|---------|
| "StandardScaler und MinMaxScaler **anwenden**" | "StandardScaler und MinMaxScaler **verstehen und interpretieren**" |
| "Elbow-Methode **anwenden**" | "Elbow-Methode **nachvollziehen und interpretieren**" |
| "K-Means **durchführen**" | "K-Means-Workflow **verstehen**" |

**Vorteil:** Konsistenz zwischen Anspruch und Material  
**Nachteil:** Niedrigere Bloom-Stufe, weniger Kompetenzentwicklung

### Option B: Code-Lücken einfügen (empfohlen)

Die betroffenen Arbeitsblätter um gezielte Lücken ergänzen:

#### UL-02 Beispiel:
```python
# Vorher (komplett):
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Nachher (mit Lücke):
scaler = _______________  # Wähle den richtigen Scaler
X_scaled = scaler._______________(X)  # Welche Methode?
```

#### UL-03 Beispiel:
```python
# Vorher (komplett):
for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans.fit(X_scaled)
    inertias.append(kmeans.inertia_)

# Nachher (mit Lücke):
for k in K_range:
    kmeans = KMeans(n_clusters=___, random_state=42, n_init=10)
    _______________  # Trainiere das Modell
    inertias.append(_______________)  # Was speichern wir?
```

**Vorteil:** Echtes "Anwenden" wird gefordert  
**Nachteil:** Überarbeitung aller betroffenen Blätter nötig

### Option C: Progressives Scaffolding

Die Code-Vollständigkeit systematisch reduzieren:

| Phase | Arbeitsblatt | Code-Strategie |
|-------|--------------|----------------|
| 1. Grundlagen | UL-01 bis UL-04 | Vollständiger Code (Verstehen) |
| 2. Übergang | UL-05, UL-06 | Code + Interpretations-Lücken |
| 3. Anwendung | UL-07, UL-08 | Code-Gerüste mit Lücken |
| 4. Projekt | UL-09, UL-10 | Nur Struktur, keine Lösung |

**Aktueller Stand:** UL-07 und UL-10 folgen diesem Muster bereits ✅

**Empfehlung:** UL-08 anpassen (mehr Lücken), optionale Blätter als "Referenz-Material" kennzeichnen.

---

## Konkreter Aktionsplan

### Priorität 1: UL-08 überarbeiten
Das Arbeitsblatt liegt in Phase 2 (Anwendung), hat aber Phase-1-Code-Stil.

**Änderungen:**
1. Feature-Auswahl: Nur Optionen auflisten, Entscheidung beim Lernenden
2. Algorithmen-Vergleich: Nur Struktur, kein fertiger Loop
3. Cluster-Profile: Lücken für Interpretation

### Priorität 2: UL-02 bis UL-04 wahlweise anpassen
**Entweder:**
- Lernziele zu "Verstehen" ändern
- **Oder:** Gezielte Code-Lücken für Kern-Konzepte

### Priorität 3: Optionale Blätter kennzeichnen
Header ergänzen:
```markdown
!!! info "Referenz-Material"
    Dieses Arbeitsblatt enthält fertigen Code als Referenz. 
    Ziel ist das Verständnis der Techniken, nicht die eigenständige Implementierung.
```

---

## Fazit

Die Lernsituation zeigt eine **solide Grundstruktur** mit erkennbarem Scaffolding (UL-01 → UL-10). Die Hauptprobleme sind:

1. **Inkonsistenz in Phase 1:** Lernziele sagen "Anwenden", Code sagt "Kopieren"
2. **UL-08 Bruch:** Sollte wie UL-07 mehr Eigenleistung fordern
3. **Optionale Blätter:** Zu viel fertiger Code für fortgeschrittene Themen

**Empfohlene Strategie:** Option C (Progressives Scaffolding) konsequent umsetzen.

Die gute Nachricht: **UL-07 und UL-10 sind bereits didaktisch vorbildlich!** Diese sollten als Modell für Überarbeitungen dienen.

---

## Durchgeführte Änderungen (28.01.2026)

### UL-02: Datenvorverarbeitung
- ✅ Aufgabe 2: `StandardScaler()` und `fit_transform()` als Lücken
- ✅ Aufgabe 4: `MinMaxScaler` Anwendung als Lücke
- ✅ Aufgabe 5: `.corr()` als Lücke
- ✅ Hilfe-Boxen für Unterstützung ergänzt

### UL-03: K-Means Clustering
- ✅ Elbow-Schleife: `n_clusters`, `fit()`, `inertia_` als Lücken
- ✅ Aufgabe 3: `k` und `fit_predict()` als Lücken
- ✅ Silhouette-Schleife: Parameter als Lücken

### UL-04: PCA
- ✅ Aufgabe 1: `n_components` und `fit_transform()` als Lücken
- ✅ Aufgabe 4: `fit_predict()` Aufrufe als Lücken
- ✅ Aufgabe 5: Mehrere Lücken für eigenständige Anwendung

### UL-08: Weinqualität-Analyse
- ✅ Aufgabe 3: Feature-Auswahl komplett eigenständig (nur Optionen als Anregung)
- ✅ Aufgabe 4: Elbow-Schleife nur als Struktur, Implementierung selbst
- ✅ Aufgabe 6: Algorithmen-Vergleich mit mehr Lücken

### Optionale Arbeitsblätter
- ✅ UL-OPT-01 & UL-OPT-02: Als "Referenz-Material" gekennzeichnet
- ✅ Hinweis auf Fokus Verständnis statt Eigenimplementierung

### Resultierende Scaffolding-Struktur

| Phase | Arbeitsblatt | Code-Strategie |
|-------|--------------|----------------|
| 1. Grundlagen | UL-01 | Minimaler Code, Exploration |
| 1. Grundlagen | UL-02, UL-03, UL-04 | **Code mit Lücken + Hints** |
| 2. Übergang | UL-05, UL-06 | Code + Interpretations-Lücken |
| 3. Anwendung | UL-07 | Gerüste mit vielen Lücken |
| 3. Anwendung | **UL-08** | **Jetzt auch: Eigenständige Implementierung** |
| 4. Vorbereitung | UL-09 | Vollständiger Code als Vorlage |
| 5. Projekt | UL-10 | Nur Struktur, keine Lösung |
| Optional | UL-OPT-01/02 | **Referenz-Material (gekennzeichnet)** |
