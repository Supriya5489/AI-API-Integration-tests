# Endpoint: GET /api/v1/CoverPhotos

**Endpoint ID:** EP-COVERPHOTO-001
**Domain:** COVERPHOTO
**Workflow(s):** WF-COVERPHOTO-001
**Risk tier:** Low — see RISK-COVERPHOTO-001
**Deprecated:** No

---

## Description

Returns the full collection of book cover photos.

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
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- `idBook` implies a relationship to `Book.id`; referential integrity not documented at the contract level.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec defines only `200`; no documented behavior for empty collections or errors.
- `url` format is `uri` but no validation of reachability/format is documented.
