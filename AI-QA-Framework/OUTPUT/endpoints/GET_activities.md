# Endpoint: GET /api/v1/Activities

**Endpoint ID:** EP-ACTIVITY-001
**Domain:** ACTIVITY
**Workflow(s):** WF-ACTIVITY-001
**Risk tier:** Low — see RISK-ACTIVITY-001
**Deprecated:** No

---

## Description

Returns the full collection of activities (to-do/task-like items with a title, due date, and completion flag).

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none documented in spec — no filtering/pagination)_

**Headers**

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Array of `Activity` (see below) |

```json
[
  {
    "id": "integer(int32)",
    "title": "string, nullable",
    "dueDate": "string(date-time)",
    "completed": "boolean"
  }
]
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- None documented in spec beyond the `Activity` schema shape.

## Dependencies

- **Upstream (must run before this):** None (read-only, no precondition documented)
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec defines only a `200` response; behavior for empty collections, server errors, etc. is not documented and must be verified empirically.
- No pagination/filtering parameters exist — unclear if the live API caps result size for a large dataset.
