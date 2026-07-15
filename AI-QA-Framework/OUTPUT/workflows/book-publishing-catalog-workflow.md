# Business Workflow: Book Publishing & Catalog Assembly

**Workflow ID:** WF-CATALOG-001
**Domain:** CATALOG (cross-domain: BOOK, AUTHOR, COVERPHOTO)
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** Critical — see RISK-CATALOG-001

---

## Description

The one genuinely cross-endpoint business process this API supports: assembling a complete catalog entry for a book — the book record itself, its author(s), and its cover photo — then verifying the assembled entry can be retrieved as a coherent whole. This is the composite of `WF-BOOK-001`, `WF-AUTHOR-001`/`WF-AUTHOR-002`, and `WF-COVERPHOTO-001`/`WF-COVERPHOTO-002`, and represents the realistic end-to-end flow a catalog-management client (e.g., an admin tool or the AI Mentor Bot's content pipeline) would perform when publishing a new book.

## Trigger

A new book needs to be published/cataloged with its full metadata (authorship + cover art).

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-BOOK-002 | POST /api/v1/Books | Create the book record | `idBook` (= book's `id`) |
| 2 | EP-AUTHOR-002 | POST /api/v1/Authors | Create one or more authors, each referencing `idBook` from Step 1 | `idBook` |
| 3 | EP-COVERPHOTO-002 | POST /api/v1/CoverPhotos | Create the cover photo, referencing `idBook` from Step 1 | `idBook` |
| 4 | EP-BOOK-003 | GET /api/v1/Books/{id} | Confirm the book record | — |
| 5 | EP-AUTHOR-003 | GET /api/v1/Authors/authors/books/{idBook} | Confirm the author(s) are associated with the book | — |
| 6 | EP-COVERPHOTO-003 | GET /api/v1/CoverPhotos/books/covers/{idBook} | Confirm the cover photo is associated with the book | — |

## State Dependencies

- Steps 2–3 both require `idBook` from Step 1; they can run in either order or concurrently relative to each other, but both must follow Step 1.
- Steps 4–6 are verification reads and require Steps 1–3 to have completed successfully.
- If Step 1 fails, Steps 2–6 have no valid `idBook` to reference and cannot meaningfully proceed.

## Success Criteria

A book, at least one author, and a cover photo all exist with a consistent `idBook` linkage, and each is independently retrievable and correctly cross-referenced via Steps 4–6.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 1 succeeds but Step 2 or 3 fails (e.g., network error) | No transactional/rollback behavior documented — the book would exist without full metadata; no compensating-action endpoint is documented |
| A book is deleted (EP-BOOK-005) after Steps 2–3 have run | Authors/CoverPhotos become orphaned references with `idBook` pointing at a non-existent book — cascade behavior undocumented, a key Phase 3/5 finding |
| Step 2/3 use an `idBook` typo (valid integer, wrong book) | No referential-integrity check documented — silently succeeds, associating the wrong book |

## Related Workflows

- WF-BOOK-001, WF-AUTHOR-001, WF-AUTHOR-002, WF-COVERPHOTO-001, WF-COVERPHOTO-002 — the individual lifecycle/lookup workflows this composite process is built from.

## Open Questions / Ambiguities

- The API provides no transactional guarantee across Book/Author/CoverPhoto creation — this is architecturally expected for a simple REST CRUD API but should be explicitly called out as an accepted-risk item in Phase 3 rather than assumed safe.
- No documented cascade/restrict policy on book deletion is the single highest-value data-integrity question to resolve before Phase 5 test case authoring for this workflow.
