---
name: outreach
description: Reusable capability for multi-channel candidate and university outreach — Outlook campaigns, Teams messages, Bookings invitations, Forms collection, and personalized nudge sequences. Callable by any agent that needs to communicate with candidates, placement cells, or panelists; primarily used by operations-agent.
---

# Skill: Outreach

A repeatable workflow for sending the right message to the right audience through the right Microsoft 365 channel — with personalization, tracking, and audit trail.

## When to invoke

- New job posting needs to be broadcast to a shortlisted university set
- A candidate cohort needs a status update (shortlisted, rejected, offer extended)
- Placement cell coordinator needs a recurring touchpoint
- Interview panelists need a slot-confirmation nudge
- Joiners need a pre-onboarding checklist reminder

## Inputs required

| Input | Required? | Example |
|---|---|---|
| Audience segment | Yes | "All FY26 SDE-1 offer-stage candidates from Tier-1 universities" |
| Channel | Yes | Outlook / Teams / Bookings / Forms |
| Message intent | Yes | Status update / invite / reminder / collection |
| Personalization tokens | No | First name, university, role |
| Send window | No | "Tomorrow 10:00 IST" |

## Channel selection rules

| Intent | Preferred channel | Why |
|---|---|---|
| Formal status updates, offers, rejections | Outlook | On-record, attachable, signature-bearing |
| Real-time coordination with panel / recruiters | Teams | Quick, threaded, presence-aware |
| Self-service slot booking | Bookings | No scheduling ping-pong |
| Document / answer collection | Forms | Structured, auto-flows into SharePoint |
| Cohort newsletter | Outlook + SharePoint News | Searchable, persistent |

## Procedure

1. **Define the audience** as a query against the `Candidates` SharePoint list (or `Universities` / `Joiners`). Store the segment definition, not a static list.
2. **Pick the channel** using the rules above.
3. **Compose with templates** — pull from `/Sites/CampusHiring/Templates/`. Never write a one-off if a template covers it.
4. **Personalize** via merge fields. Validate every token resolves for every recipient. If any token is null, hold the send.
5. **Send via Power Automate** — never a manual blast. The flow logs to `OutreachLog` SharePoint list.
6. **Track** — opens/clicks for Outlook campaigns via Viva Engage analytics or Customer Voice; replies routed to a shared mailbox monitored by ops.
7. **Throttle** — never > 25 individual emails in 5 minutes from a single mailbox; use shared mailbox + send-as for larger sends.

## Personalization token catalog

```
{{FirstName}}, {{LastName}}, {{University}}, {{Role}},
{{InterviewDate}}, {{PanelLead}}, {{OfferCTC}}, {{JoiningDate}},
{{RecruiterName}}, {{RecruiterEmail}}, {{BookingURL}}
```

## Output format when invoked

```
## Outreach: <campaign name>
- Audience: <segment definition>, n = <count>
- Channel: <Outlook / Teams / Bookings / Forms>
- Template: <path>
- Send time: <ISO timestamp>
- Power Automate flow: <flow name>
- Logged in: `OutreachLog` row IDs <start–end>
- Reversible? <yes/no — how>
```

## Hard constraints

- No mass send > 25 recipients without explicit human approval recorded in the audit trail.
- No personalization token left unresolved — hold the send.
- All outbound mail uses the corporate signature block and the appropriate sensitivity label.
- Never send from an individual's mailbox for cohort campaigns — always shared mailbox.
- Honor unsubscribe / quiet-period flags on the candidate record.
