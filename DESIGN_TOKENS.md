# DESIGN_TOKENS.md — CSVHorse

UI design-tokens. Alle visuele waarden expliciet vastgelegd voor consistentie en wijzigingsimpact-tracking.

## Kleuren (dark default)

| Token | Waarde | Toepassing | Rationale |
|-------|--------|-----------|-----------|
| `--color-bg` | `#0f1115` | Page background | Diep neutraal, oogvriendelijk voor lange CSV-sessies |
| `--color-surface` | `#161a22` | Toolbar, panel, header, footer | Subtiele tint boven bg |
| `--color-border` | `#2a3140` | Cell borders, panel borders, hr | Genoeg contrast, niet hard |
| `--color-text` | `#e6e9ef` | Hoofdtekst, cell-content | Off-white, geen pure-wit (oogcomfort) |
| `--color-text-dim` | `#9aa3b2` | Secundaire tekst, badges, footer | Hierarchy via opacity-equivalent |
| `--color-accent` | `#59c5a8` | Brand-accent (iCt Horse teal), confirm-knoppen | Consistent met SeaMenu (zie iCt_Horse) |
| `--color-accent-2` | `#4ab3ff` | Links, secundaire actie | Onderscheid van primary accent |
| `--color-danger` | `#ff6b6b` | Delete-actie, error-state, destructive confirm | Standaard rood-accent |
| `--color-selection` | `#264f3a` | Cell-selectie achtergrond | Doorzichtig genoeg om text te lezen |
| `--color-edit-active` | `#3a4a2a` | Cell-in-edit-mode | Visueel onderscheid van geselecteerd |
| `--color-search-hit` | `#f0c674` | Zoek-vervang highlight | Amber, contrastend op donkere bg |

## Typografie

| Token | Waarde |
|-------|--------|
| `--font-ui` | `system-ui, -apple-system, "Segoe UI", Roboto, sans-serif` |
| `--font-mono` | `ui-monospace, "JetBrains Mono", Menlo, Consolas, monospace` (cell-content + SQL panel) |
| `--font-size-base` | `14px` |
| `--font-size-small` | `12px` (badges, footer) |
| `--font-size-large` | `16px` (titels) |
| `--font-size-cell-min` | `10px` (uitgebreide opmaak per-cel min) |
| `--font-size-cell-max` | `24px` (per-cel max) |
| `--line-height-ui` | `1.4` |
| `--line-height-cell` | `1.2` (denser voor tabel) |

## Spacing

| Token | Waarde | Gebruik |
|-------|--------|---------|
| `--space-1` | `4px` | Inline gap |
| `--space-2` | `8px` | Cell-padding-y |
| `--space-3` | `12px` | Cell-padding-x |
| `--space-4` | `16px` | Section gap |
| `--space-5` | `24px` | Panel padding |

## Cell-dimensies

| Token | Waarde |
|-------|--------|
| `--cell-min-width` | `60px` |
| `--cell-default-width` | `120px` |
| `--cell-max-width` | `400px` |
| `--cell-row-height` | `28px` (vast voor virtual scroll) |

## Borders & Radius

| Token | Waarde |
|-------|--------|
| `--border-thin` | `1px solid var(--color-border)` |
| `--radius-sm` | `4px` |
| `--radius-md` | `8px` |
| `--radius-pill` | `9999px` (badges) |

## Z-index hierarchy

| Token | Waarde | Element |
|-------|--------|---------|
| `--z-table` | `1` | Tabel-cellen |
| `--z-fixed-header` | `2` | Vaste header-rij |
| `--z-toolbar` | `5` | Toolbar |
| `--z-dropdown` | `10` | Filter-dropdowns |
| `--z-modal` | `100` | Export-dialog, Settings, Zoek/vervang |
| `--z-toast` | `200` | Autosave-feedback, errors |

## Iconen

Inline SVG (geen icon-font, geen externe asset — single-file constraint). Bewaard als JS-string `const icons = {...}`. Stijl: stroke-based, 1.5px, hetzelfde formaat 16×16 of 20×20.

## Light-mode (toekomstig, niet in v0.0.1)

Toggle via `prefers-color-scheme` of expliciete Settings-keuze. Token-namen blijven gelijk; waardes per scheme apart. Niet in MVP-scope.

## Toepassings-regels

- **NOOIT** hard-coded hex in CSS — altijd via `var(--color-*)`
- **NOOIT** inline font-size buiten cell-style range
- Cell-opmaak heeft hoogste specificiteit (inline style); base CSS gebruikt tokens
- Bij wijziging van een token: zoek alle gebruiken via grep, controleer visuele regressie in `index.html`-preview
