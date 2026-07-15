# Automation Plan — {{Project Name}}

**Planned on:** {{YYYY-MM-DD}}
**Planned by:** {{name/agent}}

---

## Automation Classification

| Test Case ID | Priority | Classification | Rationale |
|---|---|---|---|
| TC-{{DOMAIN}}-001 | P0 | Automate Now | {{e.g., core path, stable contract, runs every build}} |
| TC-{{DOMAIN}}-002 | P2 | Automate Later | {{e.g., valuable but low ROI until contract stabilizes}} |
| TC-{{DOMAIN}}-003 | P3 | Manual/Exploratory Only | {{e.g., one-off edge case, high setup cost relative to value}} |

**Classification definitions:**

| Classification | Meaning |
|---|---|
| Automate Now | Included in this cycle's automated suite |
| Automate Later | Valuable to automate, deferred — tracked with a target date |
| Manual/Exploratory Only | Not planned for automation; run manually when needed |

---

## Tooling

| Layer | Tool/Framework | Notes |
|---|---|---|
| API test execution | {{e.g., Postman/Newman, REST Assured, pytest + requests}} | {{notes}} |
| Contract/schema validation | {{e.g., Dredd, Schemathesis}} | {{notes}} |
| CI integration | {{e.g., GitHub Actions, Jenkins}} | {{notes}} |

## Execution Triggers

| Trigger | Suite Run |
|---|---|
| Every pull request | {{P0 smoke suite}} |
| Nightly | {{Full regression suite}} |
| Pre-release | {{Full suite + manual exploratory pass}} |
| On spec change | {{Affected endpoints/workflows only — see WORKFLOW.md Phase 9}} |

## Test Data & Environment Requirements

- **Environment(s):** {{e.g., staging, sandbox}}
- **Test accounts/roles needed:** {{list}}
- **Data setup/teardown:** {{e.g., seeded fixtures, cleanup scripts}}
- **Secrets handling:** {{reference to secrets manager — never embed real values, see STANDARDS.md §8}}

## Automate-Later Backlog

| Test Case ID | Target Automation Date | Blocker |
|---|---|---|
| TC-{{DOMAIN}}-002 | {{YYYY-MM-DD}} | {{e.g., contract instability, tooling gap}} |
