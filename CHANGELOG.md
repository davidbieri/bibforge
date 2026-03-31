# BibForge — Changelog

All notable changes are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [v1.0] — March 2026

### Added
- **CSV Import with Header Mapping** — Import any `.csv` file via a modal dialog that auto-detects column → BibTeX field mappings. Normalizes common header names (`Authors`, `Publication Year`, `ISBN-13`, etc.) to the correct BibTeX fields. Shows a sample value from the first row for each column. Supports all standard BibTeX fields plus `type` and `key`. Duplicate citation keys are auto-resolved with letter suffixes. Entries stamped `datasource = {csv-import}`.
- **Library Insights Modal** — Six-panel analytics dashboard accessible from the toolbar:
  - *Overview*: total entries, JEL/LOC classification rates, datasource breakdown, recent (≤5yr) percentage
  - *Citation Age*: publication year histogram (up to 40 buckets), median and average age, decade breakdown
  - *Authors*: top 20 authors by entry count, multi-author rate, unique author count
  - *Venues*: top 20 journals/booktitles by frequency, venue coverage rate
  - *JEL Coverage*: top codes with description snippets, category letter roll-up (A–Z), unique codes used
  - *Quality*: per-entry metadata completeness score (8 fields), distribution (Excellent/Good/Partial/Sparse), per-field fill rate with color coding
- **GoatCounter Analytics** — Privacy-friendly page analytics via `gc.zgo.at/count.js`. No cookies, no personal data collection. Public stats available at `https://bibforge.goatcounter.com`.
- **Full SEO Metadata** — `<meta>` tags for description, keywords (targeting JabRef, Zotero, Mendeley, Paperpile, Citavi, EndNote, BibDesk, ReadCube Papers, and related BibTeX ecosystem searches), Open Graph, Twitter Card, and canonical URL.
- **`csv-import` datasource value** — New provenance value for CSV-imported entries, filterable in the sidebar datasource selector.
- **Version in header** — Logo subtitle now shows `· v1.0`.
- **Updated shortcuts modal** — Reflects CSV Import and Insights additions.

### Changed
- `Import .csv` button added to header (alongside existing `Import .bib`).
- `Insights` toolbar button added (between Stats and Find & Replace).
- Keyboard shortcuts console log updated to reflect new toolbar layout.
- Escape key handler now also closes `insights-modal` and `csv-mapping-modal`.

---

## [v0.9] — initial release candidate

### Added
- BibTeX parser from scratch: nested braces, `@string`/`@preamble` blocks, `#` concatenation, malformed entry graceful skip, multi-byte/accented character support.
- Virtual scrolling with fixed 76px row height — rendering cost independent of library size.
- Full-text search across title, author, citation key, DOI, JEL codes, keywords, datasource. Results within 200 ms.
- Sidebar filters: entry type (with live counts), year range (from/to), JEL code dropdown, LOC class dropdown, datasource selector, sort mode.
- DOI Quick-Add via CrossRef (`Cmd/Ctrl+D`): title, authors, year, journal, volume, issue, pages, publisher, DOI, ISBN, ISSN, URL, abstract.
- ISBN lookup via Open Library with automatic Google Books fallback; WorldCat link-out button.
- Title/author search via Open Library and Google Books.
- JEL auto-classification: fetches AEA EconLit XML tree on first use, keyword-scoring engine with specificity bonus for 3-char leaf codes, top 3 suggestions as ranked chips. Falls back to built-in 200+ code table if CORS-blocked.
- LOC subclass suggestion for book-type entries (`@book`, `@inbook`, `@incollection`, `@proceedings`).
- `datasource` provenance field (`crossref`, `openlibrary`, `googlebooks`, `manual`) written to exported `.bib`.
- Find & Replace modal: field-targeted or all-fields, case-sensitive, whole-field match options; live match preview.
- Raw BibTeX pane in detail panel: collapsible, syntax-highlighted (amber/blue/green), read-only.
- File System Access API write-back (Chrome/Edge); download `.bib` export fallback for all browsers.
- Duplicate entry from toolbar.
- Dark / light theme toggle with `localStorage` persistence.
- `BIBFORGE_CONFIG.autoLoad` — auto-fetch `.bib` from same-repo path on startup.
- Responsive layout: 3-panel desktop (≥900px), sidebar-drawer tablet (600–900px), single-panel mobile (<600px) with bottom navigation bar.
- Keyboard shortcuts: `Cmd+D`, `Cmd+F`, `Cmd+S`, `Cmd+H`, `↑↓`, `Escape`.
- Stats modal: total entries, JEL/LOC/DOI counts, by-type bar chart, by-datasource bar chart, publication year histogram.
