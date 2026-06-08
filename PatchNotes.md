# Net Worth Tracker — Patch Notes

All changes listed newest first.

---

## v3.5 — Codebase Cleanup & Documentation
- Removed stale server files (index.js, package.json, data.json) left over from an early Node.js prototype
- Cleaned up verbose multi-line comment blocks in style.css and app.js
- Added PatchNotes.md, Design.md, PRD.md, TRD.md
- Fixed minor text inconsistency: "As of No data yet" → "No data yet"
- Updated README with accurate button labels and current feature list

## v3.4 — Nav Button Polish
- Removed emojis from all navigation buttons for a cleaner look
- Support button moved to sit between Add Snapshot and Guide

## v3.3 — Unified Button Style
- All navigation buttons across both pages now use the same uniform outline style
- Landing page section links (Setup, Import, Export) converted from text links to matching buttons
- Add Snapshot button changed from green to outline style — consistent with all other nav buttons

## v3.2 — Navigation Cleanup
- Guide and App buttons added to the far right of navigation on all pages
- Active page is highlighted with an accent border so you always know where you are

## v3.1 — Landing Page & Guide
- Added a dedicated landing page at the site root with a step-by-step how-to guide (Setup, Import, Export)
- Tracker app moved to app.html; the landing page now serves as the public entry point
- Added Guide button to the app navigation bar linking back to the landing page

## v3.0 — Support Button Repositioned
- Moved the Support button to the left of the Guide button in the navigation bar

## v2.9 — Footer & Support Link
- Added "Built by Azqato" footer on all pages linking to azqato.github.io
- Added Support button to the navigation bar linking to the support page

## v2.8 — Money Bag Favicon
- Added 💰 money bag emoji favicon as an SVG for crisp rendering in all browsers and bookmarks

## v2.7 — Enhanced Monochrome Printer Visibility
- All print text now uses bold font-weight (700–900) for maximum visibility on monochrome laser printers
- Increased border widths from 1px to 2–3px for prominent, crisp definition
- Font sizes increased 10–20% for improved legibility
- Increased line-height and chart line-width (3px) for crisp rendering

## v2.6 — Export Format Standardization
- Export now uses the same column format as the template and import
- Column widths standardized across export, template, and import
- Exported files can be re-imported directly without reformatting

## v2.5 — Monochrome Laser Printer Optimization
- Print report uses pure black (#000000) exclusively for all text, borders, and lines
- Eliminates dithering inconsistencies across different printer models and toner levels

## v2.4 — Print Report Improvements
- Snapshot history on print report expanded from last 5 to last 12 months
- Net Worth Over Time chart shows only the total net worth line (category lines removed)
- Net Worth Over Time chart takes its own full-width row; Allocation chart removed from print layout

## v2.3 — Clear All Data
- Added Clear All button to the Snapshot History header to delete all snapshots at once
- Confirmation prompt required before data is removed

## v2.2 — Simplified Column Format
- Template and import now use condensed column headers: Date, Net Worth, Cash, Investments, Retirement, Real Estate
- Date column uses M/D/YYYY format matching standard spreadsheet conventions

## v2.1 — Excel Import
- Import Data button now accepts an Excel (.xlsx) file instead of raw JSON
- Rows with missing or malformed dates are automatically skipped

## v2.0 — Allocation Chart Labels
- Allocation chart legend now shows percentage inline next to each category (e.g., "Cash (5%)") — no hover required

## v1.4 — Print & PDF Report
- Upgraded print function produces a clean 8.5×11 letter-portrait PDF report
- Report includes KPI grid, Net Worth chart, and recent 12-snapshot history table
- Dedicated @media print CSS layer forces light mode and hides all UI chrome
- Chart images captured via Canvas API for accurate print fidelity

## v1.3 — Collapsible History & Print Preview
- Snapshot History section now has a collapse/expand toggle button
- Patch Notes section retains its collapse/expand toggle

## v1.2 — Excel Template & MoM Indicators
- Download Excel (.xlsx) template with pre-formatted fields for all four categories
- Instructions tab included in template workbook
- Month-over-month percentage change badges added to all four category cards and Total card

## v1.1 — Chart Enhancements & Allocation Fix
- Hover tooltips on all charts now show both dollar value and percentage of portfolio
- Fixed allocation percentage calculations — now correctly computed from latest snapshot total
- Line chart shows all five series (Net Worth + 4 categories) with category percentages in tooltips

## v1.0 — Initial Release
- Net worth tracking across four categories: Cash Equivalents, Taxable Investments, Retirement Investments, Real Estate
- Add Snapshot modal for monthly data entry
- Net Worth Over Time line chart and Allocation doughnut chart
- Snapshot history table with delete support
- Export to JSON and Paste/Import data
- Data stored in browser localStorage — persists across visits until cache is cleared
