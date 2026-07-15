# Endpoint: GET /api/v1/Authors

**Endpoint ID:** EP-AUTHOR-001
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-001
**Risk tier:** Low — see RISK-AUTHOR-001
**Deprecated:** No

---

## Description

Returns the full collection of authors.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none documented in spec)_

**Headers**

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Array of `Author` |

```json
[
  {
    "id": "integer(int32)",
    "idBook": "integer(int32)",
    "firstName": "string, nullable",
    "lastName": "string, nullable"
  }
]
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- `idBook` implies a relationship to `Book.id`, but referential integrity is not documented at the contract level.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec defines only `200`; no documented behavior for empty collections or errors.
- No filtering by `idBook` on this list endpoint — that's handled by a separate endpoint (EP-AUTHOR-003).
