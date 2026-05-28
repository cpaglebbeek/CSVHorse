# STATUS.md — CSVHorse

## Huidige fase

**v0.0.2-Arabian** — werkende CSV import + render (geen edit/SQL/opmaak nog).

| Aspect | Status |
|--------|--------|
| Repo lokaal | ✓ aangemaakt |
| GitHub remote | ✓ public `cpaglebbeek/CSVHorse` |
| Skeleton-bestanden | ✓ alle docs compleet (v0.0.1) |
| Architectuur-viewer (`architectuur/CSVHorse_viewer.html`) | ✓ 8 ArchiMate-views, deterministisch |
| **CSV-import + render (`index.html`)** | ✓ **werkend** (v0.0.2-Arabian) |
| **PapaParse vendored** | ✓ v5.4.1 in `vendor/`, SHA256 vastgepind |
| Cell-edit + undo/redo | ⏸ v0.0.3-Andalusian (volgende stap) |
| UI-filter | ⏸ v0.1.0-Lipizzaner |
| SQL-panel + AlaSQL | ⏸ v0.1.1-Akhal-Teke |
| Opmaak (bold/italic/kleur/...) | ⏸ v0.1.2-Appaloosa |
| Autosave + zoek/vervang | ⏸ v0.2.0-Mustang |
| Virtual scrolling 100k+ | ⏸ v0.3.0-Haflinger |
| Export met `__style_*` roundtrip | ⏸ v0.4.0-Shire |
| Deployment `icthorse.nl/CSVHorse/` | ⏸ v0.5.0-Hanoverian |
| `/sanitycheck` op skeleton | ✓ uitgevoerd 2026-05-28 |

## Volgende milestones

| Versie | Codenaam | Scope |
|--------|----------|-------|
| v0.0.1 | Friesian | Repo-skeleton (huidig) |
| v0.0.2 | Arabian | Werkende CSV-import + render (zonder edit, zonder SQL) |
| v0.0.3 | Andalusian | + Cell-edit + undo/redo |
| v0.1.0 | Lipizzaner | + UI-filter (dropdown query) |
| v0.1.1 | Akhal-Teke | + SQL-panel (AlaSQL) |
| v0.1.2 | Appaloosa | + Opmaak-toolbar (bold/italic/etc) |
| v0.2.0 | Mustang | + Autosave + Zoek/vervang |
| v0.3.0 | Haflinger | + Virtual scrolling voor 100k+ |
| v0.4.0 | Shire | + Export-dialog met `__style_*` roundtrip toggle |
| v0.5.0 | Hanoverian | Deploy `icthorse.nl/CSVHorse/` + handmatige QA |
| v1.0.0 | Lusitano | Public stable release |

## Wijzigingslog

| Datum | Versie | Wijziging |
|-------|--------|----------|
| 2026-05-28 | 0.0.1-Friesian | Repo-skeleton aangemaakt — newp protocol |
| 2026-05-28 | 0.0.1-Friesian | ArchiMate-architectuurviewer toegevoegd (`architectuur/CSVHorse_viewer.html`) |
| 2026-05-28 | 0.0.2-Arabian | Werkende CSV import + render: PapaParse 5.4.1 vendored, DataStore + IO + Renderer modules, drag-drop, dialect-auto-detect, sticky header, comfortabel tot ~5k rijen |
