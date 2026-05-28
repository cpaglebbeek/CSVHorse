# DEPENDENCIES.md — CSVHorse

Component-afhankelijkheden + wijzigings-impact matrix. Bij wijziging in een component: check welke andere geraakt worden vóór commit.

## Externe afhankelijkheden (vendored)

| Lib | Versie | Licentie | Rol | Bron | Status |
|-----|--------|----------|-----|------|--------|
| PapaParse | **5.4.1** | MIT | CSV parse + unparse | https://www.papaparse.com/ | ✓ vendored sinds v0.0.2-Arabian |
| AlaSQL | 4.x | MIT | SQL-engine over JS-arrays | https://github.com/AlaSQL/alasql | ⏸ wacht op v0.1.1-Akhal-Teke |

### PapaParse 5.4.1

- **Bron-bestand:** `vendor/papaparse-5.4.1.min.js` (19.469 bytes)
- **SHA-256:** `b8e870c5d2b29772f10c9fa9a693c8b896aac8540ed6701e3cc6304c683febdb`
- **CDN-URL bij download:** https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js
- **Inline opgenomen in:** `index.html` `<script>`-tag met versie + hash-comment

**Verificatie-commando:**
```bash
shasum -a 256 vendor/papaparse-5.4.1.min.js
# moet zijn: b8e870c5d2b29772f10c9fa9a693c8b896aac8540ed6701e3cc6304c683febdb
```

**Vendoring-strategie:**
- Minified `.js`-bestanden **inline** in `index.html` `<script>`-tags (single-file constraint, P2)
- Bron in `vendor/` map blijft als traceerbare source-of-truth + hash-verificatie-bron
- Update-procedure:
  1. Download release van CDN
  2. Vergelijk SHA-256 met published hash
  3. Vervang `vendor/<naam>-<versie>.min.js`
  4. Regenereer `index.html` (concat head + vendor + tail)
  5. Update versie + SHA hier
  6. Bump CSVHorse versie + codenaam in `version.json` (oranje bij design-impact, groen bij minor)
  7. Regressietest manueel doorlopen

**Geen externe runtime-deps verder** — geen npm, geen CDN, geen polyfills (browser-target = moderne evergreen).

## Interne component-afhankelijkheden

```
IO Layer       depends on → PapaParse
DataStore      depends on → (niemand, single source)
SQLEngine      depends on → AlaSQL, DataStore
Renderer       depends on → DataStore, SQLEngine.viewSet
EditController depends on → DataStore, CommandHistory, Renderer
CommandHistory depends on → DataStore (apply/revert)
Toolbar UI     depends on → DataStore, SQLEngine, EditController
AutosaveService depends on → DataStore (observed)
```

## Wijzigings-impact matrix

Wanneer je iets wijzigt in *rij* X, controleer alle *kolommen* met ✓:

|  Wijziging \ Raakt | IO | DataStore | SQLEngine | Renderer | EditController | CommandHistory | Toolbar | Autosave |
|--------------------|----|-----------|-----------|----------|----------------|----------------|---------|----------|
| **DataStore schema** | ✓ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **DataStore.styles model** | ✓ | — | — | ✓ | ✓ | ✓ | ✓ | ✓ |
| **CSV dialect-opties** | ✓ | ✓ | — | — | — | — | ✓ | — |
| **PapaParse versie** | ✓ | — | — | — | — | — | — | — |
| **AlaSQL versie** | — | — | ✓ | — | — | — | ✓ | — |
| **viewSet-format** | — | — | ✓ | ✓ | — | — | — | — |
| **Cell-row-height (design)** | — | — | — | ✓ | — | — | — | — |
| **Opmaak-attribuut toevoegen** | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | ✓ |
| **Command-types lijst** | — | ✓ | — | — | ✓ | ✓ | — | — |
| **localStorage quota-strategie** | — | — | — | — | — | — | — | ✓ |
| **__style_* naming-conventie** | ✓ | — | — | — | — | — | — | — |
| **Color-tokens (DESIGN_TOKENS)** | — | — | — | ✓ | — | — | ✓ | — |

## Verboden afhankelijkheidsrichtingen

- DataStore **mag NIET** Renderer kennen (one-way binding via observer)
- CommandHistory **mag NIET** Renderer kennen (alleen DataStore)
- SQLEngine **mag NIET** Renderer of EditController kennen (puur compute)
- AutosaveService **mag NIET** DOM-API gebruiken (alleen DataStore + storage)

Reden: voorkomt circulaire afhankelijkheden en houdt SQL/Storage/Compute scheidbaar van DOM-rendering — essentieel voor toekomstige worker-offload of headless-tests.

## Build / bundle

- Geen build-tool in MVP
- Vendoring handmatig (kopie van minified lib in `vendor/` map, of inline)
- Toekomstig: een `tools/build.sh` die `index.dev.html` + `src/*.js` + `vendor/*.js` concat tot één `index.html`

## Update-protocol externe deps

1. Check release-notes lib
2. Download minified release
3. Verify SHA256 tegen ge-published hash
4. Run handmatige regressie-suite (CSV import variatie + SQL queries + export round-trip)
5. Update versie + hash in deze file
6. Versie-bump CSVHorse (groen/oranje/rood afhankelijk van impact)
7. Codenaam volgende paardenras
8. Commit met motivatie
