<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-17T01:05:00Z — Assigned each story to the Unit whose finish makes it work end to end, so the assistant's stories sit in U2 and US2.2 in U5, while U1 appears in every story's "Also needs".

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-17T01:05:00Z — Added a sixth Unit (installation, with launchd and systemd examples) at the person's request after the first summary confirmation, above the "4 to 5" they gave in Q2; and wrote story points to unit-points.csv for the project panel, which the stage itself does not ask for.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
