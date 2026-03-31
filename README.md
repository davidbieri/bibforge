# BibForge

A single-file, client-side academic reference manager for large BibTeX libraries, deployable on GitHub Pages with no build step and no backend.

Built for academic collections with disciplinary emphasis on urban planning, spatial and regional economics, monetary history, and political economy. Handles libraries of 10,000+ entries without performance degradation.

---

## Quick Start

**First time:**
1. Open the app and drop your `.bib` file onto the landing screen, or click **Import .bib**
2. The library loads in a few seconds — a 12,000-entry file takes 2–4 seconds
3. Use the search bar or sidebar filters to find entries; click any row to open its detail

**Adding a new entry:**
- Press `Ctrl/Cmd+D` → paste a DOI → click **Fetch** → **Add to Library** *(fastest path)*
- Or click **New** in the header to open the full edit form with all fetch options

**Fetching metadata for a book without a DOI:**
1. Click **New**, then switch to the **ISBN / Open Library** tab in the edit form
2. Enter the ISBN and click **Open Library ↗** — Google Books is tried automatically if the result is sparse
3. If neither API has it, click **WorldCat ↗** to search directly in a new tab

**Classifying an entry:**
- Scroll to the **Classification** section of the edit form
- Click **Fetch JEL** — the app suggests the top 3 codes ranked by relevance; click chips to accept
- For books, click **Suggest LOC** — picks the best Library of Congress subclass from title and keywords

**Saving your work:**
- Click **Export .bib** in the header at any point — the file includes all edits and custom fields
- The app holds everything in memory; nothing is saved automatically between sessions

**Theme:** click the sun/moon icon in the top-right corner to switch between dark and light modes. The preference is remembered in your browser.

---

## Deployment

1. Rename `bibforge.html` to `index.html`
2. Push to any GitHub repository
3. Enable GitHub Pages: **Settings → Pages → Source: main branch, / (root)**
4. Access at `https://yourusername.github.io/your-repo/`

No dependencies to install. No build process. No server required. The file is self-contained — all fonts load from Google Fonts; all classification data is embedded.

---

## Features

### Importing and Exporting

| Action | How |
|---|---|
| Import `.bib` | Click **Import .bib** in the header, or drag and drop onto the landing screen |
| Export `.bib` | Click **Export .bib** — exports the full library with all edits and custom fields |
| Start fresh | Click **Start with empty library** to begin without a file |

All standard BibTeX entry types are supported: `@article`, `@book`, `@inbook`, `@incollection`, `@inproceedings`, `@techreport`, `@phdthesis`, `@misc`, `@unpublished`, and working papers.

> **Note:** Changes are held in memory only. Export before closing the browser tab.

---

### Search and Filtering

The left sidebar and header search bar work together to narrow the entry list.

**Search** (`Ctrl/Cmd+F` to focus) covers title, author, citation key, DOI, JEL codes, keywords, and data source simultaneously. Results update within 200 ms.

**Sidebar filters:**

| Filter | Behavior |
|---|---|
| Entry type | Click to show only that type; live counts update as other filters change |
| Year range | From / To inputs; either or both can be set |
| JEL code | Dropdown of codes present in your library |
| LOC class | Dropdown of classes present in your library |
| Data source | Filter by CrossRef, Open Library, Google Books, or Manual |
| Sort | Year ↓↑, Author A–Z / Z–A, Title A–Z, Citation key A–Z |

---

### Adding and Editing Entries

#### Quick Add via DOI — `Ctrl/Cmd+D`

The fastest path for any entry with a DOI. Press `Ctrl/Cmd+D`, paste a DOI or full `https://doi.org/…` URL, click **Fetch**, review the preview, and click **Add to Library**. Populates title, authors, year, journal, volume, issue, pages, publisher, DOI, ISBN, ISSN, URL, and abstract.

#### Full Edit Form

Click **New** in the header, or select an entry and click **Edit** in the detail panel. The form opens in the right panel with three metadata fetch tabs at the top:

**Tab 1 — DOI / CrossRef**
For journal articles and recent books. Paste a DOI and click **CrossRef ↗**.

**Tab 2 — ISBN / Open Library**
For books, edited volumes, and pre-ISBN monographs where you have an ISBN.

- **Open Library ↗** — queries the [Open Library API](https://openlibrary.org/developers/api), which aggregates WorldCat data. Returns title, authors, year, publisher, city, ISBN, LC classification, subjects, pages, and edition. Automatically falls back to Google Books if the result is missing title or author.
- **Google Books ↗** — queries the Google Books API directly, as a parallel or manual fallback.
- **WorldCat ↗** — opens a WorldCat search in a new tab, pre-filled with the ISBN or title/author from the form. Use this for volumes not found by either API; copy the BibTeX from WorldCat and paste it manually.

**Tab 3 — Title Search**
When you have no ISBN or DOI. Enter title keywords and optionally an author surname.

- **Open Library ↗** — if a matching record has an ISBN, the full ISBN lookup runs automatically to retrieve richer metadata
- **Google Books ↗** — parallel title/author search against Google's index
- **WorldCat ↗** — same link-out, pre-filled from the title and author fields

#### From existing entries

Select any entry and click **Edit** in the detail panel to modify it in place.

---

### Metadata Provenance — `datasource` Field

Every entry fetched via an API is stamped with a `datasource` field identifying where the metadata came from:

| Value | Source |
|---|---|
| `crossref` | CrossRef API (DOI lookup) |
| `openlibrary` | Open Library |
| `googlebooks` | Google Books (direct call or fallback) |
| `manual` | Hand-entered or imported without a fetch |

The badge is shown in the detail panel. The field is written to the exported `.bib` file:

```bibtex
datasource   = {openlibrary},
```

This lets you filter the sidebar by source to audit which entries still need review, and tracks provenance through the full workflow from initial import to LaTeX manuscript.

---

### Classification

#### JEL Codes (Journal of Economic Literature)

The **Fetch JEL** button is in the Classification section of the edit form.

1. Fill in at minimum the entry's title; abstract and keywords improve accuracy
2. Click **Fetch JEL** — on first use the full AEA EconLit classification tree is fetched from `https://www.aeaweb.org/econlit/classificationTree.xml` and cached for the session
3. A keyword-scoring engine matches the entry text against all code descriptions, with a specificity bonus for 3-character leaf codes over 2-character group codes
4. The top 3 codes are shown as ranked chips with their full descriptions and the matched terms that drove the suggestion
5. Click any chip to add it to the entry's JEL field; chips turn green when accepted

If the AEA endpoint is blocked by CORS, the app falls back silently to its built-in table of 200+ codes. A status note indicates which source was used.

JEL codes can also be searched and selected manually using the searchable picker below the suggestion strip. Click a selected tag to remove it.

**Output format:**

```bibtex
jel          = {R11; R31; H72},
```

#### Library of Congress Classification

The **Suggest LOC** button is active only when the entry type is a book variant (`@book`, `@inbook`, `@incollection`, `@proceedings`). It is disabled for articles and other types.

1. Set entry type to a book variant
2. Fill in title; keywords and abstract improve accuracy
3. Click **Suggest LOC** — up to 3 candidate classes appear as clickable chips
4. Click a chip to apply it to the dropdown; the dropdown can also be set manually

The engine uses an extended keyword table weighted toward economics, social science, and urban planning subclasses: HB (economic theory), HC (economic history), HD (industries, land, labor), HG (finance), HJ (public finance), HT (communities and urban), HX (socialism/political economy), NA (architecture), JC (political theory), JS (local government), and others.

**Output format:**

```bibtex
lcc          = {HD},
```

---

### Entry Detail Panel

Selecting an entry opens the detail panel on the right with the full bibliographic record: venue, volume, pages, publisher, city, edition, DOI (clickable, opens in browser), ISBN, URL, JEL codes with descriptions, LOC class, abstract, keywords, note, data source badge, and citation key.

Action buttons:

| Button | Action |
|---|---|
| **Edit** | Opens the full edit form in place |
| **Copy Key** | Copies the citation key to clipboard (for use in LaTeX `\cite{}`) |
| **Copy BibTeX** | Copies the complete formatted BibTeX entry to clipboard |
| **Delete** | Removes the entry after confirmation |

#### Raw BibTeX Pane

At the bottom of the detail panel, a collapsible **Raw BibTeX** section shows the exact output that will be written to the exported `.bib` file. Click the row to expand or collapse it.

The pane is syntax-highlighted: entry type and citation key in amber, field names in blue, values in green, braces in muted grey — matching the overall BibForge colour scheme. It updates automatically as you switch between entries, and closes when you open the edit form to avoid showing stale content mid-edit.

Use it to:
- Verify field names (`journal` vs `booktitle`, `lcc`, `datasource`) before export
- Check brace nesting and special characters in titles and author strings
- Confirm the citation key matches what's in your `.tex` file
- Spot missing fields that the formatted view may not make obvious

The pane is read-only. To edit, use the **Edit** button — the raw view refreshes automatically once you save.

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

When a fetch generates a key that already exists in the library, a letter suffix is appended automatically (`smithb`, `smithc`, etc.).

---

### Compatibility with Zotero and JabRef

The exported `.bib` file is standard BibTeX and imports directly into:

- **Zotero** — File → Import → BibTeX
- **JabRef** — File → Import → BibTeX
- **LaTeX** — `\bibliography{library_export}` and `\cite{key}`

The custom fields (`jel`, `lcc`, `datasource`) are preserved through round-trips in both Zotero and JabRef, appearing under Extra or custom field tabs. They do not interfere with standard BibTeX processing in LaTeX.

---

## Auto-Loading a Library from the Repository

BibForge can fetch a `.bib` file automatically on startup if it lives in the same GitHub repository. No manual import step, no file picker — the library opens immediately when the page loads.

### Setup

Commit your `.bib` file anywhere in the repo, then edit the config block near the top of `index.html`:

```javascript
const BIBFORGE_CONFIG = {
  autoLoad: '/bib/library_inventory.bib',   // path relative to repo root
  autoLoadLabel: 'library_inventory.bib',   // display name shown in the UI
};
```

The path must start with `/` and match the file's location in the repository exactly. Any subfolder name works — use whatever makes sense for your project structure:

```javascript
autoLoad: '/bib/library_inventory.bib'        // bib/ subfolder
autoLoad: '/data/library.bib'                 // data/ subfolder
autoLoad: '/references/main.bib'              // references/ subfolder
autoLoad: '/library_inventory.bib'            // repo root
```

Set `autoLoad: null` to disable auto-loading and always show the manual import screen instead.

### How it works

On startup BibForge issues a `fetch()` to the configured path. Because the `.bib` file is served from the same GitHub Pages origin as the app, the request succeeds without CORS restrictions or authentication. The progress bar fills as the file is fetched and parsed. If the path is wrong or the file is missing, BibForge falls back gracefully to the manual import screen and logs the specific HTTP error to the browser console.

If you ever rename the file or move it to a different subfolder, update the `autoLoad` line and push — no other changes are needed.

### Workflow with version control

With auto-load enabled, the recommended editing cycle is:

1. Open BibForge — library loads automatically from the repo
2. Browse, add entries, fetch metadata, classify
3. Click **Export .bib** or press `Ctrl/Cmd+S` — save the file back to the same local subfolder
4. Commit and push — the updated library is live on the next page load

This keeps the `.bib` file under version control. Every commit is a dated snapshot of the library state, diffs show exactly which entries changed, and the file is accessible from any browser without local software.

### File size

GitHub Pages serves files up to 100 MB. A 12,000-entry `.bib` file is typically 5–15 MB, well within bounds.

### OneDrive / Google Drive

Direct API access from a static page is not supported without OAuth, which would require a registered app ID and an authentication flow. The practical path for cloud-synced libraries is to use the **desktop sync client** (OneDrive or Google Drive for Desktop), which mirrors the repo folder to the local filesystem. BibForge opens the file via the manual import picker, and the sync client handles propagating the exported `.bib` back to the cloud automatically.

---

## Technical Notes

### Performance

The entry list uses virtual scrolling — only the rows currently visible in the viewport are rendered as DOM elements, regardless of library size. Scrolling through 12,000 entries costs the same as scrolling through 50.

The BibTeX parser is written from scratch with no external dependencies. It handles nested braces, quoted strings, `@string` and `@preamble` blocks (skipped gracefully), `#` concatenation, malformed entries (skipped without halting the parse), and multi-byte / accented Latin characters.

### Privacy and Outbound Requests

All processing happens in the browser. No library data is ever transmitted. The only outbound requests are:

| Endpoint | When | What is sent |
|---|---|---|
| `api.crossref.org` | DOI lookup | The DOI only |
| `openlibrary.org` | ISBN or title lookup | The ISBN or search query |
| `googleapis.com/books` | Google Books lookup or fallback | The ISBN or search query |
| `www.aeaweb.org/econlit/classificationTree.xml` | First Fetch JEL click | Nothing (GET request) |
| `fonts.googleapis.com` | App startup | Nothing (GET request) |
| Same-repo `.bib` path | Auto-load on startup | Nothing (same-origin GET) |

WorldCat opens in a new browser tab — no data passes through the app.

### Browser Storage

Theme preference (light/dark) is saved to `localStorage`. All library data lives in memory only for the duration of the session — nothing is written to disk automatically. Export your `.bib` file to save work.

---

## File Structure

The `.bib` file can live in any subfolder. The only requirement is that `BIBFORGE_CONFIG.autoLoad` in `index.html` matches the actual path. Three common layouts:

```
your-repo/                         your-repo/                    your-repo/
├── index.html                     ├── index.html                ├── index.html
├── bib/                           ├── data/                     ├── library_inventory.bib
│   └── library_inventory.bib      │   └── library.bib           └── README.md
└── README.md                      └── README.md
   autoLoad: '/bib/...'               autoLoad: '/data/...'         autoLoad: '/library...'
```

No `_config.yml`, Jekyll configuration, or custom domain setup required.

---

## Developed for

David Bieri · Virginia Tech SPIA  
Academic library inventory workflow — ~550 volume collection spanning urban planning, spatial/regional economics, monetary history, political economy, and social theory.

*Paired with `library_inventory.bib`, `library_zotero.csv`, and `inventory_workflow.md`.*
