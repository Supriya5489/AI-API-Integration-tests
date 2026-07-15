# Endpoint: GET /api/v1/CoverPhotos/books/covers/{idBook}

**Endpoint ID:** EP-COVERPHOTO-003
**Domain:** COVERPHOTO
**Workflow(s):** WF-COVERPHOTO-002, WF-CATALOG-001
**Risk tier:** Low — see RISK-COVERPHOTO-001
**Deprecated:** No

---

## Description

Returns all cover photos associated with a given book ID. Note the irregular path shape (`/CoverPhotos/books/covers/{idBook}`) — verified as-is from the live spec, not a documentation error.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| idBook | integer (int32) | Yes | Book identifier to look up cover photos for |

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
| 200 | Success | Array of `CoverPhoto` |

```json
[
  {
    "id": "integer(int32)",
    "idBook": "integer(int32)",
    "url": "string(uri), nullable"
  }
]
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: empty array or 404 for unknown `idBook` | — |

## Business Rules & Constraints

- Filters the CoverPhotos collection by `idBook`; exact filter semantics not documented.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create book), EP-COVERPHOTO-002 (create cover photo with matching `idBook`)
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- Unclear whether a non-existent `idBook` returns an empty array vs. 404 — needs empirical verification.
