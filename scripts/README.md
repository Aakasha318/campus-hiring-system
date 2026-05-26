# Scripts

PowerShell, Graph API, and Python utilities supporting the Campus Hiring Operating System.

## What lives here

- **PowerShell** (`.ps1`) — default for Windows-native automation, Graph calls, SharePoint provisioning
- **Python** (`.py`) — only when calling libraries unavailable in PowerShell (ML, advanced data wrangling)
- **Bicep / ARM** — Azure resource provisioning for Logic Apps, Functions, Key Vault, Storage

## Naming convention

`<verb>-<noun>[-<scope>].ps1` — e.g., `provision-sharepoint-lists.ps1`, `export-candidates-snapshot.ps1`

## Conventions

- Every script begins with a comment block: purpose, author, last-tested date, required scopes
- Authenticate via **managed identity** (when running in Azure) or **app registration with certificate** (never client secrets in source)
- Pull secrets from Azure Key Vault — never hardcode
- Idempotent by default — running twice produces the same end state
- Log to a transcript file in `$env:TEMP\campus-hiring-scripts\`
- Exit code 0 on success, non-zero on failure

## Typical scripts

| Script | Purpose |
|---|---|
| `provision-sharepoint-lists.ps1` | Create the canonical SharePoint List schema in a new site |
| `bulk-load-universities.ps1` | Seed the `Universities` list from a CSV |
| `export-candidates-snapshot.ps1` | Export `Candidates` to Fabric Lakehouse gold layer |
| `audit-orphan-flows.ps1` | Flag Power Automate flows missing a `FlowRegistry` entry |
| `rotate-app-secrets.ps1` | Rotate app registration secrets and update Key Vault |

## Hard constraints

- No production candidate PII written to local disk outside `$env:TEMP` (auto-cleaned)
- No credentials in source — flagged by pre-commit hook
- No `Invoke-Expression` on untrusted input
