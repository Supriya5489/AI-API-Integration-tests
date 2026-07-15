# Endpoint: PUT /api/v1/Authors/{id}

**Endpoint ID:** EP-AUTHOR-005
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-001
**Risk tier:** High — see RISK-AUTHOR-002
**Deprecated:** No

---

## Description

Updates an existing author identified by `id`.

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

- `Content-Type: application/json` (or `text/json`, `application/*+json`)

**Body schema**

```json
{
  "id": "integer(int32)",
  "idBook": "integer(int32)",
  "firstName": "string, nullable",
  "lastName": "string, nullable"
}
```

Schema has `additionalProperties: false`.

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
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id`, 400 for malformed body | — |

## Business Rules & Constraints

- Path `id` vs. body `id` mismatch behavior is undocumented.

## Dependencies

- **Upstream (must run before this):** EP-AUTHOR-002 (create) — resource must exist
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Behavior when updating `idBook` to a non-existent book is undocumented.
