---
date: 2026-05-28
repo: CSVHorse
status: open
resume: "verder met CSVHorse MVP-implementatie (PapaParse + AlaSQL vendoring + DataStore + Renderer + toolbar)"
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
