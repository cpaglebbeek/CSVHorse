# ACTIONS.md — CSVHorse

Open punten met `- [ ]`. Afgeronde met `[x]`. Conventie: `(DD-MM)` voor datum, bron tussen `[]`.

## Open

### Skeleton → MVP

- [x] **Vendoring PapaParse** — v5.4.1, SHA `b8e870c5...` vastgepind in DEPENDENCIES.md (28-05, v0.0.2-Arabian)
- [x] **Vendoring AlaSQL** — v4.17.3, SHA `a53ec7d6...` vastgepind in DEPENDENCIES.md (28-05, v0.1.1-Akhal-Teke)
- [x] **DataStore (basic)** — rows/cols/dialect/fileName model + observer-pattern (28-05, v0.0.2-Arabian; styles + Command-stack volgen later)
- [x] **IO-laag (upload + parse + dialect-detect)** — File API + drag-drop + PapaParse.parse (28-05, v0.0.2-Arabian; export volgt v0.4.0-Shire)
- [x] **CSV-roundtrip styles** — `__style_*`-kolommen liften bij import, flatten bij export, dialog-toggle (default AAN) — gerealiseerd in v0.1.4-Trakehner (zie hierboven)
- [x] **Renderer (basis)** — HTML `<table>` met sticky thead, comfortabel tot ~5k rijen (28-05, v0.0.2-Arabian)
- ➡ **SheetHorse** ~~Renderer met virtual scroll~~ — verschoven naar SheetHorse-branch (CSVHorse-light blijft simpel full-render tot ~5k rijen)
- [x] **EditController** — cell-edit-mode (dubbelklik/Enter/F2), paste, keyboard navigatie (Arrow/Tab/Enter/Home/End), Escape=cancel (28-05, v0.0.3-Andalusian)
- [x] **CommandHistory** — atomic edits, undo/redo (Ctrl/Cmd+Z / Ctrl/Cmd+Shift+Z / Ctrl/Cmd+Y), onbeperkt (28-05, v0.0.3-Andalusian)
- [x] **Toolbar — opmaak** — bold/italic/underline/strikethrough/kleur/background/font-size/alignment via `DataStore.styles{}` (28-05, v0.1.3-Knabstrupper). __style_*-roundtrip blijft v0.4.0-Shire.
- [x] **Toolbar — UI-filter** — kolom-dropdown + 8 operators + waarde-input → viewSet (28-05, v0.1.0-Lipizzaner; compile-naar-SQL niet nodig — eigen Filter-module gekozen i.p.v. SQL-shortcut)
- [x] **SQL-panel** — textarea + Run-knop + AlaSQL 4.17.3 vendored, mode-switch Renderer (28-05, v0.1.1-Akhal-Teke)
- [x] **AutosaveService** — throttled (2 sec) localStorage-snapshot + restore-banner + AAN/UIT toggle + quota-bewaking (28-05, v0.2.0-Mustang)
- [x] **Zoek/vervang dialog** — regex/case-toggle/scope (alles/kolom/selectie)/volgende-vorige + Vervang/Vervang-alle (batch, 1 undo) (28-05, v0.2.0-Mustang)
- [ ] **Settings-modal** — dialect-keuze, autosave-toggle, export-opties, theme (toekomstig) (v0.2.0+)
- [x] **Export-dialog** — `Met opmaak (default AAN)` checkbox + filename-input + UTF-8 BOM toggle (28-05, v0.1.4-Trakehner — **scope-shift** van v0.4.0)
- [x] **CSV-roundtrip styles** — `__style_*`-kolommen liften bij import, flatten bij export (28-05, v0.1.4-Trakehner)

### Documentatie

- [ ] **Roadmap-uitwerking** — per release expliciete scope (begin gedaan in STATUS.md, finetunen)
- [ ] **Vendoring-protocol concreet maken** — SHA-256 hashes van gebruikte versies in DEPENDENCIES.md
- [ ] **Voorbeeld-CSV's** — `examples/` map met `simple.csv`, `with-styles.csv`, `large-100k.csv` (gegenereerd)
- [ ] **docs/USER_JOURNEY.md** — cell-edit state-machine + filter/sort/SQL/export-flow expliciet uitwerken (P2 uit sanitycheck 2026-05-28)
- [ ] **docs/screens/** — visuele mockups (ASCII of SVG) van hoofdscherm, filter-dropdown geopend, SQL-panel geopend, export-dialog, settings-modal (P2 uit sanitycheck 2026-05-28)
- [ ] **CONTENT_INVENTORY.md** — pagina-doel + key-messages + CTA voor single-page-app (P2 uit sanitycheck 2026-05-28)

### Deploy

- [ ] **icthorse.nl/CSVHorse/** — sub-directory aanmaken op Hostinger + index.html rsyncen (na MVP, niet eerder)
- [ ] **icthorse.nl menu-integratie** — SeaMenu item toevoegen of via diensten-pagina linken (zie `iCt_Horse/docs/SEAMENU_ANATOMY.md`)

### Tests / QA

- [ ] **Manueel-test-checklist** — `docs/MANUAL_TEST.md` met scenario's (import, query, edit, opmaak, export-roundtrip)
- [ ] **Browser-matrix-test** — Chrome/Firefox/Safari/Edge latest-2

## Afgerond

- [x] **2026-05-28** — Repo-skeleton aangemaakt via newp protocol (verifyrules + WhatIf akkoord agpl/vanilla/friesian/akkoord)
- [x] **2026-05-28** — ArchiMate-viewer `architectuur/CSVHorse_viewer.html` aangemaakt (8 views, 74+5 elementen, 117 relaties, Visio-look, JSON/.archimate/SVG export, deterministisch); stijl gebaseerd op WerkDB telefonie-viewer
- [x] **2026-05-28** — v0.0.2-Arabian opgeleverd: werkende CSV-import + render. PapaParse 5.4.1 vendored (`vendor/papaparse-5.4.1.min.js` + inline in `index.html`), DataStore + IO + Renderer modules in vanilla JS, drag-drop op hele pagina, dialect-auto-detect (komma/puntkomma/tab/pipe), sticky header, file-counter, toast-feedback, lege-state CTA. Comfortabel tot ~5k rijen (virtual scroll volgt v0.3.0-Haflinger)
- [x] **2026-05-28** — v0.0.3-Andalusian opgeleverd: cell-edit + onbeperkte undo/redo. EditController (dubbelklik/Enter/F2 om te starten; Enter/Tab/click-buiten commit; Escape cancel; Shift+Tab terug). CommandHistory (onbeperkte undo-stack met SetCellCommand). Selection (single-cell met arrow/Home/End navigatie, visuele highlight, scrollIntoView). Delete/Backspace = wis cel via command. Single-cell paste (multi-cell volgt). Undo/redo-counters in toolbar. Flash-animatie bij cell-update.
- [x] **2026-05-28** — v0.1.0-Lipizzaner opgeleverd: UI-filter. Filter-module met viewSet-mechanisme (kolom → operator → waarde). 8 operators: bevat / bevat niet / is gelijk aan / is niet gelijk aan / begint met / eindigt met / is leeg / is niet leeg. Filter-paneel via ⏚-toggle. Renderer respecteert viewSet (alleen zichtbare rijen). Selection.move loopt door visible-rows-volgorde. Filter persistent na cell-edit. Gefilterde kolom-header met ⏚-marker. Stats: "X van Y rijen — kolom op waarde". Oranje versiebump (design-impact).
- [x] **2026-05-28** — B-001 opgelost: jsDelivr CDN-cache gepurged + GitHub Pages ingeschakeld. Preferred preview-URL: https://cpaglebbeek.github.io/CSVHorse/. Patroon DEPLOY-CDN-001 vastgelegd in BUGLIST.md.
- [x] **2026-05-28** — v0.1.1-Akhal-Teke opgeleverd: SQL-panel via AlaSQL 4.17.3 vendored (511KB minified, SHA a53ec7d6...). SQLEngine-module wrapt `alasql(query, [DataStore.asObjects()])`. Renderer mode-switch: 'data'-mode (DataStore + Filter) ↔ 'sql'-mode (read-only result-tabel). Toolbar `≡ SQL`-knop opent textarea-paneel met Run / Wis SQL knoppen. Ctrl/Cmd+Enter = Run; Escape = sluit. Default placeholder `SELECT * FROM data LIMIT 50`. Tabel-naam = `data`. Edit/undo/paste/Filter gedisabled in SQL-mode + toast bij dubbelklik. Filter + SQL wederzijds exclusief. Phase-badge wordt "SQL-resultaat" met gele accent. Stats-counter toont SQL-rij-en-kolom-count met bron-vermelding. Groen versiebump (+0.0.1; logische architectuur consistent).
- [x] **2026-05-28** — B-002 (groen) opgelost: SQL "table does not exist: data". Fix: `alasql.tables.data.data = objects` registratie vóór `alasql(query)`. Patroon SQL-002 vastgelegd in BUGLIST. v0.1.1.1.
- [x] **2026-05-28** — v0.1.2-Appaloosa opgeleverd: **basic CSV export** (scope-shift van oorspronkelijk geplande opmaak — die verschuift naar v0.1.3). ExportService.exportCurrent() detecteert mode contextueel (SQL/filtered/all), gebruikt `Papa.unparse()` met behoud van origineel dialect (komma/`;`/TAB/pipe), genereert filename `<basis>_<modus>_<YYYYMMDD_HHMM>.csv`, Blob → download. ⬇ Exporteer-knop in toolbar enabled na data-load. Toast met rij-aantal. Groen versiebump.
- [x] **2026-05-28** — B-003 (groen, geen code-fix): opmaak verdween bij re-import = browser-cache. Hard-refresh loste op. Patroon DEPLOY-CACHE-002 vastgelegd. Diagnostic logging + toast `opmaak hersteld: N cellen` toegevoegd.
- [x] **2026-05-28** — v0.2.0.2: nieuwe feature **Lege CSV starten** via `🆕 Nieuw`-knop in toolbar. Modal-dialog met kolom-aantal (1-100, default 5) + rij-aantal (0-10000, default 1). Auto-named `col_1..col_N`. Confirm bij bestaande data. Reset alle state (Selection/Edit/Filter/SQL/Builder/Search) bij aanmaak. Groen versiebump. JS-blok +3KB.
- [x] **2026-05-28** — v0.2.0.1: AutosaveService.setEnabled(false) wist nu ook bestaande snapshot uit localStorage (UX-fix, groen). Voorkomt verwarrende restore-banner na uitschakeling.
- [x] **2026-05-28** — v0.2.0-Mustang opgeleverd: autosave + zoek/vervang. **Oranje versiebump** (design-impact: persistente storage + nieuw paneel). AutosaveService throttled snapshot naar localStorage (`csvhorse:autosave:v1`), AAN/UIT toggle (`💾 Auto`) in toolbar, last-saved-tijd op de knop, quota-check met auto-disable, restore-banner bij page-load. SearchReplace module met scan/next/prev/replaceOne/replaceAll, regex + case-toggle + scope (all/col/selection), BatchCommand factory voor "vervang alle" als 1 undo-step. Visuele highlights via `.search-hit` (onderstreping geel) + `.search-current` (gele bg + outline). Renderer.applySearchHighlights na elke renderAll. Toetsen: Enter=Volgende, Shift+Enter=Vorige, Escape=sluit-paneel. 4 functionele test-scenario's Node-bevestigd: case-insensitive, case-sensitive, regex `^[A-Z]`, vervang-werkt-correct. Codenaam Mustang (wild + robuust). Bundle +16KB JS (71 → 88KB).
- [x] **2026-05-28** — v0.1.5-Tinker opgeleverd: visuele SQL query-builder. SQLBuilder-module bouwt state → SQL-string. Toggle `🧩 Builder`-knop in SQL-paneel opent uitklap-panel met 5 clauses: SELECT (mode all/cols + per-kolom aggregate via UI), WHERE (dynamisch lijst conditions met 11 operators + AND/OR-logica), GROUP BY (multi-select), ORDER BY (dynamisch lijst kolom+richting), LIMIT (numeric). Add/✕ buttons voor where/order rijen. Drie hoofdacties: Bouw query, Wis builder, Bouw+Run. Identifiers ge-escapeerd met [...]-brackets. 5 functionele test-scenario's bevestigd correct (LIMIT, SELECT+WHERE, GROUP+COUNT, BETWEEN+IS NULL, WHERE OR). Codenaam Tinker (Gypsy Vanner — sierlijk+krachtig). Groen versiebump.
- [x] **2026-05-28** — v0.1.4-Trakehner opgeleverd: **scope-shift** v0.4.0-Shire → v0.1.4. `__style_*` CSV-roundtrip via IO.flattenStyles (export) + IO.liftStyles (import). Export-dialog modal met bestandsnaam-input + "Met opmaak" checkbox (default AAN, disabled in SQL-mode) + "UTF-8 BOM" checkbox (default UIT). Last-used-values onthouden binnen sessie. Filter-mode hermapt styles naar nieuwe row-indexen. Roundtrip-test via Node-simulatie: 100% identiek styles-object voor en na export+import. Stats-counter toont `opmaak:N cellen`. Codenaam Trakehner (Duits sport, betrouwbaarheid). Groen versiebump.
- [x] **2026-05-28** — v0.1.3-Knabstrupper opgeleverd: opmaak-toolbar per cel. Styles-module met sparse `DataStore.styles[r][c]` storage. 8 attributen: bold/italic/underline/strikethrough (toggles met active-state in toolbar) + color/background (native `<input type="color">` + wis-knop) + fontSize (dropdown 10/12/14/16/18/24 px) + align (L/C/R). SetStyleCommand + ClearStyleCommand voor undo-baar. Renderer.applyCellStyle in `renderAll` + `updateCell` via inline `style.cssText` met `Styles.cssFor()`. Opmaak-paneel mutex met Filter + SQL. Knoppen reflect huidige cell-style. Toolbar B I U Opmaak-knop active na data-load (paneel openbaar zonder selectie, controles disabled tot selectie). Gedisabled in SQL-mode (read-only). Codenaam Knabstrupper (gespikkeld paardenras). Groen versiebump.
