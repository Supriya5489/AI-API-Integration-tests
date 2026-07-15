# Business Workflow: Book Catalog Lifecycle Management

**Workflow ID:** WF-BOOK-001
**Domain:** BOOK
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** High overall (RISK-BOOK-002) — Step 4 (DELETE) is individually Critical (see RISK-BOOK-003) due to cross-domain orphaning risk

---

## Description

The core catalog record lifecycle: a book is created, retrieved, updated, and eventually removed. This is the anchor resource for the cross-domain publishing process (see `WF-CATALOG-001`), since both `Author` and `CoverPhoto` reference a book via `idBook`.

## Trigger

Client creates a new book record in the catalog.

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-BOOK-002 | POST /api/v1/Books | Create a new book | `id` (becomes `idBook` for related records) |
| 2 | EP-BOOK-003 | GET /api/v1/Books/{id} | Retrieve the created book | `id` |
| 3 | EP-BOOK-004 | PUT /api/v1/Books/{id} | Update book details | `id` |
| 4 | EP-BOOK-005 | DELETE /api/v1/Books/{id} | Remove the book | — |

`EP-BOOK-001` (list all) supports this workflow as a read-only entry point.

## State Dependencies

- Steps 2–4 require the `id` from Step 1 to still reference an existing book.
- **Step 4 (delete) has a downstream effect on `WF-CATALOG-001`:** any `Author`/`CoverPhoto` records with `idBook` pointing at this book become orphaned references, since no cascade behavior is documented (see EP-BOOK-005 Open Questions).

## Success Criteria

A book can be created, retrieved, updated, and deleted, with reads reflecting the latest writes and its `id` correctly usable as `idBook` in related resources.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 4 (delete) runs while Authors/CoverPhotos still reference this `idBook` | Undocumented — no cascade/restrict behavior specified; a key data-integrity test candidate for Phase 5 |
| Step 1 is retried after timeout | No idempotency key documented — risk of duplicate book records |

## Related Workflows

- WF-CATALOG-001 — this lifecycle is the anchor step of the full Book Publishing & Catalog Assembly workflow.
- WF-AUTHOR-002, WF-COVERPHOTO-002 — both depend on a book existing (via `idBook`) to produce meaningful lookup results.

## Open Questions / Ambiguities

- No documented uniqueness constraint on `title` — duplicate books with identical titles appear permitted by the contract.
- Cascade/orphan behavior on delete (see Step 4) is the most significant open question for this workflow and should be prioritized in Phase 3 risk scoring.
