# BUGLIST.md — CSVHorse

Bug-historie + preventieregels. Conform Meta_Master `templates/BUGLIST_TEMPLATE.md`.

## Open bugs

_Geen._

## Opgeloste bugs

### B-010 — "Wis SQL"-knop leegt textarea niet (geel)

**Datum:** 2026-05-28
**Versie:** v0.6.0.2-Hanoverian
**Symptoom:** Klik op "Wis SQL"-knop in SQL-paneel: resultaat verdwijnt (view gaat terug naar data), maar de SQL-query blijft in de textarea staan + eventuele foutmelding blijft zichtbaar.

**RCA (3 niveaus):**
- **Functioneel:** "Wis" is voor de gebruiker een totaal-reset; halve reset wekt de indruk dat de knop deels werkt.
- **Technisch:** `btnClearSql`-handler deed alleen `SQLEngine.clear()` + `DataStore.clearQueryResult()` + toast — vergat `sqlText.value = ''` en `sqlError.textContent = ''`.
- **Architectonisch:** ontbrekende state-reset bij een "clear all"-actie. State leeft op meerdere plekken (engine, store, textarea-DOM, error-DOM) maar niet alle paden waren gevoegd.

**Fix:** handler aangevuld met textarea-leegmaken en error-leegmaken. Toast bijgewerkt naar "SQL-resultaat + query gewist" voor accuratere feedback.

### B-009 — XLSX-import lift `__style_*`-kolommen niet, ze blijven als datakolommen zichtbaar (geel)

**Datum:** 2026-05-28
**Versie:** v0.6.0-Hanoverian (zichtbaar), gefixt in v0.6.0.1-Hanoverian
**Symptoom:** Bij import van een XLSX met `__style_<prop>__<colname>` headers (door een CSV→Excel-conversie via een externe tool) bleven die kolommen zichtbaar als data, in plaats van als opmaak op de echte kolommen gelift te worden. Niet zichtbaar bij pure SheetHorse-roundtrip (daar gaan styles via native Excel cell-styles), wel bij externe conversie van CSV-met-metadata.

**RCA (3 niveaus):**
- **Functioneel:** XLSX-content krijgt geen opmaak; gebruiker ziet "rauwe" `__style_*`-kolommen.
- **Technisch:** `_parseXlsxFile` riep `ExcelIO.liftSheetData` aan (lift native cell-styles) maar NIET `IO.liftStyles` op de gelifte data. Dat laatste is CSV-specifiek en werd alleen door `parseFile`-CSV-pad en `liftMultiSheetCsv` per TABLE-blok aangeroepen.
- **Architectonisch:** style-lifting was alleen voor CSV-pad gemodelleerd. Het feit dat een XLSX óók `__style_*`-kolommen kan bevatten (na externe conversie) was niet meegenomen.

**Fix:** `_parseXlsxFile` controleert per sheet of headers `IO.STYLE_COL_PATTERN` matchen. Zo ja: `IO.liftStyles(rows, cols)` aanroepen, dan native Excel-styles (uit `data.styles`) **overlayen** op de gemapte kolom-indexen — Excel-cell-styles winnen bij conflict (zijn altijd "echter" dan CSV-tag-based). Header-index-shift correct toegepast via `colMap`.

**Diagnostiek:** XLSX-import logt nu `[SheetHorse XLSX-import] file=X · N sheets · cell-styles: M · __style_*-cols gelift: K · _relations: JA/nee · _query: JA/nee`. Toast meldt `· K __style_*-kolom(men) gelift uit headers` bij positief detect.

**Preventie:** patroon `STYLE-LIFTING-PARITY-001` — bij elke parse-path die headers + rows oplevert, controleer of het CSV-specifieke `IO.liftStyles` óók nodig is naast eventuele native-format style-lifting. Geldt voor XLSX, en in de toekomst potentieel ODS, JSON, etc.

### B-008 — Nieuw werkblad start altijd als 1×1, geen schema-keuze (geel feature-gap)

**Datum:** 2026-05-28
**Versie:** v0.6.0-Hanoverian
**Symptoom:** Klik op "+ Nieuw werkblad" maakt direct een 1-rij × 1-kolom sheet. Voor een serieuze multi-tabel workflow moet de gebruiker meteen het initiële schema kunnen kiezen.

**Fix:** `_addBlankSheet` toont nu dezelfde dialog als 🆕 Nieuw (`newCsvModal`), maar in `_blankSheetMode = true`. Voorgevuld 3×5. `confirmNewCsv` dispatcht op het flag: blank-mode = `DataStore.addSheet({rows, cols, ...}, true)`; vervang-mode = bestaande replace-pad.

### B-007 — Geen feedback bij multi-tabel CSV detect (geel)

**Datum:** 2026-05-28
**Versie:** v0.6.0-Hanoverian
**Symptoom:** Bij import van multi-tabel CSV met markers krijgt de gebruiker geen indicatie of detect aansloeg of niet — toast meldt alleen "geladen: N rijen × M kolommen" voor sheets[0].

**Fix:** `IO.parseFile` logt nu `[SheetHorse import] file=X size=Y multi-tabel-markers detected=true/false` + bij positief: aantal tabellen, relaties, query-status, gelifte style-cellen. `handleFile` toast meldt voor `csv-multi`: `· multi-tabel CSV: N tabs + M relaties + 1 query`.

### B-006 — Voorbeeldbestand `multi-table-example.csv` had verkeerd style-schema (geel)

**Datum:** 2026-05-28
**Versie:** v0.6.0-Hanoverian
**Symptoom:** Bij import van het voorbeeldbestand bleven `__style_naam`, `__style_vip` etc. als data-kolommen zichtbaar in plaats van als opmaak op de echte kolommen gelift te worden.

**RCA (3 niveaus):**
- **Functioneel:** voorbeeld toont format dat het zelf niet implementeert.
- **Technisch:** voorbeeld gebruikte verzonnen compact-schema `__style_<colname>` met inline KV-pairs `b=1;c=#ff0000`. Werkelijke `liftStyles`-regex eist `__style_<prop>__<colname>` (twee underscores tussen prop en colnaam) — één kolom per `(prop, col)`-combinatie.
- **Architectonisch:** format-contract-mismatch tussen documentatie/voorbeeld en parser. Geen test die voorbeeld-files round-tript.

**Fix:** voorbeeldbestand herschreven met echt schema. Voor klanten 4 style-cols (`__style_bold__naam`, `__style_bold__vip`, `__style_color__vip`, `__style_background__vip`); producten 3; bestellingen 2. README al klopt over format-spec.

**Preventie:** patroon `FORMAT-CONTRACT-001` uitgebreid: bij elk publiek voorbeeldbestand een round-trip test draaien tegen de eigen parser/writer vóór publicatie.

### B-005 — Handmatig relatie aanmaken werkt niet: col-dropdown blijft leeg (geel)

**Datum:** 2026-05-28
**Versie:** v0.5.0-Knabstrupper (intro), gefixt in v0.5.0.1-Knabstrupper
**Symptoom:** In de 🔗 Relaties-tab: user kiest een werkblad in de From-dropdown, maar de bijbehorende From-col-dropdown blijft op `--kolom--` zonder opties. Knop "+ Voeg toe" → toast "Vul alle 4 kolommen in".

**RCA (3 niveaus):**
- **Functioneel:** add-row in relaties-tab is feitelijk dood — niemand kan handmatig een relatie toevoegen.
- **Technisch:** `_buildRelRow` met `isAddRow=true` bouwt de col-`<select>` met `cols = sheet ? sheet.cols : []`. Op render-tijd is `state.from_sheet` leeg → 0 opties. Sheet-change updatet alleen `state`, niet de DOM. Voor bestaande rels werkt het: `updateRelation` → `notify('relations-changed')` → volledige her-render van `renderRelationsPanel()`. Voor add-row is er géén `rel.id`, dus geen notify, dus stale dropdown.
- **Architectonisch:** stale-closure UI-bug — geen feedback-loop tussen lokale transiente state en DOM voor de add-row. De observer-pattern dekt alleen committed DataStore-state.

**Fix:** sheet-select-handler voor `isAddRow=true` rebouwt nu inline de bijbehorende col-`<td>` via een `colCells`-map. Col-select krijgt `disabled=true` als er nog geen sheet gekozen is (visuele feedback).

**Preventie:** voor transiente UI-state altijd een lokale rebuild-pad bewaren; observer-notifies alleen voor committed state. Patroon-ID: `UI-STATE-001` — toegevoegd onder "Terugkerende patronen".

### B-004 — RELATIONS-blok in multi-tabel CSV niet herkend (geel)

**Datum:** 2026-05-28
**Versie:** v0.5.0-Knabstrupper (intro), gefixt in v0.5.0.1-Knabstrupper
**Symptoom:** Het meegeleverde `docs/examples/multi-table-example.csv` heeft 2 relaties, maar na import in SheetHorse staat het relaties-tabblad leeg. Geen warning of toast.

**RCA (3 niveaus):**
- **Functioneel:** de format-spec gepubliceerd op LinkedIn + voorbeeldbestand gebruiken `from_table`/`to_table` (lezerstaal); parser zoekt `from_sheet`/`to_sheet` (interne canonical). Hele blok wordt stil overgeslagen.
- **Technisch:** `IO.liftMultiSheetCsv` regel ~3143: `iFs = idx('from_sheet')`. Geen alias-fallback. Regel ~3146: `if (iFs < 0 || iTs < 0) continue` zonder `console.warn` of toast.
- **Architectonisch:** format-contract-mismatch tussen publicatie en implementatie. De CSV is een **mensgericht uitwisselingsformaat**; tolerant accepteren van synoniemen hoort daarbij.

**Fix:** parser accepteert nu beide schema's via `findFirst([alias1, alias2])`-helper. Aliases:
- `from_sheet` ⇄ `from_table`
- `to_sheet` ⇄ `to_table`
- `from_col` ⇄ `from_column`
- `to_col` ⇄ `to_column`
- `cardinality` ⇄ `kardinaliteit`
- `name` ⇄ `naam`

Writer (`flattenMultiSheetCsv`) blijft canonical `from_sheet`-schema schrijven → bestaande v0.5.0-exports zijn backwards-compatible.

**Preventie:** bij elk publiek CSV/JSON-format vooraf vastleggen welke header-synoniemen worden geaccepteerd, en dat valideren via een testbestand met **beide** vormen. Patroon-ID: `FORMAT-CONTRACT-001` — toegevoegd onder "Terugkerende patronen".

### B-003 — Opmaak verdwijnt bij re-import na export (groen)

**Datum:** 2026-05-28
**Versie:** v0.1.4-Trakehner (manifestatie; geen code-bug)
**Symptoom:** Na opmaak + export + heropen-import in browser: opmaak werd niet getoond, ondanks dat `__style_*` kolommen wel in CSV stonden.

**RCA (3 niveaus):**
- **Functioneel:** gebruiker zag oude UI/JS-code zonder roundtrip-functionaliteit
- **Technisch:** **browser-cache** — vorige sessie had v0.1.3 (zonder lift) geladen, browser cached `index.html` + bijbehorende JS. Pagina opnieuw openen via normale F5 trekt cache aan zonder een revalidate van de inline scripts.
- **Architectonisch:** zelfde categorie als B-001 (CDN-cache, server-side). Nu client-side: Pages levert juiste versie, browser leest oude versie uit eigen cache.

**Fix:** geen code-wijziging nodig. **Hard-refresh (`Ctrl/Cmd+Shift+R`)** lost het op. Verifieerd door gebruiker: "hard refresh gedaan, alles werkt".

**Preventie:**
- Bij testen van een net-gedeployde nieuwe versie: altijd **hard-refresh** doen, niet alleen F5
- Voor publieke deploy (v0.5.0-Hanoverian → `icthorse.nl`) overwegen: `<meta http-equiv="cache-control" content="no-cache, must-revalidate">` of versie-hash in `<script>` src (niet relevant nu, want single-file)
- Diagnose-pad nu vast: `[CSVHorse import]` + `[CSVHorse load]` console.log + toast `opmaak hersteld: N cellen` — gebruiker kan direct in console zien wat er gelift is

**Patroon-ID:** `DEPLOY-CACHE-002` — toegevoegd onder "Terugkerende patronen".

### B-002 — SQL "table does not exist: data" (groen)

**Datum:** 2026-05-28
**Versie:** v0.1.1-Akhal-Teke (intro), gefixt in v0.1.1.1
**Symptoom:** Bij `SELECT * FROM data ...` in het SQL-panel kreeg gebruiker AlaSQL-fout "table does not exist: data".

**RCA (3 niveaus):**
- **Functioneel:** gebruiker typt `FROM data` maar AlaSQL kent geen tabel met die naam
- **Technisch:** initiële implementatie gebruikte `alasql(query, [objects])` — dat bindt `objects` aan positional `?`-placeholders. Dat werkt alleen als de query `FROM ?` schrijft, niet `FROM data` letterlijk.
- **Architectonisch:** verkeerd AlaSQL-binding-patroon gekozen. Voor named-table-syntax (de natuurlijke manier waarop een eindgebruiker SQL schrijft) moet je `alasql.tables.<naam>.data = objects` registreren voorafgaand aan de query, NIET positional params.

**Fix:** in `SQLEngine.run()` vóór `alasql(query)` aanroepen:
```js
if (!window.alasql.tables.data) window.alasql('CREATE TABLE data');
window.alasql.tables.data.data = objects;
const res = window.alasql(query);  // geen 2e arg meer
```
Re-bind elke run zodat actuele DataStore (incl. cell-edits) zichtbaar is voor elke nieuwe SELECT.

**Preventie:**
- Vóór nieuwe AlaSQL-features: test eerst de query-syntax die gebruiker écht zal typen, niet wat in vendor-docs als eerste voorbeeld staat
- Het `alasql.tables.<naam>.data`-patroon werkt voor alle `CREATE TABLE`-namen die je vooraf registreert

**Patroon-ID:** `SQL-001` — toegevoegd onder "Terugkerende patronen".

### B-001 — CDN cache serveert verouderde versie (geel)

**Datum:** 2026-05-28
**Versie:** v0.1.0-Lipizzaner (manifestatie)
**Symptoom:** Gebruiker zag in source-view `v0.0.2-Arabian` op `cdn.jsdelivr.net/gh/cpaglebbeek/CSVHorse@main/index.html` terwijl op disk + GitHub al v0.1.0 stond. Dubbelklik-edit werkte niet (omdat v0.0.2 die feature nog niet had).

**RCA (3 niveaus):**
- **Functioneel:** gebruiker krijgt oudere versie ondanks recente push
- **Technisch:** jsDelivr cached `@main`-URL's tot 7 dagen (`cache-control: max-age=604800, s-maxage=43200`). GitHub raw heeft slechts 5 min cache (`max-age=300`). De CDN-versie was 2099s oud.
- **Architectonisch:** `jsDelivr@main` is per design **niet** geschikt voor live preview tijdens active development — het is een immutable-CDN pattern voor stable releases met versie-tags, niet voor moving branches.

**Fix:**
1. **One-shot purge:** `curl https://purge.jsdelivr.net/gh/cpaglebbeek/CSVHorse@main/index.html` — werkt binnen seconden
2. **Permanente oplossing:** GitHub Pages ingeschakeld → `https://cpaglebbeek.github.io/CSVHorse/` updates automatisch na elke push (geen CDN tussen, 600s browser-cache max)
3. Status `built` na ~24s

**Preventie:**
- Tijdens active development: gebruik **GitHub Pages** als primaire preview-URL
- Voor één-keer bezoekers of stable releases: jsDelivr met **commit-hash** (`@<sha>`) of **versie-tag** in plaats van `@main`
- Bij `Pull` zonder dat je je laatste push terugziet: check `curl -sI` → `age:` header
- jsDelivr documenteert: purge-API geaccepteerd via **GET** (niet POST)

**Patroon-ID:** `DEPLOY-CDN-001` — toegevoegd onder "Terugkerende patronen".

## Terugkerende patronen (preventie)

Te vullen tijdens MVP-implementatie en daarna. Initiële verwachte categorieën gebaseerd op tech-stack:

| Patroon-ID | Categorie | Beschrijving | Preventieregel |
|------------|-----------|-------------|----------------|
| CSV-001 | Parse | CRLF/LF-verwarring bij export-roundtrip | Dialect altijd expliciet opslaan in DataStore, niet auto-detecteren bij export |
| CSV-002 | Parse | BOM in eerste cell-value | PapaParse `skipEmptyLines` + custom BOM-strip vóór parse |
| SQL-001 | Engine | AlaSQL case-sensitivity op kolomnamen met spaties | Kolomnamen escapen met `[...]` of `"..."` consequent |
| SQL-002 | Engine | `alasql(query, [objects])` werkt alleen met `FROM ?`-positional, niet met `FROM <naam>` letterlijk → "table does not exist" | Vóór query: `alasql('CREATE TABLE data'); alasql.tables.data.data = objects;` daarna `alasql(query)` zonder 2e arg. Re-bind elke run om edits te reflecteren. |
| STYLE-001 | Roundtrip | `__style_*`-kolom collisie met user-data | Naming-conventie strikt valideren bij import; user-warning bij conflict |
| RENDER-001 | Virtual scroll | Scroll-jump bij rij-verwijdering | Anchor-row-ID behouden, scrollTop bijwerken na DataStore mutatie |
| UNDO-001 | History | Stack-explosie bij paste van groot blok | Paste = 1 batch-command, geen N losse cell-edits |
| STORE-001 | Autosave | localStorage quota-exceeded bij groot dataset | Quota-check vóór write, schuif uitschakelen + toast bij overflow |
| DEPLOY-CDN-001 | Deploy | jsDelivr CDN cached `@main` tot 7 dagen → gebruiker ziet oude versie | Tijdens active development = **GitHub Pages** als primaire preview-URL; jsDelivr alleen voor stable releases met commit-hash of versie-tag |
| DEPLOY-CACHE-002 | Deploy/Client | Browser cached `index.html` → user ziet oude JS-versie ondanks Pages-deploy van nieuwe | Bij elk feature-test: **hard-refresh** (Ctrl/Cmd+Shift+R) i.p.v. F5. Voor publieke deploy: cache-control headers (nog niet nodig in dev-fase) |
| FORMAT-CONTRACT-001 | Parse | Header-naming-mismatch tussen publicatie (`from_table`) en parser (`from_sheet`) → blok stil overgeslagen | Mensgerichte CSV/JSON-formats: lijst van geaccepteerde header-synoniemen vooraf vastleggen + testbestand met beide vormen; nooit `continue` zonder `console.warn` bij onbekende headers |
| UI-STATE-001 | UI | Add-row dropdowns afhankelijk van eerder gekozen dropdown her-renderen niet bij sheet-change → input feitelijk dood | Voor transiente UI-state een lokale rebuild-pad bewaren (td-references in een map); observer-notifies dekken alleen committed DataStore-state, niet pre-commit form-state |
| ROUNDTRIP-EXAMPLE-001 | Docs | Publiek voorbeeldbestand gebruikt format dat eigen parser niet implementeert | Bij elk voorbeeld in `docs/examples/`: import → export → diff-check vóór publicatie; format-spec in README moet 1-op-1 met parser-regex matchen |
| STYLE-LIFTING-PARITY-001 | Parse | Per-format style-lifting niet gelijkaardig toegepast (CSV-pad lift `__style_*`, XLSX-pad niet) | Bij elke nieuwe parse-path die headers + rows oplevert: ook altijd `IO.liftStyles` proberen naast native-format style-lifting; merge met native styles, native wint bij conflict |

## Globale referentie

Cross-repo patronen: zie `Meta_Master/BUGS_GLOBAL.md` (CACHE/DEPLOY/ZSH/FEATURE).

## Protocol bij bugfix

1. **STOP** — geen impulsfix
2. **Kleur bepalen** — groen (fysiek) / geel (logisch) / rood (conceptueel) / loop (vastloop)
3. **RCA 3 niveaus** — functioneel / technisch / architectonisch
4. **WhatIf** — plan + impact + akkoord vóór fix
5. Bij rood: security-review meenemen
6. Na fix: deze BUGLIST.md updaten + versie-bump + RCA-notitie in commit-message
