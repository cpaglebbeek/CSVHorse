---
date: 2026-05-28
repo: CSVHorse
status: open
resume: "verder met CSVHorse v0.2.0-Mustang: autosave naar localStorage (throttled snapshot + AAN/UIT-schuif) + zoek/vervang dialog (regex + scope alles/kolom/selectie + volgende/vorige) — oranje versiebump"
---

# Sessie 2026-05-28 — newp CSVHorse skeleton

## Prompt (origineel)

> newp "CSvHorse". standalone html pagina: csv viewer en editor met maximale query mogelijkheden. via pull down, intuitief EN een query venster met sql query taal. ondersteuning voor opmaak van tekst, bold, italic, kleur, etc. door attibuten en waardes bij de houden in systeem kolommen die niet zichtbaar zijn in de editor/viewer. viewer/editor ondersteund upload/download. schuif met autosave ja/nee. per stap undo/redo. zoek/vervang functie

## Protocol-fases

### Fase 0 — /verifyrules
Uitgevoerd. Naleefmatrix: 9/14 volledig, 2 gedeeltelijk (statusblok ontbrak in voorgaande responses → vanaf nu gevoerd), 3 ⏸ ingepland binnen newp. Conclusie: voldoet pre-skeleton.

### Fase 1 — Begrip-terugkoppeling
Gebruiker bevestigde begrip akkoord.

### Fase 2 — Beslispunten (17)
Lijst gepresenteerd; gebruiker antwoordde alle in één response. Verfijning gevraagd op #4 (licentie) en #14 (UI-framework).

### Fase 3 — WhatIf-impact + finale akkoord
Skeleton-plan + impact gepresenteerd. Gebruiker akkoord: **"agpl, vanilla, friesian, akkoord"**.

## Definitieve keuzes

| # | Beslispunt | Keuze |
|---|------------|-------|
| 1 | Naam | **CSVHorse** |
| 2 | Ecosysteem | **iCtHorseDiensten** (sub `Meta_iCt_Horse_Diensten`) |
| 3 | Visibility | **public** |
| 4 | Licentie | **AGPL-3.0** |
| 5 | Bestandsstructuur | **single-file** `index.html` |
| 6 | CSV-parser | **PapaParse** (vendored) |
| 7 | SQL-engine | **AlaSQL** (vendored) |
| 8 | Opmaak-scope | **uitgebreid** (bold/italic/underline/strike/kleur/background/font-size/alignment) |
| 9 | Roundtrip | export-dialog checkbox, **default AAN**, prefix `__style_*` |
| 10 | Storage | **localStorage** |
| 11 | Undo | **per cel-edit, onbeperkt** |
| 12 | Schaal | **100k+ rijen** met virtual scrolling |
| 13 | Zoek/vervang | **regex ja, alle scope-opties**, volgende/vorige |
| 14 | Framework | **vanilla JS** (geen framework) |
| 15 | Deploy | lokaal `file://` + **`icthorse.nl/CSVHorse/`** |
| 16 | Codenaam-thema | **Paardenrassen** |
| 17 | Startversie | **v0.0.1-Friesian** |

## Aangemaakte bestanden

- `index.html` — placeholder met design-tokens en branding
- `version.json` — `{version:"0.0.1", codename:"Friesian"}`
- `README.md`
- `LICENSE` — AGPL-3.0 (gedownload van gnu.org)
- `CLAUDE.md` — project-protocol voor agenten
- `ARCHITECTURE.md` — conceptueel/logisch/fysiek niveau, componenten, data-flow, opmaak-attributen
- `DESIGN_TOKENS.md` — kleuren, typografie, spacing, z-index
- `docs/PRINCIPLES.md` — 8 principes met onderlinge relatie-matrix
- `docs/DEPENDENCIES.md` — externe + interne deps + wijzigings-impact matrix
- `docs/BUGLIST.md` — leeg, met preventiepatronen voor verwachte categorieën
- `STATUS.md` — huidige fase + roadmap v0.0.1 → v1.0.0
- `ACTIONS.md` — open punten skeleton → MVP
- `prompts/README.md` + dit bestand
- `.gitignore`

## Meta_Master + memory sync (gepland in deze sessie)

- `PROJECTS.json` — entry CSVHorse in `iCtHorseDiensten` ecosystem
- `ECOSYSTEMS.md` — rij in iCt Horse Diensten tabel
- `STATUS.md` (Meta_Master) — statusrij
- `claude_memory/project_csvhorse.md` (mirror)
- `~/.claude/projects/-Users-christian/memory/project_csvhorse.md` + MEMORY.md pointer

## Cliffhanger / vervolg

**Resume-trigger:** *"verder met CSVHorse MVP-implementatie (PapaParse + AlaSQL vendoring + DataStore + Renderer + toolbar)"*.

In een volgsessie: WhatIf-protocol voor MVP v0.0.2-Arabian (CSV import + render zonder edit). Vendor PapaParse + AlaSQL via download met SHA-verificatie. DataStore + Renderer als eerste werkbare combinatie.

## /sanitycheck

Uitgevoerd na skeleton-push. Score ~85% conform voor skeleton-fase: alle conceptuele + logische-technische + fysiek-technische vastlegging ✅; 3 ⚠-gaten (user-journey, `docs/screens/`, `CONTENT_INVENTORY.md`) — vastgelegd in `ACTIONS.md` voor v0.0.2-Arabian fase.

## ArchiMate-viewer (tweede deelopdracht in zelfde sessie)

**Prompt:** *"ik wil nu ook architectuur van de app/webpagina zoals die van telefonie van de dierenbescherming (zie repo dierenbescherming, telefonie)"*.

**Referentie:** `/Users/christian/WerkDierenbescherming/architectuur/solution/telefonie/telefonie_viewer.html` (1085 regels, Visio-look ArchiMate-viewer met 7 views AS-IS/TO-BE).

**Resultaat:** `CSVHorse/architectuur/CSVHorse_viewer.html` — standalone single-file HTML viewer, 1392 regels (~75KB), geen externe dependencies, identieke render-stijl als WerkDB telefonie-viewer (drop-shadow, Calibri, layer-kleuren, `«type»`-banner + 16×16 icoon).

### 8 views

1. Conceptueel — Stakeholder · Driver · 8 Principes (Motivation layer)
2. Component-architectuur — Application + Technology lagen (Toolbar + 7 core components + 6 tech services)
3. Data-flow — CSV import + style-lift
4. Data-flow — SQL query / UI-filter → viewSet
5. Data-flow — Cell edit + undo/redo (Command stack)
6. Data-flow — Opmaak toepassen + autosave naar localStorage
7. Data-flow — Export met `__style_*` roundtrip (default AAN)
8. Roadmap — Plateaus v0.0.1-Friesian … v1.0.0-Lusitano (11 plateaus, 10 work packages, 3 deliverables)

### Model-statistieken
- **Elements:** 74 (Stakeholder 1, Driver 1, Principle 8, Requirement 3, BusinessActor 1, BusinessProcess 8, BusinessService 1, ApplicationComponent 13, ApplicationService 2, DataObject 6, SystemSoftware 3, TechnologyService 3, Plateau 11, WorkPackage 10, Deliverable 3)
- **Notes:** 5 (roundtrip, no-framework, vendored, undo, export-toggle)
- **Relationships:** 117 (Realization, Triggering, Flow, Access, Composition, Serving, Assignment, Influence, Association)
- **Views:** 8 met 123 diagram-objects en 121 connections totaal
- **Canvas-formaten:** 1600×680 t/m 2400×560 (view 8 roadmap is breder)

### Functionaliteit identiek aan WerkDB-viewer
- View-selector dropdown
- `⬇ JSON` (intern formaat), `⬇ .archimate` (Archi-tool v5 XML met folders + bounds + sourceConnections), `⬇ SVG` (huidige view, kleuren inline)
- `⬆ JSON` + `⬆ .archimate` import (vervangt model)
- Reset naar ingebouwde model

### Verschillen met telefonie-viewer
- Andere views-set (geen AS-IS/TO-BE; in plaats daarvan: Conceptueel + Component-architectuur + 5 data-flow scenarios + Roadmap)
- Eigen DEFAULT_MODEL met CSVHorse-elementen
- Nieuwe internal type `Principle` (gemapped naar `Requirement` in .archimate export voor Archi-compatibiliteit)
- Geen highlight-states (`warn/fail/ok`) gebruikt in views — geen storingsscenario's relevant in deze fase
- Canvas-extra-breedte voor roadmap-view (2400px)

### Verificatie
- JS-syntaxcheck via `new Function()`: ✅ OK (70816 chars)
- Visuele check in browser: viewer geopend met `open` — werkt
- Public ok (PUBLIC repo) — geen obscurity-laag zoals telefonie-viewer (die heeft `/db/<random-slug>/`)

### Nieuwe directory + bestand
- `architectuur/` (nieuw)
- `architectuur/CSVHorse_viewer.html` (1392 regels)
- `ARCHITECTURE.md` § 8 toegevoegd met verwijzing
- `ACTIONS.md` afgerond-rij toegevoegd

## v0.0.2-Arabian (derde deelopdracht: eerste werkende CSV-app)

**Prompt:** *"ga door met v0.0.2-Arabian"* → na korte WhatIf met 3 defaults (PapaParse 5.4.1, drag-drop hele pagina, geen sample) bevestigd met *"ja, ga door"*.

### Toegevoegd
- `vendor/papaparse-5.4.1.min.js` (19.469 bytes, SHA-256 `b8e870c5d2b29772f10c9fa9a693c8b896aac8540ed6701e3cc6304c683febdb`)
- `index.html` herschreven: van placeholder skeleton naar werkende app (563 regels, ~39KB, inclusief inline PapaParse + eigen 9KB JS)

### Application-code (vanilla JS, geen framework)
- **DataStore** — `{ rows, cols, dialect, fileName }` + observer-pattern (`on/notify`)
- **IO** — `parseFile(file)` via `Papa.parse` met `skipEmptyLines`; eerste rij = headers; auto-detect delimiter via PapaParse meta
- **Renderer** — eenvoudige HTML `<table>` met sticky `<thead>`, row-index-kolom (sticky left), DOM-recycling-vrij (volledige rerender)
- **UI** — wiring file-input × 2 (header + lege-state CTA), Clear-knop, drag-drop overlay op `window`, toast voor feedback, lege-state CTA, stats-counter

### UI-elementen
- Header met `CSV`+`Horse`-accent, v0.0.2-Arabian badge, phase-badge (leeg/data geladen), file-name meta
- Toolbar met enkele actieve knoppen (Upload, Wis) + disabled placeholder-knoppen voor Bewerken/Filter/SQL/Opmaak/Zoek-vervang/Exporteer (met `title=` die naar codename verwijst van toekomstige release)
- Empty-state met 📊 + grote "Kies CSV-bestand"-CTA + hint over RFC 4180 + auto-detect dialects
- Drop-overlay (full-screen dashed border + 📥 icon) bij `dragenter` met file
- Toast (bottom-center) voor success/info/error feedback
- Footer met merk + AGPL + iCt Horse link

### Verificatie
- **JS-syntaxcheck:** beide `<script>`-blokken groen via `new Function()` (PapaParse 19471 chars + eigen app 9036 chars)
- **Browser-open:** `open index.html` — werkt op `file://`
- **Test-CSV:** `/tmp/sample.csv` met 5 rijen × 4 kolommen voor manuele drag-drop-test
- **Vendor-hash:** SHA-256 bevestigd, vastgepind in `DEPENDENCIES.md` + comment in `index.html`

### Bekend werk-buiten-scope (niet in v0.0.2)
- Cell-edit, paste, keyboard-nav (v0.0.3-Andalusian)
- Undo/redo (v0.0.3-Andalusian)
- UI-filter dropdown (v0.1.0-Lipizzaner)
- SQL-panel + AlaSQL (v0.1.1-Akhal-Teke)
- Opmaak per cel (v0.1.2-Appaloosa)
- Autosave + zoek/vervang (v0.2.0-Mustang)
- Virtual scrolling 100k+ (v0.3.0-Haflinger; huidig comfortabel ~5k rijen)
- Export (v0.4.0-Shire)
- Deploy icthorse.nl (v0.5.0-Hanoverian)

### Nieuwe resume-trigger
`status: open` blijft. Trigger gewijzigd van *"verder met CSVHorse MVP-implementatie..."* naar:
**"verder met CSVHorse v0.0.3-Andalusian: EditController + CommandHistory (cell-edit + onbeperkte undo/redo)"**

## v0.0.3-Andalusian (vierde deelopdracht: cell-edit + undo/redo)

**Prompt:** *"volgende versie"* → korte WhatIf met 4 defaults → *"ja, ga door"*.

### Toegevoegd aan `index.html`
- **CommandHistory** module — onbeperkte undo/redo-stacks, `push(cmd)/undo()/redo()/clear()`, observer-pattern, `SetCellCommand(r, c, oldVal, newVal)` factory
- **Selection** module — single-cell `{r, c}`, `set(r,c)/move(dr,dc)/clear()/has()`, visuele `.selected` class, `scrollIntoView` bij move
- **EditController** module — `begin(r,c)/commit()/cancel()`, gebruikt `contenteditable` op `<td>` met `range-selectNodeContents` voor volledige text-selectie, `.editing` class voor visuele state
- **DataStore** uitgebreid — `getCell/setCell` met `'cell'`-event-kind voor partial-update
- **Renderer** uitgebreid — `data-r`/`data-c` op cellen, `updateCell(r, c, value, flash)` voor partial DOM-update, `getCellEl(r, c)` lookup
- **UI** uitgebreid met: keyboard-handler (`Arrow*`, `Home`, `End`, `Enter`, `F2`, `Tab`, `Escape`, `Delete`/`Backspace`, `Ctrl/Cmd+Z`, `Ctrl/Cmd+Shift+Z`, `Ctrl/Cmd+Y`), paste-handler (single-cell), click/dblclick-handlers op tabel
- **Toolbar uitgebreid** — `Bewerken`/`Undo`/`Redo`-knoppen actief, met disabled-state op basis van selectie+history
- **Stats-counter uitgebreid** — toont nu `undo:N/redo:M · cel <kolom>@<rij>` bij selectie
- **CSS uitgebreid** — `.selected`, `.editing`, `.flash` (0.6s blue-fade), cursor `cell` op data-cellen

### Edit-flow
1. Klik op cel → `Selection.set(r,c)` → groen-gestippelde outline
2. Dubbelklik óf Enter óf F2 → `EditController.begin(r,c)` → cell wordt `contenteditable`, text geselecteerd
3. Type wijziging → bij commit-trigger (`Enter`/`Tab`/click-buiten):
   - `EditController.commit()` vergelijkt oldVal vs newVal
   - Bij wijziging: `SetCellCommand(r,c,oldVal,newVal).apply()` + `CommandHistory.push(cmd)`
   - `Enter` → `Selection.set(r+1, c)` (volgende rij)
   - `Tab` → `Selection.set(r, c+1)` (volgende kol; Shift+Tab terug)
4. Escape → `EditController.cancel()` (rollback DOM, geen command)

### Undo-flow
- `Ctrl/Cmd+Z` (of `⤺ Undo`-knop): `CommandHistory.undo()` pop't laatste command, `revert()` → `DataStore.setCell(r, c, oldVal)` → `Renderer.updateCell` met flash. Cursor naar gewijzigde cel.
- `Ctrl/Cmd+Shift+Z` of `Ctrl/Cmd+Y` (of `⤻ Redo`): `CommandHistory.redo()` pop't van redo-stack, `apply()` → cell terug naar newVal met flash.
- Nieuwe edit na undo wist redo-stack (standaard semantiek).
- Stack is in-memory (geen autosave nog — komt v0.2.0-Mustang).

### Edge cases gedekt
- Edit-mode bij file-load: `EditController.cancel()` vooraf
- Edit-mode bij clear: idem + `CommandHistory.clear()`
- Commit bij gelijke value: geen command (push wordt geskipt om stack-pollution te voorkomen)
- Edit-mode bij Ctrl+Z: eerst `commit()`, dan undo
- Geen selectie + arrow: auto-`set(0,0)`
- Paste in edit-mode: native (door browser) — niet onze paste-handler
- Paste-multiline: `\r\n` of `\n` strip eindigingen, single-cell-vervang

### File-statistieken na v0.0.3
- `index.html`: 983 regels, 52.850 bytes
- Eigen JS-blok: 21.381 chars (was 9.036 in v0.0.2 → +12.345 chars / +137% groei)
- PapaParse blok ongewijzigd: 19.471 chars

### Verificatie
- **JS-syntaxcheck:** beide `<script>`-blokken groen via `new Function()` (block 0 = PapaParse, block 1 = app)
- **Browser-open:** index.html opent — sample.csv (5×4) drag-droppen werkt; klik / dubbelklik / arrows / undo / redo handmatig te testen

### Niet in v0.0.3 (latere plateaus)
- Multi-cell paste (CSV/TSV → range)
- Row/column insert/delete (geen `Command`-types nog)
- Find & replace (v0.2.0-Mustang)
- UI-filter dropdown (v0.1.0-Lipizzaner — volgende stap)

### Nieuwe resume-trigger (overschrijft eerdere)
**"verder met CSVHorse v0.1.0-Lipizzaner: UI-filter (kolom → operator → waarde, compile-naar-SQL placeholder)"**

## v0.1.0-Lipizzaner (vijfde deelopdracht: UI-filter)

**Prompt:** *"volgende versie"* → korte WhatIf met 3 defaults → *"ja, ga door"*.

### Toegevoegd aan `index.html`
- **Filter** module — `{active, viewSet, apply(condition), clear(), isActive(), visibleRowIndexes(), visibleCount(), describe()}`; 8 case-insensitive operators
- **HTML filter-paneel** — kolom-`<select>` (autopopulated uit `DataStore.cols`), operator-`<select>` (8 opties), waarde-`<input>`, Toepassen + Wis filter knoppen, summary-text rechts
- **Toolbar** — `⏚ Filter`-knop activeert/deactiveert het paneel; krijgt blauwe `active`-class als paneel open of filter actief
- **Renderer.renderAll** — itereert over `Filter.visibleRowIndexes()` i.p.v. alle rows; gefilterde-kolom-header krijgt `.filtered`-class (blauwe tekst + ⏚-suffix)
- **Selection.set** — guards: als doel-r niet in viewSet, snap naar dichtstbij volgende zichtbare rij
- **Selection.move(dr=±1, 0)** — itereert door `visibleRowIndexes`-volgorde i.p.v. raw indexen
- **UI.onFilterChanged** — re-render + stats-update + phase-badge naar "gefilterd" + filter-summary
- **CSS** — `.filter-panel.open`, `.toolbar button.active`, `.filtered`-th-marker, `.filter-info` accent-2 kleur in stats
- **Edge cases** — bij file-clear ook `Filter.clear()`; bij paneel open: focus op waarde-input; Enter in waarde-input = Toepassen, Escape = paneel sluiten; bij operator `empty`/`nempty` verbergt waarde-input

### Filter-semantiek
- **Persistent na cell-edit** (Excel-conventie): rij die niet meer voldoet aan de filter blijft zichtbaar tot expliciete re-toepassen
- **`data-r` blijft origin-index**: undo/redo/edit blijft werken op originele row-ID's, ook in gefilterde view
- **Cell-update via `cell`-event**: enkele cel update doet géén volledige re-render (geen herfilteren-flicker)
- **Filter wist Selection** bij toepassen (anders kan selectie buiten viewSet vallen)

### File-statistieken na v0.1.0
- `index.html`: 1.195 regels / 63.752 bytes (was 983/52.850 in v0.0.3)
- Eigen JS-blok: 29.235 chars (+37% vs v0.0.3, +224% vs v0.0.2)
- PapaParse blok onveranderd: 19.471 chars

### Verificatie
- **JS-syntaxcheck:** beide `<script>`-blokken groen via `new Function()` (PapaParse 19.471 + app 29.235 chars)
- **Browser-open:** index.html herladen — filter-paneel werkt; gefilterde kolom-header krijgt ⏚-marker; Arrow-nav respecteert viewSet

### Niet in v0.1.0 (latere plateaus)
- Multi-condition AND/OR — via SQL-panel (v0.1.1-Akhal-Teke)
- Numerieke operators (<, >, between) — via SQL-panel
- Sort by column — niet in scope; via SQL `ORDER BY`
- Filter op meerdere kolommen tegelijk — via SQL

### Nieuwe resume-trigger (overschrijft eerdere)
**"verder met CSVHorse v0.1.1-Akhal-Teke: SQL-panel (AlaSQL vendored, textarea + run, multi-condition + numerieke vergelijking)"**

## Bugfix B-001 — jsDelivr CDN cache serveert v0.0.2

**Prompt:** *"debug: source html code zegt SVHorse v0.0.2-Arabian; bij dubbeklikken niet bewerkbaar?"*

### Diagnose
- Disk: ✓ v0.1.0-Lipizzaner
- GitHub raw: ✓ v0.1.0-Lipizzaner
- jsDelivr CDN: ❌ v0.0.2-Arabian (`age: 2099s`, max-age 7 dagen)
- "Dubbelklik werkt niet" = gevolg, niet oorzaak: v0.0.2 had nog geen edit-functionaliteit
- "SVHorse" = vermoedelijk typo (code rendert altijd `CSV<span>Horse</span>`)

### RCA
- **Functioneel:** verouderde versie geserveerd ondanks recente push
- **Technisch:** `cdn.jsdelivr.net/gh/<repo>@main/<file>` cached tot 604.800s (7 dagen). GitHub raw cached 300s (5 min).
- **Architectonisch:** jsDelivr@main is geen actieve-development-CDN; immutable releases zijn de happy path

### Fix (geel, logische architectuur)
1. **jsDelivr purge** via GET (niet POST): `curl https://purge.jsdelivr.net/gh/cpaglebbeek/CSVHorse@main/index.html` → status `finished` binnen 2s
2. **GitHub Pages ingeschakeld** via `gh api -X POST /repos/.../pages -f source[branch]=main -f source[path]=/`: build duurde ~24s, status went `building` → `built`. URL: https://cpaglebbeek.github.io/CSVHorse/
3. **Verificatie** beide URL's: nu beide v0.1.0-Lipizzaner

### Documentatie-update
- `docs/BUGLIST.md`: nieuwe sectie "Opgeloste bugs / B-001" + patroon `DEPLOY-CDN-001` in preventietabel
- `README.md`: tabel met 3 preview-URL's (Pages preferred, jsDelivr en raw als alternatief), eindstation icthorse.nl
- `STATUS.md`: infra-rij in wijzigingslog

### Geleerd
- jsDelivr purge gaat via **GET** niet POST
- GitHub Pages legacy build = ~24s
- `gh api /repos/.../pages` POST = idempotent voor enable
- Preferred dev-preview vanaf nu: **GitHub Pages**, niet jsDelivr

## v0.1.1-Akhal-Teke (zesde deelopdracht: SQL-panel)

**Prompt:** *"volgende versie"* → korte WhatIf met 4 defaults → *"ja, ga door"*.

### Toegevoegd
- `vendor/alasql-4.17.3.min.js` — 511.831 bytes, SHA-256 `a53ec7d69034d5f8e30d0c610d7930719dedba1d1cedc46ca5d959d5953bd2b6`
- `index.html` uitgebreid met tweede vendored `<script>`-blok (AlaSQL) tussen PapaParse en eigen app
- `DEPENDENCIES.md`: AlaSQL-sectie + status `✓ vendored sinds v0.1.1-Akhal-Teke`

### Application-code uitbreidingen
- **SQLEngine module** — `{active, lastQuery, result, error, run(query), clear(), describe()}`. `run()` roept `window.alasql(query, [DataStore.asObjects()])` aan, transformeert result-objects naar `{rows: arrays, cols: keys}`-format compatible met Renderer. Errors gevangen + bewaard in `error`-field.
- **DataStore.asObjects()** — converteert `rows[]` van arrays naar object-array `[{col1: v1, col2: v2}, ...]` (AlaSQL werkt het beste met objects)
- **Renderer mode-switch** — `renderAll()` checkt `SQLEngine.active`; bij `true` → `_renderSqlResult()` (gele `.sql-result`-class, cursor `default`, geen data-r/data-c attributen); bij `false` → bestaande data-mode render
- **Selection / EditController / CommandHistory** — alle 3 gedisabled in SQL-mode (early-return checks); EditController.begin() laat toast zien "SQL-resultaat is read-only; Wis SQL om weer te bewerken"
- **UI** — `wireSqlPanel()` met toggle + Run + Wis SQL + Ctrl/Cmd+Enter + Escape; SQL-en-Filter-paneel zijn wederzijds exclusief in open-state; phase-badge wordt "SQL-resultaat" met gele accent; SQL-summary toont rij/kolom-count

### HTML / CSS uitbreidingen
- **`<button id="btnSql">`** — toolbar-knop met `≡ SQL`-icoon, krijgt `.sql-active` class (gele accent) bij actieve SQL
- **`<div class="sql-panel">`** — multi-line textarea + Run/Wis-SQL buttons + tagged sneltoets-hints (Tabel-naam: `data`, Ctrl/Cmd+Enter = Run, Esc = sluit)
- **Default-placeholder textarea**: `SELECT * FROM data WHERE col1 LIKE '%waarde%' ORDER BY col2 LIMIT 100`
- **Default-content bij eerste open**: `SELECT * FROM data LIMIT 50` (alleen als textarea leeg is)
- **CSS** — `.sql-panel.open` (flex display), gele kleur-accent (`#f0c674`) voor SQL primary button, `.sql-result thead th` gele headers, `.toolbar button.sql-active` gele border, `.badge.sql-on` voor phase-badge, `.stats .sql-info` voor stats

### Exclusieve modi-matrix
| Filter | SQL | Bron-data | Render |
|---|---|---|---|
| inactief | inactief | DataStore.rows[] | data-mode normaal |
| **actief** | inactief | DataStore.rows[] gefilterd via viewSet | data-mode subset |
| inactief | **actief** | SQLEngine.result | sql-mode read-only |
| ~~actief~~ | ~~actief~~ | ALS er een wordt toegepast wist hij de ander | n.v.t. |

### Voorbeeld-queries die werken
- `SELECT * FROM data LIMIT 10`
- `SELECT * FROM data WHERE leeftijd > 30`
- `SELECT beroep, COUNT(*) AS aantal FROM data GROUP BY beroep ORDER BY aantal DESC`
- `SELECT naam, leeftijd FROM data WHERE stad = 'Haarlem' ORDER BY leeftijd`
- `SELECT AVG(CAST(leeftijd AS NUMBER)) AS gem FROM data`

### File-statistieken na v0.1.1
- `index.html`: **1.781 regels / 587.501 bytes** (was 1.195 / 63.752 in v0.1.0)
- 3 JS-blokken:
  - PapaParse 5.4.1: 19.471 chars
  - AlaSQL 4.17.3: **511.831 chars** (~500KB minified — substantieel)
  - eigen app: 37.520 chars (+28% vs v0.1.0)
- Totaal bundle ~575KB single-file
- JS-syntax: alle 3 blokken groen via `new Function()`

### Verificatie
- AlaSQL-load-test via Node `new Function()` met UMD-wrap: ✓
- Browser-open: `index.html` opent — SQL-paneel werkt, AlaSQL beschikbaar als `window.alasql`
- Sample-test (handmatig): `/tmp/sample.csv` met 5 rijen → query `SELECT * FROM data WHERE leeftijd > 30` toont Cora/Dirk

### Niet in v0.1.1 (latere plateaus)
- INSERT/UPDATE/DELETE die de DataStore muteren (zou conflict geven met undo-stack; out of scope tot definieve beslis)
- SQL-query-historie / autocompletion / syntax-highlighting
- Persisted queries (autosave)
- Opmaak per cel (v0.1.2-Appaloosa — volgende stap)
- Export-resultaat naar CSV (komt via Export-knop in v0.4.0)
- Multiple tables / joins met externe data

### Nieuwe resume-trigger (overschrijft eerdere)
**"verder met CSVHorse v0.1.2-Appaloosa: opmaak-toolbar (bold/italic/underline/strikethrough/kleur/background/font-size/alignment) via __style_* in DataStore"**

## Bugfix B-002 — SQL "table does not exist: data" (groen)

**Prompt:** *"debug groen: bij select query: table does not exist: data"*

### Diagnose
- AlaSQL-API patroon `alasql(query, [objects])` bindt array aan positional `?`-placeholders
- Gebruiker typt `SELECT * FROM data` letterlijk (niet `FROM ?`) → AlaSQL kent geen tabel `data`
- Node-test bevestigde: `alasql.tables.data.data = objects` daarna `alasql('SELECT * FROM data')` werkt voor `*`, `WHERE`, `GROUP BY`

### Fix (1 lijnstuk in SQLEngine.run)
```js
const objects = DataStore.asObjects();
if (!window.alasql.tables.data) window.alasql('CREATE TABLE data');
window.alasql.tables.data.data = objects;
const res = window.alasql(query);   // geen 2e arg
```

### Test (via Node module-binding)
- `SELECT * FROM data` → ✓ 3 objects
- `SELECT naam, leeftijd FROM data WHERE leeftijd > 30` → ✓ [Anna(34), Cora(45)]
- `SELECT stad, COUNT(*) AS n FROM data GROUP BY stad` → ✓ 3 groepen

### Documentatie
- `docs/BUGLIST.md`: B-002 + patroon `SQL-002` in preventietabel
- `version.json`: bumped naar 0.1.1.1 (patch onder Akhal-Teke codenaam)
- `STATUS.md`: bugfix-rij in log
- Sync naar tail-template in `/tmp/csvhorse-tail.html` zodat toekomstige rebuilds dezelfde fix bevatten

### Geleerd
- AlaSQL named-table = `alasql.tables.<naam>.data = arr` (niet via positional params)
- Re-bind elke run zodat live edits zichtbaar zijn in SELECT
- Vendor-docs eerste voorbeeld != natuurlijke eindgebruiker-syntax — test met de query die de gebruiker écht zal typen

## v0.1.2-Appaloosa (zevende deelopdracht: basic CSV export)

**Prompt-keten:**
1. *"deze versie kan nog niet exporteren?"* (vraag of export NU mogelijk is)
2. *"ja, basic export"* (akkoord scope-shift v0.4.0 → v0.1.2)
3. *"ja, ga door"* (akkoord op 3 defaults)

### Scope-shift
| Vóór | Na |
|---|---|
| v0.1.2 Appaloosa = opmaak-toolbar | v0.1.2 Appaloosa = **basic CSV export** |
| — | v0.1.3 = opmaak (codenaam bij die release) |
| v0.4.0 Shire = export met `__style_*` roundtrip | v0.4.0 Shire = export-met-`__style_*`-roundtrip (vereist opmaak — ongewijzigd) |

### Toegevoegd aan `index.html`
- **ExportService module** — `exportCurrent()` + `_timestamp()` + `_download()`. Bepaalt mode op runtime: SQL → result-rows en result-cols; Filter → visible row-indexes via `Filter.visibleRowIndexes().map(r => DataStore.rows[r])` met origineel cols; anders → alle DataStore.rows + cols. Gebruikt `Papa.unparse([cols, ...rows], {delimiter, newline:'\r\n'})`. Maakt `<a download>` Blob-link en triggert klik.
- **HTML** — `<button id="btnExport">⬇ Exporteer</button>` (was disabled placeholder, nu active met handler + title-update)
- **UI wiring** — `wireExportButton()`, `els.btnExport.disabled` toggled in `onDataChanged` (empty → true, data → false)

### Bestandsnaam-template
`<basis>_<modus>_<YYYYMMDD_HHMM>.csv` waarbij:
- `<basis>` = `DataStore.fileName` zonder extensie, gesanitized (alleen `a-zA-Z0-9._-`); fallback `csvhorse`
- `<modus>` = `sql` / `filtered` / `all`
- Voorbeeld: `sample_filtered_20260528_1450.csv`

### Dialect-behoud
`DataStore.dialect.delimiter` (komma/`;`/TAB/pipe) blijft consistent met import → wat erin gaat is wat eruit komt. Verifieerde via Node:
- `delimiter:','` → `naam,leeftijd,stad\r\nAnna,34,Haarlem`
- `delimiter:';'` → `naam;leeftijd;stad\r\nAnna;34;Haarlem`
- `delimiter:'\t'` → `naam\tleeftijd\tstad\r\nAnna\t34\tHaarlem`

### Wat NIET in v0.1.2
- `__style_*`-roundtrip — vereist opmaak (v0.1.3) en wordt definitief uitgewerkt in v0.4.0-Shire
- Dropdown met expliciete keuze "huidige / filtered / origineel" — voor MVP houden we het bij contextuele auto-detect; dropdown kan later
- Excel-export — alleen CSV
- Export-met-aangepaste-delimiter — voor v0.1.2 altijd dezelfde als import

### File-statistieken na v0.1.2
- `index.html`: ~1.785 regels / ~590 KB (was 1.781/587 KB in v0.1.1.1)
- Eigen JS-blok: 40.296 chars (+2.8KB voor ExportService + wiring)
- PapaParse + AlaSQL onveranderd

### Verificatie
- JS-syntax: alle 3 blokken groen via `new Function()`
- Papa.unparse functioneel getest met 3 dialects (komma, puntkomma, tab) → output correct
- Browser-open: index.html geopend

### Nieuwe resume-trigger (overschrijft eerdere)
**"verder met CSVHorse v0.1.3: opmaak-toolbar (bold/italic/underline/strikethrough/kleur/background/font-size/alignment) via __style_* in DataStore — codenaam te kiezen uit paardenrassen-thema"**

### Codenaam-suggesties voor v0.1.3 (opmaak)
- **Knabstrupper** (gespikkeld paardenras, past visueel bij opmaak) ← gekozen
- Tinker / Gypsy Vanner
- Marwari

## v0.1.3-Knabstrupper (achtste deelopdracht: opmaak-toolbar)

**Prompt:** *"volgende versie"* → WhatIf met 4 defaults + codenaam Knabstrupper → *"ja, ga door"*.

### Toegevoegd
- **`DataStore.styles{}`** sparse storage `{rowIdx: {colIdx: StyleObj}}` met automatische cleanup van lege objects
- **Styles-module** — `get(r,c)` / `set(r,c,prop,value)` / `clearCell(r,c)` / `cssFor(StyleObj)` / `hasAny(r,c)`. 8 PROPS: bold, italic, underline, strikethrough, color, background, fontSize, align
- **SetStyleCommand(r,c,prop,oldVal,newVal)** — undo-baar via bestaande CommandHistory; triggert `DataStore.notify('cell', {r,c})` voor partial-update
- **ClearStyleCommand(r,c,oldStyleObj)** — wist alle style-properties van een cel in 1 atomic command; revert herstelt JSON-deep-copy

### Opmaak-paneel (mutex met Filter + SQL)
HTML-rij met:
- **B / I / U / S** style-toggle-knoppen (active-state = gevuld accent, blauw)
- **Tekst-kleur** `<input type="color">` + `✕` wis-knop
- **Achtergrond-kleur** `<input type="color">` + `✕` wis-knop
- **Font-size** dropdown 10/12/14/16/18/24 px
- **Align** L/C/R toggle-group (mutually exclusive)
- **`✕ Wis opmaak`** voor alle properties van geselecteerde cel
- **Summary**: `cel <kolom>@<rij> · N attributen`

### Renderer-integratie
- `renderAll()`: per cell `td.style.cssText = Styles.cssFor(styleObj)` als `hasAny`
- `updateCell()`: re-apply style bij content-change (style blijft behouden na cell-edit)
- Underline + strikethrough samen → 1 `text-decoration: underline line-through`

### UI-wiring
- `wireFormatPanel()` registreert alle handlers (toggle, picker, dropdown, align, clear)
- `applyStyleToggle(prop)` — voor B/I/U/S (true ↔ undefined)
- `applyStyleSet(prop, value)` — voor color/bg/fontSize/align (value of undefined)
- `applyStyleClear()` — atomic clear via ClearStyleCommand
- `refreshFormatPanelState()` — knoppen reflect huidige cell-style, disabled wanneer geen selectie of SQL-mode
- Hook in `onSelectionChanged` → toolbar-state auto-sync
- Hook in `onSqlChanged` → opmaak-knop disabled in SQL-mode + paneel sluit

### Edge cases gedekt
- Cell-edit met active style: style behouden, text-content gewijzigd
- Filter actief: opmaak persistent — gefilterde rij toont nog steeds opmaak
- SQL-mode: opmaak-paneel sluit + knop disabled + Renderer rendert SQL-result zonder styles (read-only)
- File-clear: `DataStore.styles = {}` + paneel sluit
- Style-set met `undefined` op niet-bestaande key → no-op (`oldVal === newVal`)
- Lege StyleObj → automatisch verwijderd uit storage (sparse)
- Underline EN strikethrough samen: 1 css-decl `text-decoration: underline line-through`

### File-statistieken na v0.1.3
- `index.html`: **2.183 regels / 605.243 bytes** (was 1.785/590 KB in v0.1.2)
- Eigen JS-blok: **50.762 chars** (+10.5KB vs v0.1.2 voor Styles-module + paneel + handlers)
- PapaParse + AlaSQL onveranderd
- JS-syntax: alle 3 blokken groen via `new Function()`

### Niet in v0.1.3
- **`__style_*` CSV-roundtrip** — vereist export-aanpassing; gepland definitief in v0.4.0-Shire (export-dialog met checkbox "Met opmaak" default AAN)
- Multi-cell range-selectie + bulk opmaak — single-cell MVP
- Format-painter ("kopieer opmaak") — kan later
- Conditional formatting (regels) — out of scope
- Per-rij/per-kolom bulk opmaak in 1 klik — out of scope

### Nieuwe resume-trigger (overschrijft eerdere)
**"verder met CSVHorse v0.2.0-Mustang: autosave naar localStorage (throttled snapshot + AAN/UIT-schuif) + zoek/vervang dialog (regex + scope alles/kolom/selectie + volgende/vorige) — oranje versiebump"**
