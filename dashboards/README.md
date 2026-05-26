# Dashboards

Power BI assets for the Campus Hiring Operating System.

## What lives here

- `.pbix` files — Power BI Desktop reports
- `.pbit` files — Power BI templates (parameterized, no data)
- Semantic model definitions (TMDL or `.bim`) for source control
- Custom theme JSON (corporate palette)
- `metrics-glossary.md` — canonical definitions for every metric (the single source of truth)

## Workspaces

| Workspace | Purpose | Audience | Refresh |
|---|---|---|---|
| `Campus Hiring — Exec` | Leadership scorecard | CHRO, business heads | Hourly |
| `Campus Hiring — Ops` | Day-to-day recruiter dashboard | Recruiters, ops team | 15 min |
| `University Scorecard` | Per-university ROI | Campus team | Daily |
| `Diversity Lens` | DEI funnel view | DEI council | Weekly |
| `Joiner Forecast` | 30/60/90-day joining forecast | Workforce planning | Daily |

## Semantic model

The single shared semantic model is `Campus Hiring Core`. All reports connect to it via a live connection — never import duplicate data into a `.pbix`.

## Conventions

- Reports follow the corporate Power BI theme (file: `corporate-theme.json`)
- Every chart has a 1-sentence "so what" caption
- Every page has a "Data refreshed: <timestamp>" footer
- Aggregated views only — no individual candidate PII visible
- Charts with n < 30 underlying records are labeled "low confidence"

## Distribution

- Power BI Apps per workspace, with row-level security where applicable
- Teams tab embeds in `#campus-hiring-leadership` and `#campus-hiring-ops`
- Weekly subscription email of the exec scorecard to leadership distro
