# Business Workflow: Author-to-Book Lookup

**Workflow ID:** WF-AUTHOR-002
**Domain:** AUTHOR
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** Low — see RISK-AUTHOR-001

---

## Description

A read-oriented cross-reference process: given a book, retrieve all authors associated with it. This supports a business need like "show all authors of this book" on a book detail page.

## Trigger

Client requests the list of authors for a specific book (e.g., viewing a book's detail page).

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-BOOK-002 (precondition, from WF-BOOK-001) | POST /api/v1/Books | A book must already exist | `idBook` |
| 2 | EP-AUTHOR-002 (precondition, from WF-AUTHOR-001) | POST /api/v1/Authors | One or more authors created with matching `idBook` | — |
| 3 | EP-AUTHOR-003 | GET /api/v1/Authors/authors/books/{idBook} | Retrieve all authors for the given book | Array of `Author` |

## State Dependencies

- Step 3 depends on Steps 1–2 having been completed (a book and at least one matching author must exist) to return non-empty results.
- If the book was deleted after Step 2, Step 3's behavior toward the now-orphaned authors is undocumented.

## Success Criteria

Given a valid `idBook` with associated authors, Step 3 returns exactly those authors; given a book with no authors, it returns an empty array (assumed, not documented).

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| `idBook` does not correspond to any existing book | Undocumented — empty array vs. 404 unconfirmed (see EP-AUTHOR-003 Open Questions) |
| `idBook` corresponds to a book with zero authors | Assumed empty array — not explicitly documented |

## Related Workflows

- WF-BOOK-001 — supplies the book this lookup filters by.
- WF-AUTHOR-001 — supplies the author records this lookup returns.
- WF-CATALOG-001 — this lookup is a verification step in the full publishing workflow.

## Open Questions / Ambiguities

- Exact filter semantics (empty result vs. error for unknown `idBook`) require empirical confirmation before Phase 5 test cases can assert expected behavior with confidence.
