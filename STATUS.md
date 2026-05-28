# STATUS.md — CSVHorse

## Huidige fase

**v0.1.0-Lipizzaner** — UI-filter (kolom → operator → waarde) bovenop edit/undo/render.

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
| **UI-filter (kolom → operator → waarde)** | ✓ **werkend** (v0.1.0-Lipizzaner) |
| **8 operators (bevat / niet bevat / =/!= / begint met / eindigt met / leeg / niet leeg)** | ✓ |
| **viewSet → Renderer toont subset** | ✓ |
| **Arrow-nav respecteert viewSet** | ✓ |
| **Filter blijft actief na cell-edit (Excel-conventie)** | ✓ |
| SQL-panel + AlaSQL | ⏸ v0.1.1-Akhal-Teke |
| Opmaak (bold/italic/kleur/...) | ⏸ v0.1.2-Appaloosa |
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
