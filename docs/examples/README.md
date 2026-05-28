# SheetHorse — voorbeeldbestanden

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
