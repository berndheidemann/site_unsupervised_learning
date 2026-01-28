# Vorlage: Lernsituation als GitHub Pages

Diese Vorlage ermöglicht es, Lernsituationen mit Unterseiten als MkDocs-Website auf GitHub Pages zu deployen.

## Schnellstart

### 1. Repository erstellen

1. Dieses Repository als Vorlage nutzen (Template) oder forken
2. Neues Repository mit passendem Namen erstellen (z.B. `lf05_ticketsystem`)

### 2. Anpassungen vornehmen

#### mkdocs.yml

Folgende Felder anpassen:

```yaml
site_name: "[TITEL] – Lernsituation"          # z.B. "Ticketsystem – React Lernsituation"
site_url: https://[GITHUB-USER].github.io/[REPO-NAME]/   # z.B. https://mustermann.github.io/lf05_ticketsystem/
```

Navigation anpassen – User Stories und Umsetzungshilfen einkommentieren und umbenennen:

```yaml
nav:
  - Start: index.md
  - Allgemeine Umsetzungshilfe: allgemeine-umsetzungshilfe.md
  - Guideline Chatbots: guideline-chatbots.md
  - User Stories:
      - Story 1 – Titel: stories/story-01-titel.md
      - Story 2 – Titel: stories/story-02-titel.md
  - Umsetzungshilfen:
      - Hilfen Story 1: umsetzungshilfen/story-01-titel.md
      - Hilfen Story 2: umsetzungshilfen/story-02-titel.md
```

#### docs/index.md

Folgende Platzhalter ersetzen:

- `[TITEL]` – Titel der Lernsituation
- `[PROJEKTNAME]` – Name des zu entwickelnden Projekts
- Projektbeschreibung verfassen
- Hauptfunktionen auflisten
- User Story-Links anpassen

#### docs/allgemeine-umsetzungshilfe.md

- Setup-Anweisungen für den verwendeten Technologie-Stack anpassen
- Code-Strukturierung an Technologien anpassen (z.B. React, Angular, Vue, Spring Boot, etc.)

### 3. User Stories anlegen

Erstelle für jede User Story eine Datei in `docs/stories/`:

```
docs/stories/
  story-01-titel.md
  story-02-titel.md
  ...
```

**Empfohlene Struktur einer Story:**

```markdown
# Story X – Titel

**Als** [Rolle] **möchte ich** [Funktion] **damit** [Nutzen].

## Akzeptanzkriterien
- Gegeben [Kontext], dann [erwartetes Verhalten].
- ...

## Tasks
- [ ] Task 1
- [ ] Task 2
- ...

## Umsetzungshilfe
- [Umsetzungshilfe – Story X](../umsetzungshilfen/story-0X-titel.md)
```

### 4. Umsetzungshilfen anlegen (optional)

Erstelle für jede Story eine detaillierte Hilfe in `docs/umsetzungshilfen/`:

```
docs/umsetzungshilfen/
  story-01-titel.md
  story-02-titel.md
  ...
```

### 5. Lokal testen

```bash
# MkDocs und Dependencies installieren
pip install mkdocs-material mkdocs-awesome-pages-plugin mkdocs-minify-plugin

# Development Server starten
mkdocs serve
```

Die Seite ist dann unter `http://localhost:8000` erreichbar.

### 6. GitHub Pages aktivieren

Diese Variante nutzt den `gh-pages` Branch und ist bereits in `.github/workflows/gh-pages.yml` konfiguriert.

1. Im GitHub Repository: **Settings** → **Pages**
2. Unter "Build and deployment" → **Source**: **Deploy from a branch**
3. Branch: **gh-pages** / `/ (root)` auswählen
4. **Save** klicken

Nach dem ersten Push auf `main` wird automatisch der `gh-pages` Branch erstellt und die Seite deployed. Der Branch ist erst nach dem allersten Deployment sichtbar.


```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'
          
      - name: Install dependencies
        run: |
          pip install mkdocs-material mkdocs-awesome-pages-plugin mkdocs-minify-plugin
          
      - name: Build site
        run: mkdocs build
        
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Nach dem Push auf `main` wird die Seite automatisch gebaut und deployed.

---

## Projektstruktur

```
.
├── mkdocs.yml                      # MkDocs Konfiguration
├── README.md                       # Diese Datei
├── docs/
│   ├── index.md                    # Startseite
│   ├── allgemeine-umsetzungshilfe.md
│   ├── guideline-chatbots.md
│   ├── assets/
│   │   └── js/
│   │       └── progress.js         # Fortschritts-Tracking
│   ├── stories/                    # User Stories (selbst anlegen)
│   │   ├── story-01-titel.md
│   │   └── ...
│   └── umsetzungshilfen/           # Detaillierte Hilfen (selbst anlegen)
│       ├── story-01-titel.md
│       └── ...
└── .github/
    └── workflows/
        └── deploy.yml              # GitHub Actions Workflow (selbst anlegen)
```

---

## Checkliste: Von Vorlage zur produktiven Seite

- [ ] Repository erstellt und geklont
- [ ] `mkdocs.yml`: `site_name` angepasst
- [ ] `mkdocs.yml`: `site_url` angepasst
- [ ] `mkdocs.yml`: Navigation mit Stories und Hilfen ergänzt
- [ ] `docs/index.md`: Titel und Beschreibung angepasst
- [ ] `docs/index.md`: Hauptfunktionen beschrieben
- [ ] `docs/index.md`: Story-Links aktualisiert
- [ ] `docs/allgemeine-umsetzungshilfe.md`: Setup-Anweisungen angepasst
- [ ] `docs/stories/`: Alle User Story-Dateien angelegt
- [ ] `docs/umsetzungshilfen/`: Alle Hilfe-Dateien angelegt (optional)
- [ ] `.github/workflows/deploy.yml`: Workflow angelegt
- [ ] GitHub Pages aktiviert (Settings → Pages → GitHub Actions)
- [ ] Lokal getestet mit `mkdocs serve`
- [ ] Änderungen gepusht und Deployment geprüft

---

## Anpassen des Themes

In `mkdocs.yml` können Farben angepasst werden:

```yaml
theme:
  palette:
    - scheme: default
      primary: indigo      # Hauptfarbe (z.B. blue, red, green, purple)
      accent: indigo       # Akzentfarbe
```

Verfügbare Farben: `red`, `pink`, `purple`, `deep-purple`, `indigo`, `blue`, `light-blue`, `cyan`, `teal`, `green`, `light-green`, `lime`, `yellow`, `amber`, `orange`, `deep-orange`, `brown`, `grey`, `blue-grey`

---

## Lizenz

Diese Vorlage ist für schulische Zwecke am Schulzentrum Utbremen erstellt.




# Prompt
Im Ordner old ist die aktuelle Lernsituation hinterlegt. es sind klassiche Arbeitsblätter (Kürzel AB) in Form von Word-Dokumenten. Zusätzlich gibt es einige Infobläter (IB) zu den Arbeitsblättern. 

Ich möchte diese Lernsitation in eine moderne lernsituation überführen. D.h. alle Arbeitsblättern werden als Markdown formatiert, entsprechend verlinkt und werden dann durch github actions als Webseite bereitgestellt. Wir befinden uns bereits in einem vorbereiteten Projekt, in dem eine solche Lernsituation bereitgestellt werden kann. 

1. Erstelle die Markdowns zu den Arbeits- und Infoblättern. 
2. aktualisiere die mkdocs.yml entsprechend.