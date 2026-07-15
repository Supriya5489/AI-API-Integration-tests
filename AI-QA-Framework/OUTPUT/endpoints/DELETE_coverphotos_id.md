# Endpoint: DELETE /api/v1/CoverPhotos/{id}

**Endpoint ID:** EP-COVERPHOTO-006
**Domain:** COVERPHOTO
**Workflow(s):** WF-COVERPHOTO-001
**Risk tier:** High — see RISK-COVERPHOTO-002
**Deprecated:** No

---

## Description

Deletes a cover photo identified by `id`.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | Cover photo identifier |

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

- No documented cascade behavior.

## Dependencies

- **Upstream (must run before this):** EP-COVERPHOTO-002 (create)
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- Destructive operation on shared UAT data — requires scope confirmation before Phase 8 execution per `AGENT.md`.
