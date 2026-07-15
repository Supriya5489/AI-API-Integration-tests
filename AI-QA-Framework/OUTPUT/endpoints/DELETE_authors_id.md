# Endpoint: DELETE /api/v1/Authors/{id}

**Endpoint ID:** EP-AUTHOR-006
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-001
**Risk tier:** High — see RISK-AUTHOR-002
**Deprecated:** No

---

## Description

Deletes an author identified by `id`.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | Author identifier |

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

- No documented cascade effect on related `Book`/`CoverPhoto` records.

## Dependencies

- **Upstream (must run before this):** EP-AUTHOR-002 (create)
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- Destructive operation on shared UAT data — requires scope confirmation before Phase 8 execution per `AGENT.md`.
- Idempotency of repeated deletes on the same `id` is undocumented.
