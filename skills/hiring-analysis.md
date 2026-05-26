---
name: hiring-analysis
description: Reusable capability for funnel analytics, conversion analysis, diversity reporting, recruiter productivity, hiring forecasts, and university-performance scoring. Callable by any agent that needs to compute or visualize hiring metrics; primarily used by analytics-agent.
---

# Skill: Hiring Analysis

A repeatable workflow for turning operational hiring data into decision-grade metrics, dashboards, and forecasts inside Microsoft Fabric / Power BI.

## When to invoke

- Stakeholder asks "what's the number for X?"
- Weekly / monthly business review preparation
- A new requisition needs a baseline projection
- A new university needs to be evaluated for ROI
- An existing dashboard needs to be refreshed or extended

## Inputs required

| Input | Required? | Example |
|---|---|---|
| Time window | Yes | "Last 90 days", "FY26 YTD" |
| Slice dimension | No | "by university", "by recruiter", "by role family" |
| Comparison baseline | No | "vs same period last year", "vs target" |

## Canonical metric set (do not redefine — pull from `dashboards/metrics-glossary.md`)

| Metric | Formula | Source |
|---|---|---|
| Time-to-fill | Days from req opened to offer accepted | `Candidates` |
| Offer acceptance rate | offers accepted / offers extended | `Offers` |
| Joining rate | joined / offers accepted | `Joiners` |
| Renege rate | 1 − joining rate | derived |
| Yield ratio | hires / applications | `Candidates` |
| 90-day retention | active at d90 / joined | `Joiners` + HRIS |
| Cost per hire | (recruiter cost + travel + tooling + offer differential) / hires | finance + ops |

## Procedure

1. **Identify the question** — confirm metric, time window, slice, and baseline.
2. **Source the data** — query the **Fabric Lakehouse gold layer** (`hiring_gold` schema). Never query operational SharePoint lists directly for published reports.
3. **Validate freshness** — confirm last refresh timestamp is within SLA (24h for ops dashboards, 1h for exec dashboards).
4. **Compute** — pull from the Power BI semantic model `Campus Hiring Core` if a measure already exists. Only create a new DAX measure when nothing fits.
5. **Sanity-check** — compare to last period; flag deltas > 25% for investigation before publishing.
6. **Visualize** — match the visual to the question: line for trend, bar for ranked compare, funnel for stage drop-off, scatter for two-dimension trade-offs.
7. **Annotate** — every chart gets a 1-sentence "so what" caption.

## Output format

```
## <Metric name> — <time window>
- Value: <number> (Target: <number> | Prior: <number> | Δ: <±%>)
- Source: Fabric Lakehouse `hiring_gold.<table>`, refreshed <ISO timestamp>
- Confidence: <high / medium / low>
- Power BI: <workspace> > <report> > <page>
- So what: <one sentence>
```

## Standard dashboards to maintain

| Dashboard | Audience | Refresh |
|---|---|---|
| `Campus Hiring — Exec` | Leadership | Hourly |
| `Campus Hiring — Ops` | Recruiters | 15 min |
| `University Scorecard` | Campus team | Daily |
| `Diversity Lens` | DEI council | Weekly |
| `Joiner Forecast` | Workforce planning | Daily |

## Constraints

- Aggregated views only — never publish an individual candidate's PII on a dashboard.
- Metric definitions are canonical — propose changes via a pull request, never silently redefine.
- Mark any chart with n < 30 underlying records as "low confidence" in the title.
- Use the corporate Power BI theme — never custom colors.
