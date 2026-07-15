# Traceability Matrix — Book store

**Last updated:** 2026-07-15
**Spec version:** OpenAPI 3.0.1 — "FakeRESTApi.Web V1", version `v1` (fetched 2026-07-15, `INPUT/swagger.json`)

This matrix is the single source of truth linking every endpoint through to its final test result, per [STANDARDS.md §7](../STANDARDS.md#7-traceability-rules).

**Legend:** Automation Status/Test Case tags — **(Now)** = Automate Now (implemented in `OUTPUT/automation/`), **(Later)** = Automate Later (per `OUTPUT/Automation_Plan.md` backlog, not yet automated or executed).

---

## Full Traceability Chain

| Endpoint ID | Workflow ID | Risk ID / Tier | Coverage ID | Test Case ID(s) | Automation Status | Automation Collection Ref | Last Result |
|---|---|---|---|---|---|---|---|
| EP-ACTIVITY-001 | WF-ACTIVITY-001 | RISK-ACTIVITY-001 / Low | COV-ACTIVITY-001 | TC-ACTIVITY-001 (Now) | 1 Now | ACTIVITY folder — "TC-ACTIVITY-001" | Passed |
| EP-ACTIVITY-002 | WF-ACTIVITY-001 | RISK-ACTIVITY-002 / High | COV-ACTIVITY-002 | TC-ACTIVITY-003 (Now), TC-ACTIVITY-004 (Later), TC-ACTIVITY-005 (Later) | 1 Now / 2 Later | ACTIVITY folder — "TC-ACTIVITY-003" | Passed (Now); Not Started (Later) |
| EP-ACTIVITY-003 | WF-ACTIVITY-001 | RISK-ACTIVITY-001 / Low | COV-ACTIVITY-001 | TC-ACTIVITY-002 (Now) | 1 Now | ACTIVITY folder — "TC-ACTIVITY-002" | Passed |
| EP-ACTIVITY-004 | WF-ACTIVITY-001 | RISK-ACTIVITY-002 / High | COV-ACTIVITY-002 | TC-ACTIVITY-006 (Now), TC-ACTIVITY-007 (Later), TC-ACTIVITY-008 (Now) | 2 Now / 1 Later | ACTIVITY folder — "TC-ACTIVITY-006", "TC-ACTIVITY-008 (call 1/2)" | Passed (Now); Not Started (Later) |
| EP-ACTIVITY-005 | WF-ACTIVITY-001 | RISK-ACTIVITY-002 / High | COV-ACTIVITY-002 | TC-ACTIVITY-009 (Now), TC-ACTIVITY-010 (Later) | 1 Now / 1 Later | ACTIVITY folder — "TC-ACTIVITY-009" | Passed (Now); Not Started (Later) |
| EP-AUTHOR-001 | WF-AUTHOR-001 | RISK-AUTHOR-001 / Low | COV-AUTHOR-001 | TC-AUTHOR-001 (Now) | 1 Now | AUTHOR folder — "TC-AUTHOR-001" | Passed |
| EP-AUTHOR-002 | WF-AUTHOR-001, WF-CATALOG-001 | RISK-AUTHOR-002 / High | COV-AUTHOR-002 | TC-AUTHOR-004 (Now), TC-AUTHOR-005 (Later), TC-AUTHOR-006 (Later); also exercised by TC-CATALOG-001/003/006 (Now) | 4 Now / 2 Later | AUTHOR folder — "TC-AUTHOR-004"; CATALOG folder — create-author steps | Passed (Now); Not Started (Later) |
| EP-AUTHOR-003 | WF-AUTHOR-002, WF-CATALOG-001 | RISK-AUTHOR-001 / Low | COV-AUTHOR-001 | TC-AUTHOR-003 (Now); also exercised by TC-CATALOG-001/003/006 (Now) | 4 Now | AUTHOR folder — "TC-AUTHOR-003"; CATALOG folder — author-lookup steps | Passed |
| EP-AUTHOR-004 | WF-AUTHOR-001 | RISK-AUTHOR-001 / Low | COV-AUTHOR-001 | TC-AUTHOR-002 (Now) | 1 Now | AUTHOR folder — "TC-AUTHOR-002" | Passed |
| EP-AUTHOR-005 | WF-AUTHOR-001 | RISK-AUTHOR-002 / High | COV-AUTHOR-002 | TC-AUTHOR-007 (Now), TC-AUTHOR-008 (Later) | 1 Now / 1 Later | AUTHOR folder — "TC-AUTHOR-007" | Passed (Now); Not Started (Later) |
| EP-AUTHOR-006 | WF-AUTHOR-001 | RISK-AUTHOR-002 / High | COV-AUTHOR-002 | TC-AUTHOR-009 (Now), TC-AUTHOR-010 (Later) | 1 Now / 1 Later | AUTHOR folder — "TC-AUTHOR-009" | Passed (Now); Not Started (Later) |
| EP-BOOK-001 | WF-BOOK-001 | RISK-BOOK-001 / Low | COV-BOOK-001 | TC-BOOK-001 (Now) | 1 Now | BOOK folder — "TC-BOOK-001" | Passed |
| EP-BOOK-002 | WF-BOOK-001, WF-CATALOG-001 | RISK-BOOK-002 / High | COV-BOOK-002 | TC-BOOK-003 (Now), TC-BOOK-004 (Later); also exercised by TC-CATALOG-001/003/006 (Now) | 4 Now / 1 Later | BOOK folder — "TC-BOOK-003"; CATALOG folder — create-book steps | Passed (Now); Not Started (Later) |
| EP-BOOK-003 | WF-BOOK-001, WF-CATALOG-001 | RISK-BOOK-001 / Low | COV-BOOK-001 | TC-BOOK-002 (Now); also exercised by TC-CATALOG-001 (Now, logged-only step) | 2 Now | BOOK folder — "TC-BOOK-002"; CATALOG folder — "TC-CATALOG-001 (4/6)" | Passed |
| EP-BOOK-004 | WF-BOOK-001 | RISK-BOOK-002 / High | COV-BOOK-002 | TC-BOOK-005 (Now), TC-BOOK-006 (Later) | 1 Now / 1 Later | BOOK folder — "TC-BOOK-005" | Passed (Now); Not Started (Later) |
| EP-BOOK-005 | WF-BOOK-001, WF-CATALOG-001 | RISK-BOOK-003 / **Critical** | COV-BOOK-003 | TC-BOOK-007 (Now), TC-BOOK-008 (Later), TC-BOOK-009 (Later), TC-BOOK-010 (Now), TC-BOOK-011 (Now), TC-BOOK-012 (Now), TC-BOOK-013 (Later); also exercised by TC-CATALOG-003 (Now) | 5 Now / 3 Later | BOOK folder — "TC-BOOK-007/010/011/012"; CATALOG folder — "TC-CATALOG-003 (4/6)" | Passed (Now, incl. logged orphaning findings — see Automation_Collection_Notes.md); Not Started (Later) |
| EP-COVERPHOTO-001 | WF-COVERPHOTO-001 | RISK-COVERPHOTO-001 / Low | COV-COVERPHOTO-001 | TC-COVERPHOTO-001 (Now) | 1 Now | COVERPHOTO folder — "TC-COVERPHOTO-001" | Passed |
| EP-COVERPHOTO-002 | WF-COVERPHOTO-001, WF-CATALOG-001 | RISK-COVERPHOTO-002 / High | COV-COVERPHOTO-002 | TC-COVERPHOTO-004 (Now), TC-COVERPHOTO-005 (Later), TC-COVERPHOTO-006 (Later); also exercised by TC-CATALOG-001/003/006 (Now) | 4 Now / 2 Later | COVERPHOTO folder — "TC-COVERPHOTO-004"; CATALOG folder — create-cover-photo steps | Passed (Now); Not Started (Later) |
| EP-COVERPHOTO-003 | WF-COVERPHOTO-002, WF-CATALOG-001 | RISK-COVERPHOTO-001 / Low | COV-COVERPHOTO-001 | TC-COVERPHOTO-003 (Now); also exercised by TC-CATALOG-001/003/006 (Now) | 4 Now | COVERPHOTO folder — "TC-COVERPHOTO-003"; CATALOG folder — cover-photo-lookup steps | Passed |
| EP-COVERPHOTO-004 | WF-COVERPHOTO-001 | RISK-COVERPHOTO-001 / Low | COV-COVERPHOTO-001 | TC-COVERPHOTO-002 (Now) | 1 Now | COVERPHOTO folder — "TC-COVERPHOTO-002" | Passed |
| EP-COVERPHOTO-005 | WF-COVERPHOTO-001 | RISK-COVERPHOTO-002 / High | COV-COVERPHOTO-002 | TC-COVERPHOTO-007 (Now), TC-COVERPHOTO-008 (Later) | 1 Now / 1 Later | COVERPHOTO folder — "TC-COVERPHOTO-007" | Passed (Now); Not Started (Later) |
| EP-COVERPHOTO-006 | WF-COVERPHOTO-001 | RISK-COVERPHOTO-002 / High | COV-COVERPHOTO-002 | TC-COVERPHOTO-009 (Now), TC-COVERPHOTO-010 (Later) | 1 Now / 1 Later | COVERPHOTO folder — "TC-COVERPHOTO-009" | Passed (Now); Not Started (Later) |
| EP-USER-001 | WF-USER-001 | RISK-USER-001 / Medium | COV-USER-001 | TC-USER-001 (Now), TC-USER-004 (Later) | 1 Now / 1 Later | USER folder — "TC-USER-001" | Passed (Now); Not Started (Later) |
| EP-USER-002 | WF-USER-001 | RISK-USER-002 / **Critical** | COV-USER-002 | TC-USER-005 (Now), TC-USER-006 (Later), TC-USER-007 (Later), TC-USER-012 (Later) | 1 Now / 3 Later | USER folder — "TC-USER-005" | Passed (Now); Not Started (Later) |
| EP-USER-003 | WF-USER-001 | RISK-USER-001 / Medium | COV-USER-001 | TC-USER-002 (Now), TC-USER-003 (Later), TC-USER-004 (Later) | 1 Now / 2 Later | USER folder — "TC-USER-002" | Passed (Now); Not Started (Later) |
| EP-USER-004 | WF-USER-001 | RISK-USER-002 / **Critical** | COV-USER-002 | TC-USER-008 (Now), TC-USER-009 (Later), TC-USER-010 (Now), TC-USER-012 (Later) | 2 Now / 2 Later | USER folder — "TC-USER-008", "TC-USER-010 (call 1/2)" | Passed (Now); Not Started (Later) |
| EP-USER-005 | WF-USER-001 | RISK-USER-002 / **Critical** | COV-USER-002 | TC-USER-011 (Now), TC-USER-012 (Later), TC-USER-013 (Later) | 1 Now / 2 Later | USER folder — "TC-USER-011 (delete + repeat delete)" | Passed (Now); Not Started (Later) |

**Note on `WF-CATALOG-001` (cross-domain composite workflow):** it has no single dedicated endpoint of its own — it spans `EP-BOOK-002/003/005`, `EP-AUTHOR-002/003`, and `EP-COVERPHOTO-002/003`, each listed above with the applicable `TC-CATALOG-*` test cases appended. `RISK-CATALOG-001` (Critical) and `COV-CATALOG-001` apply to this workflow as a whole; see `OUTPUT/Risk_Assessment.md` and `OUTPUT/Test_Coverage.md`.

---

## Orphan Check

| Type | ID | Issue |
|---|---|---|

_(Empty — no orphans. All 27 endpoints link to ≥1 workflow, all 8 workflows link to a risk entry, all 11 risk entries link to a coverage item, all 12 coverage items link to ≥1 test case, and all 62 test cases have an automation classification.)_

## Exclusions Log

| ID | Type | Reason | Approved By |
|---|---|---|---|

_(Empty — all 27 endpoints in the spec are in scope; no endpoints, workflows, or test cases have been excluded from this project.)_

## Coverage Completeness Summary

| Metric | Count | % |
|---|---|---|
| Total endpoints | 27 | 100% |
| Endpoints with ≥1 workflow link | 27 | 100% |
| Endpoints with ≥1 test case | 27 | 100% |
| Endpoints with 100% Passed test cases | 10 | 37% |
| Critical/High risk endpoints fully covered (dimensions per Test_Coverage.md) | 15 / 15 | 100% |

**Note on "100% Passed":** only the 10 endpoints whose test cases are entirely `Automate Now` (EP-ACTIVITY-001/003, EP-AUTHOR-001/003/004, EP-BOOK-001/003, EP-COVERPHOTO-001/003/004) currently show 100% Passed — every other endpoint has ≥1 `Automate Later` test case still `Not Started`, by design, per the Phase 6/7 classification policy. This is expected at this stage of the lifecycle, not a gap: the 27 deferred test cases are tracked with target dates in `Automation_Plan.md`'s backlog, and "Critical/High risk endpoints fully covered" (100%) confirms depth of *planned* coverage is already complete — what remains is execution, which is Phase 9's job.

## Sign-off

- [x] No unresolved orphans
- [x] All exclusions documented and approved (none exist)
- [x] Critical/High risk items fully traceable end-to-end (all 15 Critical/High endpoints have an unbroken endpoint → workflow → risk → coverage → test case → automation status chain; remaining execution work is explicitly tracked, not missing)
