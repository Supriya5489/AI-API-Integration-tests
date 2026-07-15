# Business Workflow: Author Lifecycle Management

**Workflow ID:** WF-AUTHOR-001
**Domain:** AUTHOR
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** High — see RISK-AUTHOR-002

---

## Description

Standalone CRUD lifecycle for an author record. Every author is associated with a book via `idBook`, but this workflow covers the record management itself (create/read/update/delete); the book-association lookup is modeled separately in `WF-AUTHOR-002`.

## Trigger

Client creates a new author record, typically for a book already in the catalog.

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-AUTHOR-002 | POST /api/v1/Authors | Create a new author (with `idBook` reference) | `id` |
| 2 | EP-AUTHOR-004 | GET /api/v1/Authors/{id} | Retrieve the created author | `id` |
| 3 | EP-AUTHOR-005 | PUT /api/v1/Authors/{id} | Update author details | `id` |
| 4 | EP-AUTHOR-006 | DELETE /api/v1/Authors/{id} | Remove the author | — |

`EP-AUTHOR-001` (list all) supports this workflow as a read-only entry point.

## State Dependencies

- Steps 2–4 require the `id` from Step 1 to still reference an existing author.
- Step 1 references `idBook`, which is expected (but not contractually enforced) to correspond to an existing `Book.id` — see `WF-BOOK-001`.

## Success Criteria

An author can be created, retrieved, updated, and deleted, with reads reflecting the latest writes.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 1 supplies an `idBook` that does not exist | Undocumented — no referential-integrity error is specified; likely accepted as-is given no FK enforcement is visible in the contract |
| Step 4 runs on an author whose book was already deleted | Undocumented cascade/orphan behavior |

## Related Workflows

- WF-BOOK-001 — an author's `idBook` is expected to reference a book from this lifecycle.
- WF-AUTHOR-002 — the book-association lookup that consumes authors created here.
- WF-CATALOG-001 — the full cross-domain publishing process this lifecycle feeds into.

## Open Questions / Ambiguities

- No documented validation that `idBook` corresponds to a real book — flagged as a negative-test candidate for Phase 5.
