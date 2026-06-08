# Net Worth Tracker — Technical Requirements Document

## Architecture

**Type**: Static web app — no server, no backend, no API calls for user data.  
**Deployment**: GitHub Pages (Azqato/Net-Worth-Tracker, `main` branch). Files served directly from the repo root.  
**Data persistence**: Browser `localStorage` only, scoped to the origin and browser profile. Clearing cache removes all data.

---

## File Structure

```
/
├── index.html        # Landing page & guide (site entry point)
├── app.html          # Net Worth Tracker application
├── style.css         # Shared stylesheet for both pages
├── app.js            # All application logic (loaded by app.html only)
├── favicon.svg       # Money bag emoji favicon (SVG)
├── README.md         # Project overview
├── PatchNotes.md     # Full version history
├── Design.md         # Design system reference
├── PRD.md            # Product requirements
└── TRD.md            # This file
```

---

## Tech Stack

| Layer | Technology | Version | Delivery |
|---|---|---|---|
| Markup | HTML5 | — | Static file |
| Styling | CSS3 (custom properties, grid, flexbox) | — | Static file |
| Logic | Vanilla JavaScript (ES2020+) | — | Static file |
| Charts | Chart.js | 4.4.0 | jsDelivr CDN |
| Excel I/O | SheetJS (xlsx) | 0.20.1 | SheetJS CDN |
| Hosting | GitHub Pages | — | — |

No build step. No bundler. No framework. No Node.js runtime in production.

---

## Data Model

### Snapshot object

```json
{
  "id": "1717000000000",
  "date": "2024-06",
  "cash": 22000,
  "taxable": 150000,
  "retirement": 95000,
  "realEstate": 280000,
  "total": 547000
}
```

- `id`: `Date.now().toString()` — unique per save action; `Date.now() + Math.random()` for bulk imports
- `date`: ISO year-month string `YYYY-MM`
- `cash`, `taxable`, `retirement`, `realEstate`: plain numbers (dollars, no decimals stored)
- `total`: sum of the four categories (computed at save time, stored for convenience)

### localStorage keys

| Key | Type | Description |
|---|---|---|
| `networth_snapshots` | JSON array | All snapshots, sorted by date ascending |
| `inactivity_dismiss_count` | Integer string | How many times the user dismissed the inactivity modal (0, 1, or 2) |

---

## JavaScript Architecture

All logic lives in `app.js`, loaded at the end of `<body>` in `app.html`. No modules, no bundler.

### Initialization sequence

```
DOMContentLoaded
  └─ loadSnapshots()       — read from localStorage, parse JSON
  └─ renderAll()           — update all UI elements
  └─ startInactivityTimer() — attach event listeners, set first timer
```

### Render pipeline

`renderAll()` calls each renderer independently. All renderers are pure DOM writers — they read from the global `snapshots` array and update specific element IDs.

| Function | Updates |
|---|---|
| `renderTotalCard(latest, prev)` | `#totalNetWorth`, `#totalDate`, `#totalMoM` |
| `renderCategoryCards(latest, prev)` | `#cashValue`, `#cashPct`, `#cashMoM` (× 4 categories) |
| `renderLineChart()` | `#lineChart` canvas |
| `renderAllocationChart(latest)` | `#allocationChart` canvas |
| `renderSnapshotHistory()` | `#snapshotTable` innerHTML |

### Chart management

Chart.js instances are held in module-level variables `lineChart` and `allocationChart`. Each render call destroys the previous instance before creating a new one (prevents canvas memory leaks).

### Snapshot mutations

All write operations follow the same pattern:
1. Modify the `snapshots` array
2. Call `snapshots.sort((a, b) => new Date(a.date) - new Date(b.date))`
3. Call `persistSnapshots()` to write to localStorage
4. Call `renderAll()` to sync the UI

Saving a snapshot to an existing date overwrites that entry (upsert by `date`).

### Inactivity timer

- Triggered by: `mousemove`, `mousedown`, `keydown`, `scroll`, `touchstart`, `click`
- First warning: 30 seconds (`INACTIVITY_MS_1ST`)
- Second warning: 120 seconds after first dismissal (`INACTIVITY_MS_2ND`)
- After two dismissals: timer permanently stopped; state stored in `inactivity_dismiss_count`

### Import pipeline

```
FileReader.readAsArrayBuffer(file)
  └─ XLSX.read(buffer, { cellDates: true })
  └─ sheet_to_json(worksheet, { defval: '' })
  └─ parseDate(val)   — handles Date objects, YYYY-MM strings, M/D/YYYY strings
  └─ parseNum(val)    — strips $, commas; falls back to 0
  └─ filter rows where date === null
  └─ confirm(count) prompt
  └─ upsert each row into snapshots
  └─ sort → persist → renderAll
```

### Export pipeline

```
XLSX.utils.book_new()
  └─ aoa_to_sheet([ headers, ...rows ])  — columns: Date, Net Worth, Cash, Investments, Retirement, Real Estate
  └─ set column widths (!cols)
  └─ book_append_sheet(wb, ws, 'Net Worth Data')
  └─ XLSX.writeFile(wb, 'networth-export-YYYY-MM-DD.xlsx')
```

### Print pipeline

1. Gather latest/prev snapshot data
2. Build KPI HTML (5 cards)
3. Create an off-screen `<div>` at `left: -9999px`
4. Create a `<canvas>` (880×260px) and render a Chart.js line chart (Net Worth only, monochrome settings)
5. Capture canvas as PNG via `canvas.toDataURL()`
6. Destroy the Chart.js instance; remove the off-screen div
7. Build last-12-months history table HTML
8. Inject all generated HTML into `#printReport`
9. Call `window.print()`
10. `@media print` CSS hides `.no-print`, shows `.print-report`, forces pure black/white

---

## CSS Architecture

### Approach

Single shared stylesheet (`style.css`) loaded by both pages via `<link>`. The landing page adds an inline `<style>` block in `<head>` for page-specific components (`.lp-nav`, `.hero`, `.tutorial-block`, `.sheet-mock`, `.tip`, etc.) that have no overlap with the app page.

### Custom properties

All theme tokens in `:root`. No hard-coded color values outside of `:root` and `@media print`.

### Print styles

`@media print` block at the bottom of `style.css`:
- `@page { size: letter portrait; margin: 0.4in; }`
- Pure monochrome: `#000000` text/borders, `#ffffff` backgrounds
- Font weights 700–900 throughout
- `.no-print { display: none !important }`
- `.print-report { display: block !important }`

---

## External Dependencies

| Library | Purpose | CDN |
|---|---|---|
| Chart.js 4.4.0 | Line chart and doughnut chart | `https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js` |
| SheetJS 0.20.1 | Excel (.xlsx) read and write | `https://cdn.sheetjs.com/xlsx-0.20.1/package/dist/xlsx.full.min.js` |

Both CDN scripts loaded via `<script src>` in `app.html`. If either fails to load, the relevant function alerts the user and returns early.

---

## Browser Requirements

| API / Feature | Required for |
|---|---|
| `localStorage` | All data persistence |
| `FileReader` | Excel import |
| `Canvas` API | Print chart capture |
| CSS Grid | Category grid and charts row layout |
| CSS Custom Properties | Theming |
| ES2020 (numeric separators, optional chaining) | app.js |

Supported: Chrome 80+, Firefox 79+, Safari 14+, Edge 80+.

---

## Deployment

| Property | Value |
|---|---|
| Repository | `https://github.com/Azqato/Net-Worth-Tracker` |
| Branch | `main` |
| Pages source | Repo root (`/`) |
| Live URL | `https://azqato.github.io/Net-Worth-Tracker/` |
| Entry point | `index.html` |

Push to `main` → GitHub Actions builds Pages → live within ~30 seconds.
