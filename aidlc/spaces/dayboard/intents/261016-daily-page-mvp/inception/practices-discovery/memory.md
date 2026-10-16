<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-16T18:56:31Z — Read the squash-merged PR habit (#1–#3 in the workspace repo) as the suggested way of working for all three repos; the code repos have one commit each, so the habit is assumed to carry over and is put to the person in the interview.
- 2026-10-16T18:56:31Z — Left version-scoped constraints (one person, no sign-in, local only) out of the permanent Mandated/Forbidden rules; promoted as standing rules they would block adding users or hosting later. The privacy rule is worded "for as long as Dayboard has no sign-in" for the same reason.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-16T18:56:31Z — Replaced org's "deploy on merge to staging" default with "nothing is deployed; merged to main counts as released", because Dayboard runs only on the person's machine in this phase; the skeleton verification command doubles as the smoke check.
- 2026-10-16T18:56:31Z — Suggested Ruff (lint + format) for Python instead of org's Black, since one tool fits the "few dependencies" constraint and Ruff's formatter follows Black's style.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-10-16T19:50:30Z — With no walking skeleton, the scripted end-to-end check from the browser-test answer doubles as the smoke check after pulling main; it keeps the release routine without adding a separate check.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-10-16T18:56:31Z — team.md has none of the five section headings and project.md has no Mandated/Forbidden headings; the promotion step may refuse until they exist.
- 2026-10-16T18:56:31Z — The code repos are public but have no licence file and no .gitignore yet; a later stage should add both before the first push of code.
- 2026-10-16T19:50:30Z — Under the pr landing strategy each Unit also gets a workspace pull request for its records, and finishing a Unit expects it merged; the interview chose records at natural pauses only, so confirm how the two fit before Construction.
- 2026-10-16T19:50:30Z — The mvp plan turns the walking skeleton on, while the team chose never to run one; confirm Construction reads the team practice and does not offer the skeleton checkpoint.
