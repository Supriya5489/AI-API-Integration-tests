# Endpoint: GET /api/v1/Authors/{id}

**Endpoint ID:** EP-AUTHOR-004
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-001
**Risk tier:** Low — see RISK-AUTHOR-001
**Deprecated:** No

---

## Description

Returns a single author by its ID.

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

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `Author` object |

```json
{
  "id": "integer(int32)",
  "idBook": "integer(int32)",
  "firstName": "string, nullable",
  "lastName": "string, nullable"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id` | — |

## Business Rules & Constraints

- None documented beyond the `Author` schema.

## Dependencies

- **Upstream (must run before this):** EP-AUTHOR-002 (create)
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Behavior for a non-existent `id` is undocumented (spec shows only `200`).
