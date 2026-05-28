# ACTIONS.md — CSVHorse

Open punten met `- [ ]`. Afgeronde met `[x]`. Conventie: `(DD-MM)` voor datum, bron tussen `[]`.

## Open

### Skeleton → MVP

- [x] **Vendoring PapaParse** — v5.4.1, SHA `b8e870c5...` vastgepind in DEPENDENCIES.md (28-05, v0.0.2-Arabian)
- [ ] **Vendoring AlaSQL** — download v4.x minified, SHA verifiëren, inline (v0.1.1-Akhal-Teke)
- [x] **DataStore (basic)** — rows/cols/dialect/fileName model + observer-pattern (28-05, v0.0.2-Arabian; styles + Command-stack volgen later)
- [x] **IO-laag (upload + parse + dialect-detect)** — File API + drag-drop + PapaParse.parse (28-05, v0.0.2-Arabian; export volgt v0.4.0-Shire)
- [ ] **CSV-roundtrip styles** — `__style_*`-kolommen liften bij import, flatten bij export, dialog-toggle (default AAN) (v0.4.0-Shire)
- [x] **Renderer (basis)** — HTML `<table>` met sticky thead, comfortabel tot ~5k rijen (28-05, v0.0.2-Arabian)
- [ ] **Renderer met virtual scroll** — vaste rij-hoogte, DOM-recycling, 100k-rijen-stress-test (v0.3.0-Haflinger)
- [x] **EditController** — cell-edit-mode (dubbelklik/Enter/F2), paste, keyboard navigatie (Arrow/Tab/Enter/Home/End), Escape=cancel (28-05, v0.0.3-Andalusian)
- [x] **CommandHistory** — atomic edits, undo/redo (Ctrl/Cmd+Z / Ctrl/Cmd+Shift+Z / Ctrl/Cmd+Y), onbeperkt (28-05, v0.0.3-Andalusian)
- [ ] **Toolbar — opmaak** — bold/italic/underline/strikethrough/kleur/background/font-size/alignment (v0.1.2-Appaloosa)
- [ ] **Toolbar — UI-filter** — kolom-dropdown + operator-dropdown + waarde-input → compileer naar SQL (v0.1.0-Lipizzaner)
- [ ] **SQL-panel** — textarea + run-knop + result-binding aan Renderer (v0.1.1-Akhal-Teke)
- [ ] **AutosaveService** — throttled localStorage-snapshot + restore + schuif AAN/UIT + quota-bewaking (v0.2.0-Mustang)
- [ ] **Zoek/vervang dialog** — regex/case-toggle/scope (alles/kolom/selectie)/volgende-vorige (v0.2.0-Mustang)
- [ ] **Settings-modal** — dialect-keuze, autosave-toggle, export-opties, theme (toekomstig) (v0.2.0+)
- [ ] **Export-dialog** — `Met opmaak (default AAN)` checkbox, download als CSV-blob (v0.4.0-Shire)

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
