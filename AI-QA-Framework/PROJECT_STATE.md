# PROJECT_STATE.md — Living Run State

This file tracks the current state of the QA lifecycle for the project currently loaded in `INPUT/`/`PROJECT_CONTEXT.md`. It is updated at the end of every phase and every regression run (Phase 9). It is not a deliverable template like the ones in `TEMPLATES/` — it exists purely to let any session (human or agent) resume work without re-deriving context.

---

## Current Project

- **Project Name:** AI Mentor Bot
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
| 4 — Test Coverage Planning | Not Started | — | Requires user approval before Phase 5 |
| 5 — Test Case Design | Not Started | — | — |
| 6 — Automation Planning | Not Started | — | — |
| 7 — Traceability Consolidation | Not Started | — | — |
| 8 — Execution & Reporting | Not Started | — | — |
| 9 — Regression Loop | N/A | — | Triggered only after an initial full pass exists |

## Spec Version History

| Fetched On | Source | Version/Title | Notes |
|---|---|---|---|
| 2026-07-15 | `https://fakerestapi.azurewebsites.net/swagger/v1/swagger.json` | OpenAPI 3.0.1 — "FakeRESTApi.Web V1", version `v1` | Saved to `INPUT/swagger.json` (27 endpoints across 5 resources: Activities, Authors, Books, CoverPhotos, Users). No `servers` array in spec (host = fetch origin). No `securitySchemes`/`security` defined anywhere in the spec. |

## Known Gaps / Open Questions

- ~~Auth discrepancy~~ — **Resolved 2026-07-15:** user confirmed the API is unauthenticated for testing purposes. All 27 endpoint docs reflect `Auth required: No`.
- UAT base URL placeholder unfilled in `PROJECT_CONTEXT.md`. Spec has no `servers` array, so base URL defaults to `https://fakerestapi.azurewebsites.net`.
- No test credentials needed given unauthenticated scope; Phase 8 execution readiness now depends only on environment/data-isolation confirmation for destructive (PUT/DELETE) test cases.
- **No documented error responses anywhere in the spec** (only `200` is defined on every operation) — flagged in `OUTPUT/API_Inventory.md` Notes & Ambiguities; error-path test cases in Phase 5 will need empirical verification against the live UAT instance rather than the contract.
- **`User.password` is a plaintext string field with no masking documented** — flagged as a Phase 3 risk-assessment candidate.
- **Two irregular nested paths** (`/Authors/authors/books/{idBook}`, `/CoverPhotos/books/covers/{idBook}`) verified as accurate to the live spec, not transcription errors.
- **`Book`, `POST/PUT Users`, singular `GET/PUT Users/{id}` operations have no documented response body schema** (spec shows bare `200`/"Success") — needs empirical confirmation of actual shape.
- **No transactional/cascade guarantees across Book → Author/CoverPhoto.** `WF-CATALOG-001` (the one real cross-domain business workflow) has no documented rollback if a multi-step creation partially fails, and `Book` deletion can orphan `Author`/`CoverPhoto` records referencing it via `idBook`. This is the highest-value data-integrity question to resolve before Phase 5.
- 8 workflows identified: `WF-ACTIVITY-001`, `WF-USER-001`, `WF-BOOK-001`, `WF-AUTHOR-001`, `WF-AUTHOR-002`, `WF-COVERPHOTO-001`, `WF-COVERPHOTO-002`, `WF-CATALOG-001` (cross-domain composite spanning Book+Author+CoverPhoto — the only genuine multi-resource business process this API supports).
- **3 Critical risks identified** (`RISK-BOOK-003` — Book delete orphaning; `RISK-USER-002` — User create/update/delete touching plaintext `password`; `RISK-CATALOG-001` — the full cross-domain publishing workflow with no transactional guarantee). These require the deepest coverage in Phase 4/5.
- Since the entire API is unauthenticated, risk tiers are driven by data sensitivity (`User.password`) and blast radius (Book-deletion orphaning, multi-domain `WF-CATALOG-001`) rather than auth criticality — noted explicitly in `OUTPUT/Risk_Assessment.md`.

## Session Log

| Date | Action | By |
|---|---|---|
| 2026-07-15 | Framework initialized; PROJECT_STATE.md created fresh | mafreen@ariqt.com |
| 2026-07-15 | Phase 0 (Intake) complete — live spec fetched and saved to `INPUT/swagger.json`; auth discrepancy flagged for user confirmation | agent |
| 2026-07-15 | User confirmed API is unauthenticated for testing | mafreen@ariqt.com |
| 2026-07-15 | Phase 1 (API Inventory) complete — `OUTPUT/API_Inventory.md` + 27 endpoint docs under `OUTPUT/endpoints/` | agent |
| 2026-07-15 | Phase 2 (Business Workflow Mapping) complete — 8 workflows in `OUTPUT/workflows/`, all endpoint docs updated with Workflow(s) links | agent |
| 2026-07-15 | Phase 3 (Risk Assessment) complete — `OUTPUT/Risk_Assessment.md` with 11 risk entries; all endpoint and workflow docs updated with Risk tier links | agent |
