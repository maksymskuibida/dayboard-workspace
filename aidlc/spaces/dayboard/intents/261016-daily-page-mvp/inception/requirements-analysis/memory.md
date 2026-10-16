<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-16T20:22:28Z — Read "full Markdown" as CommonMark plus the common GitHub extensions, with raw HTML never run and only http, https and mailto links clickable; the person asked for full Markdown through a safe parser and said readability comes first.
- 2026-10-16T20:22:28Z — With no future pages allowed, wrote the description's "latest day on record, today or later" as today or the latest earlier day, and said so in FR3.1.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-16T20:45:00Z — At the approval the person replaced Q1's "later change wins" with a page hash every change must carry, and added live page updates (server-sent events preferred, polling acceptable); both written as changes from the approval, not as new answers to the questions file.
- 2026-10-16T20:22:28Z — Added a host-name check (FR8.5) beyond the stated privacy rule, from the corner checklist: without it a web page can reach a loopback-only server by pointing its own domain name at this computer.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-10-16T20:01:28Z — Asked only six questions and settled the remaining corners (input limits, link schemes, error states, accessibility, copy-prompt text) as defaults in the requirements; the corner checklist says to ask only what depends on the person's world, and earlier stages already decided most behaviour.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-10-16T20:01:28Z — Storage choice (files vs single database file, TC-05) is left to Domain Design, where the RAID log carries it (R5); requirements stay storage-neutral.
- 2026-10-16T20:01:28Z — project.md's Way of Working note (squash-merged pull requests) is read before team.md's "merge strategy: pr" line; check that Construction still picks the pull-request landing.
