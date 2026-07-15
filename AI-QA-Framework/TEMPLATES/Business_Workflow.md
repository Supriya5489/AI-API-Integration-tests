# Business Workflow: {{Workflow Name}}

**Workflow ID:** WF-{{DOMAIN}}-{{seq}}
**Domain:** {{domain}}
**Owner/Stakeholder:** {{if known}}
**Risk tier:** {{Critical | High | Medium | Low}} — see RISK-{{DOMAIN}}-{{seq}}

---

## Description

{{Plain-language description of the real-world business process this workflow represents, e.g., "A customer signs up, verifies their email, and completes their profile before placing their first order."}}

## Trigger

{{What initiates this workflow — user action, scheduled job, webhook, etc.}}

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-{{DOMAIN}}-{{seq}} | {{POST /resource}} | {{description}} | {{e.g., resource_id}} |
| 2 | EP-{{DOMAIN}}-{{seq}} | {{PATCH /resource/{id}}} | {{description}} | {{e.g., status}} |
| 3 | EP-{{DOMAIN}}-{{seq}} | {{GET /resource/{id}}} | {{description}} | — |

## State Dependencies

- {{e.g., Step 2 requires the resource created in Step 1 to be in "pending" state.}}
- {{e.g., Step 3 will 404 if Step 1 failed or was rolled back.}}

## Success Criteria

{{What "the workflow completed successfully" means end-to-end — e.g., final resource state, confirmation event, downstream side effect.}}

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| {{Step 2 fails after Step 1 succeeds}} | {{e.g., resource remains in "pending", retryable}} |
| {{Timeout between steps}} | {{e.g., idempotency key allows safe retry}} |

## Related Workflows

- {{WF-DOMAIN-seq — brief relation, e.g., "reversed by Refund workflow"}}

## Open Questions / Ambiguities

- {{Anything about the business process that needs confirmation from stakeholders.}}
