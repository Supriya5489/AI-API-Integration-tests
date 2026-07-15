# Traceability Matrix — {{Project Name}}

**Last updated:** {{YYYY-MM-DD}}
**Spec version:** {{version}}

This matrix is the single source of truth linking every endpoint through to its final test result, per [STANDARDS.md §7](../STANDARDS.md#7-traceability-rules).

---

## Full Traceability Chain

| Endpoint ID | Workflow ID | Risk ID / Tier | Coverage ID | Test Case ID(s) | Automation Status | Automation Collection Ref | Last Result |
|---|---|---|---|---|---|---|---|
| EP-{{DOMAIN}}-001 | WF-{{DOMAIN}}-001 | RISK-{{DOMAIN}}-001 / Critical | COV-{{DOMAIN}}-001 | TC-{{DOMAIN}}-001, TC-{{DOMAIN}}-002 | Automate Now | {{request name(s) in OUTPUT/automation/, per Automation_Collection_Notes.md}} | Passed |
| EP-{{DOMAIN}}-002 | Standalone | RISK-{{DOMAIN}}-002 / Low | COV-{{DOMAIN}}-002 | TC-{{DOMAIN}}-003 | Manual Only | N/A — not in the automation collection | Not Started |

---

## Orphan Check

| Type | ID | Issue |
|---|---|---|
| {{Endpoint}} | {{EP-...}} | {{e.g., no linked workflow or coverage item}} |

_(This table should be empty when the matrix is complete. If not empty, resolve before sign-off.)_

## Exclusions Log

| ID | Type | Reason | Approved By |
|---|---|---|---|
| {{EP-...}} | Endpoint | {{e.g., internal-only, excluded from inventory}} | {{name}} |

## Coverage Completeness Summary

| Metric | Count | % |
|---|---|---|
| Total endpoints | {{n}} | 100% |
| Endpoints with ≥1 workflow link | {{n}} | {{%}} |
| Endpoints with ≥1 test case | {{n}} | {{%}} |
| Endpoints with 100% Passed test cases | {{n}} | {{%}} |
| Critical/High risk endpoints fully covered | {{n}} | {{%}} |

## Sign-off

- [ ] No unresolved orphans
- [ ] All exclusions documented and approved
- [ ] Critical/High risk items fully traceable end-to-end
