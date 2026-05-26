# Data

Schemas, sample data, and Dataverse / SharePoint List definitions for the Campus Hiring Operating System.

## What lives here

- **Schema definitions** — JSON Schema files for SharePoint Lists, Dataverse table XML, or YAML descriptions
- **Sample data** — anonymized CSV / JSON for development and testing
- **Migration scripts** — PowerShell or Python helpers for bulk loading (placed in `../scripts/`)

## Canonical entities

| Entity | Storage | Owner |
|---|---|---|
| Candidates | Dataverse table `cr_candidate` | Operations Agent |
| Interviews | SharePoint List `Interviews` | Operations Agent |
| Offers | Dataverse table `cr_offer` | Operations Agent |
| Joiners | SharePoint List `Joiners` | Operations Agent |
| Universities | SharePoint List `Universities` | Research Agent |
| FlowRegistry | SharePoint List `FlowRegistry` | Automation Agent |
| OutreachLog | SharePoint List `OutreachLog` | Operations Agent |
| AuditLog | Dataverse table `cr_auditlog` | All agents (append-only) |

## PII policy

- **No production candidate PII** in this folder, ever.
- Sample data must be synthetic or anonymized (replace names, emails, phone numbers, university IDs).
- Schemas are safe to commit.
- Pull request reviewers must reject any commit containing real candidate data.

## Schema conventions

- All entities have: `Id`, `CreatedAt`, `CreatedBy`, `ModifiedAt`, `ModifiedBy`
- Stage / status columns use a choice column with documented values
- Foreign keys use lookup columns referencing the canonical entity
- All datetimes stored in UTC; display layer converts to IST
