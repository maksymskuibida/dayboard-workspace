# User Stories Questions — Dayboard

Context: the approved requirements (`inception/requirements-analysis/requirements.md`) settle what Dayboard does. These questions settle how the stories are cut: who they are written for, how they are grouped, how big they are, and which ones the first version can't ship without. The stories feed Units Generation, where the two developers will claim pieces of work, so the cut matters for how cleanly that work splits.

Every story will follow "As a [persona], I want [goal], so that [benefit]", with Given/When/Then acceptance criteria (the inception rule) and stable IDs (`US1.1`, `AC1.1.1`).

## Q1. Who are the personas?

The description names two actors on every page: you, and your assistant, which writes most of the page through the MCP server. This decides whose stories exist.

A. Two personas: the person who runs Dayboard, and their AI assistant as a non-human persona with its own goals (prepare the day, keep notes, carry work over)
B. One persona, the person. The assistant's work is written as the person's goal ("I want my assistant to…")
C. The two personas in A, plus a later "second person who installs their own copy", kept only to check that nothing designs that case out
X. Other (please specify)

[Answer]: A

## Q2. How are the stories grouped?

This decides the epics, and so the most natural boundaries for Units later.

A. By the daily loop: preparing the day, reading it, acting on it, carrying over to tomorrow, looking back, and running it privately
B. By feature area, mirroring the requirement groups: day pages, assistant access, reading, changing, moving between days, copy prompt, look, privacy, page text
C. By actor: everything the assistant does, then everything the person does on the page
X. Other (please specify)

[Answer]: A

## Q3. How big should a story be?

Each story should be buildable and testable on its own. Size here means how much behaviour one story covers, not time. Smaller stories split more evenly between two developers; larger ones are fewer to review.

A. Small: one behaviour each (for example "mark an item done" or "read notes by dates"), roughly 30 to 40 stories
B. Medium: one screen or one MCP operation each (for example "the calendar" or "create a page"), roughly 15 to 20 stories
C. Large: one epic each with its acceptance criteria (for example "moving between days"), roughly 8 to 10 stories
X. Other (please specify)

[Answer]: C

## Q4. How are the privacy guard and the two-command run written?

The privacy rules (FR8.3 to FR8.6) and the two-command run (FR8.1) are needed from the first Unit but aren't something you do on the page. This decides whether they get stories with acceptance criteria, or stay requirements that later design stages carry.

A. As stories from your side ("As the person, I want my pages unreachable from any other site, so that…"), with acceptance criteria that become the first Unit's tests
B. As requirements only; the stories point at them, and the non-functional design stages carry them
X. Other (please specify)

[Answer]: B

## Q5. Which stories can the first version ship without? (select all that apply)

The scope document marks everything in the description as needed from day one. Since then you added live updates, and the requirements grew some extras. Priorities here inform the line Delivery Planning draws; anything not chosen stays a Must Have.

A. Live updates while the page is open (FR3.11 to FR3.15): Should Have, the page can reload instead
B. Ticking Markdown checkboxes on the page (FR3.12): Could Have
C. The assistant reading saved learnings across a date range (FR2.11): Should Have
D. The calendar (FR5.2 to FR5.4): Should Have, the arrows are enough at first
E. None: everything is a Must Have
X. Other (please specify)

[Answer]: B

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Personas (Q1):** two: the person who runs Dayboard, and their AI assistant as a non-human persona with its own goals.
- **Grouping (Q2):** by the daily loop: preparing the day, reading it, acting on it, carrying over to tomorrow, looking back, and running it privately.
- **Story size (Q3):** large: one epic per story with its acceptance criteria, roughly 8 to 10 stories.
- **Privacy guard and two-command run (Q4):** stay requirements only; stories point at them, and the non-functional design stages carry them.
- **Priorities (Q5):** ticking Markdown checkboxes on the page (FR3.12) is a Could Have; every other story is a Must Have.

- **Carried in from the reopened requirements (2026-10-16):** the stories follow the changed requirements: meetings with Yes / No / Maybe, all-day and overnight meetings, Past within 5 seconds and on earlier days, all-day first then by start time, the "This day was updated" toast only for changes not made in this tab, removal winning over typed text, the MCP server telling the assistant to keep headings to "###", and no labels on items.

- Looks correct
- Request changes

[Answer]: Looks correct
