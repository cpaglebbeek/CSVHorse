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
| **v0.0.1** | **Haflinger** ✓ (huidig) | Fork van CSVHorse v0.2.0.2 + naam-shift + skeleton-docs |
| v0.1.0 | Lipizzaner | XLSX import + export single-sheet (xlsx-populate vendored, ~250KB MIT) — opmaak-mapping naar Excel cell-styles (bold/italic/underline/color/fill/font-size/align) · embedded-objects detect-only met toast-waarschuwing |
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

## Status v0.0.1-Haflinger

| Aspect | Status |
|--------|--------|
| Branch `sheethorse` | ✓ aangemaakt 2026-05-28 vanaf `main` `ed5efaf` |
| Naam-shift in `index.html` | ✓ "CSVHorse" → "SheetHorse" in title/header/badges/generator |
| `version.json` | ✓ SheetHorse v0.0.1-Haflinger |
| `SHEETHORSE_README.md` | ✓ dit document |
| `xlsx-populate` vendoring | ⏸ v0.1.0-Lipizzaner (volgende stap) |
| Excel import/export | ⏸ v0.1.0-Lipizzaner |
| Multi-sheet support | ⏸ v0.2.0-Andalusian |
| Relaties-tabblad | ⏸ v0.3.0-Trakehner |
| SQL joins + query-tabblad | ⏸ v0.4.0-Akhal-Teke |
| CSV-met-metadata-format | ⏸ v0.5.0-Knabstrupper |

## Licentie

AGPL-3.0 (idem als CSVHorse).
