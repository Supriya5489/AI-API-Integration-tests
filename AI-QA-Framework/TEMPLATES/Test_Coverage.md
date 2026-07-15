# Test Coverage Plan — {{Project Name}}

**Planned on:** {{YYYY-MM-DD}}
**Planned by:** {{name/agent}}
**Approved by:** {{name}} on {{YYYY-MM-DD}}

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

---

## Coverage Items

| Coverage ID | Endpoint(s)/Workflow(s) | Risk Tier | Dimensions Covered | Priority | Notes |
|---|---|---|---|---|---|
| COV-{{DOMAIN}}-001 | EP-{{DOMAIN}}-001 | {{Critical}} | Functional, Auth, Negative, Boundary, Idempotency | P0 | {{notes}} |
| COV-{{DOMAIN}}-002 | WF-{{DOMAIN}}-001 | {{High}} | Functional, Data Integrity | P1 | {{notes}} |

---

## Minimum Coverage by Risk Tier

| Risk Tier | Required Dimensions |
|---|---|
| Critical | All dimensions, including Security |
| High | Functional, Contract, Auth, Negative, Error Handling |
| Medium | Functional, Contract, Negative |
| Low | Functional only |

---

## Exclusions

| Item | Reason for Exclusion | Approved By |
|---|---|---|
| {{EP/WF}} | {{e.g., third-party endpoint, no test credentials available}} | {{name}} |

---

## Sign-off

- [ ] Coverage plan reviewed against Risk Assessment
- [ ] All Critical/High risk items have full dimension coverage
- [ ] Exclusions documented and approved
- [ ] Approved to proceed to Test Case Design (Phase 5)
