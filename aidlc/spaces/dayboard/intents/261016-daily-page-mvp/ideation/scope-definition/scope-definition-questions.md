# Scope Definition Questions — Dayboard

Context: the intent statement (`ideation/intent-capture/intent-statement.md`) fixes the product boundary, and the feasibility work (`ideation/feasibility/feasibility-assessment.md`, `constraint-register.md`) found nothing that blocks it. These questions settle what the first cut holds, in what order, and two gaps the description leaves open.

## Q1. What can you do on the page yourself?

Your description says you and the assistant can both change an item's state. It doesn't say whether you can also write on the page. This decides whether the first version needs any forms at all.

A. Change item states only (open, done, delayed to tomorrow); the assistant writes everything else
B. Change states, and add, edit or remove items myself
C. Change states, and write notes for tomorrow myself
D. Everything the assistant can do
X. Other (please specify)

[Answer]: C — assistant can do it too, of course

## Q2. What happens to an item you mark "delayed to tomorrow"?

This decides whether Dayboard moves delayed items itself, or leaves it to the assistant when it prepares the next day.

A. Nothing automatic: the assistant sees delayed items when it prepares the next day and carries them over
B. Dayboard copies it onto the next day's page as soon as that page exists
C. It becomes a note for tomorrow on the same day, which the assistant reads next day
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q3. Can you open days other than the latest one on the page?

The page always opens on the latest day on record. This decides whether you can look back at earlier days, or only the assistant can.

A. Yes: step back and forward between days
B. Yes: a list of days to pick from
C. No: the page shows only the latest day; the assistant reads earlier days
D. Not yet defined
X. Other (please specify)

[Answer]: X — As B, but with calendar, not just list. Non existing days are greyed out — assistant can read and edit them too

## Q4. Which of these could wait until after the first cut without failing your two-week test? (select all that apply)

Your success check is two weeks of daily use with no fallback to the chat artifact. Anything not needed for that can come right after.

A. The "copy prompt" button (you can type or keep the instruction yourself at first)
B. Reading notes for tomorrow by a list of dates (the latest day on record is enough at first)
C. Links on items
D. Dark mode (light first)
E. None: everything in the description is needed from day one
X. Other (please specify)

[Answer]: E

## Q5. What should be built first?

A. Value first: the assistant writes a day and you can read it on the page, end to end; then states, notes and the rest
B. Risk first: lock the server to this machine before anything holds real data; then the page
C. Both in one first slice: a locked-down server and one readable page together
D. No preference
X. Other (please specify)

[Answer]: D

## Consolidated Summary Confirmation

- On the page you change item states and write notes for tomorrow yourself; the assistant can do both too, and writes everything else (Q1).
- A delayed item is not moved automatically: the assistant sees delayed items when it prepares the next day and carries them over (Q2).
- You can open earlier days from a calendar; days with no page are greyed out. The assistant can read and edit earlier days too (Q3).
- Everything in the description is needed in the first cut; nothing waits (Q4).
- No preference on build order; it is decided when the work is planned (Q5).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct

## Requested Changes Feedback

What should change?

[Answer]: On the page (Q1): assistant can do it too, of course. Earlier days (Q3): assistant can read and edit them too
