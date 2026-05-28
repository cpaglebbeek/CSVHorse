# ACTIONS.md — CSVHorse

Open punten met `- [ ]`. Afgeronde met `[x]`. Conventie: `(DD-MM)` voor datum, bron tussen `[]`.

## Open

### Skeleton → MVP

- [ ] **Vendoring PapaParse** — download v5.4 minified, SHA verifiëren, inline in `index.html` (DD-MM nog te plannen) [bron: ARCHITECTURE §IO]
- [ ] **Vendoring AlaSQL** — download v4.x minified, SHA verifiëren, inline (DD-MM) [bron: ARCHITECTURE §SQLEngine]
- [ ] **DataStore implementatie** — rows/cols/styles/dialect model + observer-pattern
- [ ] **IO-laag** — upload (File API + drag-drop), parse, format-detect, export
- [ ] **CSV-roundtrip styles** — `__style_*`-kolommen liften bij import, flatten bij export, dialog-toggle (default AAN)
- [ ] **Renderer met virtual scroll** — vaste rij-hoogte, DOM-recycling, 100k-rijen-stress-test
- [ ] **EditController** — cell-edit-mode, paste, keyboard navigatie (Arrow/Tab/Enter)
- [ ] **CommandHistory** — atomic edits, undo/redo (Ctrl+Z / Ctrl+Shift+Z), onbeperkt
- [ ] **Toolbar — opmaak** — bold/italic/underline/strikethrough/kleur/background/font-size/alignment
- [ ] **Toolbar — UI-filter** — kolom-dropdown + operator-dropdown + waarde-input → compileer naar SQL
- [ ] **SQL-panel** — textarea + run-knop + result-binding aan Renderer
- [ ] **AutosaveService** — throttled localStorage-snapshot + restore + schuif AAN/UIT + quota-bewaking
- [ ] **Zoek/vervang dialog** — regex/case-toggle/scope (alles/kolom/selectie)/volgende-vorige
- [ ] **Settings-modal** — dialect-keuze, autosave-toggle, export-opties, theme (toekomstig)
- [ ] **Export-dialog** — `Met opmaak (default AAN)` checkbox, download als CSV-blob

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
