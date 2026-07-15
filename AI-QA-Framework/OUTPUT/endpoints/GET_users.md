# Endpoint: GET /api/v1/Users

**Endpoint ID:** EP-USER-001
**Domain:** USER
**Workflow(s):** WF-USER-001
**Risk tier:** Medium — see RISK-USER-001 (sensitive `password` field)
**Deprecated:** No

---

## Description

Returns the full collection of users.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none documented in spec)_

**Headers**

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Array of `User` |

```json
[
  {
    "id": "integer(int32)",
    "userName": "string, nullable",
    "password": "string, nullable"
  }
]
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- **`password` is returned in plaintext in the list response per the spec's own schema** — no masking/redaction documented. Flagged for a dedicated Risk Assessment entry in Phase 3 even on a public demo API, since test design should treat this as a data-sensitivity finding.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec defines only `200`; no documented behavior for empty collections or errors.
- Whether `password` is genuinely returned in the live response (vs. the spec merely declaring the field) needs empirical confirmation — this is a potential PII/credential-exposure finding.
