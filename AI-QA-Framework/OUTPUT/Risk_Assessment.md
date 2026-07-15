# Risk Assessment — Book store

**Assessed on:** 2026-07-15
**Assessed by:** agent

Risk tiers and criteria are defined in [STANDARDS.md](../STANDARDS.md#5-risk-tiers-for-risk_assessmentmd).

> **Amendment (2026-07-15):** automated smoke-testing (`OUTPUT/automation/`) confirmed the live UAT instance is stateless — writes are validated/echoed but never persisted, and reads always serve a fixed seed dataset. `RISK-BOOK-003` and `RISK-CATALOG-001` below describe risk that is real for any stateful implementation of this contract, but **cannot currently be observed against this specific instance** since there is no persisted data to orphan. The risk tiers and rationale stand unchanged as guidance for testing a real implementation; this note exists so Phase 8 execution isn't misread as "the orphaning bug doesn't exist" when the true finding is "this instance has no state to corrupt."

---

## Risk Register

| Risk ID | Endpoint(s)/Workflow(s) | Data Sensitivity | Auth Criticality | State-Changing Impact | Blast Radius | Risk Tier | Justification |
|---|---|---|---|---|---|---|---|
| RISK-ACTIVITY-001 | EP-ACTIVITY-001, EP-ACTIVITY-003 | Public/non-sensitive | No auth | Read-only | Single record, no side effects | Low | Public read-only data, no auth gate, no downstream impact |
| RISK-ACTIVITY-002 | EP-ACTIVITY-002, EP-ACTIVITY-004, EP-ACTIVITY-005, WF-ACTIVITY-001 | Public/non-sensitive | No auth | Create/Update/Delete | Single record; no other domain references Activities | High | State-changing with no auth gate on a shared UAT environment; contained blast radius since no cross-domain FK points at this resource |
| RISK-AUTHOR-001 | EP-AUTHOR-001, EP-AUTHOR-003, EP-AUTHOR-004, WF-AUTHOR-002 | Public/non-sensitive | No auth | Read-only | Single record/lookup, no side effects | Low | Public read-only data and cross-reference lookup, no auth gate |
| RISK-AUTHOR-002 | EP-AUTHOR-002, EP-AUTHOR-005, EP-AUTHOR-006, WF-AUTHOR-001 | Public/non-sensitive | No auth | Create/Update/Delete | Single author record; no documented `idBook` referential check | High | State-changing with no auth gate; unchecked `idBook` allows creating authors pointing at non-existent books, but blast radius stays within the Author resource |
| RISK-BOOK-001 | EP-BOOK-001, EP-BOOK-003 | Public/non-sensitive | No auth | Read-only | Single record, no side effects | Low | Public read-only catalog data, no auth gate |
| RISK-BOOK-002 | EP-BOOK-002, EP-BOOK-004, WF-BOOK-001 | Public/non-sensitive | No auth | Create/Update | Single book record | High | State-changing with no auth gate; create/update alone don't cascade to other domains |
| RISK-BOOK-003 | EP-BOOK-005 | Public/non-sensitive | No auth | Delete | Cross-domain — orphans `Author`/`CoverPhoto` records referencing this `idBook` with no documented cascade/restrict policy | **Critical** | State-changing with wide blast radius per STANDARDS §5: deleting a book silently breaks referential integrity across two other domains (`WF-AUTHOR-002`, `WF-COVERPHOTO-002` lookups), and the failure mode (orphaned data vs. silent success) is entirely undocumented in the contract |
| RISK-COVERPHOTO-001 | EP-COVERPHOTO-001, EP-COVERPHOTO-003, EP-COVERPHOTO-004, WF-COVERPHOTO-002 | Public/non-sensitive | No auth | Read-only | Single record/lookup, no side effects | Low | Public read-only data and cross-reference lookup, no auth gate |
| RISK-COVERPHOTO-002 | EP-COVERPHOTO-002, EP-COVERPHOTO-005, EP-COVERPHOTO-006, WF-COVERPHOTO-001 | Public/non-sensitive | No auth | Create/Update/Delete | Single cover photo record; no documented `idBook`/`url` validation | High | State-changing with no auth gate; unchecked `idBook` and `url` format, but blast radius stays within the CoverPhoto resource |
| RISK-USER-001 | EP-USER-001, EP-USER-003 | **Sensitive — `password` field returned in plaintext per schema** | No auth | Read-only | Single record | Medium | Per STANDARDS §5, read operations on sensitive data score Medium even without state-changing impact — the `password` field's presence (masked or not) needs empirical confirmation |
| RISK-USER-002 | EP-USER-002, EP-USER-004, EP-USER-005, WF-USER-001 | **Sensitive — plaintext `password` field created/updated/removed** | No auth | Create/Update/Delete | Single user record, but involves credential-like data | **Critical** | Per STANDARDS §5, handling PII/credential-like data combined with state-changing impact and no auth gate warrants the highest tier, even on a test/demo API — test design should default to treating `password` as sensitive rather than assume it's safe because the API is fake |
| RISK-CATALOG-001 | WF-CATALOG-001 | Public/non-sensitive, but spans 3 domains | No auth | Create across Book, Author, CoverPhoto (multi-resource) | System-wide within the API's data model — no transactional guarantee, and orphaning risk on `Book` deletion cascades into this workflow | **Critical** | Per STANDARDS §5, multi-resource state-changing writes with no rollback/cascade guarantee and wide blast radius (affects 3 domains) meet the Critical bar regardless of data sensitivity |

---

## Scoring Guide (reference)

| Factor | Low | Medium | High |
|---|---|---|---|
| Data sensitivity | Public data | Internal/business data | PII, financial, credentials |
| Auth criticality | No auth | User-level auth | Admin/privileged auth |
| State-changing impact | Read-only | Single-resource write | Multi-resource/financial write |
| Blast radius | Single user affected | Multiple users affected | System-wide or irreversible |

**Note on this project:** since the entire API is unauthenticated (see `PROJECT_STATE.md`), "Auth criticality" does not differentiate risk here — every endpoint scores "No auth." Risk tiers below are therefore driven primarily by data sensitivity (the `User.password` field) and blast radius (cross-domain orphaning risk from `Book` deletion and the multi-step `WF-CATALOG-001`).

## Critical & High Risk Summary

| Risk ID | Item | Tier | Required Coverage Depth |
|---|---|---|---|
| RISK-BOOK-003 | EP-BOOK-005 (DELETE /api/v1/Books/{id}) | Critical | Full positive/negative/boundary suite + explicit data-integrity tests for orphaned `Author`/`CoverPhoto` records after deletion |
| RISK-USER-002 | EP-USER-002, EP-USER-004, EP-USER-005, WF-USER-001 | Critical | Full positive/negative/boundary suite + explicit tests confirming whether `password` is exposed in any response body |
| RISK-CATALOG-001 | WF-CATALOG-001 | Critical | Full end-to-end workflow test (Book→Author→CoverPhoto creation and verification) + failure-injection tests (partial failure mid-workflow, deletion of a referenced book) |
| RISK-ACTIVITY-002 | EP-ACTIVITY-002, EP-ACTIVITY-004, EP-ACTIVITY-005, WF-ACTIVITY-001 | High | Positive + negative + boundary test cases for create/update/delete |
| RISK-AUTHOR-002 | EP-AUTHOR-002, EP-AUTHOR-005, EP-AUTHOR-006, WF-AUTHOR-001 | High | Positive + negative + boundary test cases, including invalid `idBook` |
| RISK-BOOK-002 | EP-BOOK-002, EP-BOOK-004, WF-BOOK-001 | High | Positive + negative + boundary test cases for create/update |
| RISK-COVERPHOTO-002 | EP-COVERPHOTO-002, EP-COVERPHOTO-005, EP-COVERPHOTO-006, WF-COVERPHOTO-001 | High | Positive + negative + boundary test cases, including invalid `idBook`/`url` |

## Accepted / Deferred Risks

| Risk ID | Item | Reason Deferred | Approved By |
|---|---|---|---|
| _(none yet)_ | — | No risks have been deferred at this stage — all Critical/High items are in scope for Phase 4 coverage planning. | — |
