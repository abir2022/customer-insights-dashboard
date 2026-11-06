# NexaGroup Customer Understanding Dashboard

**One-liner:** A five-workstream customer intelligence suite — from "who buys from us" to "which dealers do we visit first, and what is it worth" — packaged as one self-contained HTML app.

## Problem

A diversified FMCG / agro / cement / steel group sells to tens of thousands of business
customers through 19 separate business units (SBUs). Every SBU kept its own customer
statement workbook, so nobody could answer basic questions: How many distinct customers
does the group actually serve? How concentrated is revenue? Which customers should be
surveyed first? Which territories are so poorly recorded that they cannot even be routed
on a map?

## Solution

One offline-first HTML file that chains five connected workstreams, each reading the same
in-memory dataset:

1. **Customer Understanding** — consolidated view across all SBUs: value, concentration
   (Pareto), buying behaviour, recency, channel / party / credit mix, monthly trend,
   exportable directory.
2. **Survey Design & Questionnaire** — stratified sampling plan (Cochran formula, census
   for strategic accounts), six-section questionnaire plus B2B / Distributor / Dealer /
   B2C modules and a four-stage field plan.
3. **Geography & Demography** — standard geography master (division → district → upazila),
   interactive SVG map of Bangladesh with district drill-down, and a diagnosis of
   "unlocatable" customer records.
4. **Customer Priority List & Route Plan** — dealers ranked by revenue, grouped by
   division / district / SBU / customer type for one-trip survey routing, with tiering
   (A / B / C) and CSV export.
5. **ROI & Business Case** — editable cost / benefit model showing what acting on the
   insights returns.

## Features

- Single file, zero dependencies, no build step — opens directly from disk or any static host.
- Five linked panels driven by one embedded dataset (shell page + `iframe srcdoc` panels).
- Interactive filters: SBU, region, territory, channel, party, credit, tier, plus search.
- Hand-rolled SVG charting (bars, trend lines, Pareto curves) — no chart library needed.
- Bangladesh division/district map with click-to-drill geography and coverage gaps.
- CSV and print-ready (PDF via browser print) exports from every tabular view.
- Deterministic synthetic dataset: 616 customers, 19 SBUs, Tk ~2,759 Cr revenue, Jan–Aug 2026.

## Tech stack

- Vanilla JavaScript (ES2020), HTML5, CSS3 (custom properties, grid / flex layouts)
- Inline SVG for maps and charts; `URL.createObjectURL` + Blob for CSV downloads
- Data embedded as JSON literals inside the page; no server, no external requests (fonts only)

## Synthetic demo data — disclaimer

This is a portfolio copy. All customer names, dealer / distributor identities, contact
persons, phone numbers, addresses, e-mail addresses, business-unit names and competitor
references have been replaced with **generated synthetic demo data**, and record volumes
were trimmed. Company and person names in this repository (NexaGroup, Nexa AgroFeeds,
Nexa Cement, Nexa Steel, and all customer records) are fictional. The aggregate figures,
percentages and lists in the dashboard therefore describe **no real company and contain
no confidential or personal data**. The methodology, information architecture and
visualisations are the actual work.

## Run locally

No install, no build:

```bash
# just open it
start index.html          # Windows
open index.html           # macOS
xdg-open index.html       # Linux

# or serve it
npx serve .
python -m http.server 8000
```

## Deploy

Import the folder into Vercel (or Netlify / GitHub Pages) as a static project — framework
preset "Other", no build command, no output directory. `index.html` is served as-is.
