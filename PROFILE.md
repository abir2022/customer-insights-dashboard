# NexaGroup Customer Understanding Dashboard — Research Profile

*Methodology-style write-up of a five-workstream B2B customer intelligence suite.
All data described here is synthetic demo data (see §8).*

## 1. Abstract

This project packages a full B2B customer-research cycle for a diversified
industrial group into a single self-contained HTML application. Starting from a
consolidated statement extract covering 616 business customers across 19 strategic
business units (SBUs) and roughly Tk 2,759 Cr of revenue over Jan–Aug 2026, it chains
five linked analyses: (1) customer understanding — concentration, channel, party,
credit and recency structure; (2) survey design — a reproducible sampling plan with a
structured questionnaire; (3) geography and demography — a standard
division→district→upazila master with map diagnostics; (4) a dealer priority list and
route plan for one-trip field visits; and (5) an editable ROI business case. Each
stage consumes the previous stage's output, mirroring how a real insights engagement
proceeds from data to field plan to investment decision. The artifact is deliberately
static: the value demonstrated is the analytical method and information architecture,
not server infrastructure.

## 2. Problem Framing

In groups where each SBU keeps its own customer statement, no single owner can answer
basic questions: how many distinct customers does the group actually serve, how
concentrated is revenue, which customers deserve scarce interview time, which
territories cannot even be routed because addresses are missing, and what is acting on
the findings worth. The dashboard exists to compress that chain of questions into one
navigable artifact a planning department can put in front of management: a shared
customer master, a defensible sampling plan, a geography diagnosis, an actionable visit
sequence, and a quantified case for funding the research.

## 3. Related Practice

The techniques encoded here are standard analyst practice rather than invention.
Customer-concentration work follows Pareto/ABC analysis of revenue tables; sampling
sized to a target margin of error uses Cochran's formula with a finite-population
correction, with census coverage reserved for strategic accounts; field routing groups
accounts by territory so one trip covers a cluster; and investment decisions are argued
through simple ROMI/payback ledgers (one-time research cost versus recurring
profit protected). Geocoding and data-quality scoring of address fields is the usual
first step before any mapping exercise.

## 4. Methodology & Data Design

**Source (conceptual).** A per-SBU customer statement extract (party code, name,
business type, channel, territory, credit terms, invoices, payments) consolidated into
one party-level master; the portfolio copy embeds a deterministic synthetic generation
of the same schema (616 customers, 19 SBUs, monthly series Jan–Aug 2026).

**Transformation steps.** (i) party-code deduplication across SBUs; (ii) normalisation
of revenue (Tk), volume (MT) and price (Tk/MT); (iii) derived fields: 12-month revenue,
last-purchase recency bucket, top-SBU tag, customer type (B2B / B2C / Distributor /
Dealer); (iv) geography resolution against a standard division/district/upazila master,
flagging unresolved ("unlocatable") records; (v) aggregation objects for KPIs, Pareto,
mix tables and the ranked dealer list.

**Metric definitions.** Revenue = Σ invoice values, 12-month window. Concentration =
revenue share of the top *n*% of customers sorted descending. Sample size
n₀ = Z²·p(1−p)/e², corrected n = n₀/(1 + (n₀−1)/N). Recency = months since last
invoice. ROMI = (incremental profit − survey cost) / survey cost; payback days =
survey cost ÷ (daily base profit).

## 5. Analytical Framework

The UI encodes a decision cascade: understand → target → reach → visit → justify.
Customers are tiered A/B/C by revenue contribution (top tier ≈ the Pareto head), and
the survey plan allocates census interviews to strategic accounts and stratified
random picks elsewhere, proportional to SBU revenue. Geography output gates routing:
only located customers enter the route plan, and clusters are ordered division →
district → SBU so each trip is one drive. The business case then tests sensitivity:
editables for survey cost, profit uplift and retention gain flow through payback and
ROMI live, so the recommendation changes with assumptions instead of being hardcoded.

## 6. System Design

One `index.html` (~1.8 MB): a shell page (sticky top bar, tab navigation, overview
panel) plus five analytical panels, each an `<iframe srcdoc>` carrying its own
escaped HTML/CSS/JS document. Data travels as JSON literals embedded in the iframe
scripts; every panel reads the same figures, so cross-tab consistency is structural.
Rendering is hand-rolled SVG (bars, Pareto curves, trend lines, Bangladesh choropleth
with click-to-drill) — no chart library, no runtime dependency, no network calls
except optional Google Fonts (Fira Sans / Fira Code). Styling follows an internal
design spec: CSS custom-property tokens, dark-first navy palette with an optional
light-mode toggle on the shell, KPI hover-lift cards, focus-visible rings,
prefers-reduced-motion support, responsive at 390/768/1024/1440. Static packaging is a
feature: the file opens from disk, e-mails, or any static host with zero setup.

## 7. Findings & Design Decisions

**Data story (synthetic).** ~90% of revenue sits in the top 20% of customers; 19 SBUs
share overlapping dealers; 184 of 616 records are unlocatable, making geography
hygiene a prerequisite for routing; a 150-response plan (70 census + 80 stratified)
sizes the survey; a Tk 17.8 Lac survey cost against Tk 25 Cr base annual profit yields
a payback of about three days — the case survives pessimistic uplift assumptions.
**UI decisions.** Charts are the hero (no invented testimonials); one card/radius/
heading scale everywhere; blue + amber palette only, semantic red/green reserved for
delta and risk chips; motion limited to hover/press feedback and one ≤450 ms load-in
stagger; every export (CSV/PDF) stays deterministic from the embedded data.

## 8. Limitations & Validity

All names, contacts, addresses and figures are **synthetic demo data** generated for a
portfolio copy; NexaGroup, its SBUs and every customer record are fictional, and no
real-company statistics are used or claimed. Aggregate percentages therefore describe
the dataset, not any market. Scope: single-snapshot, party-level analysis; no
competitor intelligence, no panel weighting beyond stratification, no live data
warehouse link. Conclusions about method (the cascade in §5) remain valid; the numbers
illustrate it.

## 9. Reproducibility & Deployment

Run locally: open `index.html` in any modern browser (no install, no build). Serve
optionally with `npx serve .`. Deploy: import the folder into Vercel (or Netlify /
GitHub Pages) as a static project — framework preset "Other", no build command, no
output directory. No environment variables or secrets; the only external resource is
the Google Fonts stylesheet (falls back to system fonts offline). Development
checks: every `<script>` block (shell and iframe) passes `node --check`; all embedded
JSON literals parse; a brand-term grep is clean.
