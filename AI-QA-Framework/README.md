# AI-QA-Framework

An AI-driven framework for testing API integrations. Given an OpenAPI/Swagger contract, it produces a complete, traceable QA package — inventory, business workflow mapping, risk assessment, test coverage plan, test cases, automation plan, an executable automation collection, traceability matrix, and test report — using a consistent, reusable set of templates.

It is project-agnostic: point it at any API spec and it produces the same structured deliverables, scaled to that API's size and risk profile.

---

## How It Works

1. **You provide** an API contract in `INPUT/` (`swagger.json`, `openapi.json`, or `swagger.yaml`).
2. **The agent** (governed by [`AGENT.md`](AGENT.md)) reads the contract and works through the lifecycle defined in [`WORKFLOW.md`](WORKFLOW.md).
3. **Every artifact** it produces goes into `OUTPUT/`, built from the templates in `TEMPLATES/`, and follows the naming, ID, priority, severity, and traceability rules in [`STANDARDS.md`](STANDARDS.md).

The result is a fully traceable chain for every endpoint:

```
Endpoint → Business Workflow → Risk Tier → Coverage Item → Test Case → Automation Status → Automation Collection → Result
```

## Core Documents

| Document | Purpose |
|---|---|
| [`AGENT.md`](AGENT.md) | Defines the AI's role, responsibilities, and constraints while testing |
| [`WORKFLOW.md`](WORKFLOW.md) | The end-to-end QA lifecycle, phase by phase, with deliverables |
| [`STANDARDS.md`](STANDARDS.md) | Naming, ID, priority, severity, and formatting conventions all artifacts must follow |
| `TEMPLATES/` | The reusable document structures every `OUTPUT/` artifact is built from |

## Folder Structure

```
AI-QA-Framework/
├── README.md            # This file
├── AGENT.md              # AI role, responsibilities, constraints
├── WORKFLOW.md           # Step-by-step QA lifecycle
├── STANDARDS.md          # Conventions and review checklist
├── TEMPLATES/            # Blank templates for every deliverable
│   ├── API_Inventory.md
│   ├── Endpoint.md
│   ├── Business_Workflow.md
│   ├── Risk_Assessment.md
│   ├── Test_Coverage.md
│   ├── Test_Case.md
│   ├── Automation_Plan.md
│   ├── Automation_Collection_Notes.md
│   ├── Test_Report.md
│   └── Traceability.md
├── OUTPUT/               # Generated deliverables for the current project
│   └── automation/       # Executable automation artifacts (e.g., Postman collection + environment, or a pytest suite) + Automation_Collection_Notes.md
└── INPUT/                # The API spec(s) under test
    ├── swagger.json
    ├── openapi.json
    └── swagger.yaml
```

## Using This Framework for a New Project

1. **Drop your spec in `INPUT/`.** Only one spec should be authoritative per run — if multiple formats are present, confirm which one is current.
2. **Run the lifecycle.** Work through `WORKFLOW.md` Phases 0–9 in order:
   - Phase 0–1: Validate the spec and build the API inventory.
   - Phase 2–3: Map business workflows and assess risk.
   - Phase 4: Get the test coverage plan reviewed/approved before writing test cases.
   - Phase 5–6: Write test cases and classify them for automation.
   - Phase 7: Build and execute the automation collection for every "Automate Now" test case — a plan without a validated, runnable collection isn't done.
   - Phase 8–9: Consolidate traceability, execute, and report.
3. **Check outputs against `STANDARDS.md`** before considering any deliverable final — naming, IDs, priority/severity labels, and traceability links must all comply.
4. **Re-run on spec changes.** When the API contract changes, use Phase 10 (Regression Loop) to update only the affected endpoints/workflows rather than redoing everything from scratch.

## Reusing Across Projects

This framework is not tied to any single API. To start a new project:

- Clear or archive the current contents of `INPUT/` and `OUTPUT/`.
- Drop in the new project's spec.
- Re-run the lifecycle — `AGENT.md`, `WORKFLOW.md`, `STANDARDS.md`, and `TEMPLATES/` stay unchanged and apply as-is.

Each project's `OUTPUT/` should be archived or version-controlled separately if you need historical results preserved across spec versions.
