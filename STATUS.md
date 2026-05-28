# STATUS.md — CSVHorse

## Huidige fase

**v0.1.2-Appaloosa** — basic CSV export (contextueel: SQL / filtered / alle data) bovenop SQL + UI-filter + edit/undo.

| Aspect | Status |
|--------|--------|
| Repo lokaal | ✓ aangemaakt |
| GitHub remote | ✓ public `cpaglebbeek/CSVHorse` |
| Skeleton-bestanden | ✓ alle docs compleet (v0.0.1) |
| Architectuur-viewer (`architectuur/CSVHorse_viewer.html`) | ✓ 8 ArchiMate-views, deterministisch |
| CSV-import + render | ✓ v0.0.2-Arabian |
| PapaParse vendored | ✓ v5.4.1 SHA256 vastgepind |
| Cell-edit (dubbelklik / Enter / F2) | ✓ v0.0.3-Andalusian |
| Onbeperkte undo/redo (Ctrl+Z / Ctrl+Shift+Z / Ctrl+Y) | ✓ |
| Keyboard navigatie (Arrow / Home / End / Tab / Enter) | ✓ |
| Single-cell paste | ✓ (multi-cell volgt later) |
| Selectie-tracking + Delete/Backspace = wis cel | ✓ |
| UI-filter (kolom → operator → waarde) | ✓ v0.1.0-Lipizzaner |
| 8 operators (bevat / niet bevat / =/!= / begint met / eindigt met / leeg / niet leeg) | ✓ |
| viewSet → Renderer toont subset | ✓ |
| Arrow-nav respecteert viewSet | ✓ |
| Filter blijft actief na cell-edit (Excel-conventie) | ✓ |
| SQL-panel (AlaSQL 4.17.3 vendored, full grammar) | ✓ v0.1.1-Akhal-Teke |
| Renderer mode-switch (data-mode ↔ SQL-result-mode) | ✓ |
| SQL-result read-only (edit/undo gedisabled) | ✓ |
| Filter + SQL exclusief | ✓ |
| Ctrl/Cmd+Enter = Run · Escape = sluit | ✓ |
| **Basic CSV export (contextueel: SQL / filtered / alle data)** | ✓ **werkend** (v0.1.2-Appaloosa) |
| **PapaParse.unparse + Blob download · dialect-behoud** | ✓ |
| **Bestandsnaam `<basis>_<modus>_YYYYMMDD_HHMM.csv`** | ✓ |
| Opmaak (bold/italic/kleur/...) | ⏸ **v0.1.3** (verschoven van v0.1.2 — nieuwe codenaam bij die release) |
| Autosave + zoek/vervang | ⏸ v0.2.0-Mustang |
| Virtual scrolling 100k+ | ⏸ v0.3.0-Haflinger |
| Export met `__style_*` roundtrip | ⏸ v0.4.0-Shire |
| Deployment `icthorse.nl/CSVHorse/` | ⏸ v0.5.0-Hanoverian |
| `/sanitycheck` op skeleton | ✓ uitgevoerd 2026-05-28 |

## Volgende milestones

| Versie | Codenaam | Scope |
|--------|----------|-------|
| v0.0.1 | Friesian | Repo-skeleton (huidig) |
| v0.0.2 | Arabian | Werkende CSV-import + render (zonder edit, zonder SQL) |
| v0.0.3 | Andalusian | + Cell-edit + undo/redo |
| v0.1.0 | Lipizzaner | + UI-filter (dropdown query) |
| v0.1.1 | Akhal-Teke | + SQL-panel (AlaSQL) |
| v0.1.2 | Appaloosa | + Opmaak-toolbar (bold/italic/etc) |
| v0.2.0 | Mustang | + Autosave + Zoek/vervang |
| v0.3.0 | Haflinger | + Virtual scrolling voor 100k+ |
| v0.4.0 | Shire | + Export-dialog met `__style_*` roundtrip toggle |
| v0.5.0 | Hanoverian | Deploy `icthorse.nl/CSVHorse/` + handmatige QA |
| v1.0.0 | Lusitano | Public stable release |

## Wijzigingslog

| Datum | Versie | Wijziging |
|-------|--------|----------|
| 2026-05-28 | 0.0.1-Friesian | Repo-skeleton aangemaakt — newp protocol |
| 2026-05-28 | 0.0.1-Friesian | ArchiMate-architectuurviewer toegevoegd (`architectuur/CSVHorse_viewer.html`) |
| 2026-05-28 | 0.0.2-Arabian | Werkende CSV import + render: PapaParse 5.4.1 vendored, DataStore + IO + Renderer modules, drag-drop, dialect-auto-detect, sticky header, comfortabel tot ~5k rijen |
| 2026-05-28 | 0.0.3-Andalusian | Cell-edit + onbeperkte undo/redo: EditController + CommandHistory + Selection modules. Edit-triggers: dubbelklik / Enter / F2. Commit: Enter (volgende rij) / Tab (volgende kol) / click-buiten. Cancel: Escape. Keyboard-nav: Arrow / Home / End. Delete/Backspace = wis cel (als command). Ctrl/Cmd+Z undo, Ctrl/Cmd+Shift+Z of Ctrl/Cmd+Y redo. Single-cell paste. Undo-stack-counter in toolbar. |
| 2026-05-28 | 0.1.0-Lipizzaner | UI-filter: Filter-module met viewSet (kolom → operator → waarde). 8 operators (contains/ncontains/eq/neq/starts/ends/empty/nempty). Filter-paneel toggle via ⏚-knop; kolom-dropdown + operator-dropdown + waarde-input + Toepassen/Wis-filter. Renderer respecteert viewSet. Arrow-navigatie loopt door zichtbare rijen. Filter persistent door cell-edits (Excel-conventie). Gefilterde-kolom-header krijgt ⏚-marker. Stats-counter toont "X van Y rijen — kolom op waarde". **Oranje versiebump** (design-impact). |
| 2026-05-28 | (infra) | B-001 opgelost: jsDelivr CDN-cache gepurged + **GitHub Pages ingeschakeld** op `https://cpaglebbeek.github.io/CSVHorse/` — preferred live-preview URL voor active development (jsDelivr was nog op v0.0.2 cached). Patroon `DEPLOY-CDN-001` vastgelegd in `docs/BUGLIST.md`. |
| 2026-05-28 | 0.1.1-Akhal-Teke | SQL-panel via AlaSQL 4.17.3 vendored (~500KB, SHA `a53ec7d6...`). SQLEngine-module wraps `alasql(query, [DataStore.asObjects()])`. Renderer mode-switch: data-mode (DataStore+Filter) ↔ sql-mode (read-only result-tabel met gele accent). Edit/undo/paste gedisabled in SQL-mode + toast bij dubbelklik. Filter + SQL exclusief — één toepassen wist de ander. Ctrl/Cmd+Enter = Run, Escape = sluit paneel. Default placeholder `SELECT * FROM data LIMIT 50`. Tabel-naam: `data`. **Groen versiebump** (+0.0.1; nieuw paneel maar logische architectuur consistent). |
| 2026-05-28 | 0.1.1.1 | B-002 fix (groen): SQL-panel werkte niet — `alasql(query, [objects])` bindt alleen aan `FROM ?`, niet `FROM data` letterlijk. Fix: `alasql.tables.data.data = objects` registreren vóór elke run. Patroon SQL-002 vastgelegd in BUGLIST. |
| 2026-05-28 | 0.1.2-Appaloosa | **Scope-shift**: Appaloosa = basic CSV export (was opmaak). ExportService-module met `exportCurrent()`: contextuele mode-detectie (SQL-active → `sql` / Filter-active → `filtered` / anders → `all`), `Papa.unparse()` met DataStore.dialect.delimiter behoud, Blob → browser-download. Bestandsnaam-template `<basis>_<modus>_<YYYYMMDD_HHMM>.csv`. Toast met rij-aantal + filename. Toolbar ⬇ Exporteer-knop enabled bij data-load. Opmaak verschoven naar v0.1.3 (codenaam bij die release). **Groen versiebump**. |
