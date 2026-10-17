<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-16T21:33:32Z — Read the screenshot of the person's current schedule as a shape reference only (time range, title, answer, colour-toned labels, past meetings dimmed); no name or content from it went into any file, because the repositories are public.
- 2026-10-16T21:33:32Z — Gave assistant labels five fixed tones and a 40-character, 8-per-item limit, so the page can guarantee contrast and the backend can validate them.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-16T21:33:32Z — Changed the approved requirements and stories during this stage (meeting times and answers, labels, past meetings, change cue), because the design answers changed behaviour, not only looks; the person confirmed that in the summary.

- 2026-10-16T22:58:26Z — Drew the refined mockups as a Claude Design canvas on top of the written ones, after the person moved to an account with Design; the text wireframes stay the reference for behaviour and the canvas for the look, and the confirmation Remove button got its own solid danger token.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-10-16T21:33:32Z — Used system fonts and no web fonts for the liquid-glass look, to keep dependencies and page weight low; the glass surfaces and the type scale carry the style.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-10-16T21:33:32Z — Headings written inside items render at their level (Q4), which can break the page's heading order and fail the accessibility scan; decide before Build and Test whether the assistant avoids them or the rule is waived for item content.
- 2026-10-16T21:33:32Z — A visual design artifact can be added once the Design canvas is available; the person will ask about it on Monday.
