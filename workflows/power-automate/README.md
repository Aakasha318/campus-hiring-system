# Power Automate Workflows

Exported Power Automate flow solutions for the Campus Hiring Operating System.

## What lives here

- **Solution packages** (`.zip`) exported from the Power Platform maker portal
- **Flow definition JSON** for source-controlled review
- **Connection reference manifests** documenting required identities and scopes

## Conventions

- File naming: `<solution>_<flow-name>_<env>.zip` — e.g., `CampusOps_NewApplicationIntake_prod.zip`
- One subfolder per Power Platform Solution
- Every flow registered in the `FlowRegistry` SharePoint list (name, owner, purpose, last reviewed)
- Never hand-edit exported JSON — re-export from the maker portal after changes

## Promotion path

1. Build & test in **dev** environment (`<tenant>-dev`)
2. Export solution as **managed**
3. Import to **test** environment, run UAT
4. Import to **prod** environment via Power Platform Pipelines
5. Commit the prod export to this folder

## Standard flow inventory

See `agents/automation-agent.md` for the canonical pattern list. Each pattern should have an exported solution in this folder once built.

## DLP & governance

- All connectors used must be in the tenant's Business connector group
- Premium connectors require capacity approval from IT
- No HTTP connector usage without an approved exception
