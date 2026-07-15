# Risk Assessment — {{Project Name}}

**Assessed on:** {{YYYY-MM-DD}}
**Assessed by:** {{name/agent}}

Risk tiers and criteria are defined in [STANDARDS.md](../STANDARDS.md#5-risk-tiers-for-risk_assessmentmd).

---

## Risk Register

| Risk ID | Endpoint(s)/Workflow(s) | Data Sensitivity | Auth Criticality | State-Changing Impact | Blast Radius | Risk Tier | Justification |
|---|---|---|---|---|---|---|---|
| RISK-{{DOMAIN}}-001 | EP-{{DOMAIN}}-001, WF-{{DOMAIN}}-001 | {{e.g., PII}} | {{e.g., auth required}} | {{e.g., Create}} | {{e.g., single user}} | {{Critical/High/Medium/Low}} | {{why this tier}} |

---

## Scoring Guide (reference)

| Factor | Low | Medium | High |
|---|---|---|---|
| Data sensitivity | Public data | Internal/business data | PII, financial, credentials |
| Auth criticality | No auth | User-level auth | Admin/privileged auth |
| State-changing impact | Read-only | Single-resource write | Multi-resource/financial write |
| Blast radius | Single user affected | Multiple users affected | System-wide or irreversible |

## Critical & High Risk Summary

List all Critical/High tier items requiring deepest test coverage per Phase 4 of [WORKFLOW.md](../WORKFLOW.md):

| Risk ID | Item | Tier | Required Coverage Depth |
|---|---|---|---|
| RISK-{{DOMAIN}}-001 | {{EP/WF name}} | {{Critical/High}} | {{e.g., full positive/negative/boundary/security suite}} |

## Accepted / Deferred Risks

| Risk ID | Item | Reason Deferred | Approved By |
|---|---|---|---|
| {{RISK-...}} | {{item}} | {{e.g., low traffic, scheduled for deprecation}} | {{name}} |
