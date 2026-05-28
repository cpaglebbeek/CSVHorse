# SheetHorse — voorbeeldbestanden

## `multi-table-example.xlsx` (v0.6.0.1-Hanoverian)

XLSX-equivalent met **native Excel cell-styles** in plaats van `__style_*`-kolommen.

**Bevat:**
- 3 datasheets: `klanten` (5 rijen) · `producten` (5 rijen) · `bestellingen` (8 rijen)
- 1 `_relations`-sheet met 2 FK-relaties (zelfde schema als XLSX-export)
- 1 `_query`-sheet met de laatste JOIN-query
- Cell-styles: bold/color/background per cel, identiek aan de CSV-versie

**Gebruik:**
1. Open SheetHorse: <https://cpaglebbeek.github.io/CSVHorse/sheethorse/>
2. Klik **Upload CSV** en kies `multi-table-example.xlsx` (xlsx is ook toegestaan)
3. Verwacht: 3 tabs onderaan + 🔗 Relaties-tab met badge `2` + lastQuery geladen

**Genereren:** `python3 docs/examples/_build_xlsx_example.py` (vereist `openpyxl`).

## `multi-table-example.csv`

Showcase van het CSV-met-metadata format dat in v0.5.0-Knabstrupper is geïntroduceerd.

**Bevat:**
- 3 tabellen: `klanten` (5 rijen), `producten` (5 rijen), `bestellingen` (8 rijen)
- 2 FK-relaties: `bestellingen.klant_id → klanten.id` en `bestellingen.sku → producten.sku`
- 1 JOIN-query: totale omzet per VIP-klant
- Opmaak per cel via verborgen `__style_*`-kolommen (bold + kleur + achtergrond)

**Gebruik:**
1. Open SheetHorse: <https://cpaglebbeek.github.io/CSVHorse/sheethorse/>
2. Klik **Importeren** → kies `multi-table-example.csv`
3. SheetHorse detecteert de markers automatisch en laadt de drie tabellen, relaties en query in één keer
4. Wissel tussen tabbladen onderaan om elke tabel te bekijken
5. Open het SQL-paneel — de geladen query staat klaar om uit te voeren

**Round-trip:**
Export via **Exporteren → CSV multi-tabel (met markers)** levert exact dezelfde structuur terug. Opmaak blijft per cel behouden via de `__style_*`-kolommen.

**Format-specificatie:** zie hoofdsectie in `index.html` (functie `IO.flattenMultiSheetCsv`).
