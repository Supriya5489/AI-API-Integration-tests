# Test Report — {{Project Name}}

**Report date:** {{YYYY-MM-DD}}
**Spec version tested:** {{version}}
**Environment:** {{e.g., staging}}
**Executed by:** {{name/agent}}

---

## Executive Summary

{{2-4 sentences: overall quality signal, whether release-ready, key risks remaining.}}

## Execution Summary

| Metric | Count |
|---|---|
| Total test cases planned | {{n}} |
| Executed | {{n}} |
| Passed | {{n}} |
| Failed | {{n}} |
| Blocked | {{n}} |
| Not Started | {{n}} |
| Pass rate | {{%}} |

## Results by Priority

| Priority | Total | Passed | Failed | Blocked |
|---|---|---|---|---|
| P0 | {{n}} | {{n}} | {{n}} | {{n}} |
| P1 | {{n}} | {{n}} | {{n}} | {{n}} |
| P2 | {{n}} | {{n}} | {{n}} | {{n}} |
| P3 | {{n}} | {{n}} | {{n}} | {{n}} |

## Defects Found

| Defect ID | Severity | Test Case ID | Endpoint/Workflow | Summary | Status |
|---|---|---|---|---|---|
| DEF-001 | {{Critical/High/Medium/Low}} | TC-{{DOMAIN}}-{{seq}} | EP-{{DOMAIN}}-{{seq}} | {{one-line summary}} | {{Open/Fixed/Deferred}} |

## Coverage vs. Plan

| Coverage Item | Planned | Executed | Gap/Reason |
|---|---|---|---|
| COV-{{DOMAIN}}-001 | Yes | Yes | — |
| COV-{{DOMAIN}}-002 | Yes | No | {{e.g., blocked by missing test data}} |

## Residual Risk

{{Summarize any Critical/High risk items with incomplete coverage, open defects, or known gaps that remain at report time — tie back to Risk_Assessment.md.}}

## Blocked / Skipped Items

| Test Case ID | Reason | Follow-up Owner |
|---|---|---|
| TC-{{DOMAIN}}-{{seq}} | {{reason}} | {{name}} |

## Recommendation

{{Go / No-Go / Conditional — with conditions if applicable.}}
