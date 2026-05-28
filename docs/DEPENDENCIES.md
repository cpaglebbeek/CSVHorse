# DEPENDENCIES.md — CSVHorse

Component-afhankelijkheden + wijzigings-impact matrix. Bij wijziging in een component: check welke andere geraakt worden vóór commit.

## Externe afhankelijkheden (vendored)

| Lib | Versie (gepland) | Licentie | Rol | Bron |
|-----|-----------------|----------|-----|------|
| PapaParse | 5.4.x | MIT | CSV parse + unparse | https://www.papaparse.com/ |
| AlaSQL | 4.x | MIT | SQL-engine over JS-arrays | https://github.com/AlaSQL/alasql |

**Vendoring-strategie:**
- Minified `.js`-bestanden inline opgenomen in `index.html` `<script>`-tags
- Versie + SHA-hash gedocumenteerd hier
- Update-procedure: download release, verify hash, vervang, regressietest, bump versie + codenaam

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
