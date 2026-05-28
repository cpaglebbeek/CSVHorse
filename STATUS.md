# STATUS.md — CSVHorse

## Huidige fase

**v0.1.4-Trakehner** — `__style_*` CSV-roundtrip + export-dialog modal (scope-shift van v0.4.0).

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
| Basic CSV export (contextueel: SQL / filtered / alle data) | ✓ v0.1.2-Appaloosa |
| PapaParse.unparse + Blob download · dialect-behoud | ✓ |
| Bestandsnaam `<basis>_<modus>_YYYYMMDD_HHMM.csv` | ✓ |
| **Opmaak per cel: bold / italic / underline / strikethrough** | ✓ **werkend** (v0.1.3-Knabstrupper) |
| **Tekst-kleur + achtergrond-kleur (native color-picker)** | ✓ |
| **Font-size dropdown (10/12/14/16/18/24 px)** | ✓ |
| **Alignment (links / centreren / rechts)** | ✓ |
| **SetStyleCommand + ClearStyleCommand undo-baar** | ✓ |
| **DataStore.styles{} sparse storage** | ✓ |
| **Opmaak persistent na cell-edit + filter + render** | ✓ |
| **Opmaak gedisabled in SQL-mode** | ✓ |
| **Opmaak in CSV-export (`__style_*` roundtrip)** | ✓ **werkend** (v0.1.4-Trakehner — scope-shift van v0.4.0) |
| **Export-dialog modal** (filename + checkbox met-opmaak + BOM) | ✓ |
| **Import herkent `__style_*` automatisch** | ✓ |
| **Filter-mode → styles meegemapt naar gefilterde rij-indexen** | ✓ |
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
| 2026-05-28 | 0.1.4.1 | B-003 (groen) gediagnoseerd: opmaak verdwijnt bij re-import = **browser-cache** (zelfde patroon als B-001 CDN-cache, nu client-side). Geen code-fix nodig — `Ctrl/Cmd+Shift+R` lost het op. Patroon DEPLOY-CACHE-002 vastgelegd in BUGLIST. Diagnostic logging + toast "opmaak hersteld: N cellen" toegevoegd voor toekomstige debug. |
| 2026-05-28 | 0.1.4-Trakehner | **Scope-shift** v0.4.0-Shire → v0.1.4: `__style_*` CSV-roundtrip nu live. IO.flattenStyles (export) + IO.liftStyles (import) round-trippen DataStore.styles via prefix-kolomnamen `__style_<prop>__<colname>`. Export-dialog modal met filename-input, "[x] Met opmaak" checkbox (default AAN, disabled bij SQL-mode), "[] UTF-8 BOM" checkbox. Roundtrip getest via Node-simulatie — 100% identiek styles-object voor en na. Filter-mode behoudt styles na re-mapping naar gefilterde indexen. Stats-counter toont `opmaak:N cellen`. Codenaam Trakehner (Duits sport-paard, betrouwbaar = "import wat geëxporteerd is"). **Groen versiebump**. v0.4.0-Shire scope vrijgegeven voor toekomstige feature. |
| 2026-05-28 | 0.1.3-Knabstrupper | Opmaak per cel: Styles-module + DataStore.styles{} sparse-storage. SetStyleCommand + ClearStyleCommand voor undo-baar. Opmaak-paneel (derde panel mutex met Filter + SQL): B/I/U/S toggle-knoppen, native `<input type="color">` voor tekst+achtergrond, font-size dropdown (10/12/14/16/18/24 px), alignment (L/C/R), "✕ Wis opmaak". Knoppen reflect huidige selectie-style (active class). Renderer past `Styles.cssFor()` inline toe in zowel `renderAll` als `updateCell`. Underline + strikethrough samen in 1 text-decoration. Opmaak persistent na cell-edit. Gedisabled in SQL-mode. **Geen `__style_*`-roundtrip in CSV-export nog — gepland v0.4.0-Shire**. Codenaam Knabstrupper (gespikkeld ras, past visueel bij opmaak). **Groen versiebump**. |
