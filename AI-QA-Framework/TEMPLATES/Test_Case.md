# Test Case: {{TC-DOMAIN-seq}} — {{Short Title}}

**Test Case ID:** TC-{{DOMAIN}}-{{seq}}
**Endpoint(s)/Workflow(s):** {{EP-DOMAIN-seq / WF-DOMAIN-seq}}
**Coverage Item:** COV-{{DOMAIN}}-{{seq}}
**Priority:** {{P0 | P1 | P2 | P3}}
**Type:** {{Positive | Negative | Boundary | Security | Idempotency | Contract}}
**Status:** {{Not Started | In Progress | Passed | Failed | Blocked | Deprecated}}

---

## Preconditions

- {{e.g., a valid user account exists}}
- {{e.g., auth token for role X is available}}

## Test Data / Inputs

```json
{
  "field": "value"
}
```

## Steps

1. {{Step 1 — action}}
2. {{Step 2 — action}}
3. {{Step 3 — action}}

## Expected Result

- **Status code:** {{e.g., 201}}
- **Response body:** {{expected shape/values}}
- **Side effects:** {{e.g., resource created, event emitted, downstream state changed}}

## Actual Result

{{Filled in during execution — actual status/body/behavior observed.}}

## Pass/Fail

{{Passed | Failed | Blocked — with defect ID if failed: DEF-{{seq}}}}

## Notes

- {{Any relevant context, flakiness, environment quirks.}}
