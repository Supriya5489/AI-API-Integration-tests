# Automation Plan — Book store

**Planned on:** 2026-07-15
**Planned by:** agent

---

## Classification Policy

Rather than judge each of the 62 test cases individually, classification follows a rule tied to each test case's `Type` field in `OUTPUT/Test_Case.md`, since that field already captures the deciding factor — whether the expected result can be asserted with confidence today:

| Type | Classification | Why |
|---|---|---|
| Positive | Automate Now | Self-contained (creates its own fixture data) and asserts against spec-confirmed behavior (`200` + documented/persistence-verifiable outcome). |
| Idempotency | Automate Now | Asserts consistency/no-crash behavior across repeated calls — checkable without knowing an exact undocumented status code. |
| Negative | Automate Later | Expected status code is **not documented anywhere in the spec** (only `200` is defined project-wide, per Phase 1). Automating now means hard-coding a guess; instead, Phase 8's first exploratory execution establishes the real behavior as ground truth, then the assertion is codified. |
| Boundary | Automate Later | Same reasoning as Negative — most boundary cases here hinge on an unconfirmed status code for out-of-range/malformed input. |
| Security | Automate Later | Same ground-truth-first reasoning, plus a small setup cost: safely constructing and URL-encoding malicious payloads needs a shared helper in the chosen tooling before these are added to the regression suite. |

This is a one-time-per-test-case rule, not a per-field judgment call — it keeps the plan consistent and makes re-classification after Phase 8 cycle 1 mechanical (every current "Automate Later" item becomes a candidate for "Automate Now" once its actual behavior is recorded).

---

## Automation Classification

### ACTIVITY Domain

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-ACTIVITY-001 | P3 | Automate Now | Positive — list read, spec-confirmed 200 |
| TC-ACTIVITY-002 | P3 | Automate Now | Positive — get-by-id read, spec-confirmed 200 |
| TC-ACTIVITY-003 | P1 | Automate Now | Positive — create with self-contained fixture, spec-confirmed schema |
| TC-ACTIVITY-004 | P1 | Automate Later | Negative — malformed `dueDate` expected status undocumented |
| TC-ACTIVITY-005 | P1 | Automate Later | Negative — `additionalProperties` rejection status undocumented |
| TC-ACTIVITY-006 | P1 | Automate Now | Positive — update with self-contained fixture |
| TC-ACTIVITY-007 | P1 | Automate Later | Negative — update-nonexistent status undocumented |
| TC-ACTIVITY-008 | P1 | Automate Now | Idempotency — consistency check, no unconfirmed status dependency |
| TC-ACTIVITY-009 | P1 | Automate Now | Positive — delete of self-created fixture |
| TC-ACTIVITY-010 | P1 | Automate Later | Negative — delete-nonexistent status undocumented |

### AUTHOR Domain

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-AUTHOR-001 | P3 | Automate Now | Positive — list read |
| TC-AUTHOR-002 | P3 | Automate Now | Positive — get-by-id read |
| TC-AUTHOR-003 | P3 | Automate Now | Positive — book-lookup read |
| TC-AUTHOR-004 | P1 | Automate Now | Positive — create with self-contained fixture |
| TC-AUTHOR-005 | P1 | Automate Later | Negative — malformed payload status undocumented |
| TC-AUTHOR-006 | P1 | Automate Later | Negative — non-existent `idBook` status undocumented, though the **finding itself is high-priority** (see backlog target date) |
| TC-AUTHOR-007 | P1 | Automate Now | Positive — update with self-contained fixture |
| TC-AUTHOR-008 | P1 | Automate Later | Negative — update-nonexistent status undocumented |
| TC-AUTHOR-009 | P1 | Automate Now | Positive — delete of self-created fixture |
| TC-AUTHOR-010 | P1 | Automate Later | Negative — delete-nonexistent status undocumented |

### BOOK Domain

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-BOOK-001 | P3 | Automate Now | Positive — list read |
| TC-BOOK-002 | P3 | Automate Now | Positive — get-by-id read |
| TC-BOOK-003 | P1 | Automate Now | Positive — create; initial assertion limited to status+persistence since response body schema is undocumented (see Automate-Later note for full-body assertion) |
| TC-BOOK-004 | P1 | Automate Later | Negative — malformed payload status undocumented |
| TC-BOOK-005 | P1 | Automate Now | Positive — update with self-contained fixture |
| TC-BOOK-006 | P1 | Automate Later | Negative — update-nonexistent status undocumented |
| TC-BOOK-007 | P0 | Automate Now | Positive — delete of self-created fixture with no dependents |
| TC-BOOK-008 | P0 | Automate Later | Negative — delete-nonexistent status undocumented |
| TC-BOOK-009 | P0 | Automate Later | Boundary — most sub-cases (0/-1/int32-max) hinge on the same undocumented non-existent-id status; only the overflow sub-case has a confirmed expectation |
| TC-BOOK-010 | P0 | Automate Now | Idempotency — core assertion is "no crash on repeat," checkable without the exact 2nd-call status |
| TC-BOOK-011 | P0 | Automate Now | Positive/Data Integrity — self-contained fixture (book+author); this is the **highest-priority automated regression check** in the whole suite once written, since it guards the Critical orphaning risk |
| TC-BOOK-012 | P0 | Automate Now | Positive/Data Integrity — mirrors TC-BOOK-011 for CoverPhotos |
| TC-BOOK-013 | P0 | Automate Later | Security — needs the shared malicious-payload/URL-encoding helper before automating |

### COVERPHOTO Domain

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-COVERPHOTO-001 | P3 | Automate Now | Positive — list read |
| TC-COVERPHOTO-002 | P3 | Automate Now | Positive — get-by-id read |
| TC-COVERPHOTO-003 | P3 | Automate Now | Positive — book-lookup read |
| TC-COVERPHOTO-004 | P1 | Automate Now | Positive — create with self-contained fixture |
| TC-COVERPHOTO-005 | P1 | Automate Later | Negative — malformed payload status undocumented |
| TC-COVERPHOTO-006 | P1 | Automate Later | Negative — non-existent `idBook` status undocumented |
| TC-COVERPHOTO-007 | P1 | Automate Now | Positive — update with self-contained fixture |
| TC-COVERPHOTO-008 | P1 | Automate Later | Negative — update-nonexistent status undocumented |
| TC-COVERPHOTO-009 | P1 | Automate Now | Positive — delete of self-created fixture |
| TC-COVERPHOTO-010 | P1 | Automate Later | Negative — delete-nonexistent status undocumented |

### USER Domain

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-USER-001 | P2 | Automate Now | Positive — list read |
| TC-USER-002 | P2 | Automate Now | Positive — get-by-id read (status+persistence; response schema undocumented for this op) |
| TC-USER-003 | P2 | Automate Later | Negative — get-nonexistent status undocumented |
| TC-USER-004 | P2 | Automate Later | Security — needs the payload/assertion helper for confirming password exposure; also benefits from one manual pass first given the sensitivity of the finding |
| TC-USER-005 | P0 | Automate Now | Positive — create with self-contained fixture (fake credentials) |
| TC-USER-006 | P0 | Automate Later | Negative — malformed payload status undocumented |
| TC-USER-007 | P0 | Automate Later | Boundary — no documented length constraints, so pass/fail criteria for the long-password case is genuinely unconfirmed |
| TC-USER-008 | P0 | Automate Now | Positive — update with self-contained fixture |
| TC-USER-009 | P0 | Automate Later | Negative — update-nonexistent status undocumented |
| TC-USER-010 | P0 | Automate Now | Idempotency — consistency check |
| TC-USER-011 | P0 | Automate Now | Positive — delete of self-created fixture (Error Handling half of this TC is covered separately by TC-USER-013's classification logic, but kept as one TC per Phase 5 — treat as Automate Now for the delete, defer the exact non-existent-delete status assertion) |
| TC-USER-012 | P0 | Automate Later | Security — payload/encoding helper + credential-handling sensitivity warrants a manual first pass |
| TC-USER-013 | P0 | Automate Later | Negative — delete-nonexistent status undocumented |

### CATALOG Domain (WF-CATALOG-001)

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-CATALOG-001 | P0 | Automate Now | Positive — full happy-path workflow, self-contained, spec-confirmed at each step |
| TC-CATALOG-002 | P0 | Automate Later | Negative — non-existent `idBook` status undocumented |
| TC-CATALOG-003 | P0 | Automate Now | Positive/Data Integrity — self-contained composite fixture; **top-priority regression check** guarding the Critical cross-domain orphaning risk |
| TC-CATALOG-004 | P0 | Automate Later | Negative — partial-failure status undocumented |
| TC-CATALOG-005 | P0 | Automate Later | Security — needs the shared payload/encoding helper |
| TC-CATALOG-006 | P0 | Automate Now | Positive/Data Integrity — self-contained, multi-record association is fully spec-consistent to assert |

**Classification definitions:**

| Classification | Meaning |
|---|---|
| Automate Now | Included in this cycle's automated suite (35 test cases) |
| Automate Later | Valuable to automate, deferred until Phase 8 cycle 1 establishes ground truth or tooling is built (27 test cases) |
| Manual/Exploratory Only | Not planned for automation at all — not used in this cycle; every test case here has a path to automation once its blocker clears |

---

## Tooling

**Update (2026-07-15):** the user directed that the 35 Automate-Now test cases be implemented in **Postman/Newman**, superseding the pytest recommendation originally made below. The table is kept as-is for historical context (it remains valid guidance for the deferred/Automate-Later work and for teams without an existing Postman investment); the actual implemented artifacts are:

- [`OUTPUT/automation/AI_Mentor_Bot.postman_collection.json`](automation/AI_Mentor_Bot.postman_collection.json) — 67 requests across 6 folders (ACTIVITY, AUTHOR, BOOK, COVERPHOTO, USER, CATALOG), covering all 35 Automate-Now test cases (some expand into setup + action + verify steps for self-contained fixture provisioning). Verified passing 67/67 requests, 93/93 assertions via Newman against the live UAT API.
- [`OUTPUT/automation/AI_Mentor_Bot_UAT.postman_environment.json`](automation/AI_Mentor_Bot_UAT.postman_environment.json) — defines `baseUrl`.
- Run via CLI: `newman run AI_Mentor_Bot.postman_collection.json -e AI_Mentor_Bot_UAT.postman_environment.json`.

**Important:** building this collection surfaced that the live UAT instance is stateless (writes never persist, reads always serve fixed seed data) — see the amendment notes in `PROJECT_STATE.md`, `Risk_Assessment.md`, and `Test_Case.md`. The collection's assertions were designed around this reality (seed-id discovery for "get by id" checks, echo-based checks for creates/updates, logged-not-asserted checks for the Book/CoverPhoto/Author association lookups).

| Layer | Tool/Framework | Notes |
|---|---|---|
| API test execution | **Postman/Newman** (implemented) — originally recommended pytest + `requests` | Postman chosen per explicit user direction. The `additionalProperties:false`/nullable/boundary concerns noted below still apply and are handled via `pm.test` assertions in the collection's Tests scripts. |
| Contract/schema validation | Inline `pm.test` assertions against expected shapes (implemented); `jsonschema` (Python) remains the recommendation if/when the Automate-Later suite is built in pytest | Full JSON-Schema validation against `INPUT/swagger.json` is not yet wired into the Postman suite — a reasonable next enhancement. |
| CI integration | GitHub Actions running `newman run` | Repository is already hosted on GitHub (`Supriya5489/AI-API-Integration-tests`); Newman runs as a plain CLI step, no additional runtime needed beyond Node.js. |

## Execution Triggers

| Trigger | Suite Run |
|---|---|
| Every pull request | P0 Automate-Now subset only (TC-BOOK-007/010/011/012, TC-USER-005/008/010/011, TC-CATALOG-001/003/006) — fast smoke check on the Critical paths |
| Nightly | Full Automate-Now suite (all 35 test cases) |
| Pre-release | Full Automate-Now suite + a manual/exploratory pass over all 27 Automate-Later items to refresh the ground-truth baseline |
| On spec change | Affected endpoints/workflows only, per `WORKFLOW.md` Phase 9 (Regression Loop) |

## Test Data & Environment Requirements

- **Environment(s):** UAT (`https://fakerestapi.azurewebsites.net`) — the only environment defined in `PROJECT_CONTEXT.md`. Shared, non-isolated per Phase 0 — every test case's precondition already specifies self-provisioned fixture creation rather than reliance on pre-existing external data, which keeps automated runs safe to execute repeatedly without affecting other users of the shared instance.
- **Test accounts/roles needed:** None — API confirmed unauthenticated (Phase 0).
- **Data setup/teardown:** Prefix all test-created records (e.g., `title`/`userName` fields) with a recognizable marker such as `QA-` or `qa.test.` so automated-run artifacts are trivially identifiable and can be bulk-cleaned if the shared UAT instance accumulates stale data over time. Each test case's own DELETE step (or precondition-then-delete pattern) should keep this to a minimum by design.
- **Secrets handling:** N/A currently — no auth tokens or credentials required. If the Phase 0 auth discrepancy is ever revisited and Bearer auth turns out to be genuinely required, any token must go through a secrets manager (e.g., GitHub Actions encrypted secrets) and never be embedded in test scripts, per `STANDARDS.md` §8.

## Automate-Later Backlog

| Test Case ID | Target Automation Date | Blocker |
|---|---|---|
| TC-ACTIVITY-004 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-ACTIVITY-005 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-ACTIVITY-007 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-ACTIVITY-010 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-AUTHOR-005 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-AUTHOR-006 | 2026-07-25 | Undocumented behavior — prioritized earlier given this is a Phase 3 Data Integrity finding (RISK-AUTHOR-002) |
| TC-AUTHOR-008 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-AUTHOR-010 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-BOOK-004 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-BOOK-006 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-BOOK-008 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-BOOK-009 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-BOOK-013 | 2026-08-15 | Tooling — shared malicious-payload/URL-encoding helper not yet implemented |
| TC-COVERPHOTO-005 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-COVERPHOTO-006 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-COVERPHOTO-008 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-COVERPHOTO-010 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-USER-003 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-USER-004 | 2026-07-25 | Tooling + sensitivity — prioritized earlier given this is the Critical plaintext-password finding (RISK-USER-001); one manual pass first is warranted regardless of automation status |
| TC-USER-006 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-USER-007 | 2026-08-01 | Undocumented behavior — no documented length constraints to assert against |
| TC-USER-009 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-USER-012 | 2026-08-15 | Tooling — shared malicious-payload/URL-encoding helper not yet implemented; credential sensitivity warrants a manual first pass |
| TC-USER-013 | 2026-08-01 | Undocumented behavior — pending Phase 8 cycle 1 exploratory run |
| TC-CATALOG-002 | 2026-07-25 | Undocumented behavior — prioritized earlier, mirrors the AUTHOR/COVERPHOTO referential-integrity finding at the composite-workflow level |
| TC-CATALOG-004 | 2026-07-25 | Undocumented behavior — prioritized earlier given this validates the Critical no-transactional-guarantee risk (RISK-CATALOG-001) |
| TC-CATALOG-005 | 2026-08-15 | Tooling — shared malicious-payload/URL-encoding helper not yet implemented |
