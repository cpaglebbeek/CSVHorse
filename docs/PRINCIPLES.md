# PRINCIPLES.md — CSVHorse

Conceptuele principes met hun onderlinge relaties. Dit document beschrijft het *waarom* achter de architectuur — de spelregels die de richting van toekomstige beslissingen sturen.

## P1 — Client-only

**Stelling:** Alle verwerking, opslag en weergave gebeurt in de browser. Geen netwerk-calls, geen accounts, geen telemetrie.

**Waarom:** Privacy-by-design — gebruikers werken vaak met gevoelige tabeldata (financieel, klantdata, medisch). Server-vrij = geen data-lek-risico bij hosting.

**Implicaties:**
- Geen backend te onderhouden
- Geen auth-laag
- Alles wat persistent moet zijn → browser-storage (localStorage)
- Bundle-grootte = volledige feature-set (PapaParse + AlaSQL vendored)

**Relatie:** ondersteunt P2 (single-file), P4 (privacy), conflict met "online sync" (out of scope).

## P2 — Single-file deliverable

**Stelling:** Het tool is één `.html`-bestand. Drop op disk, open in browser, klaar.

**Waarom:** Drempelloos voor de gebruiker. Geen install, geen npm, geen "open een terminal". Bestand kan via mail/USB/cloud-drive worden gedeeld.

**Implicaties:**
- CSS en JS inline (`<style>` + `<script>`)
- Vendored libs (PapaParse, AlaSQL) als minified inline JS
- Bundle-grootte = ~200KB realistisch (acceptabel — single file load)
- Geen build-stap nodig voor end-user; develop-time kan wel build hebben (minify + concat)

**Relatie:** vereist door P1, beperkt door bundle-grootte → P5 (geen framework).

## P3 — Round-trip opmaak via systeemkolommen

**Stelling:** Rich-text opmaak per cel wordt opgeslagen in verborgen systeemkolommen (prefix `__style_*`) zodat CSV-export-en-herimport opmaak behoudt.

**Waarom:** CSV is een tekst-formaat zonder opmaak-semantiek. Excel/Numbers verliezen opmaak bij CSV-export. Door styles platte tekst-kolommen te maken die de tool herkent en verbergt, blijft het bestand een geldige CSV én round-trip-baar in CSVHorse.

**Implicaties:**
- Export-dialog heeft toggle "Met opmaak" (default AAN)
- Bij import: detecteer `__style_*`-kolommen, lift naar styles-object, verberg
- Andere CSV-tools zien gewoon extra kolommen met "true"/"#FF0000" etc — geen breaking
- Naming-conventie strikt om collision met user-data te vermijden

**Relatie:** vereist door functionele scope, conflict met "minimale CSV-output" (gemitigeerd via toggle).

## P4 — Privacy-by-design

**Stelling:** De applicatie verzamelt, verzendt of logt geen data. Alle state blijft op het apparaat van de gebruiker.

**Waarom:** Vertrouwen + compliance. Een tool dat ook met persoonsgegevens werkt mag geen exfiltratie-risico introduceren.

**Implicaties:**
- Geen analytics, geen sentry, geen crash-reporter
- `localStorage` autosave is opt-out via schuif (default AAN voor UX, maar uitschakelbaar)
- Geen network-fetch in runtime (ook geen CDN voor libs — alles vendored)
- AGPL-3.0 licentie waarborgt dat forks bij hosting de broncode openbaar maken

**Relatie:** versterkt door P1, P2 (geen server om dingen te lekken), in lijn met iCt Horse-merkwaarden.

## P5 — Performance via vanilla + virtual scroll

**Stelling:** Geen UI-framework. Renderer doet virtual scrolling van rij-DOM-nodes.

**Waarom:** Schaal-doel is 100k+ rijen. Frameworks (React/Vue/Svelte) hebben reconciliation-overhead die voor grote tabellen contraproductief is. Vanilla + virtual scroll geeft directe controle over DOM-recycling.

**Implicaties:**
- State-management handmatig (pub/sub pattern of simpele observer)
- Render-loop expliciet (geen automatische reactivity)
- Cell-DOM-pool met recycling bij scroll
- Vaste rij-hoogte (geen variabele height in MVP)

**Relatie:** vereist door schaal-doel, mogelijk gemaakt door P2 (single-file → frameworks zouden bundle-grootte exploderen).

## P6 — Reversibele actie (undo/redo per atomic edit)

**Stelling:** Elke gebruikersmutatie produceert één Command in een onbeperkte stack. Ctrl+Z gaat één edit terug.

**Waarom:** CSV-bewerken is foutgevoelig. "Onbeperkt undo" elimineert angst voor verlies en moedigt experimenteren aan.

**Implicaties:**
- Memory-footprint: onbeperkte stack = lineair groeiend; cell-edit Commands zijn klein (oldVal+newVal), dus 100k edits ~10MB realistisch
- Redo-stack wordt geleegd bij nieuwe mutatie na undo (standaard semantiek)
- Bij CSV re-import: stack wordt gereset (nieuwe data = nieuwe historie)

**Relatie:** vereist door gebruikerservaring, mogelijk binnen geheugen-budget van moderne browsers.

## P7 — Dual-mode query (laagdrempelig + krachtig)

**Stelling:** Gebruiker kan filteren via een visuele kolom→operator→waarde dropdown (geen SQL-kennis nodig) én via een vrije SQL-textarea (volledige AlaSQL-grammatica).

**Waarom:** De tool wil zowel beginners als power-users bedienen. UI-filter is intuïtief; SQL is expressief (joins, aggregates, subqueries).

**Implicaties:**
- UI-filter compileert intern naar SQL (single source of execution = SQLEngine)
- SQL-panel kan altijd over de hele dataset, ook na UI-filter
- Beide modi muteren `viewSet` van Renderer, niet de DataStore zelf (read-only views)
- Bookmark-bare queries (later): URL-hash bevat encoded SQL

**Relatie:** scope-uitbreiding op klassieke CSV-viewers, mogelijk gemaakt door AlaSQL.

## P8 — Expliciete vastlegging

**Stelling:** Elke component, relatie, design-keuze en wijziging staat in de repo (ARCHITECTURE/DESIGN_TOKENS/PRINCIPLES/DEPENDENCIES/BUGLIST/prompts).

**Waarom:** Meta_Master *Expliciete Vastlegging Principe*. Documentatie is onderdeel van de wijziging, geen bijzaak. AI-agenten en menselijke onderhouders bouwen verder op de repo, niet op verloren conversaties.

**Implicaties:**
- Geen "later documenteren" — bij elke commit checken of docs nog kloppen
- Sessie-MD's in `prompts/` per sessie integraal

**Relatie:** dwarsverband over alle andere principes — borgt dat ze gevolgd blijven.

## Onderlinge relatie-matrix

```
P1 ──┬── ondersteunt ──► P4 (privacy)
     ├── vereist ──────► P2 (single-file)
     └── ondersteunt ──► P3 (round-trip)

P2 ──┬── vereist ──────► P5 (vanilla)
     └── beperkt ──────► bundle-grootte

P3 ──── ondersteund door ──► P4 (data blijft van gebruiker)

P5 ──── vereist door ──► schaal-doel 100k+

P6 ──── orthogonaal ──► alle andere

P7 ──── orthogonaal ──► andere; gebruikt SQLEngine als common path

P8 ──── overkoepelt ──► P1–P7 (vastlegging-meta-principe)
```

## Niet-doelen (expliciet uitgesloten)

- **Multi-user / collaboratie** — geen real-time co-edit, geen lock-mechanisme
- **Cloud-sync** — alleen lokaal; gebruiker beheert eigen backup
- **Excel/XLSX import-export** — alleen CSV (eventueel TSV)
- **Charts/graphs** — out of scope voor MVP (mogelijk later via aparte view)
- **Mobile-native** — alleen browser; mobile-web werkt, maar zonder native gestures
