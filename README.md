# BibForge

![Version](https://img.shields.io/github/v/tag/davidbieri/bibforge?label=version&style=flat-square&color=c8973a)
![No dependencies](https://img.shields.io/badge/dependencies-none-5a9a6e?style=flat-square)
![Deploy](https://img.shields.io/badge/deploy-GitHub%20Pages-5b8fa8?style=flat-square)

A single-file, client-side academic reference manager for large BibTeX libraries, deployable on GitHub Pages with no build step and no backend.

Built for academic collections with disciplinary emphasis on urban planning, spatial and regional economics, monetary history, and political economy. Handles libraries of 10,000+ entries without performance degradation.

**Current version: v1.2** · [Changelog](CHANGELOG.md)

---

## Quick Start

**First time:**
1. Open the app and drop your `.bib` file onto the landing screen, or click **Import .bib**
2. The library loads in a few seconds — a 12,000-entry file takes 2–4 seconds
3. Use the search bar or sidebar filters to find entries; click any row to open its detail

**Adding a new entry:**
- Press `Ctrl/Cmd+D` → paste a DOI → click **Fetch** → **Add to Library** *(fastest path)*
- Or click **New** in the header to open the full edit form with all fetch options

**Importing from CSV:**
1. Click **Import .csv** in the header
2. A column mapping dialog opens — each CSV column is auto-matched to a BibTeX field
3. Review and adjust mappings; a sample value from row 1 is shown for each column
4. Click **Import** — entries are stamped `datasource = {csv-import}` and added to the library

**Fetching metadata for a book without a DOI:**
1. Click **New**, then switch to the **ISBN / Open Library** tab in the edit form
2. Enter the ISBN and click **Open Library ↗** — Google Books is tried automatically if the result is sparse
3. If neither API has it, click **WorldCat ↗** to search directly in a new tab

**Adding an archival / primary source entry:**
1. Click **New**, select **archival** as the entry type — the form reveals the Archival Source section automatically and switches to the **Archival Search** fetch tab
2. Enter keywords in the search bar and click **Internet Archive ↗** — click any result card to apply fields
3. Fill in `repository`, `collection`, `box`, `folder`, and `access` status manually as needed
4. Use the **ArchiveGrid ↗** and **SNAC ↗** buttons to look up finding aids and name authorities
5. Save — the entry is stamped `datasource = {archive-visit}` and the Archive Access filter appears in the sidebar

**Generating a Chicago citation:**
- Select any entry in the library
- Click **Chicago NB Citation** in the detail panel (below Raw BibTeX) to expand the pane
- Click **Copy citation** — plain-text Chicago 17th ed. bibliography format, ready to paste

**Classifying an entry:**
- Scroll to the **Classification** section of the edit form
- Click **Fetch JEL** — the app suggests the top 3 codes ranked by relevance; click chips to accept
- For books, click **Suggest LOC** — picks the best Library of Congress subclass from title and keywords

**Library analytics:**
- Click **Insights** in the toolbar to open the six-panel analytics dashboard
- Tabs: Overview · Citation Age · Authors · Venues · JEL Coverage · Quality

**Saving your work:**
- Click **Export .bib** in the header at any point — the file includes all edits and custom fields
- The app holds everything in memory; nothing is saved automatically between sessions

**Theme:** click the sun/moon icon in the top-right corner to switch between dark and light modes. The preference is remembered in your browser.

---

## Deployment

1. Rename `bibforge.html` to `bibforge.html` (or `index.html` if deploying without a separate landing page)
2. Optionally rename `landing_v1.html` to `index.html` as the repo landing page
3. Push to any GitHub repository
4. Enable GitHub Pages: **Settings → Pages → Source: main branch, / (root)**
5. Access at `https://yourusername.github.io/your-repo/`

No dependencies to install. No build process. No server required.

**Recommended repo structure:**

```
your-repo/
├── index.html           ← landing_v1.html renamed (optional)
├── bibforge.html        ← the app
├── data/
│   └── library.bib      ← auto-loaded on startup
├── README.md
└── CHANGELOG.md
```

---

## Features

### Importing and Exporting

| Action | How |
|---|---|
| Import `.bib` | Click **Import .bib** in the header, or drag and drop onto the landing screen |
| Import `.csv` | Click **Import .csv** → header mapping dialog → **Import** |
| Export `.bib` | Click **Export .bib** — exports the full library with all edits and custom fields |
| Start fresh | Click **Start with empty library** to begin without a file |

All standard BibTeX entry types are supported: `@article`, `@book`, `@inbook`, `@incollection`, `@inproceedings`, `@techreport`, `@phdthesis`, `@misc`, `@unpublished`, working papers, and the custom `@archival` type for primary sources.

> **Note:** Changes are held in memory only. Export before closing the browser tab.

---

### CSV Import

The CSV import accepts any file with a header row. Column names are auto-detected against common BibTeX fields:

- Recognizes variations: `Authors`, `Publication Year`, `ISBN-13`, `Issue Number`, `Book Title`, etc.
- Shows a sample value from row 1 for each column to guide manual mapping
- Unmapped columns are skipped
- `type` column values are normalized (`Journal Article` → `article`, `Book Chapter` → `inbook`, etc.)
- Duplicate citation keys are auto-resolved with letter suffixes (`smithb`, `smithc`)
- All imported entries receive `datasource = {csv-import}` for tracking

---

### Search and Filtering

The left sidebar and header search bar work together to narrow the entry list.

**Search** (`Ctrl/Cmd+F` to focus) covers title, author, citation key, DOI, JEL codes, keywords, and data source simultaneously. Results update within 200 ms.

**Sidebar filters:**

| Filter | Behavior |
|---|---|
| Entry type | Click to show only that type; live counts update as other filters change |
| Year range | From / To inputs; either or both can be set. For `@archival` entries with a `date_range` field, uses overlap logic (e.g. `1923--1947` matches any window touching that span) |
| JEL code | Dropdown of codes present in your library |
| LOC class | Dropdown of classes present in your library |
| Data source | Filter by CrossRef, Open Library, Google Books, CSV Import, Archive Visit, or Manual |
| Archive Access | Filter by `open`, `digitized`, `by-appointment`, or `restricted` — only visible when archival entries are loaded |
| Sort | Year ↓↑, Author A–Z / Z–A, Title A–Z, Citation key A–Z |

---

### Library Insights

Click **Insights** in the toolbar to open the six-panel analytics dashboard. All analytics run on the in-memory library — no data is sent anywhere.

| Panel | Content |
|---|---|
| **Overview** | Total entries, JEL/LOC classification rates, recent (≤5yr) percentage, datasource breakdown |
| **Citation Age** | Publication year histogram (up to 40 buckets), median age, average age, decade breakdown |
| **Authors** | Top 20 authors by entry count, multi-author rate, unique author count |
| **Venues** | Top 20 journals/booktitles by frequency, venue coverage rate |
| **JEL Coverage** | Top codes with descriptions, category letter roll-up (A–Z), unique codes used |
| **Quality** | Average metadata completeness, distribution across quality tiers, per-field fill rate |

---

### Adding and Editing Entries

#### Quick Add via DOI — `Ctrl/Cmd+D`

The fastest path for any entry with a DOI. Press `Ctrl/Cmd+D`, paste a DOI or full `https://doi.org/…` URL, click **Fetch**, review the preview, and click **Add to Library**. Populates title, authors, year, journal, volume, issue, pages, publisher, DOI, ISBN, ISSN, URL, and abstract.

#### Full Edit Form

Click **New** in the header, or select an entry and click **Edit** in the detail panel.

**Tab 1 — DOI / CrossRef:** For journal articles and recent books.

**Tab 2 — ISBN / Open Library:** For books and edited volumes.
- **Open Library ↗** — queries the Open Library API; falls back to Google Books if sparse
- **Google Books ↗** — queries Google Books API directly
- **WorldCat ↗** — opens a WorldCat search in a new tab

**Tab 3 — Title Search:** When you have no ISBN or DOI.

**Tab 4 — Archival Search:** For primary sources and digitized collections.
- **Internet Archive ↗** — searches the IA Advanced Search API (no key required); returns up to 8 result cards with title, creator, date, and mediatype. Click any card to apply fields to the form.
- **DPLA ↗** — opens the Digital Public Library of America in a new tab
- **Europeana ↗** — opens Europeana's aggregated European cultural heritage search
- **ArchiveGrid ↗** / **SNAC ↗** — in the Archival Source form section, search OCLC's finding-aid aggregator and the Social Networks and Archival Context name authority

The Archival Search tab is selected automatically when the entry type is set to `archival`.

---

### Metadata Provenance — `datasource` Field

Every entry fetched via an API or imported from CSV is stamped with a `datasource` field:

| Value | Source |
|---|---|
| `crossref` | CrossRef API (DOI lookup) |
| `openlibrary` | Open Library |
| `googlebooks` | Google Books (direct call or fallback) |
| `csv-import` | CSV file import |
| `archive-visit` | Internet Archive fetch or manually recorded archival visit |
| `manual` | Hand-entered or imported without a fetch |

---

### Classification

#### JEL Codes (Journal of Economic Literature)

Click **Fetch JEL** in the Classification section of the edit form. The app fetches the full AEA EconLit classification tree on first use (cached for the session) and runs a keyword-scoring engine against title, abstract, and keywords. Top 3 codes are shown as ranked chips with matched terms and their full descriptions.

**Output format:**
```bibtex
jel          = {R11; R31; H72},
```

#### Library of Congress Classification

Click **Suggest LOC** (active for book-type entries only). Up to 3 candidate classes appear as clickable chips. The engine uses a keyword table weighted toward economics, social science, and urban planning subclasses.

**Output format:**
```bibtex
lcc          = {HD},
```

---

### Archival Research

#### @archival Entry Type

Select `archival` from the entry type dropdown to activate a dedicated Archival Source form section with the following fields:

| Field | Description |
|---|---|
| `repository` | Holding institution (e.g. *National Archives, NARA*) |
| `collection` | Named collection or record group |
| `box` | Box number within the collection |
| `folder` | Folder number within the box |
| `item` | Individual document description |
| `call_number` | Archive's own finding-aid identifier |
| `date_range` | Date span of the material, e.g. `1923--1947` |
| `access` | One of: `open`, `digitized`, `restricted`, `by-appointment` |
| `finding_aid_url` | URL to the EAD/HTML finding aid |
| `visit_date` | Date of your research visit |
| `access_note` | Permission notes or access conditions |

All fields are written to the exported `.bib` file as standard BibTeX custom fields and survive round-trips through Zotero and JabRef.

**In the entry list,** archival entries show a colour-coded access badge (`open` / `digitized` / `restricted` / `by-appointment`) and display `date_range` in place of a single year. The **Archive Access** sidebar filter appears automatically when archival entries are present in the library.

**Year filtering** uses overlap logic for `date_range`: a filter of 1930–1950 will match an entry with `date_range = {1923--1947}` because the ranges intersect.

#### Archival Search Tab

The **Archival Search** tab in the edit form (Tab 4) provides:

- **Internet Archive** — live search of the IA Advanced Search API. Returns up to 8 result cards with title, creator, date, mediatype, and subject. Clicking a card applies `title`, `author`, `year`, `url`, `publisher`, and `keywords` to the form; for archival entries it also populates `collection` and `finding_aid_url` and sets `datasource` to `archive-visit`.
- **DPLA ↗** — link-out to the Digital Public Library of America
- **Europeana ↗** — link-out to Europeana's pan-European cultural heritage search

In the **Archival Source** form section:
- **ArchiveGrid ↗** — searches OCLC ArchiveGrid using repository + collection + title
- **SNAC ↗** — searches Social Networks and Archival Context for corporate/personal name authorities

Both ArchiveGrid and SNAC link-outs also appear in the **detail read view** under the Archival Location block.

---

### Chicago Notes-Bibliography Citations

A collapsible **Chicago NB Citation** pane in the detail view (below Raw BibTeX) renders a formatted Chicago 17th edition bibliography entry for any selected entry. Click **Copy citation** for plain-text output.

Coverage by entry type:

| Type | Format |
|---|---|
| `@archival` | Creator (if any). "Item description." Date range. Collection, Box X, Folder Y. Repository. Finding aid URL. |
| `@article` | Author. "Title." *Journal* vol, no. N (year): pages. DOI. |
| `@book` / `@inbook` | Author. *Title*. Edited by … Nth ed. Place: Publisher, year. |
| `@incollection` / `@inproceedings` | Author. "Chapter title." In *Booktitle*, edited by …, pages. Place: Publisher, year. |
| `@phdthesis` | Author. "Title." PhD diss., Institution, year. |
| `@techreport` / `@working` | Author. "Title." Institution, year. No. N. |

Author formatting follows Chicago bibliography style: first author inverted (Last, First), subsequent authors in natural order, Oxford comma for three or more, `ed.`/`eds.` suffix for editor-only entries.

---

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl/Cmd+D` | Open Quick Add via DOI modal |
| `Ctrl/Cmd+F` | Focus search bar |
| `Ctrl/Cmd+S` | Save entry (while editing) |
| `↑` / `↓` | Navigate the entry list |
| `Escape` | Cancel edit / close modal |

---

### Citation Key Convention

BibForge uses the format `author:year:keyword` throughout, matching the Better BibTeX plugin pattern:

```
[auth:lower]:[year]:[title:lower:clean:truncate=20]
```

Examples: `muth:1969:cities`, `alonso:1964:location`, `keynes:1936:employment`

When a fetch or CSV import generates a key that already exists in the library, a letter suffix is appended automatically (`smithb`, `smithc`, etc.).

---

### Compatibility with Zotero and JabRef

The exported `.bib` file is standard BibTeX and imports directly into:

- **Zotero** — File → Import → BibTeX
- **JabRef** — File → Import → BibTeX
- **LaTeX** — `\bibliography{library_export}` and `\cite{key}`

The custom fields (`jel`, `lcc`, `datasource`) are preserved through round-trips in both Zotero and JabRef.

---

## Auto-Loading a Library from the Repository

Configure `BIBFORGE_CONFIG.autoLoad` to a repo-relative path and the library fetches automatically on startup:

```javascript
const BIBFORGE_CONFIG = {
  autoLoad: '/data/library.bib',   // path relative to repo root
  autoLoadLabel: 'library.bib',    // display name shown in the UI
};
```

Set `autoLoad: null` to disable. On startup BibForge issues a `fetch()` to the configured path. Because the `.bib` file is served from the same GitHub Pages origin as the app, the request succeeds without CORS restrictions or authentication.

### Recommended editing cycle with auto-load

1. Open BibForge — library loads automatically from the repo
2. Browse, add entries, fetch metadata, classify, run Insights
3. Click **Export .bib** — save the file back to `data/library.bib` locally
4. Commit and push — the updated library is live on the next page load

---

## Analytics

BibForge uses [GoatCounter](https://www.goatcounter.com) for privacy-friendly page analytics on the landing page and app. GoatCounter collects no personal data, sets no cookies, and does not track individuals across sites. Only page views and referrers are recorded. Public stats are available at [bibforge.goatcounter.com](https://bibforge.goatcounter.com).

---

## Technical Notes

### Performance

The entry list uses virtual scrolling — only the rows currently visible in the viewport are rendered as DOM elements, regardless of library size. Row height is fixed at 76px.

### Privacy and Outbound Requests

All processing happens in the browser. No library data is ever transmitted. The only outbound requests are:

| Endpoint | When | What is sent |
|---|---|---|
| `api.crossref.org` | DOI lookup | The DOI only |
| `openlibrary.org` | ISBN or title lookup | The ISBN or search query |
| `googleapis.com/books` | Google Books lookup or fallback | The ISBN or search query |
| `archive.org/advancedsearch.php` | Archival Search tab | Search query only |
| `www.aeaweb.org/econlit/classificationTree.xml` | First Fetch JEL click | Nothing (GET request) |
| `gc.zgo.at/count.js` | Page load | Page URL and referrer (no cookies) |
| `fonts.googleapis.com` | App startup | Nothing (GET request) |
| Same-repo `.bib` path | Auto-load on startup | Nothing (same-origin GET) |

WorldCat, ArchiveGrid, SNAC, DPLA, and Europeana open in a new browser tab — no data passes through the app.

### Browser Storage

Theme preference (light/dark) is saved to `localStorage`. All library data lives in memory only for the duration of the session — nothing is written to disk automatically.

---

## Version Control

Tag each release before pushing:

```bash
# Tag v1.2
git tag -a v1.2 -m "BibForge v1.2 — Archival Search tab, Internet Archive fetch, Chicago NB citations"
git push origin v1.2
```

Create a GitHub Release from the tag with the relevant CHANGELOG section as the description.

---

## Developed for

David Bieri · Virginia Tech SPIA  
Academic library inventory workflow — ~550 volume collection spanning urban planning, spatial/regional economics, monetary history, political economy, and social theory.

*Paired with `library_inventory.bib`, `library_zotero.csv`, and `inventory_workflow.md`.*
