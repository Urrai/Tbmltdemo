# TradeGuard — TBML Detection Platform (Demo)
 
A clickable, front-end-only prototype of an enterprise banking **Trade-Based Money
Laundering (TBML) Detection Platform**. It demonstrates the full compliance
workflow — from document upload through explainable alert investigation — using
realistic mock data. Built as a single-file React application styled with
Tailwind CSS.
 
> ⚠️ **Demo only.** There is no backend, database, OCR/LLM integration, or
> persistence layer. All data is hard-coded in-memory and resets on page reload.
 
---
 
## What it demonstrates
 
```
DOCUMENT UPLOAD → IDENTIFICATION → FIELD EXTRACTION → HUMAN VERIFICATION
     → TRADE DATA CONSOLIDATION → RULE ENGINE EXECUTION
     → TBML ALERTS → ALERT INVESTIGATION
```
 
## Features
 
- **Dashboard** — case/document/alert KPIs, alerts by typology / country /
  document type, rule performance table, recent trade cases
- **Trade Cases** — searchable case list
- **Document Processing wizard** — a full guided flow:
  1. Trade case details + drag-and-drop document upload
  2. Automated document identification (with confidence scores)
  3. Per-document field extraction, editable inline, with page/field source
     references (LC, Commercial Invoice, Bill of Lading, Cover Letter each
     have their own field schema)
  4. Human verification summary (documents, fields extracted, verified,
     requiring review)
  5. Trade Record consolidation — a single canonical trade object built from
     all documents, each value tagged with its source document
  6. Rule engine execution — animated processing steps, then a full results
     table (32 rules evaluated: 24 pass / 4 warnings / 4 alerts)
  7. Generated alerts summary
- **Rule Engine** — 11 named TBML rules (High Risk Country, Trade Value
  Threshold, Port/Country Mismatch, Partial Shipment Breach, Quantity
  Mismatch, Amount Mismatch, Country of Origin Mismatch, Product Description
  Mismatch, Customer/Product Profile Mismatch, Duplicate Invoice, Missing
  Certificate of Origin) with expandable descriptions, and the
  Document-Level / Cross-Document / Trade-Level rule category model
- **Alerts** — filterable alert queue with a slide-in investigation panel
  showing a plain-language "why was this alert generated" explanation,
  supporting evidence (source document / page / field / value), and
  investigate / assign / resolve / escalate actions
- **Case Management** — cases requiring action, open investigations
- **Customers** — customer/KYC-style reference table
- **Configuration** — live rule administration: enable/disable rules and edit
  thresholds
- **Audit Trail** — action log (uploads, edits, verifications, rule runs,
  alert status changes)
### Preloaded demo case: `TRD-2026-000182`
 
ABC Textiles Pvt Ltd (Applicant) / Global Trading LLC (Beneficiary), Cotton
Yarn, HS Code 5205. This case is intentionally constructed to trigger four
alerts:
 
| Rule | Issue |
|---|---|
| TBML-R002 | Trade value (USD 2,500,000) exceeds the USD 2,000,000 threshold |
| TBML-R003 | Port of Discharge (Jebel Ali, UAE) vs. declared Discharge Country (India) on the LC |
| TBML-R004 | LC prohibits partial shipment; B/L indicates partial shipment |
| TBML-R005 | Invoice quantity (10,000 KG) vs. B/L quantity (7,500 KG) — 25% variance vs. 10% tolerance |
 
---
 
## Tech stack
 
| Layer | Choice |
|---|---|
| UI library | React (function components, `useState` / `useMemo`) |
| Styling | Tailwind CSS (core utility classes only) |
| Icons | [lucide-react](https://lucide.dev/) |
| Build tool | Vite |
| State | Local component state only — no Redux/Zustand/Context |
| Data | Static mock JS objects/arrays — no API calls, no backend |
| Charts | Hand-built horizontal bar components (no charting library) |
 
## Project structure
 
```
├── src/
│   ├── App.tsx          # entire application (all pages, components, mock data)
│   ├── main.tsx          # React entry point
│   └── index.css         # Tailwind directives
├── tailwind.config.js
├── postcss.config.js
├── vite.config.ts
├── index.html
└── package.json
```
 
Everything — mock data, UI primitives (`Badge`, `SectionCard`, `StatCard`,
etc.), and all page components — lives in one file for easy portability
between sandboxes.
 
## Running locally
 
Requires [Node.js](https://nodejs.org) (LTS) and npm.
 
```bash
npm install
npm run dev
```
 
Then open the local URL Vite prints (typically `http://localhost:5173`).
 
### Dependencies to install if starting from a blank Vite + React template
 
```bash
npm install lucide-react
npm install -D tailwindcss postcss autoprefixer
```
 
`tailwind.config.js`:
```js
export default {
  content: ["./index.html", "./src/**/*.{js,jsx,ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```
 
`src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
 
## Deploying (free)
 
**Vercel (recommended)**
1. Push this project to a GitHub repository.
2. Go to [vercel.com](https://vercel.com) → sign in with GitHub.
3. **Add New → Project** → import the repo.
4. Framework preset: **Vite** (auto-detected). Build command `vite build`,
   output directory `dist`.
5. Deploy — you'll get a permanent `https://your-project.vercel.app` URL.
**Netlify** works the same way as an alternative.
 
## Customizing
 
- **Mock data** — edit the constants near the top of `App.tsx`
  (`TRADE_CASES`, `CUSTOMERS`, `FIELD_SCHEMAS`, `RULE_DEFINITIONS`,
  `RULE_EXECUTION_RESULTS`, `DEMO_ALERTS`, `INITIAL_AUDIT_LOG`, etc.)
- **Analyst name/avatar** — in the sidebar footer at the bottom of the `App`
  component (search for `Surajkumar Rai` / `PN`)
- **Rules** — add or edit entries in `RULE_DEFINITIONS` (shown on the Rule
  Engine and Configuration pages) and `RULE_EXECUTION_RESULTS` (shown after
  running TBML checks)
- **Color/severity conventions** — `severityTone`, `riskTone`, `statusTone`,
  and `resultTone` helper functions map values to Tailwind color badges
  (green = pass/low, amber = warning/medium, orange = high, red =
  critical/alert)
## Known limitations (by design, as a demo)
 
- No real OCR/LLM document parsing — extraction values are pre-scripted
- No persistence — all edits (field verification, alert status changes, rule
  config changes) reset on refresh
- Only the one full demo trade case has real extracted document data; other
  cases exist only as summary rows for table/list realism
- Document preview panel is a placeholder (no real PDF rendering)
---
 
Built as an internal demo prototype. Not for production use with real
customer or trade data.
 
