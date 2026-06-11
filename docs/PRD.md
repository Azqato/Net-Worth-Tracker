# Net Worth Tracker — Product Requirements Document

---

## Press Release

**FOR IMMEDIATE RELEASE**

### Net Worth Tracker Gives Privacy-Conscious Investors a Free, No-Account Way to Visualize Their Financial Progress

*Browser-based tool runs entirely on-device, pairs with Google Sheets, and requires zero sign-up*

Today, Net Worth Tracker launched publicly as a free, browser-based tool that lets individuals track and visualize their personal net worth over time — without creating an account, installing software, or sharing financial data with any third party.

Unlike popular financial aggregators that require users to connect bank accounts or create profiles, Net Worth Tracker stores everything locally in the user's browser. Users enter their monthly balances across four asset categories — cash, taxable investments, retirement accounts, and real estate — either by hand or by importing an Excel file from Google Sheets. The app instantly generates a net worth history chart, an asset allocation breakdown, and month-over-month change indicators.

Net Worth Tracker is free to use at [https://azqato.github.io/net-worth-tracker/](https://azqato.github.io/net-worth-tracker/). It works in any modern browser on any device, and includes a step-by-step guide for first-time users.

---

## Problem Statement

Individual investors who track their monthly finances in spreadsheets have no easy way to visualize their net worth history without either paying for a financial app (which requires sharing sensitive data) or building their own charts from scratch. Existing tools require account creation, charge subscription fees, or send financial data to third-party servers — none of which is acceptable for privacy-conscious users.

Net Worth Tracker solves this by providing a fully client-side chart and history tool that runs entirely in the browser. Users keep their data in Google Sheets (a tool they already use) and import it into the app for visualization. No accounts. No servers. No cost.

---

## Product Tenets

Tenets are ordered by priority. When two tenets conflict, the higher-numbered one loses.

### 1. Privacy Is Architecture, Not Policy

User financial data must never leave the device. This is not a preference the user can override or a policy that can be updated — it is baked into how the system works. There is no server to send data to. When a feature requires a backend (cloud sync, account creation), it is out of scope by definition, not by choice. If that changes, this tenet changes first.

*Resolves: "Should we add telemetry to understand user behavior?" → No. Knowing what users do requires observing them.*

### 2. Zero Friction to First Value

A new user must be able to open the app and see meaningful output — their own data charted — in under 10 minutes, with no installation, no account, and no technical setup. Any feature that adds a mandatory step before the user sees their first chart violates this tenet.

*Resolves: "Should we require email verification to use the app?" → No. That is a gate before first value.*

### 3. The App Must Outlive Its Author

The architecture must stay maintainable by a single developer with no active maintenance. No build pipeline to break, no framework to migrate, no npm audit failures to fix. Plain HTML, CSS, and JavaScript that works today will work in ten years with no changes. Adding a dependency or build step requires strong justification — the default answer is no.

*Resolves: "Should we use React for the UI?" → No. A framework adds upgrade debt and a build step that no one will maintain.*

### 4. Four Categories Are Enough

The four asset categories (Cash, Taxable, Retirement, Real Estate) are a product decision, not a technical limitation. Allowing custom categories increases both complexity and support burden without serving the primary user. When a user asks for more categories, the correct response is to direct them to a spreadsheet.

*Resolves: "A user asked for a 'Crypto' category — should we add it?" → No. Serve the common case well. Edge cases belong in the user's spreadsheet.*

### 5. Data Portability Over Convenience

The user must always be able to get their data out in a usable format. Export must produce a file that can be re-imported, opened in Google Sheets, or used independently. Features that make the app stickier by locking in data are rejected.

*Resolves: "Should we use a custom binary format for export?" → No. Standard Excel format first, always.*

---

## Target Users

**Primary persona: The Self-Tracking Investor**
- Maintains a personal spreadsheet of monthly account balances (checking, brokerage, 401k, home value)
- Wants to see trends and charts without paying for a service like Personal Capital or Mint
- Privacy-conscious — unwilling to connect bank accounts or share balance data with a third party
- Comfortable using Google Sheets and a web browser; not a developer
- Checks their net worth monthly, not daily

**Secondary persona: The Privacy-First Minimalist**
- Distrustful of financial aggregator apps
- Prefers owning their data locally
- Willing to do a small amount of manual data entry in exchange for full control
- Values simplicity over automation

---

## Goals

- Give users a clear visual picture of their net worth over time with zero account creation friction
- Make the data entry workflow feel as lightweight as a spreadsheet update
- Ensure no user data ever leaves the browser — full privacy by architecture
- Be deployable and runnable at zero cost indefinitely (static hosting only)

---

## Non-Goals

- Cloud sync or cross-device data persistence
- Automated account aggregation or bank connectivity
- Debt and liability tracking (negative net worth positions)
- Multiple portfolio views or custom user-defined categories
- Currency conversion or multi-currency support
- Mobile native app (iOS/Android)
- Social sharing or public profiles
- User authentication backend (a placeholder UI exists in `authModal` but is not wired up)
- Budget tracking, spending analysis, or cash flow tools

---

## User Stories

**Data entry**
- As a user, I want to record my monthly balances across four categories so that I can build a history over time.
- As a user, I want to edit a previous month's entry by re-entering the same date so that I can correct mistakes without deleting and re-adding.
- As a user, I want to delete individual snapshots so that I can remove erroneous entries.
- As a user, I want to clear all data at once so that I can start fresh.

**Visualization**
- As a user, I want to see my total net worth and each category's value for the most recent month so that I know where I stand today.
- As a user, I want to see month-over-month percentage changes on every card so that I can spot trends at a glance.
- As a user, I want to see a line chart of my net worth over time so that I can understand the trajectory.
- As a user, I want to see an allocation doughnut chart so that I can understand how my wealth is distributed.

**Data portability**
- As a user, I want to download a pre-formatted Excel template so that I can set up my Google Sheet correctly the first time.
- As a user, I want to import snapshots from an Excel file so that I can load historical data from my existing spreadsheet.
- As a user, I want to export my data to Excel so that I can keep a backup outside the browser.
- As a user, I want to print a clean PDF report so that I can save or share a point-in-time summary.

**Reminders**
- As a user, I want to be reminded to export my data after a period of inactivity so that I don't lose my work if the browser cache is cleared.

---

## Feature List

### MVP (Shipped)

| ID | Feature |
|---|---|
| F1 | Add monthly snapshot via modal (date + 4 category amounts) |
| F2 | Edit existing month by re-entering same date (upsert) |
| F3 | Delete individual snapshots from history table |
| F4 | Clear all snapshot data with confirmation prompt |
| F5 | Total net worth card (value, date, MoM change) |
| F6 | Four category cards (value, % of portfolio, MoM badge each) |
| F7 | Net Worth Over Time line chart (total + 4 category series) |
| F8 | Allocation doughnut chart with live inline percentage labels |
| F9 | Snapshot history table, newest-first, with per-row delete |
| F10 | Collapsible Snapshot History section |
| F11 | Collapsible Patch Notes section (collapsed by default) |
| F12 | Download pre-formatted Excel template with Instructions tab |
| F13 | Import snapshots from `.xlsx` file matching template format |
| F14 | Export all snapshots to `.xlsx` |
| F15 | Print-optimized Net Worth Report (letter portrait, monochrome) |
| F16 | Inactivity warning modal prompting export (max 2 reminders, then permanently suppressed) |
| F17 | Landing page and step-by-step setup/import/export guide |

### Future (Post-Launch / Planned)

| Feature | Rationale for deferral |
|---|---|
| Cloud sync / user accounts | Requires backend infrastructure; breaks zero-cost constraint |
| Debt / liability tracking | Scope expansion; requires redesigned data model |
| Custom categories | Increases complexity; primary users fit within 4 standard categories |
| Mobile native app | Separate engineering effort; web app works on mobile browsers |
| Multiple portfolio views | Low-priority edge case for primary persona |

---

## Constraints

- **No backend**: Must remain a static site deployable on GitHub Pages with zero server cost
- **No build step**: Must be plain HTML/CSS/JS with no bundler or compilation step required
- **No framework**: Vanilla JavaScript only — no React, Vue, Angular, etc.
- **CDN dependencies only**: External libraries loaded from CDN (Chart.js, SheetJS); no npm install
- **Browser localStorage limit**: ~5MB per origin; limits historical data volume (not a practical concern for monthly snapshots)
- **Single developer**: No team; decisions must favor simplicity and maintainability

---

## Assumptions

- Users already maintain their financial data in a spreadsheet (Google Sheets or Excel)
- Monthly granularity is sufficient; daily or weekly tracking is not needed
- Four asset categories (Cash, Taxable, Retirement, Real Estate) cover the primary use case
- Users accept that browser cache clearing will erase data and will export regularly
- GitHub Pages will remain a viable free hosting option

---

## FAQ

### Internal FAQ

**Q: Why build this when tools like Personal Capital and Mint already exist?**
A: Those tools require linking bank accounts or creating user profiles. Many users — particularly privacy-conscious individual investors — are unwilling to give a third party read access to their financial accounts. This tool serves that gap with zero data sharing required.

**Q: What happens to user data if the browser cache is cleared?**
A: It is lost. This is a known and intentional tradeoff — all data lives in `localStorage` with no backup. The app mitigates this by prompting users to export after inactivity and by making Excel export a one-click operation.

**Q: Why only four asset categories? What about debt?**
A: The four categories cover the primary use case for the target persona. Adding debt tracking or custom categories increases UI complexity and data model scope — that tradeoff is intentionally deferred.

**Q: What's the risk of CDN dependencies going down?**
A: Chart.js and SheetJS are loaded from CDNs. If either is unreachable, charts will not render and import/export will fail. The app shows alerts in those cases. Self-hosting the scripts would eliminate this risk but adds file maintenance overhead (acknowledged in Known Technical Debt).

**Q: How is this different from just using Google Sheets with charts?**
A: Google Sheets can do net worth charts, but requires manual setup of formulas, chart configurations, and percentage calculations. This app gives those features out of the box in a purpose-built interface.

**Q: What does success look like for this project?**
A: A user who discovers the tool is able to complete the full workflow — download template, fill it in, import, view charts — in under 10 minutes without reading any documentation. The support inbox stays empty. The app works the same way three years from now with no maintenance.

**Q: Why GitHub Pages and not Vercel, Netlify, etc.?**
A: GitHub Pages is free with no usage limits and has no vendor-specific configuration. For a fully static site with no server-side logic, it is the simplest zero-cost option with the least lock-in.

**Q: Is there a plan to monetize this?**
A: No current plan. If cloud sync is added in the future, a freemium model would be the natural path.

**Q: What must be true for this to succeed long-term?**
A: (1) GitHub Pages remains free, (2) Chart.js and SheetJS CDNs remain available, (3) browser `localStorage` behavior does not change in a breaking way.

**Q: Who maintains this?**
A: A single developer (Azqato). This directly informs every architectural decision — no build step, no dependencies to update, no framework migrations.

### External FAQ

**Q: Is this free?**
A: Yes, completely free. No subscription, no trial, no credit card.

**Q: Do I need to create an account?**
A: No. Open the app in your browser and start entering data immediately.

**Q: Where is my data stored?**
A: Entirely in your browser's local storage on your device. Nothing is sent to a server.

**Q: What happens if I clear my browser history or cache?**
A: Your data will be erased. Use the Export button regularly to save a backup Excel file — you can re-import it any time.

**Q: Can I use this on multiple devices?**
A: Not automatically. Export from one device and import on another to transfer data.

**Q: What file format does the app use for import and export?**
A: Excel (`.xlsx`). You can also use Google Sheets — the guide explains how to export in the correct format.

**Q: What are the four categories? Can I change them?**
A: Cash Equivalents, Taxable Investments, Retirement Investments, and Real Estate. They cannot be customized.

**Q: Does this track debts or liabilities?**
A: No. The app tracks assets only. Net worth displayed is the sum of all four asset categories.

**Q: Does this work on my phone?**
A: The app works in a mobile browser but is optimized for desktop use.

**Q: How do I print a report?**
A: Click the Print button in the top navigation. Your browser's print dialog will open with a clean, monochrome-optimized report.

---

## Metrics

### North Star Metric

**Monthly active users who have imported or entered at least 3 snapshots.**

This captures users who have gone beyond casual curiosity and established a real tracking habit — they've seen month-over-month comparisons and have a trend line. Raw visit count is noise.

*Measurement caveat: this app has no analytics or backend. This metric is currently unmeasured.*

### Acquisition Metrics

| Metric | Target | Timeframe | Measurement Method |
|---|---|---|---|
| Monthly unique visitors | 500/month | 6 months post-launch | Google Search Console (impressions proxy) |
| Landing page → App conversion | >40% | Ongoing | Not measurable without analytics |
| Referral sources | >50% organic search | 6 months | Google Search Console |

### Engagement Metrics

| Metric | Target | Measurement Method |
|---|---|---|
| Snapshots entered per session | ≥3 (historical import) | Not measurable without analytics |
| Import usage rate | >60% use Excel import | Not measurable without analytics |
| Export usage rate | >50% export at least once | Not measurable without analytics |
| Print usage rate | >10% print a report | Not measurable without analytics |

### Retention Metrics

| Metric | Target | Measurement Method |
|---|---|---|
| Month-2 return rate | >30% | Not measurable without analytics |
| Long-term habit (12+ snapshots) | Proxy: GitHub star growth | GitHub stars/forks (indirect) |

### Performance Targets

| Metric | Target | Measurement |
|---|---|---|
| Lighthouse Performance score | ≥ 90 | Lighthouse audit on each deploy |
| Lighthouse Accessibility score | ≥ 80 | Lighthouse audit on each deploy |
| Page load time | < 2 seconds on broadband | Lighthouse / WebPageTest |
| Zero uncaught JS errors | 0 in clean Chrome session | Manual devtools check |
| GitHub stars | 50 stars | 12 months post-launch |

### Analytics Stance

This app intentionally has no analytics script installed. Adding Google Analytics, Plausible, or any equivalent tracker would send visitor data to a third-party server — a violation of Tenet 1. The metrics above marked "not measurable without analytics" are aspirational targets that can only be baselined if a fully self-hosted, privacy-preserving solution is adopted and disclosed to users.

---

## Roadmap

### Current Phase: Stable & Documented (Phase 3)

The core product is feature-complete for the primary use case. The full import/export/chart/print workflow is shipped and working. Current focus is polish, accessibility, and stability.

### Milestone Table

| Milestone | Status |
|---|---|
| v1.0 — Initial Release | Complete |
| v2.x — Print, Excel, Charts polish | Complete |
| v3.x — Landing page, nav unification, cleanup | Complete |
| v3.6 — Documentation audit and restructure | Complete (2026-06-08) |
| v3.8 — Print chart first-click fix | Complete (2026-06-10) |
| v4.0 — Accessibility & mobile polish | Planned (Q3 2026) |
| v5.0 — Cloud sync / accounts | Planned (undated) |

### v4.0 — Accessibility & Mobile Polish (Planned, Q3 2026)
- Full responsive layout for `app.html` on mobile viewports
- ARIA labels on chart canvases
- Skip-navigation link
- Keyboard-accessible modals (focus trap, Escape key close)
- WCAG AA audit and gap remediation
- Color-blind-safe MoM badge alternative (icon in addition to color)

### v5.0 — Cloud Sync / Accounts (Planned, Undated)
- Optional user account creation (Google OAuth)
- Cloud storage for snapshots (cross-device sync)
- Freemium model: local-only remains free; cloud sync is paid
- Data migration path from localStorage to cloud

### Explicitly Deferred Items

| Feature | Reason |
|---|---|
| Debt / liability tracking | Requires redesigned data model and new UI |
| Custom asset categories | Increases complexity significantly; four-category model serves primary persona |
| Automated bank/account aggregation | Requires backend, OAuth flows, credential handling — violates Tenet 1 |
| Currency conversion / multi-currency | Niche; requires FX rate API |
| Mobile native app (iOS/Android) | Separate engineering platform |
| Budget tracking / spending analysis | Different product category |
| Social sharing / public profiles | Antithetical to privacy-first positioning |
| Self-hosted CDN scripts | Low priority until CDN reliability becomes an issue |

---

## Success Criteria

| Criterion | Measure |
|---|---|
| Core workflow is self-service | New user completes full setup (template → Sheet → import → charts) without needing support |
| Privacy commitment is absolute | Zero network requests made for personal financial data (verified by browser devtools) |
| Zero setup friction | App is usable immediately at the live URL with no installation or account |
| Data round-trip works | Exported `.xlsx` can be re-imported without reformatting |
| Print output is professional | Report reads cleanly on a monochrome laser printer |

---

## Technical Requirements

### System Architecture

**Type**: Static web app — no server, no backend, no API calls for user data.
**Deployment**: GitHub Pages (`Azqato/net-worth-tracker`, `main` branch). Files served directly from repo root.
**Data persistence**: Browser `localStorage` only, scoped to the origin and browser profile. Clearing browser cache removes all data.
**Build**: None. No bundler, no compiler, no transpiler. Every file ships as-is.

### Tech Stack

| Layer | Technology | Version | Delivery |
|---|---|---|---|
| Markup | HTML5 | — | Static file |
| Styling | CSS3 (custom properties, grid, flexbox) | — | Static file |
| Logic | Vanilla JavaScript (ES2020+) | — | Static file |
| Charts | Chart.js | 4.4.0 | jsDelivr CDN |
| Excel I/O | SheetJS (xlsx) | 0.20.1 | SheetJS CDN |
| Hosting | GitHub Pages | — | — |

No Node.js. No npm. No framework. No runtime in production.

### Folder Structure

```
/
├── index.html        # Landing page & step-by-step guide (site entry point)
├── app.html          # Net Worth Tracker application
├── style.css         # Shared stylesheet for both pages
├── app.js            # All application logic (loaded by app.html only)
├── favicon.svg       # Money bag emoji favicon (SVG)
├── README.md         # Developer-facing project overview
└── docs/
    ├── PRD.md        # This file — product, technical, security, and ops reference
    ├── DESIGN.md     # Design system, component patterns, and accessibility
    └── PATCHNOTES.md # Full version history
```

`docs/` was consolidated from 10 files to 3 in v3.8. The former TENETS.md, METRICS.md, ROADMAP.md, PRFAQ.md, TRD.md, SECURITY.md, and RUNBOOK.md were all folded into PRD.md under their respective sections. This keeps the documentation surface small and maintainable by a single developer — consistent with Tenet 3.

### Data Models

#### Snapshot Object

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

| Field | Type | Description |
|---|---|---|
| `id` | string | `Date.now().toString()` for manual saves; `Date.now() + Math.random()` for bulk imports |
| `date` | string | ISO year-month `YYYY-MM` |
| `cash` | number | Cash Equivalents balance (dollars, no decimals stored) |
| `taxable` | number | Taxable Investments balance |
| `retirement` | number | Retirement Investments balance |
| `realEstate` | number | Real Estate market value |
| `total` | number | Sum of all four categories; computed at save time, stored for convenience |

#### localStorage Keys

| Key | Type | Description |
|---|---|---|
| `networth_snapshots` | JSON array | All snapshots, sorted by date ascending |
| `inactivity_dismiss_count` | integer string | Times the inactivity modal was dismissed (0, 1, or 2) |

### Data Flow

#### Write path
1. User fills form → `saveSnapshot()` called
2. Parse and validate inputs
3. Upsert into `snapshots` array (match by `date` field)
4. Sort array by date ascending
5. `persistSnapshots()` → `localStorage.setItem('networth_snapshots', JSON.stringify(snapshots))`
6. `renderAll()` → update all DOM elements

#### Read path
1. `DOMContentLoaded` fires → `loadSnapshots()`
2. `JSON.parse(localStorage.getItem('networth_snapshots'))` → `snapshots` global array
3. `renderAll()` dispatches to each renderer

#### Import path
```
FileReader.readAsArrayBuffer(file)
  → XLSX.read(buffer, { cellDates: true })
  → sheet_to_json(worksheet, { defval: '' })
  → parseDate(val)    // handles Date objects, YYYY-MM strings, M/D/YYYY strings
  → parseNum(val)     // strips $, commas; falls back to 0
  → filter rows where date === null
  → confirm(count) prompt
  → upsert each row into snapshots
  → sort → persist → renderAll
```

#### Export path
```
XLSX.utils.book_new()
  → aoa_to_sheet([ headers, ...rows ])   // Date, Net Worth, Cash, Investments, Retirement, Real Estate
  → set column widths (!cols)
  → book_append_sheet(wb, ws, 'Net Worth Data')
  → XLSX.writeFile(wb, 'networth-export-YYYY-MM-DD.xlsx')
```

#### Print path
1. Gather latest/prev snapshot data; build KPI HTML (5 cards)
2. Create off-screen `<div>` at `left: -9999px`
3. Create `<canvas>` (880×260px), render Chart.js line chart (total only, monochrome, `duration: 0`)
4. Defer `canvas.toDataURL()` to next `requestAnimationFrame` (ensures Chart.js has painted the canvas)
5. Destroy Chart.js instance; remove off-screen div
6. Build last-12-months history table HTML
7. Inject all generated HTML into `#printReport`
8. Call `window.print()`

### State Management

All state lives in a single module-level `snapshots` array in `app.js`. No external state library.

- **Load**: read from `localStorage` on `DOMContentLoaded`
- **Mutate**: modify the array, sort by date, write back to `localStorage`
- **Render**: `renderAll()` called after every mutation; all renderers are pure DOM writers
- **Charts**: `lineChart` and `allocationChart` are module-level Chart.js instances. Each render call destroys the previous instance before creating a new one to prevent canvas memory leaks.

#### Render pipeline

| Function | DOM targets |
|---|---|
| `renderTotalCard(latest, prev)` | `#totalNetWorth`, `#totalDate`, `#totalMoM` |
| `renderCategoryCards(latest, prev)` | `#cashValue`, `#cashPct`, `#cashMoM` (× 4 categories) |
| `renderLineChart()` | `#lineChart` canvas |
| `renderAllocationChart(latest)` | `#allocationChart` canvas |
| `renderSnapshotHistory()` | `#snapshotTable` innerHTML |

#### Inactivity Timer

- **Events monitored**: `mousemove`, `mousedown`, `keydown`, `scroll`, `touchstart`, `click`
- **First warning**: 30 seconds of inactivity (`INACTIVITY_MS_1ST`)
- **Second warning**: 120 seconds after first dismissal (`INACTIVITY_MS_2ND`)
- **After two dismissals**: timer permanently stopped; state persisted in `inactivity_dismiss_count`

### Third-Party Integrations

| Library | Purpose | Source | Auth |
|---|---|---|---|
| Chart.js 4.4.0 | Line chart and doughnut chart rendering | `cdn.jsdelivr.net` | None |
| SheetJS 0.20.1 | Excel `.xlsx` read and write | `cdn.sheetjs.com` | None |

Both scripts are loaded via `<script src>` in `app.html`. If either fails to load, the relevant function alerts the user and returns early. No user data is sent to these CDNs.

### Performance Requirements

| Target | Value |
|---|---|
| Page load (cold, no data) | < 2 seconds on broadband |
| First meaningful render | < 1 second |
| Render on 100 snapshots | < 100ms (all synchronous DOM writes) |
| localStorage footprint | < 50KB for 10 years of monthly data |

### Browser Support

| API / Feature | Required for |
|---|---|
| `localStorage` | All data persistence |
| `FileReader` | Excel import |
| Canvas API | Print chart capture |
| CSS Grid | Category grid and charts row layout |
| CSS Custom Properties | Theming |
| ES2020 | `app.js` |

Supported: Chrome 80+, Firefox 79+, Safari 14+, Edge 80+.

### Deployment

| Property | Value |
|---|---|
| Repository | `https://github.com/Azqato/net-worth-tracker` |
| Branch | `main` |
| Pages source | Repo root (`/`) |
| Live URL | `https://azqato.github.io/net-worth-tracker/` |
| Entry point | `index.html` |

Push to `main` → GitHub Actions builds Pages → live within ~30 seconds. No build command, no artifact to upload.

### Known Technical Debt

| Item | Current shortcut | Correct solution |
|---|---|---|
| No input validation on snapshot form | Negative numbers and non-numeric input are coerced to 0 or NaN silently | Add client-side validation with user-visible error messages |
| Auth modal is UI-only | `authModal` renders a full sign-up form with OAuth buttons wired to stub functions that do nothing | Remove the modal entirely or connect to a real auth provider |
| Global `snapshots` array | All state in a single mutable global; no encapsulation | Wrap in a module or class with explicit read/write methods |
| `renderAll()` is always full re-render | Every mutation re-renders every chart and DOM section | Add dirty-checking to skip unchanged sections |
| No offline fallback for CDN scripts | If jsDelivr or SheetJS CDN is unreachable, charts and import/export silently fail | Bundle or self-host CDN scripts; add explicit error UI |
| No SRI hashes on CDN scripts | A compromised CDN could serve malicious JavaScript | Add `integrity` attributes with SRI hashes to both `<script>` tags |

---

## Security

### Authentication & Authorization Model

There is no authentication. The app does not identify users, create sessions, or issue tokens of any kind.

The `authModal` in `app.html` renders a full sign-up UI (Google, Apple, X OAuth buttons + email/password form) but all handlers are stubs that do nothing. This UI exists as a design placeholder. It should be considered dead code until a real auth backend is implemented.

There are no user roles or permissions. Anyone who opens the app in a browser has full read/write access to the data stored in that browser's `localStorage` for this origin. Data is scoped to the browser profile and origin — a different browser, incognito window, or device has no access to another session's data.

### Data Storage

| Data | Where stored | Protection |
|---|---|---|
| Snapshot data (`networth_snapshots`) | Browser `localStorage` | Origin-scoped by browser; inaccessible to other origins via same-origin policy |
| Inactivity dismiss count | Browser `localStorage` | Same as above |

**No data is transmitted to any server.** The only outbound requests are CDN fetches for Chart.js and SheetJS — GET requests for static JavaScript files with no user data.

**localStorage is not encrypted.** Any script running on the same origin could read the data. Users with malicious browser extensions should be aware that `localStorage` values are readable.

### Third-Party Trust

| Service | What it receives | What it does |
|---|---|---|
| jsDelivr CDN (`cdn.jsdelivr.net`) | Browser IP, User-Agent | Serves `chart.js` static JavaScript file |
| SheetJS CDN (`cdn.sheetjs.com`) | Browser IP, User-Agent | Serves `xlsx.full.min.js` static JavaScript file |
| GitHub Pages (`azqato.github.io`) | Browser IP, User-Agent | Serves all HTML, CSS, and JS files |

No financial data is sent to any of these services.

### Known Attack Surface

| Area | Risk | Mitigation |
|---|---|---|
| CDN dependency injection | A compromised CDN could serve malicious Chart.js or SheetJS | SRI hashes not implemented (see technical debt); use trusted CDNs; monitor for incidents |
| Excel import parsing | SheetJS parses user-provided `.xlsx` files; malformed files could trigger parsing bugs | No code execution from imported files; risk is low but present |
| localStorage access by browser extensions | Malicious extensions can read `localStorage` | No mitigation possible at the app level; user responsibility |
| Auth modal stub | `authModal` collects email/password into form inputs; `createAccount()` and `oauthSignIn()` are stubs that do nothing | Current behavior is safe; risk increases if stubs are ever wired to a real backend without security review |
| XSS via snapshot history rendering | `renderSnapshotHistory()` writes to `innerHTML` using data from `localStorage` | Data in `localStorage` is always generated by this app's own functions (number formatting, date strings); no user-supplied raw HTML is stored |

### Dependency Policy

| Practice | Status |
|---|---|
| CDN dependency versions are pinned | Yes — Chart.js `@4.4.0` and SheetJS `@0.20.1` pinned in `<script src>` URLs |
| Dependencies monitored for vulnerabilities | No automated monitoring (no npm, no Dependabot); manual check required |
| Dependency updates | Manual — update CDN URL version when a security patch is released |
| SRI hash validation | Not implemented; recommended for future hardening |

---

## Runbook

### Local Setup

**Prerequisites**: Git and a modern browser (Chrome 80+, Firefox 79+, Safari 14+, Edge 80+). No Node.js or npm required.

```bash
# Clone the repository
git clone https://github.com/Azqato/net-worth-tracker.git
cd net-worth-tracker

# Option A: open the file directly
open index.html          # macOS
start index.html         # Windows

# Option B: serve locally (avoids some browser restrictions)
python -m http.server 8080
# Then open http://localhost:8080
```

The app is fully functional immediately. There is no `npm install`, no environment setup, and no build step.

### Build

There is no build step. The source files are the production files. The repository root is served directly by GitHub Pages.

### Deploy

1. Commit and push changes to `main`:
   ```bash
   git push origin main
   ```
2. GitHub Actions automatically builds and publishes Pages.
3. The live site updates within ~30 seconds.

**Verify after deploy:**
1. GitHub → Actions tab — confirm the Pages workflow shows a green checkmark.
2. Open the live URL in an incognito window and confirm the landing page loads.
3. Navigate to `app.html` and confirm no console errors.

**GitHub Pages configuration** (Settings → Pages): Source: Deploy from branch / Branch: `main` / Folder: `/` (root)

### Rollback

Prefer `git revert` over `git reset --hard` to preserve history.

```bash
# Revert the most recent commit (creates a new revert commit)
git revert HEAD
git push origin main

# Revert multiple commits
git revert HEAD~3..HEAD
git push origin main
```

GitHub Pages redeploys automatically within ~30 seconds.

### Environment Configs

| Environment | URL | Branch |
|---|---|---|
| Production | `https://azqato.github.io/net-worth-tracker/` | `main` (auto-deploys on push) |
| Local dev | `http://localhost:8080` or `file://` | any branch |

There is no staging environment. Test changes locally before pushing to `main`. There are no environment variables — this is a fully static site with no secrets or API keys.

### Common Errors

| Error | Likely Cause | Fix |
|---|---|---|
| "Chart is not defined" in console | Chart.js CDN failed to load | Check internet connection; verify `cdn.jsdelivr.net` is reachable |
| Import button does nothing; "XLSX is not defined" | SheetJS CDN failed to load | Check internet connection; verify `cdn.sheetjs.com` is reachable |
| Snapshots disappear after browser restart | Browser set to clear storage on exit | Check browser settings for "Clear cookies and site data when browser closes" |
| Imported file shows 0 snapshots | Excel columns don't match expected format | Ensure columns are: Date, Net Worth, Cash, Investments, Retirement, Real Estate; use the Template from the app |
| Print report is blank or cut off | Browser print settings | Enable "Background graphics"; set margins to "None" or "Minimum"; use Chrome for best results |
| Page shows 404 after deploy | GitHub Pages not yet updated | Wait 60 seconds and hard-refresh |
| Console errors about `localStorage` | Private/incognito mode with storage disabled | Use a regular browser window |

### Monitoring

This is a static site with no server-side infrastructure. Relevant health checks:

| What to check | Where | Frequency |
|---|---|---|
| Site availability | Open live URL in browser | On each deploy |
| GitHub Pages build status | GitHub → Repository → Actions tab | On each deploy |
| JavaScript errors | Chrome DevTools → Console (clean incognito session) | On each deploy |
| CDN availability — Chart.js | jsDelivr status page | On any chart-related bug report |
| CDN availability — SheetJS | SheetJS GitHub releases (security advisories) | Monthly |
| Lighthouse score | Chrome DevTools → Lighthouse tab | On each deploy or major change |

There are no error logs, uptime monitors, or alerting systems. First diagnostic step for any reported issue: open the live URL in a clean incognito window and check the DevTools console.
