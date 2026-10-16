# Refined Mockups Questions — Dayboard

Context: the rough wireframes (`ideation/rough-mockups/wireframes.md`) already fix the layout, the screens S1 to S6, the states, the keyboard order and the dense-but-designed tone. Since then the requirements and stories added full Markdown, live updates, a page hash that can refuse a change, removing items and blocks from the page, and Markdown checkboxes. These questions settle what the refined mockups still need: labels, the visual direction, and how the new behaviours look. Everything else is refined from the wireframes as they are. Accessibility stays at WCAG 2.1 AA (requirements NFR7).

## Q1. What do the three state segments read in each block kind?

The wireframes left the done label open for schedule, open items and pull requests; waiting on others reads "Closed" (rough mockups Q4). The third segment reads "Later" everywhere. This sets the labels in the language file.

A. Schedule: Open / Done / Later · Open items: Open / Done / Later · Pull requests: Open / Merged / Later · Waiting on others: Open / Closed / Later
B. Schedule: Upcoming / Attended / Later · Open items: Open / Done / Later · Pull requests: Open / Done / Later · Waiting on others: Open / Closed / Later
C. "Done" for every kind except waiting on others ("Closed"), and "Later" everywhere
X. Other (please specify)

[Answer]: X — As A, but schedule must carry yes/no/maybe, as google calendar does, because I'll use only google calendar schedule now

## Q2. Which visual direction should the page take?

The tone is dense, with tight rows and small type steps, but still designed (rough mockups Q7: "make it look cool"). The team practice keeps dependencies few. This decides the look and how it is built.

A. Quiet and editorial: a neutral grey scale, one accent colour, a system font stack for text and a monospace face for code and times, styled with plain CSS and design tokens (no UI library)
B. Crisp and app-like: the same palette and fonts, built on Tailwind CSS with a small set of own components
C. Warm and paper-like: an off-white page, a serif face for the date and titles, a sans face for items, plain CSS and design tokens
X. Other (please specify)

[Answer]: X — liquid glass

## Q3. How does the page show that the assistant just changed something?

The page now updates by itself (requirements FR3.11). Without any cue you may not notice what changed while you were reading.

A. A changed or new item gets a soft highlight that fades over 2 seconds (no motion when the system asks for reduced motion)
B. No cue: the page just updates
C. A small "Updated just now" line under the date, plus the fading highlight from A
X. Other (please specify)

[Answer]: A — And a small toast, for 5 sec at the right top corner on wide, and on the bottom on mobile-like

## Q4. How do Markdown headings inside an item look?

Item text is full Markdown (requirements FR3.6), so the assistant can write "# Heading" inside an item. On the page the date is the only main heading and each block has its own heading; full-size headings inside items would break that hierarchy and the dense look.

A. Shown as bold text at item size, with no heading role, so the page's heading order stays date, then blocks
B. Shown as small headings one step below the block heading
X. Other (please specify)

[Answer]: X — Shown as they are, assistant instruction is what controls them

## Q5. Where do the remove controls sit for items and blocks?

You can remove an item or a whole block from the page, after a confirmation (requirements FR4.8). The wireframes have no place for that control yet, and a dense page has little room.

A. A "…" more button at the end of each item and each block heading, opening a small menu with Remove
B. A remove button that appears on hover and keyboard focus, at the end of each item and block heading
C. Removing is only in the "…" menu of the block heading, which also lists the block's items to remove
X. Other (please specify)

[Answer]: A

## Q6. Which copy-prompt wording should the language file start with?

The copy prompt is the instruction you paste into your assistant (requirements FR6.1 to FR6.3). It is plain text, contains no personal, company or tool names, and always targets today. Two drafts:

- **Draft A (short):** "Update my Dayboard for today through the Dayboard MCP server. First read the notes for tomorrow and the items left for later from my latest earlier day. Then check my tools and write today's page: schedule, open items, pull requests, waiting on others. Keep my existing changes and learnings."
- **Draft B (step by step):** "Prepare today's page in Dayboard using its MCP server. 1. Ask Dayboard what today is and read the notes for tomorrow (no dates needed). 2. Read my latest earlier day and carry over anything marked Later. 3. Look through my tools and fill today's blocks. 4. If today's page already exists, update it instead of creating a new one, and re-read it if Dayboard says it changed."

A. Draft A
B. Draft B
X. Other (please specify)

[Answer]: A

## Q7. How do the yes / no / maybe answers sit with the schedule item's state?

Follow-up to Q1: schedule items now carry a calendar-style answer (Yes, No, Maybe). Today every schedule item also has the Open / Done / Later switch (requirements FR1.6). Whichever you pick changes FR1.6 for schedule items.

A. The answer replaces the switch for schedule items: each meeting shows Yes / No / Maybe, which you or the assistant can change, and nothing else
B. Both: the answer (Yes / No / Maybe) plus the Open / Done / Later switch
C. The answer is a read-only label the assistant copies from your calendar; the Open / Done / Later switch stays
X. Other (please specify)

[Answer]: A — also scheduled task is marked somehow, when it's time has passed, as on screenshot. And assistant may add labels, as on the screenshot attached

## Q8. How is the liquid-glass look built?

Follow-up to Q2: liquid glass means translucent, blurred layers over the page with soft highlights. It needs a fallback where the system asks for reduced transparency, and text contrast must stay at least 4.5:1 on every layer (NFR7).

A. Plain CSS with design tokens (translucent surfaces with background blur), no UI library; solid surfaces when reduced transparency is asked for
B. Tailwind CSS with the same tokens and the same fallback
X. Other (please specify)

[Answer]: A

## Q9. How do you want to use Claude Design for the mockups?

You'd like the refined mockups made in Claude Design (claude.ai/design). I can read or update a Claude Design project only through `/design-sync`, which you start. Pick the order that suits you.

A. I write a design brief first (screens, states, tokens, liquid-glass direction, your answers); you build the screens in Claude Design from it; then you start `/design-sync` (or share the project) and I write the mockups and specs from what you made
B. I write the design system as local files first (tokens and component previews); you start `/design-sync` to push them into a Claude Design project; you lay out the screens there; then I write the specs from the result
C. You build in Claude Design on your own, without a brief from me, and tell me when it's ready
X. Other (please specify)

[Answer]: X — you use claude design tool, I have used it before. you can create design artifact

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **State labels (Q1, Q7):** open items Open / Done / Later; pull requests Open / Merged / Later; waiting on others Open / Closed / Later. Meetings carry Yes / No / Maybe instead of the switch, changed by you or the assistant, and stay answerable once past. A meeting is dimmed and marked Past once its own end has passed, within 5 seconds, also on earlier days; dimmed text still meets 4.5:1. Your screenshot is used only as a reference for this shape; none of its names or content go into any file.
- **Requirement changes this brings (settled in the reopened Requirements Analysis and User Stories, 2026-10-16):** a meeting is either all-day or has a start and end time (an end earlier than the start means the next day, shown as "→ 01:00"); all-day meetings show first, then by start time, ties in stored order. Labels on items were dropped for this version, so the mockups show none.
- **Look (Q2, Q8):** liquid glass: translucent, blurred surfaces with soft highlights, plain CSS with design tokens and no UI library; solid surfaces when the system asks for reduced transparency; text contrast at least 4.5:1 on every surface.
- **Change cue (Q3):** a changed or added item gets a soft highlight fading over 2 seconds (no animation with reduced motion). A toast "This day was updated" shows for 5 seconds, top right on wide screens and at the bottom at phone width, only for changes not made in this tab, with changes inside 5 seconds merged into one toast; removals show it too. When something you are typing in is removed, the removal wins and the typed text is dropped.
- **Markdown headings (Q4):** shown as written; the MCP server's description of its tools tells the assistant to keep headings inside items to "###" or smaller, so the page's heading order holds.
- **Remove controls (Q5):** a "…" more button at the end of each item and each block heading, with Remove in its menu.
- **Copy prompt (Q6):** draft A, the short one.
- **Mockups as standard Markdown (Q9, changed by you on 2026-10-16: "go standard"):** the Claude Design canvas isn't available in any session yet, so the refined mockups are written the standard way: annotated text wireframes per screen and state in `mockups.md`, with the interaction spec, design-system mapping and accessibility checklist beside it, all with made-up content. A visual design artifact can be added later, once the Design canvas is available.

- Looks correct
- Request changes

[Answer]: Looks correct
