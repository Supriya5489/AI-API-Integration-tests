# Endpoint: DELETE /api/v1/Activities/{id}

**Endpoint ID:** EP-ACTIVITY-005
**Domain:** ACTIVITY
**Workflow(s):** WF-ACTIVITY-001
**Risk tier:** High — see RISK-ACTIVITY-002
**Deprecated:** No

---

## Description

Deletes an activity identified by `id`.

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

_(none — DELETE request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | _(no body documented)_ |

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id` | — |

## Business Rules & Constraints

- No documented cascade behavior (this API has no known FK relationship pointing at Activities).

## Dependencies

- **Upstream (must run before this):** EP-ACTIVITY-002 (create) — resource must exist
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- This is a destructive/state-changing operation on a shared UAT environment — per `AGENT.md` constraints, execution against real/shared data requires explicit scope confirmation before Phase 8.
- Behavior for deleting a non-existent or already-deleted `id` is undocumented (idempotent 200 vs. 404).
