# Endpoint: GET /api/v1/Activities/{id}

**Endpoint ID:** EP-ACTIVITY-003
**Domain:** ACTIVITY
**Workflow(s):** WF-ACTIVITY-001
**Risk tier:** Low — see RISK-ACTIVITY-001
**Deprecated:** No

---

## Description

Returns a single activity by its ID.

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

_(none required)_

**Body schema**

_(none — GET request)_

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
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id` | — |

## Business Rules & Constraints

- None documented beyond the `Activity` schema.

## Dependencies

- **Upstream (must run before this):** EP-ACTIVITY-002 (an activity must exist to fetch it) — to be confirmed as a formal workflow in Phase 2
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec documents only `200` — actual behavior for a non-existent `id` (404 vs. 200 with empty body vs. 500) is unconfirmed and must be tested empirically.
- Behavior for non-numeric/malformed `id` (e.g., string, negative number) is undocumented.
