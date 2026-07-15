# Endpoint: DELETE /api/v1/Users/{id}

**Endpoint ID:** EP-USER-005
**Domain:** USER
**Workflow(s):** WF-USER-001
**Risk tier:** Critical — see RISK-USER-002 (state-changing + plaintext `password`)
**Deprecated:** No

---

## Description

Deletes a user identified by `id`.

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

- No documented cascade behavior for related records referencing this user (none evident in this API's schema set).

## Dependencies

- **Upstream (must run before this):** EP-USER-002 (create)
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- Destructive operation on shared UAT data — requires scope confirmation before Phase 8 execution per `AGENT.md`.
- Idempotency of repeated deletes on the same `id` is undocumented.
