# Endpoint: PUT /api/v1/Activities/{id}

**Endpoint ID:** EP-ACTIVITY-004
**Domain:** ACTIVITY
**Workflow(s):** WF-ACTIVITY-001
**Risk tier:** High — see RISK-ACTIVITY-002
**Deprecated:** No

---

## Description

Updates an existing activity identified by `id`.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | Activity identifier |

**Query parameters**

_(none)_

**Headers**

- `Content-Type: application/json` (or `text/json`, `application/*+json`)

**Body schema**

```json
{
  "id": "integer(int32)",
  "title": "string, nullable",
  "dueDate": "string(date-time)",
  "completed": "boolean"
}
```

Schema has `additionalProperties: false`.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `Activity` object |

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
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id`, 400 for malformed body | — |

## Business Rules & Constraints

- Whether path `id` must match body `id` is not documented — undefined behavior on mismatch.
- `additionalProperties: false` implies unknown fields should be rejected.

## Dependencies

- **Upstream (must run before this):** EP-ACTIVITY-002 (create) — resource must exist
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Behavior when path `id` and body `id` disagree is undocumented.
- Idempotency of repeated identical PUTs is not documented — assumed idempotent per REST convention, needs verification.
