# Endpoint: POST /api/v1/Activities

**Endpoint ID:** EP-ACTIVITY-002
**Domain:** ACTIVITY
**Workflow(s):** WF-ACTIVITY-001
**Risk tier:** High — see RISK-ACTIVITY-002
**Deprecated:** No

---

## Description

Creates a new activity.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none)_

**Headers**

- `Content-Type: application/json` (or `text/json`, `application/*+json` — all three accepted per spec)

**Body schema**

```json
{
  "id": "integer(int32)",
  "title": "string, nullable",
  "dueDate": "string(date-time)",
  "completed": "boolean"
}
```

Schema has `additionalProperties: false` — unknown fields should be rejected.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `Activity` object (echoes/creates the resource) |

```json
{
  "id": "integer(int32)",
  "title": "string, nullable",
  "dueDate": "string(date-time)",
  "completed": "boolean"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- No documented uniqueness, required-field, or value-range constraints beyond the schema's declared types.
- `additionalProperties: false` implies unrecognized fields in the request body should be rejected — worth a negative test case.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping (likely feeds GET/PUT/DELETE by ID for the created resource)

## Open Questions / Ambiguities

- Whether client-supplied `id` is honored or server-generated/overwritten is not documented — needs empirical verification.
- No documented validation error (e.g., malformed `dueDate`, missing `title`) — spec shows only `200`.
