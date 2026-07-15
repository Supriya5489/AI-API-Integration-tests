# Endpoint: GET /api/v1/Users/{id}

**Endpoint ID:** EP-USER-003
**Domain:** USER
**Workflow(s):** WF-USER-001
**Risk tier:** Medium — see RISK-USER-001 (sensitive `password` field)
**Deprecated:** No

---

## Description

Returns a single user by its ID.

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

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | _(no response body schema documented — spec shows only `200`/"Success" with no content, unlike the list endpoint which does document a `User` array)_ |

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id` | — |

## Business Rules & Constraints

- If the live response does include `password`, treat as a data-sensitivity finding per EP-USER-001's note.

## Dependencies

- **Upstream (must run before this):** EP-USER-002 (create)
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Response body schema is undocumented for this specific operation despite the resource clearly being `User` — confirm the actual shape empirically.
- Behavior for a non-existent `id` is undocumented.
