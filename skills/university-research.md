---
name: university-research
description: Reusable capability for producing decision-grade university intelligence — shortlists, profile briefs, course-to-role mapping, ranking translation, competitor signal capture. Callable by any agent that needs campus intel; primarily used by research-agent.
---

# Skill: University Research

A repeatable workflow for turning a hiring requisition into a ranked, defensible university shortlist plus per-university intelligence briefs.

## When to invoke

- A new hiring requisition has been opened and target campuses are not yet defined.
- An existing campus list needs to be refreshed (default cadence: every 6 months).
- A new role family is being added to the campus program.
- Leadership requests a competitor-campus comparison.

## Inputs required

| Input | Required? | Example |
|---|---|---|
| Role family | Yes | "SDE-1 freshers", "Data Analyst trainees" |
| Geography | Yes | "Pan-India", "Bangalore + Hyderabad" |
| Tier preference | Yes | "Tier-1 only", "Tier-1 + Tier-2", "Broad" |
| Headcount target | Yes | "120 over FY26" |
| Package band | No | "8–14 LPA" |
| Diversity targets | No | "Min 35% female representation in shortlist" |

If any "Yes" input is missing, stop and ask the user.

## Procedure

1. **Pull internal history** — query SharePoint List `Universities` and Dataverse `HiringHistory` for universities that hired into the same role family in the last 3 years. Compute yield ratio, acceptance rate, joining rate, 90-day retention per university.
2. **Apply filters** — geography, tier, course availability, diversity baseline.
3. **Add fresh candidates** — search public sources (NIRF, AICTE, university sites) for universities matching the criteria that aren't in internal history.
4. **Score each university** on a 1–10 scale:
   - 30% historical yield (where available)
   - 25% course relevance & batch size
   - 20% ranking & accreditation
   - 15% geographic / logistic fit
   - 10% diversity profile
5. **Generate per-university brief** using the template below.
6. **Persist** — write to SharePoint List `Campus Shortlist – <RequisitionID>`, generate a Word brief in `/Sites/CampusHiring/Briefs/`.

## Per-university brief template

```
# <University Name>
- Tier: <1 / 2 / 3>
- Location: <city, state>
- Recognition: <NIRF rank, NAAC grade, NBA accreditation>
- Relevant programs: <degree — intake — duration>
- Placement cell: <head name, public email/phone>
- Hiring window: <typical months>
- Historical performance with us:
  - Applications: <n>, Offers: <n>, Joined: <n>, 90d retention: <%>
- Last visited: <YYYY-MM-DD>
- Fit score: <x/10>
- Rationale: <3 bullets>
- Risks/notes: <exclusivity windows, scheduling friction, etc.>
```

## Outputs

- SharePoint List rows in `Campus Shortlist – <RequisitionID>`
- Word brief per university in `/Sites/CampusHiring/Briefs/<RequisitionID>/`
- Summary deck (optional) via the pptx skill

## Constraints

- Public sources only — no scraping of authenticated portals.
- Mark any field with sources older than 18 months as `[NEEDS REFRESH]`.
- Never fabricate placement statistics, ranking positions, or contact details.
