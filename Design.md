# Net Worth Tracker — Design System

## Color Palette

All colors are defined as CSS custom properties in `:root` in `style.css`.

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#0d1117` | Page background |
| `--surface` | `#161b22` | Cards, header, nav, modals |
| `--surface2` | `#21262d` | Input backgrounds, hover states, table alternates |
| `--accent` | `#00d4a0` | Brand green — CTAs, active states, highlights, brand icon |
| `--text` | `#e6edf3` | Primary text |
| `--text-muted` | `#8b949e` | Labels, secondary text, placeholders |
| `--border` | `#30363d` | All borders and dividers |
| `--positive` | `#3fb950` | Positive MoM change badges |
| `--negative` | `#f85149` | Negative MoM change badges, danger button |
| `--cash` | `#4a9eff` | Cash category (chart + icon background) |
| `--taxable` | `#a371f7` | Taxable Investments category |
| `--retirement` | `#f78166` | Retirement Investments category |
| `--realestate` | `#ffa657` | Real Estate category |

The app is dark-mode only. There is no light/dark toggle.

---

## Typography

**Font stack**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`  
**Base size**: 14px  
**Line height**: 1.5 (body), 1.6–1.7 (guide content)

| Weight | Usage |
|---|---|
| 400 | Body text |
| 500 | Form labels, nav links |
| 600 | Card headings, section titles, brand name |
| 700 | Bold values, patch note titles |
| 800 | Hero heading |
| 900 | Total net worth figure, print output |

---

## Component Library

### Buttons

| Class | Style | Usage |
|---|---|---|
| `.btn-outline` | Surface2 background, border, text color | All nav/action buttons on both pages |
| `.btn-nav-active` | Accent border + accent text (extends btn-outline) | Current page nav indicator |
| `.btn-support` | Inline-flex (extends btn-outline) | Support link — external |
| `.btn-primary` | Accent background, dark text | Available, not currently used in nav |
| `.btn-save` | Full-width accent background, 14px text | Primary action inside modals |
| `.btn-danger-sm` | Red outline, small padding | Clear All in Snapshot History |
| `.btn-dismiss` | Surface2 background, muted text | Secondary action in Inactivity modal |
| `.btn-hero-primary` | Accent background, 15px, bold | Hero primary CTA (landing page) |
| `.btn-hero-secondary` | Surface2 background, 15px | Hero secondary CTA (landing page) |
| `.collapse-btn` | Surface2, muted text, pointer-events:none | Expand/Collapse toggle (display only) |

Active nav state is hardcoded per-page (`.btn-nav-active` on the current page's link). No JavaScript needed.

### Cards

| Class | Description |
|---|---|
| `.total-card` | Full-width net worth hero card with 3px top gradient stripe |
| `.category-card` | 4-column grid, hover shows accent border |
| `.chart-card` | Chart container, `.chart-large` (3fr) and `.chart-small` (2fr) |
| `.section-card` | Collapsible section wrapper (Snapshot History, Patch Notes) |
| `.need-card` | Landing page requirement card (icon + text) |
| `.workflow-card` | Landing page numbered workflow step |
| `.tutorial-block` | Landing page step-group container (header + steps) |

### Modals

All modals use `.modal-overlay` (fixed full-screen, blur backdrop, dark scrim) wrapping `.modal` (480px card).

Dismiss methods: click the `×` button, or click the overlay background. Both call `closeModal(id)`.

| ID | Purpose |
|---|---|
| `addSnapshotModal` | Manual snapshot entry (date + 4 fields) |
| `pasteDataModal` | Excel file upload for import |
| `inactivityModal` | Backup reminder (max 2 times) |
| `authModal` | Create Account placeholder UI |

### Empty States

`.empty-state` — centered muted text shown when no data is present in charts or the history table.

`.chart-empty` — absolutely centered overlay inside chart containers, hidden when data exists.

---

## Navigation

### App page (`app.html`)

Uses `.header` — sticky, `position: sticky; top: 0; z-index: 100`. Brand left, actions right.

Button order (left to right): `Print` · `Template` · `Export` · `Import Data` · `Add Snapshot` · `Support` · `Guide` · `App`

`App` has `.btn-nav-active` (current page indicator).

### Landing page (`index.html`)

Uses `.lp-nav` (same sticky behavior, same padding). Brand left, `.lp-nav-actions` right.

Button order (left to right): `Setup` · `Import` · `Export` · `Support` · `Guide` · `App`

`Guide` has `.btn-nav-active` (current page indicator).

---

## Layout

- **Max content width**: 1400px, 24px horizontal padding
- **Category grid**: `repeat(4, 1fr)` — all four categories equal width
- **Charts row**: `3fr 2fr` — line chart wider than allocation chart
- **Responsive**: Landing page sheet mock collapses at 600px (hides columns 4–5)
- **Gap between sections**: 20px (`.container` gap)

---

## Print Design

`@media print` forces a pure monochrome output:

- **Page size**: Letter portrait, 0.4in margins
- **Color**: Pure black `#000000` for all text, borders, lines — pure white `#ffffff` for all backgrounds
- **Font weight**: 700–900 throughout for laser printer legibility
- **Border width**: 2–3px for crisp definition
- **Layout**:
  1. Report header (title + date)
  2. 5-column KPI grid (Total, Cash, Taxable, Retirement, Real Estate)
  3. Full-width Net Worth Over Time line chart (Canvas → PNG)
  4. Snapshot history table (last 12 months)
- All `.no-print` elements hidden; only `.print-report` rendered
