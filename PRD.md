# Net Worth Tracker — Product Requirements Document

## Overview

Net Worth Tracker is a free, browser-based personal finance tool that lets users track their net worth over time across four asset categories. No account, server, or installation is required — all data is stored in the browser's localStorage and never leaves the device.

Live: [https://azqato.github.io/Net-Worth-Tracker/](https://azqato.github.io/Net-Worth-Tracker/)

---

## Target Users

Individual investors and personal finance enthusiasts who:
- Track monthly financial balances in Google Sheets or Excel
- Want visual charts of their net worth history without signing up for a paid service
- Need a privacy-first solution where their data never leaves their device
- Are comfortable with basic web browser and spreadsheet usage

---

## Core Requirements

### Functional

| # | Requirement |
|---|---|
| F1 | Add a monthly snapshot with a date and four category amounts via a modal form |
| F2 | Edit an existing month's snapshot by re-entering the same date (overwrites) |
| F3 | Delete individual snapshots from the history table |
| F4 | Clear all snapshot data at once with a confirmation prompt |
| F5 | Display total net worth and per-category values for the most recent snapshot |
| F6 | Display month-over-month (MoM) percentage change for total and each category |
| F7 | Display a line chart of net worth over time (total + 4 categories) |
| F8 | Display an allocation doughnut chart with live percentage labels |
| F9 | Display a snapshot history table sorted newest-first with delete controls |
| F10 | Collapse/expand the Snapshot History section |
| F11 | Collapse/expand the Patch Notes section (collapsed by default) |
| F12 | Download a pre-formatted Excel template (.xlsx) with an Instructions tab |
| F13 | Import snapshots from an Excel file (.xlsx) matching the template format |
| F14 | Export all snapshots to an Excel file (.xlsx) |
| F15 | Generate a print-optimized Net Worth Report (letter portrait, monochrome) |
| F16 | Show an inactivity warning prompting the user to export data after 30s of inactivity (max 2 reminders, then permanently suppressed) |

### Non-Functional

| # | Requirement |
|---|---|
| N1 | All user data stays client-side — no network requests for personal data |
| N2 | No account or sign-up required |
| N3 | Works in all modern browsers (Chrome, Firefox, Safari, Edge) |
| N4 | Print report compatible with monochrome laser printers |
| N5 | No build step required — plain HTML, CSS, and JavaScript |
| N6 | Hosted on GitHub Pages at zero cost |

---

## Pages

### Landing Page (`index.html`)

Public entry point and step-by-step user guide. Accessible to anyone visiting the site root.

**Sections:**
1. Sticky navigation bar (Setup, Import, Export anchors + Support, Guide, App links)
2. Hero — headline, description, "Open the Tracker" CTA, "Read the Guide" CTA
3. Requirements — what you need (Gmail, Google Sheets, any browser — all free)
4. 3-step workflow overview
5. Tutorial 1: Setting up Google Sheets for the first time (A: Download template, B: Upload to Drive, C: Fill in data)
6. Tutorial 2: Importing data into the app (A: Export from Google Sheets, B: Upload into the app)
7. Tutorial 3: Exporting from the app back to Google Sheets (A: Download from app, B: Open in Google Sheets)
8. Monthly routine summary (4 steps, under 2 minutes)
9. Final CTA ("Open Net Worth Tracker →")
10. Footer

### App Page (`app.html`)

The tracker application itself.

**Sections:**
1. Sticky navigation bar (Print, Template, Export, Import Data, Add Snapshot, Support, Guide, App)
2. Total Net Worth card (value, date, MoM change)
3. 4-column category cards (Cash, Taxable, Retirement, Real Estate — each with value, % of portfolio, MoM badge)
4. Charts row (Net Worth Over Time line chart, Allocation doughnut)
5. Snapshot History table (collapsible, expanded by default)
6. Patch Notes (collapsible, collapsed by default)
7. Footer

---

## Data Categories

| Key | Display Name | What to include |
|---|---|---|
| `cash` | Cash Equivalents | Checking, savings, money market, CDs |
| `taxable` | Taxable Investments | Brokerage accounts, stocks, ETFs, mutual funds |
| `retirement` | Retirement Investments | 401k, IRA, Roth IRA, pension |
| `realEstate` | Real Estate | Primary home, investment properties (market value) |

---

## Navigation Design

Both pages share a consistent navigation pattern:
- **Left**: Brand mark (NW icon + "Net Worth Tracker")
- **Right**: Page-specific action buttons → Support → Guide → App
- Active page link highlighted with accent-color border (`.btn-nav-active`)
- All nav items use the same `.btn-outline` button style

---

## Out of Scope

- Cloud sync or cross-device data persistence
- User authentication backend (placeholder UI exists in authModal, not wired)
- Debt and liability tracking
- Multiple portfolio views or custom categories
- Currency conversion or multi-currency support
- Mobile native app
- Social sharing or public profiles
