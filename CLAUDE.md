# CLAUDE.md — CSVHorse

Project-specifieke regels voor agenten (Claude, Gemini, Codex) bij werk aan CSVHorse.

## Identiteit

| Attribute | Waarde |
|-----------|--------|
| Project | CSVHorse |
| Ecosysteem | iCt Horse → iCt Horse Diensten |
| Sub-master | `Meta_iCt_Horse_Diensten` |
| Lokaal | `/Users/christian/Documents/Gemini_Projects/CSVHorse` |
| GitHub | `cpaglebbeek/CSVHorse` (public) |
| Branch | `main` |
| Licentie | AGPL-3.0 |
| Hosting (gepland) | `https://icthorse.nl/CSVHorse/` (Hostinger static) |
| Lokaal (altijd) | `file://` — single-file HTML, geen server vereist |

## Tech Stack

- Single-file `index.html` (alles inline: HTML + CSS + JS)
- **Vanilla JS** — geen framework
- **PapaParse** (vendored, MIT) — CSV-parser
- **AlaSQL** (vendored, MIT) — SQL-engine over in-memory tabel
- **localStorage** — autosave-storage
- Vendored libs worden inline opgenomen in `index.html` of als sibling JS-bestand bij final build

## Versioning Mandate

Elke functionele/technische wijziging MOET de versie verhogen in `version.json` vóór commit of build.

### Color-coded versiebump (Meta_Master CLAUDE.md feature/bugfix protocol)

**Nieuwe feature:**
- **Groen** — minor (geen design/arch impact) → `+0.0.1`
- **Oranje** — design impact, logische architectuur stabiel → `+0.1.0`
- **Rood** — major (redesign, meta-implicaties) → `+1.0.0`

**Bugfix:**
- **Groen** — fysiek niveau, snel herstel
- **Geel** — logische architectuur
- **Rood** — conceptueel redesign + security review
- **Loop** — debug-loop, compleet nieuwe invalshoek

**Root Cause Analysis verplicht bij elke bugfix** op drie niveaus: Functioneel / Technisch / Architectonisch.

## Codenamen — Paardenrassen

Elke release krijgt een codenaam uit het paardenrassen-thema. Volgorde-suggestie (iconisch eerst):

| Versie | Codenaam |
|--------|----------|
| v0.0.1 | **Friesian** ← huidig |
| v0.0.2 | Arabian |
| v0.0.3 | Andalusian |
| v0.1.0 | Lipizzaner |
| v0.1.1 | Akhal-Teke |
| v0.1.2 | Appaloosa |
| v0.2.0 | Mustang |
| v0.3.0 | Haflinger |
| v0.4.0 | Shire |
| v0.5.0 | Hanoverian |
| v1.0.0 | Lusitano |

Lijst is verlengbaar — andere rassen welkom (Tinker, Quarter Horse, Thoroughbred, Connemara, Welsh Pony, Trakehner, …).

## WhatIf Protocol (verplicht, altijd)

Vóór code/bestanden wijzigen of builds starten: terugkoppelen **begrip → plan → impact → akkoord**. Zie `Meta_Master/CLAUDE.md` §WhatIf Protocol.

## Build & Deploy

- **Lokaal:** geen build — open `index.html` direct in browser
- **Deploy `icthorse.nl/CSVHorse/`:** `rsync` naar Hostinger + LiteSpeed cache purge (zie `feedback_icthorse_deploy.md`)
- **Geen APK / mobile / iOS** — out of scope

## Documentatie-verplichting

Elke wijziging update mee:
- `version.json` (versie + codenaam)
- `STATUS.md` (huidige fase)
- `ACTIONS.md` (gedaan/openstaand)
- `ARCHITECTURE.md` (bij componentwijziging)
- `DESIGN_TOKENS.md` (bij visuele wijziging)
- `docs/BUGLIST.md` (bij bugfix)
- `prompts/YYYY-MM-DD_<slug>.md` (per sessie, integraal)

## Privacy & Security

- **100% client-side** — geen netwerk-calls, geen tracking, geen accounts
- **Data blijft in de browser** — autosave alleen in localStorage van de gebruiker
- **Geen telemetrie** — ook geen anonieme

## Browser-support

Modern only — Chrome/Firefox/Safari/Edge laatste 2 versies. Geen IE / oude mobile.

## Verwante projecten

- `iCt_Horse` — moedersite icthorse.nl
- `HorseSafe` — andere iCt Horse Diensten SaaS (zero-knowledge vault)
- `VeiligDelen` — andere iCt Horse Diensten (E2E file-sharing)

## Agenten-coördinatie

Multi-session safety zoals in `Meta_Master/MULTI_SESSION_SAFETY.md`. Bij sessiestart altijd `git pull` + `git status` + recente commits scannen.
