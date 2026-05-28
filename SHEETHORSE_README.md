# SheetHorse — full variant

> **Branch:** `sheethorse` · **Forked from:** CSVHorse v0.2.0.2-Mustang (main) op 2026-05-28

SheetHorse is de **full variant** van CSVHorse — een standalone single-file HTML applicatie die functioneert als een **virtuele relationele database** met multi-tabel-ondersteuning, relaties, SQL-joins en Excel-roundtrip.

## Verschil met CSVHorse (light, main branch)

| Feature | CSVHorse light | SheetHorse full |
|---------|---------------|-----------------|
| Single-table CSV-werkbank | ✓ | ✓ |
| Opmaak via `__style_*` | ✓ | ✓ |
| SQL (single-table via AlaSQL) | ✓ | ✓ |
| Filter, edit, undo, search, autosave | ✓ | ✓ |
| Lege CSV starten | ✓ | ✓ |
| **Excel (.xlsx) import + export** met opmaak-mapping | ❌ | ✓ (v0.1.0+) |
| **Multi-sheet** in XLSX + sheet-switcher | ❌ | ✓ (v0.2.0+) |
| **Relaties-tabblad** (CRUD) | ❌ | ✓ (v0.3.0+) |
| **SQL met joins** over multiple tables | ❌ | ✓ (v0.4.0+) |
| **Query-output-tabblad** + opslaan als metadata | ❌ | ✓ (v0.4.0+) |
| **Multi-tabel in 1 CSV** via metadata-markers | ❌ | ✓ (v0.5.0+) |

## Roadmap

| Versie | Codenaam | Scope |
|--------|----------|-------|
| v0.0.1 | Haflinger | Fork van CSVHorse v0.2.0.2 + naam-shift + skeleton-docs |
| **v0.1.0** | **Lipizzaner** ✓ (huidig) | XLSX import + export single-sheet (xlsx-populate 1.21.0 vendored, ~640KB MIT) — opmaak-mapping naar Excel cell-styles (bold/italic/underline/strikethrough/color/fill/font-size/align) · embedded-objects detect-only met toast-waarschuwing · multi-sheet via sheet-picker dialog bij import |
| v0.2.0 | Andalusian | Multi-sheet in XLSX: lees alle sheets, switcher in toolbar, 1 actieve sheet bewerkbaar; per-sheet state |
| v0.3.0 | Trakehner | Relaties-tabblad: CRUD op `(from-table, from-col, to-table, to-col)`-tuples; FK-suggesties op basis van kolom-naam-match |
| v0.4.0 | Akhal-Teke | SQL met joins over multi-sheet (AlaSQL multi-table); query-output als tabblad; query+output opslaan als metadata in XLSX (extra sheet `_query`) |
| v0.5.0 | Knabstrupper | **CSV-met-metadata-format**: marker-conventie voor multi-tabel in 1 CSV — `# TABLE klanten` / `# RELATIONS` / `# QUERY` op kop-regels; parser splitst in logische tabellen |

## CSV-met-metadata-format (v0.5.0)

```
# TABLE klanten
id,naam,stad
1,Anna,Haarlem
2,Bram,Amsterdam

# TABLE orders
id,klant_id,bedrag
1,1,49.99
2,1,29.50
3,2,15.00

# RELATIONS
from_table,from_col,to_table,to_col,type
orders,klant_id,klanten,id,many-to-one

# QUERY
sql=SELECT klanten.naam, SUM(orders.bedrag) AS totaal FROM klanten JOIN orders ON klanten.id = orders.klant_id GROUP BY klanten.naam
# QUERY_OUTPUT
naam,totaal
Anna,79.49
Bram,15.00
```

## Branch-management

- **Bug-fixes** die in beide branches gelden: eerst op `main` (CSVHorse) committen, dan cherry-pick naar `sheethorse`
- **`/bugcheck`-skill** scant beide branches (zie `feedback_bugcheck_hele_tree.md` in claude memory)
- **Pages-deployment** voor SheetHorse: nog te regelen — opties: GitHub Actions die `sheethorse`-branch naar subpath `/sheethorse/` deployt onder dezelfde Pages-domain
- **Code-deduplicatie:** vooralsnog geen; beide branches evolueren vanuit gedeelde root v0.2.0.2-Mustang

## Status v0.1.0-Lipizzaner

| Aspect | Status |
|--------|--------|
| Branch `sheethorse` | ✓ aangemaakt 2026-05-28 vanaf `main` `ed5efaf` |
| Naam-shift in `index.html` | ✓ |
| `version.json` | ✓ SheetHorse v0.1.0-Lipizzaner |
| **xlsx-populate vendoring** | ✓ v1.21.0 inline (~640KB, SHA `33aa41e7...`) |
| **Excel-export** met native cell-styles | ✓ — format-dropdown in export-dialog |
| **Excel-import** met cell-style-lift | ✓ — auto-detect via `.xlsx`-extensie |
| **Multi-sheet import** via sheet-picker dialog | ✓ — kies 1 sheet bij >1 sheets |
| **Embedded-objects detect** + toast-waarschuwing | ✓ |
| Multi-sheet runtime support (switcher) | ⏸ v0.2.0-Andalusian |
| Relaties-tabblad | ⏸ v0.3.0-Trakehner |
| SQL joins + query-tabblad | ⏸ v0.4.0-Akhal-Teke |
| CSV-met-metadata-format | ⏸ v0.5.0-Knabstrupper |

## v0.1.0-Lipizzaner werking

### Export
1. Klik `⬇ Exporteer` → modal opent met **Format-dropdown**: CSV of Excel
2. Bij Excel: BOM-checkbox verbergt; filename auto-update naar `.xlsx`
3. **Met opmaak** vinkje werkt voor beide formaten:
   - CSV: extra `__style_*` kolommen
   - Excel: native cell-styles
4. Toast: `Geëxporteerd Excel: N rijen → naam.xlsx · M cell-styles`

### Import
1. Sleep `.xlsx` op de pagina of klik Upload
2. Bij `.xlsx`-extensie: auto-detectie → xlsx-route
3. Bij >1 sheets: modal "📑 Excel: meerdere tabbladen" met dropdown
4. Bij embedded objects: toast-waarschuwing (NIET geïmporteerd)
5. Cell-styles → `DataStore.styles[r][c]`

### Style-mapping bidirectional
| StyleObj-property (intern) | Excel cell-style |
|---|---|
| `bold` | `bold` |
| `italic` | `italic` |
| `underline` | `underline: 'single'` |
| `strikethrough` | `strikethrough` |
| `color: '#ff0000'` | `fontColor: 'ff0000'` |
| `background: '#ffff00'` | `fill: 'ffff00'` |
| `fontSize: 14` | `fontSize: 14` |
| `align: 'center'` | `horizontalAlignment: 'center'` |

## Licentie

AGPL-3.0 (idem als CSVHorse).
