# API Inventory — {{Project Name}}

**Spec source:** `INPUT/{{spec-file-name}}`
**Spec version:** {{version}}
**Base URL(s):** {{base-url}}
**Global auth scheme:** {{e.g., Bearer JWT, API Key, OAuth2}}
**Inventoried on:** {{YYYY-MM-DD}}
**Inventoried by:** {{name/agent}}

---

## Summary

| Metric | Count |
|---|---|
| Total endpoints | {{n}} |
| Domains/resource groups | {{n}} |
| Endpoints requiring auth | {{n}} |
| State-changing endpoints (POST/PUT/PATCH/DELETE) | {{n}} |
| Deprecated endpoints | {{n}} |

---

## Endpoints by Domain

### Domain: {{e.g., USER}}

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-USER-001 | POST | /users | Create a new user | Yes | [POST_users.md](../OUTPUT/endpoints/POST_users.md) |
| EP-USER-002 | GET | /users/{id} | Get user by ID | Yes | [GET_users_id.md](../OUTPUT/endpoints/GET_users_id.md) |

### Domain: {{e.g., PAYMENT}}

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-PAYMENT-001 | POST | /payments | Initiate a payment | Yes | [POST_payments.md](../OUTPUT/endpoints/POST_payments.md) |

_(Repeat a table per domain/resource group.)_

---

## Notes & Ambiguities

- {{Any undocumented behavior, missing examples, unclear auth scopes, or spec inconsistencies discovered during inventory.}}

## Exclusions

| Path/Method | Reason for exclusion |
|---|---|
| {{e.g., /internal/health}} | {{e.g., internal-only, not part of tested contract}} |
