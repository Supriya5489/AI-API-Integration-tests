# Endpoint: GET /api/v1/Authors/authors/books/{idBook}

**Endpoint ID:** EP-AUTHOR-003
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-002, WF-CATALOG-001
**Risk tier:** Low — see RISK-AUTHOR-001
**Deprecated:** No

---

## Description

Returns all authors associated with a given book ID. Note the irregular path shape (`/Authors/authors/books/{idBook}`, with a duplicated lowercase `authors` segment) — verified as-is from the live spec, not a documentation error.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| idBook | integer (int32) | Yes | Book identifier to look up authors for |

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
| _(not documented in spec — see Open Questions)_ | Expected: empty array or 404 for unknown `idBook` | — |

## Business Rules & Constraints

- Filters the Authors collection by `idBook` — exact filter semantics (case sensitivity, exact match only) not documented.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create book), EP-AUTHOR-002 (create author with matching `idBook`)
- **Downstream (depends on this):** None known

## Open Questions / Ambiguities

- Unclear whether a non-existent `idBook` returns an empty array vs. 404 — needs empirical verification.
- The unusual path structure itself is worth flagging to the API owner as a potential naming/routing inconsistency, though it is in-scope as documented.
