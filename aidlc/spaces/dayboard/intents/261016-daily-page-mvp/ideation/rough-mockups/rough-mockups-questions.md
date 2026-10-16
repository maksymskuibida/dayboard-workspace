# Rough Mockups Questions — Dayboard

Context: the scope document (`ideation/scope-definition/scope-document.md`) fixes what the page holds: a day with six block kinds, item states, notes for tomorrow you can write, a calendar of earlier days, and a copy-prompt button, in light and dark, at phone width, opened on this computer. These questions settle how it is laid out and how it behaves. Accessibility follows WCAG 2.1 AA with full keyboard use unless you say otherwise.

## Q1. How should the six blocks be laid out?

A. One column, blocks stacked in a fixed order, at every width
B. Two columns on a wide window (schedule on one side, the rest on the other), one column when narrow
C. The schedule as a timeline strip across the top, the other blocks in a grid below
D. No preference; propose one
X. Other (please specify)

[Answer]: A

## Q2. Which block comes first when you open the page?

What you see first is what the page is for. The rest follow in a fixed order.

A. Schedule (the shape of the day)
B. Open items (what to do)
C. Notes for tomorrow written yesterday (what you told yourself)
D. The description's order: schedule, open items, pull requests, learnings, waiting on others, notes for tomorrow
X. Other (please specify)

[Answer]: X — let an assistant, who writes the day items write the blocks in order, and display in that order. So, we'll have blocks list, defined on creation

## Q3. What happens to an item when it is marked done?

A. It stays where it is, dimmed and struck through
B. It moves to the bottom of its block, dimmed
C. Done items fold into a "3 done" line at the bottom of the block that you can open
X. Other (please specify)

[Answer]: C

## Q4. How do you set an item's state on the page?

An item has three states: open, done, delayed to tomorrow.

A. A checkbox for done, plus a small "delay" action on each item
B. A three-way switch on each item (open, done, delayed)
C. Clicking the item's marker cycles through the states
X. Other (please specify)

[Answer]: B — for some blocks, for example, waits on you, `done` sounds weird, something like `closed` or something sounds better

## Q5. Where does the calendar of days live?

A. The date in the page heading opens a calendar
B. Previous and next arrows beside the date, and the date opens a calendar
C. A calendar always visible beside the page on a wide window; behind the date when narrow
X. Other (please specify)

[Answer]: B

## Q6. What should the page show when there is no page for today yet?

The page opens on the latest day on record. Early in the morning that may be yesterday, or there may be no days at all on the first run.

A. The latest day as it is, with a banner "No page for today yet" and the copy-prompt button
B. An empty state for today with the copy-prompt button; the latest day one click away
C. The latest day as it is, with no banner
X. Other (please specify)

[Answer]: X — Put the date in url. If opened without date — fallback to the latest. If there is no date saved yet — show some placeholder — it redirects to the latest day, if it exists, if not — placeholder on home page

## Q7. What visual tone should the page have?

There is no existing brand. This sets the starting direction for type, colour and density.

A. Calm and paper-like: warm neutrals, generous spacing, a characterful heading face
B. Crisp product interface: cool neutrals, one accent colour, compact spacing
C. Dense dashboard: maximum information per screen
D. No preference; propose one
X. Other (please specify)

[Answer]: C — But still make it look cool

## Q8. Does every day have all six block kinds? (follow-up to Q2)

You said the assistant writes the blocks in order and the page shows them in that order, with the block list set when the day is created. This settles whether a day may leave some kinds out.

A. A day has only the blocks the assistant wrote, in its order; kinds it left out don't appear
B. Every day has all six kinds in the assistant's order; an empty block shows a short placeholder
C. Every day has all six kinds in the assistant's order; empty blocks are hidden
X. Other (please specify)

[Answer]: A

## Q9. Can a day have two blocks of the same kind? (follow-up to Q2)

For example, two "open items" blocks with their own titles, such as one per project.

A. No: at most one block of each kind per day
B. Yes: several blocks of a kind, each with its own title
X. Other (please specify)

[Answer]: B

## Consolidated Summary Confirmation

- Layout: one column, blocks stacked, at every width (Q1).
- Block order: the assistant writes a day's blocks in an order, and the page shows them in that order; the block list is set when the day is created (Q2).
- A day has only the blocks the assistant wrote; kinds it left out don't appear (Q8).
- A day can have several blocks of the same kind, each with its own title (Q9).
- Done items fold into a "N done" line at the bottom of their block, which you can open (Q3).
- Each item has a three-way switch: open, done, delayed. The label for "done" fits the block kind: for example "closed" in a waiting-on-others block, where "done" reads oddly (Q4).
- Previous and next arrows sit beside the date, and the date opens a calendar (Q5).
- Each day has its own address with the date in it. The home page redirects to the latest day on record when one exists; when no day exists yet, the home page shows a placeholder (Q6).
- Tone: a dense dashboard with maximum information per screen, that still looks good (Q7).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct

## Requested Changes Feedback

What should change?

[Answer]: Item state (Q4): for some blocks, for example, waits on you, `done` sounds weird, something like `closed` or something sounds better. Address (Q6): it redirects to the latest day, if it exists, if not — placeholder on home page
