# Automation Collection Notes — {{Project Name}}

**Built on:** {{YYYY-MM-DD}}
**Built by:** {{name/agent}}
**Tool:** {{e.g., Postman/Newman, pytest + requests}}

---

## Artifacts

| File | Purpose |
|---|---|
| `{{filename}}` | {{e.g., Collection covering all Automate-Now test cases}} |
| `{{filename}}` | {{e.g., Environment/config file defining base URL and other variables}} |

## Structure

{{Describe the folder/grouping convention — e.g., one folder per domain, one request per test case, or one request per step within a multi-step test case. Note any self-contained fixture-provisioning pattern used (each request/folder creates the data it needs rather than depending on pre-existing state).}}

## Test Case → Request Mapping

| Test Case ID | Request/Test Name | Notes |
|---|---|---|
| TC-{{DOMAIN}}-{{seq}} | {{request or test name}} | {{e.g., self-contained; provisions its own fixture inline}} |

## Execution Record

| Date | Run By | Result | Notes |
|---|---|---|---|
| {{YYYY-MM-DD}} | {{name/agent}} | {{n}}/{{n}} requests, {{n}}/{{n}} assertions passing | {{link to run output/log if available}} |

_(An unexecuted collection is not a valid Phase 7 deliverable per `AGENT.md` — this table must have at least one real run before the phase is considered complete.)_

## Environment-Behavior Findings

{{Document anything discovered while building/running the suite that contradicts assumptions made in earlier phases (e.g., "API does not persist writes," "unexpected status code on X"). Cross-reference the amendment notes added to affected docs — `Risk_Assessment.md`, `Test_Case.md`, `Automation_Plan.md` — rather than duplicating the full explanation here.}}

## How to Run

```
{{e.g., newman run <collection>.json -e <environment>.json}}
{{e.g., pytest tests/}}
```

## Known Limitations / Excluded Cases

{{Note any "Automate Now" test case that could not be automated as planned, and why (e.g., a tooling gap discovered during implementation) — do not silently drop coverage without recording it here.}}
