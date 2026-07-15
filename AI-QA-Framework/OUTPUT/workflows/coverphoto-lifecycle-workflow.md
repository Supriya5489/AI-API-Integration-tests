# Business Workflow: Cover Photo Lifecycle Management

**Workflow ID:** WF-COVERPHOTO-001
**Domain:** COVERPHOTO
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** High — see RISK-COVERPHOTO-002

---

## Description

Standalone CRUD lifecycle for a cover photo record. Every cover photo is associated with a book via `idBook`; the book-association lookup is modeled separately in `WF-COVERPHOTO-002`.

## Trigger

Client creates a new cover photo record, typically for a book already in the catalog.

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-COVERPHOTO-002 | POST /api/v1/CoverPhotos | Create a new cover photo (with `idBook` reference and `url`) | `id` |
| 2 | EP-COVERPHOTO-004 | GET /api/v1/CoverPhotos/{id} | Retrieve the created cover photo | `id` |
| 3 | EP-COVERPHOTO-005 | PUT /api/v1/CoverPhotos/{id} | Update cover photo details (e.g., replace `url`) | `id` |
| 4 | EP-COVERPHOTO-006 | DELETE /api/v1/CoverPhotos/{id} | Remove the cover photo | — |

`EP-COVERPHOTO-001` (list all) supports this workflow as a read-only entry point.

## State Dependencies

- Steps 2–4 require the `id` from Step 1 to still reference an existing cover photo.
- Step 1 references `idBook`, expected (but not contractually enforced) to correspond to an existing `Book.id` — see `WF-BOOK-001`.

## Success Criteria

A cover photo can be created, retrieved, updated, and deleted, with reads reflecting the latest writes.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 1 supplies an `idBook` that does not exist | Undocumented — no referential-integrity error specified |
| Step 1 supplies a malformed `url` | Undocumented — no format validation specified in the contract |

## Related Workflows

- WF-BOOK-001 — a cover photo's `idBook` is expected to reference a book from this lifecycle.
- WF-COVERPHOTO-002 — the book-association lookup that consumes cover photos created here.
- WF-CATALOG-001 — the full cross-domain publishing process this lifecycle feeds into.

## Open Questions / Ambiguities

- No documented validation of `url` well-formedness — flagged as a negative-test candidate for Phase 5.
