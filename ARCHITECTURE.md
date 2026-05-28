# ARCHITECTURE.md — CSVHorse + SheetHorse

> **2026-05-28 — Branch-split:** Deze repo bevat vanaf nu twee productvarianten:
> - **CSVHorse** (`main` branch, **dit document**) — light, single-table CSV-werkbank, stabiel op v0.2.0.2-Mustang
> - **SheetHorse** (`sheethorse` branch) — full, virtuele relationele DB met multi-tabel + Excel + relaties + SQL-joins
>
> Bij `/bugcheck`: altijd beide branches scannen (zie `feedback_bugcheck_hele_tree.md` in claude memory).

Architectuurvastlegging conform Meta_Master *Expliciete Vastlegging Principe*. Alle componenten, relaties, data-flow, oorzaak/gevolg en afhankelijkheden worden hier expliciet beschreven.

## 1. Conceptueel niveau

**Doel:** Een browser-native, server-vrije CSV-werkbank waarmee een individuele gebruiker CSV-data kan bekijken, queryen, bewerken en opmaken — met round-trip-bare opmaak via verborgen systeemkolommen.

**Kernprincipes:**
1. **Client-only** — geen backend, geen netwerk, geen tracking
2. **Single-file** — één `.html` voor download en gebruik
3. **Round-trip opmaak** — visuele rijkdom bovenop een gewone CSV, opslaan zonder dataverlies
4. **Dual-mode query** — laagdrempelig (UI-filter) én krachtig (SQL) tegelijk beschikbaar
5. **Schaalbaar** — 100k+ rijen via virtual scrolling
6. **Reversibel** — undo/redo per atomic cell-edit, onbeperkte stack

## 2. Logisch niveau — componenten

```
┌────────────────────────────────────────────────────────────────────┐
│                          CSVHorse (browser)                        │
│                                                                    │
│  ┌──────────────┐    ┌──────────────┐    ┌────────────────────┐    │
│  │  IO Layer    │───►│  DataStore   │◄───│  CommandHistory    │    │
│  │  - Upload    │    │  - rows[]    │    │  - undo/redo       │    │
│  │  - Download  │    │  - cols[]    │    │  - stack           │    │
│  │  - Parse     │    │  - styles{}  │    └────────────────────┘    │
│  │  - Format    │    │  - dialect   │              ▲               │
│  └──────────────┘    └───────┬──────┘              │               │
│         ▲                    │                     │               │
│         │                    ▼                     │               │
│         │            ┌──────────────┐              │               │
│         │            │  SQLEngine   │              │               │
│         │            │  - AlaSQL    │              │               │
│         │            │  - viewSet   │              │               │
│         │            └──────┬───────┘              │               │
│         │                   │                      │               │
│         │                   ▼                      │               │
│         │            ┌──────────────┐    ┌─────────┴──────────┐    │
│         │            │  Renderer    │◄───│  EditController    │    │
│         │            │  - VirtList  │    │  - cell edit       │    │
│         │            │  - cell DOM  │    │  - paste           │    │
│         │            │  - style apply    │  - keyboard nav    │    │
│         │            └──────┬───────┘    └────────────────────┘    │
│         │                   │                                      │
│         │                   ▼                                      │
│         │            ┌──────────────┐                              │
│         │            │  Toolbar UI  │                              │
│         │            │  - Format    │                              │
│         │            │  - FilterUI  │                              │
│         │            │  - SQLPanel  │                              │
│         │            │  - Search    │                              │
│         │            │  - Settings  │                              │
│         │            └──────────────┘                              │
│                                                                    │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │              AutosaveService (localStorage)                  │  │
│  │  - throttled snapshot van DataStore                          │  │
│  │  - schuif AAN/UIT                                            │  │
│  │  - quota-bewaking                                            │  │
│  └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

### Component-specificaties

| Component | Verantwoordelijkheid | Afhankelijkheden |
|-----------|---------------------|------------------|
| **IO Layer** | CSV-upload (File API + drag-drop), parse via PapaParse, format-detectie (dialect), export naar `.csv` (met of zonder `__style_*` kolommen) | PapaParse |
| **DataStore** | In-memory tabel: `rows[]`, `cols[]`, `styles{ [rowIdx]: { [colName]: StyleObj } }`, `dialect`. Single source of truth. | — |
| **SQLEngine** | AlaSQL-instance op DataStore; produceert `viewSet` (geselecteerde rijen-IDs) voor Renderer | AlaSQL, DataStore |
| **Renderer** | Virtual-scroll `<table>`, render alleen zichtbare rij-range, herbruikt rij-DOM-nodes; past style-objecten toe als inline CSS op `<td>` | DataStore, SQLEngine viewSet |
| **EditController** | Vangt cell-edits, paste, keyboard-navigatie; genereert command-objecten voor CommandHistory; muteert DataStore | DataStore, CommandHistory, Renderer |
| **CommandHistory** | Stack van commando's (cell-edit, style-change, row-insert, row-delete); ondersteunt undo/redo onbeperkt | DataStore (apply/revert) |
| **Toolbar UI** | Format-knoppen (bold/italic/etc), FilterUI (kolom + operator + waarde), SQLPanel (textarea + run), Search/Replace dialog, Settings (autosave-toggle, dialect-keuze, export-opties) | DataStore, SQLEngine, EditController |
| **AutosaveService** | Bij elke mutatie in DataStore: throttled snapshot naar localStorage; restore bij page-load; respecteert schuif AAN/UIT; waarschuwt bij quota-grens | DataStore |

## 3. Fysiek niveau — bestanden

```
CSVHorse/
├── index.html            # Single-file applicatie (HTML + CSS + JS inline)
├── version.json          # Versie + codenaam (machine-leesbaar)
├── README.md             # Project-overview
├── LICENSE               # AGPL-3.0
├── CLAUDE.md             # Project-protocol voor agenten
├── ARCHITECTURE.md       # Dit bestand
├── DESIGN_TOKENS.md      # Kleuren, typografie, spacing
├── STATUS.md             # Huidige fase
├── ACTIONS.md            # Open punten
├── .gitignore
├── docs/
│   ├── PRINCIPLES.md     # Conceptuele principes
│   ├── DEPENDENCIES.md   # Component-afhankelijkheden + wijzigings-impact
│   └── BUGLIST.md        # Bug-historie + preventieregels
└── prompts/
    ├── README.md
    └── YYYY-MM-DD_*.md   # Sessie-MD's
```

**Build-output (toekomstig):** geen — `index.html` is direct deploybaar. Vendoring van PapaParse + AlaSQL gebeurt door minified JS inline in `<script>`-tags op te nemen.

## 4. Data-flow

### 4a. CSV importeren
```
File → IO.parse (PapaParse) → dialect-detect → DataStore.load(rows, cols)
  → if (cols matchen __style_*-patroon) → IO.liftStyles → DataStore.styles
  → SQLEngine.bind(DataStore)
  → Renderer.render(viewSet = alle rijen)
  → AutosaveService.snapshot (als AAN)
```

### 4b. SQL-query draaien
```
SQLPanel → AlaSQL.exec(query, DataStore.rows)
  → result-set rijen-IDs → viewSet
  → Renderer.render(viewSet)
  → DataStore ongewijzigd (query is read-only)
```

### 4c. Cell-edit
```
User typt in cel → EditController.beginEdit
  → User confirms → Command{type:'set', row, col, oldVal, newVal}
  → CommandHistory.push(cmd) + cmd.apply(DataStore)
  → Renderer.invalidate(row,col)
  → AutosaveService.snapshot (throttled)
```

### 4d. Opmaak toepassen
```
User selecteert cel(len) → Toolbar.bold-klik
  → Command{type:'style', row, col, prop:'bold', oldVal, newVal}
  → CommandHistory.push + apply
  → DataStore.styles[row][col].bold = true
  → Renderer.applyInlineStyle(td)
  → AutosaveService.snapshot
```

### 4e. Undo
```
Ctrl+Z → CommandHistory.pop().revert(DataStore)
  → Renderer.invalidate
  → AutosaveService.snapshot
```

### 4f. CSV exporteren
```
Toolbar.export → dialog met checkbox "Met opmaak (default AAN)"
  → if (with-styles) → IO.flattenStyles → extra __style_* kolommen
  → PapaParse.unparse(rows + style-cols, dialect)
  → Blob → download
```

## 5. Opmaak-attributen (uitgebreid)

Per cel kan een StyleObj de volgende properties hebben (alles optioneel):

| Prop | Type | CSS-equivalent |
|------|------|---------------|
| `bold` | boolean | `font-weight: 700` |
| `italic` | boolean | `font-style: italic` |
| `underline` | boolean | `text-decoration: underline` |
| `strikethrough` | boolean | `text-decoration: line-through` |
| `color` | hex string | `color` |
| `background` | hex string | `background-color` |
| `fontSize` | number (px) | `font-size` |
| `align` | "left" \| "center" \| "right" | `text-align` |

**CSV-roundtrip kolomnamen:** `__style_bold`, `__style_italic`, `__style_underline`, `__style_strikethrough`, `__style_color`, `__style_background`, `__style_fontsize`, `__style_align`. Per zichtbare kolom kan een set van deze `__style_*`-kolommen bestaan met patroon `__style_<prop>__<colname>`.

## 6. Relaties met andere projecten

| Project | Relatie |
|---------|---------|
| `iCt_Horse` | Moeder-website icthorse.nl — CSVHorse wordt later gehost op `icthorse.nl/CSVHorse/` |
| `Meta_iCt_Horse_Diensten` | Sub-master orchestratie van iCt Horse-diensten — CSVHorse is een van de diensten |
| `HorseSafe` | Andere iCt Horse Diensten SaaS (vault); geen technische koppeling |
| `VeiligDelen` | Andere iCt Horse Diensten (file-sharing); geen technische koppeling |
| `Meta_Master` | Bron-van-waarheid voor protocollen + project-metadata |

## 7. Oorzaak/gevolg-matrix

| Wijziging in | Mogelijke impact op |
|--------------|---------------------|
| DataStore schema | SQLEngine, Renderer, AutosaveService, IO export-format |
| Opmaak-attributen lijst | DataStore.styles model, Renderer.applyInlineStyle, IO.flatten/liftStyles, DESIGN_TOKENS |
| AlaSQL versie | SQLEngine (grammatica-features), tests, bundle-grootte |
| PapaParse versie | IO parser-config, edge-cases (CRLF/BOM/quotes), bundle |
| localStorage quota | AutosaveService, fallback-strategie |
| Virtual-scroll algoritme | Renderer, performance bij grote datasets, scroll-anchor gedrag |

Zie [docs/DEPENDENCIES.md](docs/DEPENDENCIES.md) voor uitgebreide afhankelijkheidsmatrix.

## 8. ArchiMate-viewer

Voor een visuele weergave van bovenstaande architectuur in ArchiMate-stijl (8 views, Visio-look, deterministisch):

**Bestand:** [`architectuur/CSVHorse_viewer.html`](architectuur/CSVHorse_viewer.html) — standalone single-file HTML, geen dependencies, opent via `file://`.

**Views:**
1. Conceptueel — Stakeholder · Driver · 8 Principes (Motivation layer)
2. Component-architectuur — Application + Technology lagen
3. Data-flow — CSV import + style-lift
4. Data-flow — SQL query / UI-filter → viewSet
5. Data-flow — Cell edit + undo/redo (Command stack)
6. Data-flow — Opmaak + autosave naar localStorage
7. Data-flow — Export met `__style_*` roundtrip (default AAN)
8. Roadmap — Plateaus v0.0.1-Friesian … v1.0.0-Lusitano
9. **Ecosysteem-branches** — CSVHorse-light (main) + SheetHorse-full (sheethorse); fork-relatie, gedeelde + branch-specifieke features (toegevoegd 2026-05-28 branch-split)

**Functionaliteit:** view-selector, JSON-export (intern), `.archimate`-export (Archi-tool v5 compatibel), SVG-export per view, JSON+`.archimate` import om model te vervangen, reset naar ingebouwde model.

**Stijl:** identiek aan `WerkDierenbescherming/architectuur/solution/telefonie/telefonie_viewer.html` — Visio-look met drop-shadow, Calibri/Arial, `«ElementType»`-banner + 16×16 ArchiMate-icoon, layer-kleuren conform ArchiMate-conventie.

**Model-statistieken:** 81 elementen + 7 notes, 127 relaties, 9 views met 132 diagram-objects en 131 connections.
