# Requirements Analysis Questions — Dayboard

Context: the intent statement, scope document, backlog, feasibility work, wireframes and team practices already settle most of what the first version does. These questions cover the gaps those records leave open, the ones that decide how a requirement is written and tested. Everything else (limits on what the assistant writes, error states, accessibility, the copy-prompt text) gets a sensible default in the requirements, which you can correct when you review them.

## Q1. What happens when you and the assistant change the same thing at nearly the same moment?

The feasibility RAID log (risk R4) asks for one stated rule, so a change is never lost without anyone noticing. Example: you mark an item Done on the page while the assistant is rewriting that block.

A. The later change wins. The page shows the saved state the next time it loads, and nothing tells you a change was replaced.
B. The later change wins, but the page notices on its next save that the item changed underneath it, reloads that block and shows "This changed while you were looking".
C. A change made against an outdated copy is refused, and the one who made it (you or the assistant) is told to reload and try again.
X. Other (please specify)

[Answer]: A

## Q2. Which date counts as "today", and can a page exist for a future date?

The page always opens on "the latest day on record, today or later" (intent statement). The RAID log (assumption A1) proposes that "today" follows this computer's clock and time zone. This decides which page opens, and whether the assistant may prepare tomorrow's page in advance.

A. Today follows this computer's clock and time zone. The assistant may create a page for a future date, and if one exists the page opens on the latest one.
B. Today follows this computer's clock and time zone. Pages can't be created for future dates, so the page opens on today or the latest earlier day.
C. Today follows this computer's clock and time zone. Future pages are allowed, but the page opens on today when today has a page, and on the latest day otherwise.
X. Other (please specify)

[Answer]: B

## Q3. When the assistant asks for notes for tomorrow without naming dates, which day's notes does it get?

The description says "the latest day on record when no dates are given (the Friday page when asked on Monday)". If the assistant has already started Monday's page, Monday is the latest day on record, and its notes are not the ones it wants. This decides the rule the backend follows.

A. The latest day on record before today. On Monday that is Friday, even if Monday's page already exists.
B. The latest day on record, whatever it is, including today.
C. The latest day on record before today, plus today's notes if today's page has any
X. Other (please specify)

[Answer]: C

## Q4. How is the text of items, notes and learnings shown?

The assistant writes most of the text. The RAID log (risk R3) asks that the page show it safely as text. This decides whether the assistant can use formatting, and how much the page has to handle.

A. Plain text only. Line breaks are kept, and nothing else is formatted.
B. Plain text plus inline code (text between backticks shown in a code style), which suits pull request names, branches and commands
C. A small Markdown subset: bold, italic, inline code and line breaks, and nothing that makes headings, images or HTML
X. Other (please specify)

[Answer]: X — allow full markdown, via safe markdown parser, I believe such should exist, if not — build. It is very important to make the text readable

## Q5. What can be removed, and by whom? (select all that apply)

The assistant "writes or updates blocks and items" (description). You discard notes and learnings with a confirmation (wireframes). Nothing says whether an item, a whole block or a whole day can be removed, which matters when the assistant gets a page wrong.

A. The assistant can remove an item
B. The assistant can remove a whole block
C. The assistant can remove a whole day's page
D. You can remove an item or block from the page, with a confirmation
E. Nothing beyond discarding notes and learnings: wrong content is fixed by rewriting it
X. Other (please specify)

[Answer]: A, B, C, D

## Q6. Which languages does the first version's page come in?

Your team practice keeps every piece of page text in one file per language, read through a single helper, with English first. This decides whether the first version ships more than one file, and how the page picks one.

A. English only. The per-language structure is there, so adding a language later is adding one file.
B. English plus one more language (say which in Other), picked from the browser's language setting
C. English plus one more language, with a switch in the header next to the theme switch
X. Other (please specify)

[Answer]: A

## Q7. Should pictures written in Markdown load on the page?

Follow-up to Q4: full Markdown includes pictures that load from any web address. When the page loads one, that address's owner learns that the picture was opened, from this computer and at what time. That sits uneasily with the rule that only this machine can read your pages. Raw HTML in the text is never run: the safe parser shows it as text or drops it.

A. Don't load pictures: show each one as a link with its description, which opens in a new tab
B. Load pictures from any web address
C. Load pictures only after you press "Show picture" on each one
X. Other (please specify)

[Answer]: B — It is my assistant who writes it, and overall, there is no harm in that. because they still cannot read the data, so, we're safe

## Consolidated Summary Confirmation

Does this all look correct before I generate the requirements artifact?

- **Simultaneous changes (Q1, replaced by you at the approval on 2026-10-16):** every page read returns a hash of the page; every change carries the hash of its last read and is refused when the page changed since, so nothing is overwritten silently; whether storage also locks is decided once storage is chosen.
- **Today and future pages (Q2):** "today" follows this computer's clock and time zone; pages can't be created for future dates, so the page opens on today, or on the latest earlier day when today has no page.
- **Notes with no dates named (Q3):** the assistant gets the notes of the latest day on record before today, plus today's notes if today's page has any.
- **Text (Q4):** items, notes and learnings support full Markdown, rendered through a safe Markdown parser (an existing one, or built if none fits); raw HTML is never run; readability comes first.
- **Removing things (Q5):** the assistant can remove an item, a whole block and a whole day's page; you can remove an item or a block from the page, with a confirmation; you can't remove a whole day from the page.
- **Languages (Q6):** English only in the first version, with one text file per language so a language is added by adding a file.
- **Pictures (Q7):** Markdown pictures load from any web address, because your assistant writes them and the picture's host still can't read your pages.
- **Added at the approval (2026-10-16):** the open page updates by itself within 2 seconds (server-sent events preferred, polling acceptable), never disturbing what you're typing, and reconnects on its own.
- **From the review findings you sorted:** stable identifiers for blocks and items; the assistant has your rights on learnings and notes, and a forbidden change is refused; at most one notes block and one learnings block per page; ticking a Markdown checkbox changes only that marker; removing a whole day through the assistant is an accepted risk.
- **From Refined Mockups (2026-10-16):** meetings have a start and end time and a Yes / No / Maybe answer instead of Open / Done / Later; pull requests read Merged; past meetings are dimmed and marked Past; the assistant can add short toned labels to items; a change from the assistant gets a fading highlight and a 5-second toast.
- **From the re-check findings R-10 to R-17:** a checkbox tick carries the page hash; when something is removed, removal wins over typed text and open confirmations; the toast shows only for changes not made in this tab, merged within 5 seconds, "This day was updated"; labels are dropped for this version; all-day and overnight meetings are allowed and stay answerable when past; headings inside items are kept to "###" or smaller through the MCP server's own instructions.
- **From the fresh-check findings R-18 to R-23:** an end earlier than the start means the next day (22:00 to 01:00 is three hours), an equal end or a malformed time is refused, and times are cleared when a meeting becomes all-day; a meeting is Past only once its own end has passed, within 5 seconds, also on earlier days; all-day meetings show first, then by start time; the assistant reads and writes the meeting's all-day flag, times and answer, and a failed answer save reverts like a state change.

- Looks correct
- Request changes

[Answer]: Looks correct
