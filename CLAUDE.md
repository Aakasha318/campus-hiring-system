# Campus Hiring System

A Microsoft 365-first, AI-powered Campus Hiring Operating System for sourcing, engaging, assessing, hiring, and onboarding fresher talent at scale.

## Repository Purpose

This repo is the source of truth for the **Campus Hiring Operating System**: agent definitions, reusable skills, automation flows, data schemas, dashboards, and operational scripts. Anything related to campus recruitment workflow design, automation, analytics, or AI orchestration lives here.

## Ecosystem (non-negotiable)

All solutions must prioritize the **Microsoft 365 stack** before considering third-party tools.

| Layer | Default tooling |
|---|---|
| Collaboration | Microsoft Teams, Outlook, SharePoint, OneDrive, Loop, Bookings |
| Forms & lists | Microsoft Forms, Microsoft Lists, Planner |
| Automation | Power Automate, Power Apps, Logic Apps, Azure Functions, Graph API |
| AI / agents | Microsoft Copilot, Copilot Studio, Azure OpenAI, Semantic Kernel |
| Analytics | Power BI, Excel, Microsoft Fabric, Azure Data Factory, Dataverse |
| Data storage | SharePoint Lists, Dataverse, Excel Online, Azure SQL, Fabric Lakehouse |
| Knowledge mgmt | SharePoint, OneNote, Loop, Word Online |

Third-party tools (specialized ATS, coding assessment platforms, university engagement networks) may be proposed **only** when the Microsoft stack cannot meet the requirement. Even then, integration routes through Power Automate, Logic Apps, Graph API, or Azure APIs.

## Repository Structure

```
campus-hiring-system/
├── CLAUDE.md                    # This file — entry point and conventions
├── agents/                      # Subagent definitions (frontmatter + prompt)
│   ├── master-agent.md          # Orchestrator
│   ├── research-agent.md        # University & talent market intel
│   ├── operations-agent.md      # Hiring ops, scheduling, coordination
│   ├── analytics-agent.md       # Funnel, dashboards, forecasting
│   └── automation-agent.md      # Power Automate / Logic Apps builder
├── skills/                      # Reusable capability modules
│   ├── university-research.md
│   ├── hiring-analysis.md
│   └── outreach.md
├── workflows/
│   └── power-automate/          # Exported flow JSON, solution packages
├── data/                        # Schemas, sample data, Dataverse exports
├── dashboards/                  # Power BI .pbix, semantic models, themes
└── scripts/                     # PowerShell / Graph API / Python utilities
```

## Conventions

- **Agents** live in `agents/` as Markdown files with YAML frontmatter (`name`, `description`, `tools`, `model`).
- **Skills** live in `skills/` and are tool-agnostic capability docs callable by any agent.
- **Workflows** exported from Power Automate live in `workflows/power-automate/` as `.zip` solution packages or `.json` definitions — never hand-edit; re-export from the maker portal.
- **Dashboards** in `dashboards/` are `.pbix` files plus a sibling `README.md` describing data sources and refresh cadence.
- **Scripts** in `scripts/` default to **PowerShell** (`.ps1`) for Windows-native automation; use Python (`.py`) only when calling libraries unavailable in PowerShell.
- **Data** schemas in `data/` use JSON Schema or Dataverse table definitions. No production candidate PII committed to the repo — use anonymized samples.

## Operating Principles

1. **Automation-first** — every recurring manual task is a candidate for Power Automate.
2. **Single source of truth** — candidate data lives in **Dataverse** or **SharePoint Lists**, never duplicated.
3. **Executive-ready outputs** — reports must be Power BI-compatible and exportable to PowerPoint / Word.
4. **Security & governance** — follow tenant DLP policies; PII handling per the enterprise compliance baseline.
5. **No tool sprawl** — adding a new SaaS product requires written justification against the Microsoft-first policy.

## How agents collaborate

The **master-agent** is the orchestrator. It decomposes a campus hiring request and delegates to specialized agents:

- `research-agent` → university intel, course mapping, talent market signals
- `operations-agent` → SharePoint lists, Teams coordination, Bookings, candidate comms
- `analytics-agent` → Power BI dashboards, funnel metrics, conversion forecasts
- `automation-agent` → Power Automate flow design, Logic Apps, Graph API integrations

See each agent's file in `agents/` for capabilities, inputs, and outputs.
