# PROJECT_STATE.md — Living Run State

This file tracks the current state of the QA lifecycle for the project currently loaded in `INPUT/`/`PROJECT_CONTEXT.md`. It is updated at the end of every phase and every regression run (Phase 10). It is not a deliverable template like the ones in `TEMPLATES/` — it exists purely to let any session (human or agent) resume work without re-deriving context.

---

## Current Project

- **Project Name:** Book store
- **Project Type:** REST API
- **Spec Source:** Live Swagger UI — `https://fakerestapi.azurewebsites.net/index.html` (no static file committed yet; raw spec endpoint to be confirmed in Phase 0)
- **Environment:** UAT (shared, non-isolated)
- **Auth Scheme:** None — confirmed unauthenticated for testing by user on 2026-07-15 (contradicts stale Bearer Token note in `PROJECT_CONTEXT.md`; superseded by this confirmation)

## Lifecycle Progress

| Phase | Status | Date | Notes |
|---|---|---|---|
| 0 — Intake | Complete | 2026-07-15 | Spec fetched live and confirmed parseable; see Spec Version History and Known Gaps below |
| 1 — API Inventory | Complete | 2026-07-15 | 27/27 endpoints documented in `OUTPUT/API_Inventory.md` + `OUTPUT/endpoints/` |
| 2 — Business Workflow Mapping | Complete | 2026-07-15 | 8 workflows identified in `OUTPUT/workflows/`; all 27 endpoints traced to ≥1 workflow |
| 3 — Risk Assessment | Complete | 2026-07-15 | 11 risk entries in `OUTPUT/Risk_Assessment.md`, covering all 27 endpoints and 8 workflows; 3 Critical, 4 High, 2 Medium/Low groupings |
| 4 — Test Coverage Planning | Complete (approved) | 2026-07-15 | `OUTPUT/Test_Coverage.md` — 12 coverage items across all risk tiers; approved by mafreen@ariqt.com |
| 5 — Test Case Design | Complete | 2026-07-15 | 62 test cases consolidated into a single `OUTPUT/Test_Case.md` (one table per domain: ACTIVITY, AUTHOR, BOOK, COVERPHOTO, USER, CATALOG) — every coverage item's promised dimensions are exercised |
| 6 — Automation Planning | Complete | 2026-07-15 | `OUTPUT/Automation_Plan.md` — 35 test cases Automate Now, 27 Automate Later (rule: Positive/Idempotency → Now, Negative/Boundary/Security → Later pending ground truth or tooling); Postman/Newman on GitHub Actions (see Phase 7) |
| 7 — Automation Implementation | Complete | 2026-07-15 | `OUTPUT/automation/` — Postman collection (67 requests) + environment + `Automation_Collection_Notes.md`, covering all 35 Automate-Now test cases; executed via Newman against live UAT, 67/67 requests / 93/93 assertions passing. Newly formalized as its own WORKFLOW.md phase (was ad hoc under Phase 6) |
| 8 — Traceability Consolidation | Not Started | — | — |
| 9 — Execution & Reporting | Not Started | — | — |
| 10 — Regression Loop | N/A | — | Triggered only after an initial full pass exists |

## Spec Version History

| Fetched On | Source | Version/Title | Notes |
|---|---|---|---|
| 2026-07-15 | `https://fakerestapi.azurewebsites.net/swagger/v1/swagger.json` | OpenAPI 3.0.1 — "FakeRESTApi.Web V1", version `v1` | Saved to `INPUT/swagger.json` (27 endpoints across 5 resources: Activities, Authors, Books, CoverPhotos, Users). No `servers` array in spec (host = fetch origin). No `securitySchemes`/`security` defined anywhere in the spec. |

## Known Gaps / Open Questions

- **CRITICAL — the live UAT API is stateless (confirmed 2026-07-15 via Postman/Newman smoke-test against the real endpoint):** `POST`/`PUT`/`DELETE` are validated and echoed with `200` but **never actually persisted**. `GET` only ever serves a fixed, unchanging seed dataset (~ids 1–30 per resource) regardless of any writes — a freshly created record 404s on immediate `GET` by its own id, and a "deleted" pre-seeded record still returns `200` on `GET` immediately after. This means:
  - The "orphaning" Data Integrity findings (`RISK-BOOK-003`, `RISK-CATALOG-001`) describe what **would** be true for a real stateful implementation of this contract, but cannot be observed against this specific live instance since nothing is actually stored to orphan in the first place. See amendment notes added to `OUTPUT/Risk_Assessment.md`, `OUTPUT/Test_Case.md`, and `OUTPUT/Automation_Plan.md`.
  - All "verify via `GET` after create/update" assertions had to be redesigned to check request/response echo and seed-data behavior instead of persistence. The rebuilt Postman collection (`OUTPUT/automation/`) reflects this; it passes 67/67 requests, 93/93 assertions clean against the live API.
  - This does not change the framework's guidance for testing a real (stateful) implementation of this contract — it's a property of this specific FakeRestAPI demo instance, not a flaw in the QA deliverables' reasoning.
- ~~Auth discrepancy~~ — **Resolved 2026-07-15:** user confirmed the API is unauthenticated for testing purposes. All 27 endpoint docs reflect `Auth required: No`.
- UAT base URL placeholder unfilled in `PROJECT_CONTEXT.md`. Spec has no `servers` array, so base URL defaults to `https://fakerestapi.azurewebsites.net`.
- No test credentials needed given unauthenticated scope; Phase 9 execution readiness now depends only on environment/data-isolation confirmation for destructive (PUT/DELETE) test cases.
- **No documented error responses anywhere in the spec** (only `200` is defined on every operation) — flagged in `OUTPUT/API_Inventory.md` Notes & Ambiguities; error-path test cases in Phase 5 will need empirical verification against the live UAT instance rather than the contract.
- **`User.password` is a plaintext string field with no masking documented** — flagged as a Phase 3 risk-assessment candidate.
- **Two irregular nested paths** (`/Authors/authors/books/{idBook}`, `/CoverPhotos/books/covers/{idBook}`) verified as accurate to the live spec, not transcription errors.
- **`Book`, `POST/PUT Users`, singular `GET/PUT Users/{id}` operations have no documented response body schema** (spec shows bare `200`/"Success") — needs empirical confirmation of actual shape.
- **No transactional/cascade guarantees across Book → Author/CoverPhoto.** `WF-CATALOG-001` (the one real cross-domain business workflow) has no documented rollback if a multi-step creation partially fails, and `Book` deletion can orphan `Author`/`CoverPhoto` records referencing it via `idBook`. This is the highest-value data-integrity question to resolve before Phase 5.
- 8 workflows identified: `WF-ACTIVITY-001`, `WF-USER-001`, `WF-BOOK-001`, `WF-AUTHOR-001`, `WF-AUTHOR-002`, `WF-COVERPHOTO-001`, `WF-COVERPHOTO-002`, `WF-CATALOG-001` (cross-domain composite spanning Book+Author+CoverPhoto — the only genuine multi-resource business process this API supports).
- **3 Critical risks identified** (`RISK-BOOK-003` — Book delete orphaning; `RISK-USER-002` — User create/update/delete touching plaintext `password`; `RISK-CATALOG-001` — the full cross-domain publishing workflow with no transactional guarantee). These require the deepest coverage in Phase 4/5.
- Since the entire API is unauthenticated, risk tiers are driven by data sensitivity (`User.password`) and blast radius (Book-deletion orphaning, multi-domain `WF-CATALOG-001`) rather than auth criticality — noted explicitly in `OUTPUT/Risk_Assessment.md`.
- **Phase 4 approved 2026-07-15** — user confirmed to proceed to Phase 5; both proposed exclusions (Auth & Permissions dimension project-wide; rate-limiting/DoS security sub-checks) stand as approved.
- **Phase 5 complete:** 62 test cases authored per `TEMPLATES/Test_Case.md`'s fields, consolidated into a single `OUTPUT/Test_Case.md` (table format, one row per test case, grouped by domain) rather than one file per test case — changed at user request on 2026-07-15. All rows are `Status: Not Started` (no execution has occurred yet — that's Phase 9).
- **Destructive test cases requiring environment confirmation before execution:** all DELETE-based and many PUT-based test cases (e.g., TC-BOOK-007/010/011/012, TC-USER-011/012, TC-CATALOG-003) run against the shared UAT instance. Per `AGENT.md`, confirm environment/data-isolation scope before Phase 9 execution begins.
- **Key empirical unknowns to resolve during Phase 9 execution** (all flagged across Phases 1–5, no assumptions baked into pass/fail criteria): whether Book deletion orphans Author/CoverPhoto records (TC-BOOK-011/012, TC-CATALOG-003); whether `User.password` is actually exposed in responses (TC-USER-004); actual error status codes across all domains (spec documents only `200` everywhere).
- **Phase 6 complete:** 35 test cases classified Automate Now (all Positive/Idempotency types — self-contained fixtures, spec-confirmed assertions), 27 Automate Later (Negative/Boundary/Security types — blocked on Phase 9 cycle 1 ground truth or a payload-encoding tooling helper). Backlog targets: most 2026-08-01, high-priority Data Integrity findings (TC-AUTHOR-006, TC-CATALOG-002/004) and the Critical password-exposure check (TC-USER-004) pulled forward to 2026-07-25.
- **Phase 7 (Automation Implementation) formalized 2026-07-15:** what was originally an ad hoc extension of Phase 6 (building the Postman collection) is now its own numbered WORKFLOW.md phase, since the framework should always require executing an automation suite at least once — not just planning it — before moving on. `WORKFLOW.md`, `AGENT.md`, `STANDARDS.md`, `README.md`, and a new `TEMPLATES/Automation_Collection_Notes.md` were all updated. Phases formerly numbered 7/8/9 (Traceability/Execution/Regression) are now 8/9/10.

## Session Log

| Date | Action | By |
|---|---|---|
| 2026-07-15 | Framework initialized; PROJECT_STATE.md created fresh | mafreen@ariqt.com |
| 2026-07-15 | Phase 0 (Intake) complete — live spec fetched and saved to `INPUT/swagger.json`; auth discrepancy flagged for user confirmation | agent |
| 2026-07-15 | User confirmed API is unauthenticated for testing | mafreen@ariqt.com |
| 2026-07-15 | Phase 1 (API Inventory) complete — `OUTPUT/API_Inventory.md` + 27 endpoint docs under `OUTPUT/endpoints/` | agent |
| 2026-07-15 | Phase 2 (Business Workflow Mapping) complete — 8 workflows in `OUTPUT/workflows/`, all endpoint docs updated with Workflow(s) links | agent |
| 2026-07-15 | Phase 3 (Risk Assessment) complete — `OUTPUT/Risk_Assessment.md` with 11 risk entries; all endpoint and workflow docs updated with Risk tier links | agent |
| 2026-07-15 | Phase 4 (Test Coverage Planning) drafted — `OUTPUT/Test_Coverage.md` with 12 coverage items; awaiting user approval per WORKFLOW.md hard gate before Phase 5 | agent |
| 2026-07-15 | User approved Test Coverage Plan and directed to proceed to Phase 5 | mafreen@ariqt.com |
| 2026-07-15 | Phase 5 (Test Case Design) complete — 62 test cases across all 12 coverage items in `OUTPUT/test_cases/` | agent |
| 2026-07-15 | Consolidated 62 individual test case files into a single `OUTPUT/Test_Case.md` table per user request; `OUTPUT/test_cases/` directory removed | agent |
| 2026-07-15 | Phase 6 (Automation Planning) complete — `OUTPUT/Automation_Plan.md`, 35/27 Now/Later split, tooling and CI triggers defined | agent |
| 2026-07-15 | User requested Postman (not pytest) for the 35 Automate-Now test cases; built `OUTPUT/automation/AI_Mentor_Bot.postman_collection.json` + environment file | mafreen@ariqt.com / agent |
| 2026-07-15 | Smoke-tested the collection via Newman against the live UAT API — discovered the API is fully stateless (writes never persist); rewrote affected assertions to test request/response echo and seed-data behavior instead. User chose to fix assertions + add caveat notes to upstream docs rather than deep-revise them. Suite now passes 67/67 requests, 93/93 assertions | agent |
| 2026-07-15 | User asked to formalize Postman-collection generation as a framework phase. Added Phase 7 (Automation Implementation) to `WORKFLOW.md`, renumbered old Phases 7–9 to 8–10; updated `AGENT.md` (responsibility #7 + new operating principle), `STANDARDS.md` (naming + traceability rule), `README.md`; added `TEMPLATES/Automation_Collection_Notes.md`; retroactively completed Phase 7 for this project via `OUTPUT/automation/Automation_Collection_Notes.md` | mafreen@ariqt.com / agent |
