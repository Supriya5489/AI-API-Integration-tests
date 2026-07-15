# Endpoint: PUT /api/v1/Users/{id}

**Endpoint ID:** EP-USER-004
**Domain:** USER
**Workflow(s):** WF-USER-001
**Risk tier:** Critical — see RISK-USER-002 (state-changing + plaintext `password`)
**Deprecated:** No

---

## Description

Updates an existing user identified by `id`.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | User identifier |

**Query parameters**

_(none)_

**Headers**

- `Content-Type: application/json` (or `text/json`, `application/*+json`)

**Body schema**

```json
{
  "id": "integer(int32)",
  "userName": "string, nullable",
  "password": "string, nullable"
}
```

Schema has `additionalProperties: false`.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | _(no response body schema documented)_ |

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id`, 400 for malformed body | — |

## Business Rules & Constraints

- Password-change semantics (e.g., whether omitting `password` preserves the existing value vs. nulling it) are undocumented.

## Dependencies

- **Upstream (must run before this):** EP-USER-002 (create) — resource must exist
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Path `id` vs. body `id` mismatch behavior undocumented.
- Whether a partial body (e.g., `password` omitted) is treated as "no change" or "clear field" is undocumented — important for negative/edge-case test design in Phase 5.
