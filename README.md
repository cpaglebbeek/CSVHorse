# CSVHorse

Standalone single-file HTML CSV viewer + editor met maximale querymogelijkheden, rich-text opmaak, autosave, undo/redo en zoek/vervang. Eén `.html` openen in een moderne browser — geen server, geen build, geen account.

**Versie:** v0.0.1-Friesian (skeleton)
**Status:** Pre-MVP — repo-skeleton; werkende applicatie volgt.
**Licentie:** AGPL-3.0
**Ecosysteem:** iCt Horse → iCt Horse Diensten

## Kernfeatures (gepland)

| Feature | Beschrijving |
|---------|-------------|
| **CSV upload/download** | Drag-and-drop of bestandskeuze; RFC 4180 + configureerbaar dialect (delimiter/quote/decimal) |
| **Dual-mode query** | Intuïtieve dropdown-filter (kolom → operator → waarde) **én** SQL-querytekstvak (volledige AlaSQL grammatica) |
| **Rich-text opmaak per cel** | Bold, italic, underline, strikethrough, kleur, achtergrond, font-size, alignment — opgeslagen in verborgen `__style_*` systeemkolommen |
| **CSV-roundtrip met opmaak** | Export met systeemkolommen (default AAN, toggle in export-dialog); herimport herkent en herstelt opmaak |
| **Autosave** | Schuifschakelaar AAN/UIT; werkt via `localStorage`; getrottled snapshots |
| **Undo/redo** | Per cel-edit, onbeperkte stackdiepte |
| **Zoek/vervang** | Regex-optie, case-toggle, scope-keuze (alles/kolom/selectie), volgende/vorige navigatie |
| **Schaal** | Doel: 100k+ rijen met virtual scrolling |
| **Privacy** | 100% client-side; geen netwerk, geen accounts, geen tracking |

## Tech Stack

- **HTML/CSS/JS** — vanilla, geen framework
- **CSV-parser:** [PapaParse](https://www.papaparse.com/) (vendored, MIT)
- **SQL-engine:** [AlaSQL](https://github.com/AlaSQL/alasql) (vendored, MIT)
- **Storage:** Browser `localStorage`
- **Bundle:** single `.html` file (alles inline)

## Gebruik

```
1. Open CSVHorse.html in een moderne browser (Chrome/Firefox/Safari/Edge)
2. Upload een CSV-bestand of typ data in
3. Filter via dropdown of SQL
4. Wijzig cellen, pas opmaak toe
5. Download bewerkte CSV
```

## Status & Roadmap

Zie [STATUS.md](STATUS.md), [ACTIONS.md](ACTIONS.md) en [docs/PRINCIPLES.md](docs/PRINCIPLES.md).

## Bijdragen

Repo is publiek. Issues + PR's welkom volgens AGPL-3.0 voorwaarden.

## Licentie

AGPL-3.0 — zie [LICENSE](LICENSE). Bij gebruik (ook gehost) moet de volledige broncode beschikbaar zijn.

## Contact

iCt Horse — info@icthorse.nl — https://icthorse.nl
