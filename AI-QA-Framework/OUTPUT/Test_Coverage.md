# Test Coverage Plan — AI Mentor Bot

**Planned on:** 2026-07-15
**Planned by:** agent
**Approved by:** mafreen@ariqt.com on 2026-07-15 (approved via direct instruction to proceed to Phase 5)

---

## Coverage Dimensions (reference)

| Dimension | Description |
|---|---|
| Functional | Core happy-path behavior matches the spec |
| Contract/Schema | Request/response shapes match the OpenAPI schema exactly |
| Auth & Permissions | Access control enforced correctly for all roles/scopes |
| Negative/Edge Cases | Invalid input, missing fields, wrong types |
| Boundary Values | Min/max lengths, numeric limits, empty/null values |
| Idempotency | Repeated identical requests behave safely |
| Error Handling | Correct status codes and error bodies for all failure modes |
| Data Integrity | Data remains consistent across related endpoints/workflows |
| Security (QA-level) | Injection attempts, auth bypass attempts, rate limiting behavior |

**Project-wide adjustment:** the API is confirmed unauthenticated (Phase 0). The **Auth & Permissions** dimension is therefore N/A for every coverage item below rather than silently omitted — see Exclusions.

---

## Coverage Items

| Coverage ID | Endpoint(s)/Workflow(s) | Risk Tier | Dimensions Covered | Priority | Notes |
|---|---|---|---|---|---|
| COV-ACTIVITY-001 | EP-ACTIVITY-001, EP-ACTIVITY-003 | Low | Functional | P3 | Read-only, public data; minimum coverage per Low tier |
| COV-ACTIVITY-002 | EP-ACTIVITY-002, EP-ACTIVITY-004, EP-ACTIVITY-005, WF-ACTIVITY-001 | High | Functional, Contract, Negative, Boundary, Idempotency, Error Handling | P1 | Boundary/Idempotency added beyond the High-tier minimum because the spec documents no idempotency key and no error responses for this domain (Phase 1/2 open questions) — must be verified empirically |
| COV-AUTHOR-001 | EP-AUTHOR-001, EP-AUTHOR-003, EP-AUTHOR-004, WF-AUTHOR-002 | Low | Functional | P3 | Read-only, public data; minimum coverage per Low tier |
| COV-AUTHOR-002 | EP-AUTHOR-002, EP-AUTHOR-005, EP-AUTHOR-006, WF-AUTHOR-001 | High | Functional, Contract, Negative, Error Handling, Data Integrity | P1 | Data Integrity added to test unchecked `idBook` referential behavior (create/update an author against a non-existent book) — flagged in Phase 1/2 as undocumented |
| COV-BOOK-001 | EP-BOOK-001, EP-BOOK-003 | Low | Functional | P3 | Read-only, public data; minimum coverage per Low tier |
| COV-BOOK-002 | EP-BOOK-002, EP-BOOK-004, WF-BOOK-001 | High | Functional, Contract, Negative, Error Handling | P1 | Standard High-tier coverage for create/update |
| COV-BOOK-003 | EP-BOOK-005 | Critical | Functional, Contract, Negative, Boundary, Idempotency, Error Handling, Data Integrity, Security | P0 | Full-dimension coverage per Critical minimum; Data Integrity is the priority focus — must explicitly test that deleting a book with existing Authors/CoverPhotos does not silently corrupt or hide the orphaned references |
| COV-COVERPHOTO-001 | EP-COVERPHOTO-001, EP-COVERPHOTO-003, EP-COVERPHOTO-004, WF-COVERPHOTO-002 | Low | Functional | P3 | Read-only, public data; minimum coverage per Low tier |
| COV-COVERPHOTO-002 | EP-COVERPHOTO-002, EP-COVERPHOTO-005, EP-COVERPHOTO-006, WF-COVERPHOTO-001 | High | Functional, Contract, Negative, Error Handling, Data Integrity | P1 | Data Integrity added to test unchecked `idBook` referential behavior and malformed/non-URI `url` values |
| COV-USER-001 | EP-USER-001, EP-USER-003 | Medium | Functional, Contract, Negative, Security (targeted) | P2 | A narrow Security check is added beyond the Medium-tier minimum specifically to confirm whether `password` is exposed in list/detail responses — justified by the plaintext-credential finding in Phase 3, not a full security sweep |
| COV-USER-002 | EP-USER-002, EP-USER-004, EP-USER-005, WF-USER-001 | Critical | Functional, Contract, Negative, Boundary, Idempotency, Error Handling, Data Integrity, Security | P0 | Full-dimension coverage per Critical minimum; Security focus is credential handling (does `password` round-trip in any response?) rather than injection/auth-bypass, since there is no auth layer to bypass |
| COV-CATALOG-001 | WF-CATALOG-001 | Critical | Functional, Contract, Negative, Error Handling, Data Integrity, Security | P0 | Full end-to-end workflow coverage (Book→Author→CoverPhoto); Data Integrity and Error Handling are the priority focus — no transactional guarantee is documented, so partial-failure and book-deletion-mid-workflow scenarios are required test cases, not optional extras |

---

## Minimum Coverage by Risk Tier

| Risk Tier | Required Dimensions |
|---|---|
| Critical | All dimensions, including Security |
| High | Functional, Contract, Auth, Negative, Error Handling — **Auth dimension N/A project-wide (see above)**; Data Integrity added where a Phase 1/2 open question specifically calls for it |
| Medium | Functional, Contract, Negative |
| Low | Functional only |

---

## Exclusions

| Item | Reason for Exclusion | Approved By |
|---|---|---|
| Auth & Permissions dimension (all coverage items) | API confirmed unauthenticated on 2026-07-15 — no access-control behavior exists to test | mafreen@ariqt.com (Phase 0 confirmation) |
| Rate-limiting / DoS-style Security sub-checks (all coverage items, including Critical) | No documented rate limiting in the spec; per `AGENT.md` constraints, the agent does not design destructive/load-style tests against a shared UAT instance without explicit separate approval | _(not yet requested — flagging as excluded by default)_ |

---

## Sign-off

- [x] Coverage plan reviewed against Risk Assessment
- [x] All Critical/High risk items have full dimension coverage
- [x] Exclusions documented and approved
- [x] Approved to proceed to Test Case Design (Phase 5)
