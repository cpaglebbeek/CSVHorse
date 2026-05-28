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

Volgt direct na deze skeleton-push, conform `feedback_newp_includes_verify_sanity.md`.
