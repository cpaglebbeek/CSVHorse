---
date: 2026-05-28
repo: CSVHorse (branch sheethorse)
status: done
resume: ""
---

# SheetHorse v0.5.0-Knabstrupper → v0.6.0.2-Hanoverian

Sessie 2026-05-28 — voortzetting van eerdere SheetHorse-werk. Eén sessie waarin v0.5.0-Knabstrupper (feature CSV-met-metadata format) is afgemaakt, gevolgd door bugfixes (v0.5.0.1) en een tweede majorbump (v0.6.0-Hanoverian) met rij/kolom-CRUD + multi-tabel SQLBuilder, gevolgd door 2 patches (v0.6.0.1 + v0.6.0.2).

## Releases in deze sessie

| Versie | Type | Inhoud |
|---|---|---|
| v0.5.0-Knabstrupper | oranje feature | CSV-met-metadata format afgerond — `_exportMultiCsv`, dispatch op `csv-multi`, 3e export-dropdown-optie, BOM-sync. Eerste commit `c8218a7`. |
| v0.5.0.1-Knabstrupper | geel bugfix × 2 | **B-004** parser-schema-alias (`from_table` ⇄ `from_sheet`); **B-005** add-row col-dropdown live rebuild via `colCells`-map. Commit `0902980`. |
| (interim) | docs | `docs/examples/multi-table-example.csv` aangemaakt + commit `3978393`. |
| v0.6.0-Hanoverian | oranje batch A + B | **A:** B-006 voorbeeldbestand naar echt `__style_<prop>__<colname>` schema; B-007 multi-tabel-detect diagnostiek (console.log + toast); B-008 blank-sheet-dialog 3×5. **B:** F-001 rij-CRUD `Insert/RemoveRowCommand`; F-002 kolom-CRUD `Insert/RemoveColCommand` met style-shift; F-003 SQLBuilder FROM-dropdown + JOIN-rij-builder met auto-suggest uit `DataStore.relations`. Commit `fcf1b3f`. |
| v0.6.0.1-Hanoverian | geel bugfix | **B-009** `_parseXlsxFile` past nu óók `IO.liftStyles` toe op headers — als XLSX `__style_*`-kolommen bevat worden ze gelift en samengevoegd met native Excel cell-styles (Excel wint bij conflict). + diagnostiek-log. + `docs/examples/multi-table-example.xlsx` via openpyxl-script. Commit `c78d9cd`. |
| v0.6.0.2-Hanoverian | geel bugfix | **B-010** "Wis SQL"-knop leegt nu óók `sqlText.value` + `sqlError.textContent`. Commit `d727a31`. |

## Acties (chronologisch)

1. **Vervolg v0.5.0-Knabstrupper** — ExportService `_exportMultiCsv`, format-dropdown 3e optie, `confirmExport` dispatch, BOM-visibility. Syntax-check 4/4 OK.
2. **Pages-deploy + verificatie** — `c8218a7` push → workflow run `26589393249` success → curl bevestigt `SheetHorse v0.5.0-Knabstrupper`.
3. **Multi-table-example.csv** — 3 tabellen (5+5+8 rijen) + 2 FK-relaties + 1 JOIN-query + fake style-cols (later gecorrigeerd in B-006).
4. **Debug geel × 2** — relaties niet herkend (parser-bug) + add-row col-dropdown leeg (UI-bug). RCA 3 niveaus per bug → v0.5.0.1.
5. **Debug geel × 5** — 1 was UX-misinterpretatie (B-006 maskeerde "automatisch herkennen"), 1 was voorbeeldbestand-fout (B-006), 2 waren feature-gaps (F-001/F-002 rij+kolom-CRUD), 1 was query-builder single-table (F-003). WhatIf gesplitst in **batch A** (geel) + **batch B** (oranje); akkoord op beide → v0.6.0-Hanoverian.
6. **Debug geel B-009** — XLSX-import liftte `__style_*`-kolommen niet. Fix + `multi-table-example.xlsx` via `openpyxl`-script (5 sheets met native Excel cell-styles) → v0.6.0.1.
7. **SSH tunnel restart** — `/Users/christian/MacTerminal/tunnel.sh restart` → tunnel verified `LISTEN 127.0.0.1:2222` op HC55; health-check `{"status":"ok","tunnel":"connected"}`.
8. **Debug geel B-010** — Wis SQL-knop leegde textarea niet. Triviale 2-regel-fix → v0.6.0.2.

## Patronen toegevoegd aan BUGLIST

| Patroon | Beschrijving |
|---|---|
| `FORMAT-CONTRACT-001` | Mensgerichte CSV/JSON-formats moeten header-synoniemen vooraf vastleggen + testbestand met beide vormen |
| `UI-STATE-001` | Transiente UI-state (form-state vóór commit) vereist lokaal rebuild-pad; observer dekt alleen committed DataStore-state |
| `ROUNDTRIP-EXAMPLE-001` | Bij elk voorbeeld in `docs/examples/`: import→export→diff-check vóór publicatie; format-spec in README moet 1-op-1 met parser-regex matchen |
| `STYLE-LIFTING-PARITY-001` | Per-format style-lifting gelijkaardig toepassen — bij elke parse-path die headers+rows oplevert, ook `IO.liftStyles` proberen naast native-format style-lifting |

## Open items

- Geen — alle gele bugs gefixt, beide feature-batches gemerged.
- Toekomstige releases vrij voor: virtual scrolling (v0.7.0 oranje), of further SheetHorse-features.

## Commits in deze sessie (sheethorse branch)

```
d727a31 v0.6.0.2-Hanoverian — B-010 Wis SQL leegt textarea
c78d9cd v0.6.0.1-Hanoverian — B-009 XLSX __style_* lift + xlsx voorbeeld
fcf1b3f v0.6.0-Hanoverian — bugfix batch A + feature batch B
3978393 docs: voorbeeldbestand multi-table-example.csv
0902980 v0.5.0.1-Knabstrupper — B-004 + B-005
c8218a7 v0.5.0-Knabstrupper — CSV-met-metadata format
```

## Live verificatie

`https://cpaglebbeek.github.io/CSVHorse/sheethorse/` toont `<span class="badge">v0.6.0.2-Hanoverian</span>`.

Test-bestanden:
- `https://cpaglebbeek.github.io/CSVHorse/sheethorse/docs/examples/multi-table-example.csv`
- `https://cpaglebbeek.github.io/CSVHorse/sheethorse/docs/examples/multi-table-example.xlsx`
