<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-17T01:55:00Z — Left whole-day create and remove off the HTTP contract, because the person can't remove a day from the page (FR4.8) and days are created by the assistant; only the MCP tools have them.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-17T02:15:00Z — Changed how review findings reach the person, at their request: each finding as a bullet with its ID, severity and one plain sentence (never a table); resolved or accepted findings in their own list first, each starting with "resolved" or "accepted", then the open ones; the list shown before asking whether to put the open findings into a question form or continue; and with a form, no approval question until every finding in it is answered.
- 2026-10-17T02:30:00Z — At the person's request, every revision made after review findings gets a fresh check by the reviewer before the approval question, every time, so the person never approves changes nobody re-checked.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
