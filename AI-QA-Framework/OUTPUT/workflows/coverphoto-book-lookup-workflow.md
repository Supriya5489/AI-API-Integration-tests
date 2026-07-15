# Business Workflow: Cover-Photo-to-Book Lookup

**Workflow ID:** WF-COVERPHOTO-002
**Domain:** COVERPHOTO
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** Low — see RISK-COVERPHOTO-001

---

## Description

A read-oriented cross-reference process: given a book, retrieve all cover photos associated with it — e.g., to render cover art on a book detail page.

## Trigger

Client requests the list of cover photos for a specific book.

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-BOOK-002 (precondition, from WF-BOOK-001) | POST /api/v1/Books | A book must already exist | `idBook` |
| 2 | EP-COVERPHOTO-002 (precondition, from WF-COVERPHOTO-001) | POST /api/v1/CoverPhotos | One or more cover photos created with matching `idBook` | — |
| 3 | EP-COVERPHOTO-003 | GET /api/v1/CoverPhotos/books/covers/{idBook} | Retrieve all cover photos for the given book | Array of `CoverPhoto` |

## State Dependencies

- Step 3 depends on Steps 1–2 having been completed (a book and at least one matching cover photo must exist) to return non-empty results.
- If the book was deleted after Step 2, Step 3's behavior toward the now-orphaned cover photos is undocumented.

## Success Criteria

Given a valid `idBook` with associated cover photos, Step 3 returns exactly those photos; given a book with none, it returns an empty array (assumed, not documented).

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| `idBook` does not correspond to any existing book | Undocumented — empty array vs. 404 unconfirmed |
| `idBook` corresponds to a book with zero cover photos | Assumed empty array — not explicitly documented |

## Related Workflows

- WF-BOOK-001 — supplies the book this lookup filters by.
- WF-COVERPHOTO-001 — supplies the cover photo records this lookup returns.
- WF-CATALOG-001 — this lookup is a verification step in the full publishing workflow.

## Open Questions / Ambiguities

- Exact filter semantics (empty result vs. error for unknown `idBook`) require empirical confirmation before Phase 5 test cases can assert expected behavior with confidence.
