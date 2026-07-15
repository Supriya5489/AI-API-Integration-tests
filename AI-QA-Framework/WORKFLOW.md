# WORKFLOW.md — End-to-End QA Lifecycle

This document defines the step-by-step process the agent follows to go from a raw API contract to a fully tested, traceable, reported deliverable. Each phase has a defined input, activity, output template, and exit criteria. Phases are sequential but iterative — later findings may require revisiting earlier phases (e.g., a discovered workflow may reveal a missed endpoint).

---

## Phase 0 — Intake

**Input:** Files in `INPUT/` (`swagger.json`, `openapi.json`, or `swagger.yaml`)

**Activities:**
- Confirm which spec file(s) are authoritative (flag if multiple conflict).
- Validate the spec parses correctly (valid JSON/YAML, valid OpenAPI/Swagger version).
- Note base URL(s), global auth scheme(s), and spec version.

**Output:** None (validation gate only)

**Exit criteria:** Spec is parseable and its version/scope is confirmed with the user if ambiguous.

---

## Phase 1 — API Inventory

**Input:** Validated spec from Phase 0

**Activities:**
- Enumerate every endpoint: path, method, parameters, request/response schemas, status codes, auth requirements.
- Group endpoints by resource/domain area.

**Output:** `OUTPUT/API_Inventory.md` (from `TEMPLATES/API_Inventory.md`) + one `OUTPUT/endpoints/<name>.md` per endpoint (from `TEMPLATES/Endpoint.md`)

**Exit criteria:** Every path+method in the spec has a corresponding inventory row and endpoint doc.

---

## Phase 2 — Business Workflow Mapping

**Input:** API Inventory (Phase 1) + any available business context (docs, user input)

**Activities:**
- Identify multi-step sequences across endpoints that represent a real user/business process (e.g., signup → verify → login; order → pay → fulfill → refund).
- Note state dependencies and data that must flow between steps.

**Output:** `OUTPUT/Business_Workflow.md` entries (from `TEMPLATES/Business_Workflow.md`), one per identified workflow

**Exit criteria:** Every endpoint is mapped to at least one workflow, or explicitly marked as standalone/utility.

---

## Phase 3 — Risk Assessment

**Input:** Inventory + Workflows

**Activities:**
- Score each endpoint/workflow on: data sensitivity, auth/authz criticality, state-changing impact (create/update/delete/financial), and blast radius of failure.
- Assign a risk tier (e.g., Critical / High / Medium / Low).

**Output:** `OUTPUT/Risk_Assessment.md` (from `TEMPLATES/Risk_Assessment.md`)

**Exit criteria:** Every endpoint/workflow has an assigned risk tier with justification.

---

## Phase 4 — Test Coverage Planning

**Input:** Risk Assessment

**Activities:**
- Define what will be tested at each risk tier: functional, contract/schema validation, auth & permissions, negative/edge cases, boundary values, idempotency, error handling, data integrity.
- Explicitly state exclusions and the reason (e.g., "out of scope — third-party endpoint, no test credentials available").

**Output:** `OUTPUT/Test_Coverage.md` (from `TEMPLATES/Test_Coverage.md`)

**Exit criteria:** Coverage plan reviewed and approved by the user before test case authoring begins.

---

## Phase 5 — Test Case Design

**Input:** Approved Coverage Plan

**Activities:**
- Author individual test cases: preconditions, inputs, steps, expected results.
- Include positive, negative, boundary, and security-relevant scenarios per the coverage plan.
- Reference the endpoint(s) and workflow(s) each case validates.

**Output:** `OUTPUT/Test_Case.md` entries (from `TEMPLATES/Test_Case.md`), one per scenario

**Exit criteria:** Every item in the coverage plan has at least one corresponding test case.

---

## Phase 6 — Automation Planning

**Input:** Test Cases

**Activities:**
- Classify each test case as automate-now, automate-later, or manual/exploratory-only, with rationale.
- Recommend tooling, suggested CI stage/trigger, and required test data/environment setup.

**Output:** `OUTPUT/Automation_Plan.md` (from `TEMPLATES/Automation_Plan.md`)

**Exit criteria:** Every test case has an automation classification.

---

## Phase 7 — Automation Implementation

**Input:** Approved Automation Plan (the "Automate Now" classification from Phase 6)

**Activities:**
- Build an executable automation collection/suite covering every "Automate Now" test case, using the tool chosen in `Automation_Plan.md` (e.g., Postman/Newman, pytest + requests).
- Design each automated request/test to be self-contained: it provisions any fixture data it needs, exercises the endpoint(s) under test, and cleans up (e.g., deletes what it created) rather than depending on pre-existing external state — this keeps the suite safely re-runnable against a shared environment.
- **Validate by actually executing the suite** at least once against the target environment (e.g., via Newman CLI, `pytest`) before considering this phase complete. An unexecuted collection is not a valid deliverable.
- If the live run surfaces behavior that contradicts assumptions baked into earlier phases (e.g., undocumented persistence behavior, unexpected status codes, an endpoint that behaves differently than the spec implies), do not silently rewrite earlier conclusions — flag it explicitly and add a clearly labeled amendment note to the affected upstream docs (`Risk_Assessment.md`, `Test_Case.md`, `Automation_Plan.md`) so the discovery is traceable to when/how it was found.

**Output:** `OUTPUT/automation/` containing the executable artifacts (e.g., `<ProjectName>.postman_collection.json` + `<ProjectName>_<Env>.postman_environment.json`, or an equivalent pytest suite) plus `OUTPUT/automation/Automation_Collection_Notes.md` (from `TEMPLATES/Automation_Collection_Notes.md`) documenting the tool, structure, Test-Case-ID-to-request mapping, execution record, and any environment-behavior findings.

**Exit criteria:** Every "Automate Now" test case has a corresponding automated request/test; the suite has been run at least once against the target environment with real results recorded in `Automation_Collection_Notes.md`; any environment-behavior surprises are documented as amendments to affected upstream docs rather than left unreconciled.

---

## Phase 8 — Traceability Consolidation

**Input:** All prior phase outputs

**Activities:**
- Build/update the full traceability matrix: endpoint → workflow → risk tier → coverage item → test case → automation status → last result.
- Flag any broken chain (e.g., endpoint with no test case, test case with no risk linkage).

**Output:** `OUTPUT/Traceability.md` (from `TEMPLATES/Traceability.md`)

**Exit criteria:** No orphaned endpoints, workflows, or test cases; all chains complete or explicitly justified as excluded.

---

## Phase 9 — Execution & Reporting

**Input:** Test cases (manual or automated) + Traceability matrix

**Activities:**
- Execute test cases per the automation plan (or hand off to CI/automation tooling).
- Record pass/fail/blocked status per case.
- Summarize results, defects found, coverage achieved vs. planned, and residual risk.

**Output:** `OUTPUT/Test_Report.md` (from `TEMPLATES/Test_Report.md`)

**Exit criteria:** Report delivered; traceability matrix updated with latest results.

---

## Phase 10 — Regression / Maintenance Loop

**Trigger:** Spec change in `INPUT/`, new business workflow, or scheduled re-run.

**Activities:**
- Diff the new spec against the last inventoried version.
- Re-run Phases 1–3 only for changed/added endpoints (delta analysis), then update coverage, test cases, automation plan, automation collection, and traceability accordingly.

**Output:** Updated versions of all affected `OUTPUT/` artifacts.

**Exit criteria:** Traceability matrix reflects the current spec version with no gaps introduced by the change.

---

## Deliverables Summary

| Phase | Deliverable | Template |
|---|---|---|
| 1 | API Inventory + Endpoint docs | `API_Inventory.md`, `Endpoint.md` |
| 2 | Business Workflows | `Business_Workflow.md` |
| 3 | Risk Assessment | `Risk_Assessment.md` |
| 4 | Test Coverage Plan | `Test_Coverage.md` |
| 5 | Test Cases | `Test_Case.md` |
| 6 | Automation Plan | `Automation_Plan.md` |
| 7 | Automation Collection (executable) + Notes | `Automation_Collection_Notes.md` |
| 8 | Traceability Matrix | `Traceability.md` |
| 9 | Test Report | `Test_Report.md` |

Each phase's output feeds the next; skipping a phase or template requires an explicit, documented justification in `Test_Coverage.md`.
