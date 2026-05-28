# BUGLIST.md — CSVHorse

Bug-historie + preventieregels. Conform Meta_Master `templates/BUGLIST_TEMPLATE.md`.

## Open bugs

_Geen._

## Opgeloste bugs

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

## Globale referentie

Cross-repo patronen: zie `Meta_Master/BUGS_GLOBAL.md` (CACHE/DEPLOY/ZSH/FEATURE).

## Protocol bij bugfix

1. **STOP** — geen impulsfix
2. **Kleur bepalen** — groen (fysiek) / geel (logisch) / rood (conceptueel) / loop (vastloop)
3. **RCA 3 niveaus** — functioneel / technisch / architectonisch
4. **WhatIf** — plan + impact + akkoord vóór fix
5. Bij rood: security-review meenemen
6. Na fix: deze BUGLIST.md updaten + versie-bump + RCA-notitie in commit-message
