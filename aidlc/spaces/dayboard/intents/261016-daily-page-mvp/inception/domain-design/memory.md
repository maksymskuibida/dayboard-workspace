<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-16T23:25:00Z — Modelled RequestGuard as calling the two interfaces (it hands admitted requests on) and ApiClient as calling RequestGuard, so the catalogue shows the real call chain into the backend; the assistant reaches McpInterface the same way from outside.
- 2026-10-16T23:25:00Z — Kept note and learning state fields on DayPages' Item while CarryOver owns their transitions (Q8 A); the interfaces route note and learning changes to CarryOver.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-16T23:25:00Z — Added Q9 after the person's change request on the summary: the first summary named Live updates but not how a write reaches it. The answer (any process may write; Live updates watches a change log in storage) replaced the in-process notice I had assumed.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-10-16T23:25:00Z — Polling SQLite's change counter about every 500 ms instead of in-process notices: a small steady cost and up to half a second of delay, in exchange for live updates from any writer process.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-10-16T23:25:00Z — How long the DayChange log is kept before trimming; decide in Functional Design.
