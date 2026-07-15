# Endpoint: DELETE /api/v1/Books/{id}

**Endpoint ID:** EP-BOOK-005
**Domain:** BOOK
**Workflow(s):** WF-BOOK-001, WF-CATALOG-001
**Risk tier:** Critical — see RISK-BOOK-003 (cross-domain orphaning risk on Author/CoverPhoto)
**Deprecated:** No

---

## Description

Deletes a book identified by `id`.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | Book identifier |

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

- No documented cascade behavior for dependent `Author`/`CoverPhoto` records that reference this book's `id` via `idBook` — a likely orphaned-reference risk worth flagging in Phase 3.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create)
- **Downstream (depends on this):** EP-AUTHOR-003, EP-COVERPHOTO-003 (both filter by `idBook` and may return stale/orphaned data after deletion)

## Open Questions / Ambiguities

- Destructive operation on shared UAT data — requires scope confirmation before Phase 8 execution per `AGENT.md`.
- Whether deleting a book cascades to or orphans related authors/cover photos is undocumented and is a key data-integrity test candidate for Phase 5.
