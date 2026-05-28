# BUGLIST.md — CSVHorse

Bug-historie + preventieregels. Conform Meta_Master `templates/BUGLIST_TEMPLATE.md`.

## Open bugs

_Geen — repo is in skeleton-fase, geen runtime-applicatie nog._

## Opgeloste bugs

_Nog niet van toepassing._

## Terugkerende patronen (preventie)

Te vullen tijdens MVP-implementatie en daarna. Initiële verwachte categorieën gebaseerd op tech-stack:

| Patroon-ID | Categorie | Beschrijving | Preventieregel |
|------------|-----------|-------------|----------------|
| CSV-001 | Parse | CRLF/LF-verwarring bij export-roundtrip | Dialect altijd expliciet opslaan in DataStore, niet auto-detecteren bij export |
| CSV-002 | Parse | BOM in eerste cell-value | PapaParse `skipEmptyLines` + custom BOM-strip vóór parse |
| SQL-001 | Engine | AlaSQL case-sensitivity op kolomnamen met spaties | Kolomnamen escapen met `[...]` of `"..."` consequent |
| STYLE-001 | Roundtrip | `__style_*`-kolom collisie met user-data | Naming-conventie strikt valideren bij import; user-warning bij conflict |
| RENDER-001 | Virtual scroll | Scroll-jump bij rij-verwijdering | Anchor-row-ID behouden, scrollTop bijwerken na DataStore mutatie |
| UNDO-001 | History | Stack-explosie bij paste van groot blok | Paste = 1 batch-command, geen N losse cell-edits |
| STORE-001 | Autosave | localStorage quota-exceeded bij groot dataset | Quota-check vóór write, schuif uitschakelen + toast bij overflow |

## Globale referentie

Cross-repo patronen: zie `Meta_Master/BUGS_GLOBAL.md` (CACHE/DEPLOY/ZSH/FEATURE).

## Protocol bij bugfix

1. **STOP** — geen impulsfix
2. **Kleur bepalen** — groen (fysiek) / geel (logisch) / rood (conceptueel) / loop (vastloop)
3. **RCA 3 niveaus** — functioneel / technisch / architectonisch
4. **WhatIf** — plan + impact + akkoord vóór fix
5. Bij rood: security-review meenemen
6. Na fix: deze BUGLIST.md updaten + versie-bump + RCA-notitie in commit-message
