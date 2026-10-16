# Requirements — Dayboard, first version

Inputs: the intent statement (`ideation/intent-capture/intent-statement.md`), the scope document and intent backlog (`ideation/scope-definition/scope-document.md`, `intent-backlog.md`), the feasibility work (`ideation/feasibility/feasibility-assessment.md`, `constraint-register.md`, `raid-log.md`), the rough mockups (`ideation/rough-mockups/wireframes.md`, `user-flow.md`), the team practices (`inception/practices-discovery/team-practices.md`) and the answers in `requirements-analysis-questions.md` (Q1–Q7), together with your later decisions, all listed under Sources. Every requirement names its source in brackets. All example content is made up.

## Intent Analysis

**Goal.** One person replaces the daily report their AI assistant keeps as a chat artifact with a real page of their own: the assistant writes most of it through a local MCP server, and the person reads it and works with it during the day [desc].

**Success test.** The person uses Dayboard every working day for two consecutive weeks with one daily assistant and never falls back to the chat-artifact report (intent statement, Success Metrics). Everything in the scope document is needed for that test from day one (scope document, Minimum Viable Scope).

**What decides trade-offs.** A page that is pleasant and quick to read during the day comes first (intent statement, Initiative Trigger); readable text is "very important" [Q4].

**Request type.** A new product, greenfield, standard complexity: two code repositories (`dayboard-api`, `dayboard-web`), one person, local only.

## Functional Requirements

### FR1 Day pages and their content

| ID | Requirement | Source |
|---|---|---|
| FR1.1 | The system shall keep at most one page per calendar date. A date is a calendar day written `YYYY-MM-DD`, with no time and no time zone. | [desc]; scope document |
| FR1.2 | A page shall have a title of plain text. A page whose title is empty shows no title line; nothing is substituted. | [desc]; wireframes S1 |
| FR1.3 | A page shall hold an ordered list of blocks. The order is the one the writer set, and every reader sees that same order. | rough-mockups Q2; wireframes |
| FR1.4 | Every block shall be one of six fixed kinds: schedule, open items, pull requests, learnings, waiting on others, notes for tomorrow. Schedule, open items, pull requests and waiting-on-others blocks may appear more than once on a page; a page holds at most one learnings block and at most one notes-for-tomorrow block, and a write that would add a second one is refused. Each block has its own optional title. A write naming any other kind is refused. | [desc]; constraint register TC-07; wireframes (Q8, Q9); your decision on review finding R-04 |
| FR1.5 | A block shall hold an ordered list of items. An item has text and zero or more links; a link has a web address and an optional label. | [desc] |
| FR1.6 | Items in open items, pull requests and waiting-on-others blocks shall have exactly one of three states: open, done, delayed to tomorrow. A new item is open. Items in a schedule block have no such state; they carry a meeting answer instead (FR1.13). | [desc]; wireframes (Q4); your decision in Refined Mockups (Q7), 2026-10-16 |
| FR1.7 | Items in a learnings block shall be unsaved, saved or discarded. A new learning is unsaved; a saved learning can be discarded but never returns to unsaved; a discarded learning is gone for the person and the assistant. A change the rules don't allow (saved back to unsaved, or anything out of discarded) is refused with an error naming the item and the change, and nothing is stored. | wireframes, Learnings; your decision on review finding R-03 |
| FR1.8 | Items in a notes-for-tomorrow block shall be kept or discarded. A discarded note is gone for the person and the assistant. Notes have no other state. | wireframes, Notes |
| FR1.9 | "Today" shall be the calendar date on this computer's clock in its local time zone at the moment of the request. | Q2; RAID log A1 |
| FR1.10 | No page shall exist for a date after today. Creating a page, or writing to one, for a future date is refused with an error saying the date is in the future. | Q2 |
| FR1.11 | Nothing on a page shall move to another day by itself. A delayed item stays on its day with the delayed state until someone acts on it. | scope document (Q2) |
| FR1.12 | Every block and every item shall have its own identifier, given when it is created, that never changes and is never reused, including when blocks or items are reordered or others are added or removed. Every change to a block or item names it by that identifier, never by its position. | your decision on review finding R-02 |
| FR1.13 | A schedule item shall be either timed or all-day, and shall carry a meeting answer: yes, no or maybe, or none yet. A timed item has a start and an end time in local time (`HH:MM`). An end later than the start is on the same day; an end earlier than the start is on the next day, for an event running past midnight (22:00 to 01:00 is a three-hour event ending at 01:00 the next day). Because both are clock times, an event is always shorter than 24 hours of clock time; on a daylight-saving night the real length can differ by an hour, and a local time that doesn't exist or occurs twice that night counts as its first valid instant. A timed item whose end equals its start is refused, and so is any time not written as a valid `HH:MM` (for example `25:61`) (FR2.10). An all-day item has no times; when an item becomes all-day its times are cleared, and a change that makes an item timed must give both its start and end, or it is refused. A new schedule item without an answer has none; either the person or the assistant can change the answer at any time, also after the meeting has passed. | your decision in Refined Mockups (Q7); your decisions on review findings R-15, R-18 and R-22, 2026-10-16 |
| FR1.14 | Withdrawn: labels on items are not part of this version (see Out of Scope). The ID is kept so it is not reused. | your decision on review finding R-14, 2026-10-16 |

### FR2 Assistant access through the MCP server

| ID | Requirement | Source |
|---|---|---|
| FR2.1 | The system shall offer an MCP server that any assistant on this computer can use without signing in, with the same operations for every assistant. | [desc]; constraint register TC-06, SC-03 |
| FR2.2 | The assistant shall be able to list the dates that have a page, oldest first, optionally limited to a date range. The answer also states today's date as the system sees it (FR1.9). | [desc]; Q2 |
| FR2.3 | The assistant shall be able to read the page for one date: title, blocks in order, items in order with their states and links, a schedule item's all-day flag, start and end times and meeting answer (FR1.13), every block's and item's identifier (FR1.12), and the page hash (FR4.9). Discarded learnings and notes are never returned. Reading a date with no page returns a "no page for this date" answer, not an error about the request. | [desc]; wireframes |
| FR2.4 | The assistant shall be able to create a page for today or any earlier date. Creating a page for a date that already has one is refused with an error saying so. | [desc]; scope document (Q1, Q3 and its clarification) |
| FR2.5 | The assistant shall be able to change a page's title; add, change, reorder and retitle blocks; and add, change, reorder items and their links, and a schedule item's all-day flag and times (FR1.13), on any existing page. | [desc]; scope document |
| FR2.6 | The assistant shall be able to set the state of any item (FR1.6–FR1.8) and the meeting answer of any schedule item (FR1.13), on any existing page, with the same rights as the person: it can save and discard learnings and discard notes. The usual loop is that the assistant suggests learnings, the person keeps or discards them, and the assistant saves the kept ones into its own skill and marks them saved, or removes them. | [desc]; scope document; your decision on review finding R-03 |
| FR2.7 | The assistant shall be able to remove an item, a whole block, or a whole day's page. A removed day disappears from the list of days and from the calendar. | Q5 (A, B, C) |
| FR2.8 | The assistant shall be able to read notes for tomorrow for a list of dates it names. For each named date it gets that day's kept notes, or a "no page" answer for a date without one. | [desc] |
| FR2.9 | When the assistant names no dates, it shall get the kept notes of the latest day on record before today, plus today's kept notes when today's page exists and has any. The answer states which dates the notes came from. With no earlier day on record, the answer says there are none. | [desc]; Q3 |
| FR2.10 | Every write shall be checked before it is stored. A refused write changes nothing and returns an error that names the field, the value's problem and, for a limit, the limit (NFR3). | RAID log R3 |
| FR2.11 | The assistant shall be able to read learnings across a date range, and that read returns only learnings that were saved and not discarded. Reading a page (FR2.3) also returns its unsaved learnings, marked unsaved, so the assistant can tell what is still waiting for the person. | wireframes, Learnings |
| FR2.12 | The MCP server's own description of its tools shall tell the assistant how to write item text, including that headings inside an item use level 3 ("###") or smaller, so the page's heading order (date, then blocks) holds. | your decision on review finding R-16, 2026-10-16 |

### FR3 Reading a day on the page

| ID | Requirement | Source |
|---|---|---|
| FR3.1 | Opening the home address shall go to the latest day on record. Because no future pages exist (FR1.10), that is today or the latest earlier day. | [desc]; Q2; wireframes (Q6) |
| FR3.2 | When no day is on record, the home address shall show the first-run view (S4) with the copy-prompt button. | wireframes S4 (Q6) |
| FR3.3 | Every day shall have its own address with the date in it, for example `/2026-10-16`. An address for a date with no page shows "No page for this day" with a way to the latest day (S5). An address that is not a valid date shows the same view, not an error page. | wireframes (Q6), S5 |
| FR3.4 | The page shall show the date as the main heading, then the title, then every block in its stored order, each headed by its kind label and its own title. A block with no items shows "Nothing here". | wireframes S1, S6 |
| FR3.5 | Done items in task kinds shall fold into one "N done" line that opens in place to show them, dimmed, each with its state switch. | wireframes (Q3) |
| FR3.6 | Item, note and learning text shall be shown as rendered Markdown (CommonMark plus the common GitHub extensions: tables, strikethrough, task-list markers and autolinks) through a safe Markdown renderer. Headings inside item text render at the level written; the assistant is told to keep them at level 3 or smaller (FR2.12). | Q4; Refined Mockups (Q4); review finding R-16 |
| FR3.7 | Raw HTML inside text shall never run: it is shown as text or dropped. Only `http`, `https` and `mailto` addresses become clickable; any other scheme (for example `javascript:` or `data:`) shows as plain text. | Q4; RAID log R3 |
| FR3.8 | Pictures written in Markdown shall load from the web address given, with their description as alternative text. | Q7 |
| FR3.9 | Links shall open in a new tab without giving the opened page access to Dayboard. An item's links show under its text as short labels, or as the address when no label was given. | wireframes, Links |
| FR3.10 | Discarded notes and learnings shall never be shown. | wireframes |
| FR3.11 | The open page shall stay current without a reload. When the assistant (or another open tab) changes the day being shown, the change appears on the page within 2 seconds (95th percentile, measured as NFR2 says); when a day is added or removed, the calendar and the previous and next arrows follow. | your request at the requirements approval (2026-10-16), replacing the "reload after the assistant writes" assumption in `user-flow.md` |
| FR3.12 | Ticking or unticking a Markdown task-list checkbox (FR3.6) on the page shall change only that one checkbox marker in the stored text and save straight away, like a state change (FR4.2): it shows at once, and returns to the stored state with "Couldn't save, try again" if saving fails. Like every change, the tick carries the page hash (FR4.9) and is refused with "This changed, try again" when the page changed since it was read. | your decisions on review findings R-08, R-09 and R-10 |
| FR3.13 | An update arriving while the person is typing in a field shall never clear or change what they typed, and shall not move the keyboard focus or close an open calendar, confirmation or "N done" list. The one exception is a removal: when the day, block or item being typed into or confirmed is removed, the removal wins, and the typed text and the open confirmation are dropped without a notice. | your request at the requirements approval; corner checklist, screens; your decision on review finding R-11 |
| FR3.14 | When the open day is removed by the assistant, the page shall switch to the "No page for this day" view (S5) for that date. | your request at the requirements approval; FR2.7 |
| FR3.15 | When the page loses its live connection to the backend, it shall keep trying to reconnect on its own and, once back, show every change it missed. While the backend can't be reached it shows the message in FR7.5. | your request at the requirements approval; corner checklist, network |
| FR3.16 | When a live update brings a change not made in this tab (by the assistant or from another tab), the page shall show a toast "This day was updated" for 5 seconds, at the top right on wide screens and at the bottom at phone width, announced through the polite live region. Updates arriving within 5 seconds of each other merge into one toast. Every changed or added item gets a soft highlight fading over 2 seconds (shown without animation when the system asks for reduced motion); removed items get no highlight. Changes made in this tab show neither. | your decision in Refined Mockups (Q3); your decision on review finding R-12, 2026-10-16 |
### FR4 Changing a day on the page

| ID | Requirement | Source |
|---|---|---|
| FR4.1 | Each item in open items, pull requests and waiting-on-others blocks shall have a three-way state switch: Open, the done state, and Later (delayed to tomorrow). The done segment reads "Done" in open items, "Merged" in pull requests and "Closed" in waiting on others. Each schedule item instead has a three-way answer control: Yes, No, Maybe, with none selected until an answer is given. | wireframes (Q4); Refined Mockups (Q1, Q7) |
| FR4.2 | A state change, and a meeting answer change, shall show at once and be saved straight away. If saving fails, the control returns to the stored value and "Couldn't save, try again" appears. | wireframes S6; user-flow |
| FR4.3 | Each unsaved learning shall offer Save and Discard; a saved learning is marked "Saved" and offers only Discard. Discard asks for confirmation ("Discard this learning?") and has no undo. | wireframes, Learnings |
| FR4.4 | Each note shall offer Discard with a confirmation ("Discard this note?") and no undo. | wireframes, Notes |
| FR4.5 | The person shall be able to add a note for tomorrow and add a learning, on any day that has a page. A learning the person adds starts unsaved, like one the assistant wrote. | scope document (Q1); wireframes |
| FR4.6 | When a day has no notes block (or no learnings block), a standing "Add a note for tomorrow" (or "Add a learning") field shall sit at the end of the day and create that block on first use; once the block exists, its own field is used and the standing field is gone. | wireframes S1b |
| FR4.7 | An empty or whitespace-only entry shall not be added. A failed add keeps the typed text in the field and shows "Couldn't save, try again"; an add refused because the page changed (FR4.9) also keeps the typed text. | wireframes S1b |
| FR4.8 | The person shall be able to remove an item or a whole block from the page, after a confirmation naming what is removed. The person can't remove a whole day from the page. | Q5 (D) |
| FR4.9 | Every read of a single day's page, by the assistant or by the web page, shall return a hash of that page's whole stored content, which changes whenever anything on the page changes. Every change request, from the assistant or from the web page, shall carry the hash of the last read of that page. When the hash is not the page's current one, the change is refused with an error saying the page changed since it was read, and nothing is stored; the assistant reads the page again and retries, and the web page returns the control to the stored state with "This changed, try again" and shows the current page. A test covers the assistant reordering a block's items while the person changes one item's state from an earlier read: the second change is refused and the first is kept. Whether storage also uses a lock is decided in the application design, once storage is chosen. | your change at the requirements approval (2026-10-16), replacing the "later change wins" answer to Q1; RAID log R4; review finding R-02 |
| FR4.10 | A schedule item shall be shown dimmed and marked "Past" once its own end has passed by this computer's current local time: a timed item when its end passes, which for an event past midnight is on the next day; an all-day item when its day ends. This holds on earlier days too: at 00:30 today, yesterday's 22:00 to 01:00 item is not Past, and at 01:00 it is. The mark follows the clock as it reads now, including after a clock or daylight-saving change, and appears within 5 seconds of the end passing, without a reload, while the page is open. In a schedule block, all-day items show first with "All day" in place of times, then timed items by start time, ties in stored order; reordering a schedule block doesn't change how it shows. An event running past midnight shows its end as "→ HH:MM". Past items stay answerable, and their dimmed text keeps the 4.5:1 contrast of NFR7. | your decision in Refined Mockups (Q7); your decisions on review findings R-15, R-19, R-20 and R-23, 2026-10-16 |

### FR5 Moving between days

| ID | Requirement | Source |
|---|---|---|
| FR5.1 | Previous and next arrows shall go to the nearest earlier and later days that have a page. At the first day on record the previous arrow is disabled and named "No earlier day"; at the latest day the next arrow is disabled and named "No later day". | wireframes (Q5), S2 |
| FR5.2 | Pressing the date shall open a month calendar. Every date keeps its number; dates without a page are greyed out and can't be chosen; month arrows change the month; "Go to latest day" opens the latest day on record. | scope document (Q3); wireframes S2 |
| FR5.3 | The calendar shall work by keyboard: arrow keys move between days that have a page, Enter opens one, Escape closes the calendar and returns focus to the date. | wireframes S2 |
| FR5.4 | At phone width the calendar opens as a full-width panel under the header with a close button. | wireframes S2 |

### FR6 Copy prompt

| ID | Requirement | Source |
|---|---|---|
| FR6.1 | A "Copy prompt" button in the header, and on the first-run view, shall put on the clipboard the instruction the person pastes into their assistant to prepare or update today's page through the MCP server. It always targets today, even when an earlier day is open. | [desc]; wireframes |
| FR6.2 | The prompt shall ask the assistant to read the notes for tomorrow (FR2.9) and the delayed items of the latest earlier day before writing today's page. | [desc]; user-flow step 3 |
| FR6.3 | The prompt text shall come from the language file (FR9.1) and contain no personal, company or tool-specific names. | rules (Mandated, Forbidden); constraint register SC-04 |
| FR6.4 | After copying, the button shall read "Copied" for 2 seconds. When the browser refuses the copy, the page shows "Couldn't copy. Select the prompt below." with the prompt as selectable text. | wireframes S6; user-flow |

### FR7 Look, theme and layout

| ID | Requirement | Source |
|---|---|---|
| FR7.1 | A three-way theme switch (Light, Dark, Auto) shall sit in the header. Auto is the default and follows the system setting; the person's choice is remembered in this browser. | wireframes, Theme |
| FR7.2 | Every width shall use one single-column layout. From 360 px wide upwards nothing scrolls sideways, long text wraps, and the state switch moves under the item text at phone width. | wireframes (Q1), S3 |
| FR7.3 | At phone width "Copy prompt" shall become an icon button named "Copy prompt", and the header's name, that icon and the theme switch fit on one line. | wireframes S3 |
| FR7.4 | While a day loads, the header shall stay in place and each block shows a grey placeholder row. | wireframes S6 |
| FR7.5 | When the backend can't be reached, the page shall show "Can't reach Dayboard on this computer. Is the backend running?" with Try again. When one day fails to load while the backend is up, it shows "Couldn't load this day." with Try again, and the header and date controls stay usable. | wireframes S6; user-flow, Error Paths |
| FR7.6 | A block the page can't show shall read "This block can't be shown" while the other blocks show normally. | wireframes S6 |

### FR8 Running it and keeping it private

| ID | Requirement | Source |
|---|---|---|
| FR8.1 | One command shall start the backend (its HTTP interface and the MCP server), and one command shall start the web app, on this computer. Each person runs their own copy. | constraint register TC-02, TC-06 |
| FR8.2 | Both shall work on a first run with no day data, no cache and no configuration, creating whatever data location they need. | corner checklist, first run; [desc] |
| FR8.3 | The backend shall accept connections only from this computer. A connection from another machine is never served. | rules (Mandated); constraint register SC-01, SC-02; RAID log R1 |
| FR8.4 | The backend shall answer browser requests only from the Dayboard web app's own address. A request carrying any other web origin is refused before it reads or changes anything. | rules (Mandated); RAID log R1 |
| FR8.5 | The backend shall refuse a request whose host name is not this computer's (`localhost`, `127.0.0.1` or `::1`), so a web page can't reach it by pointing its own domain name at this computer. | RAID log R1; corner checklist |
| FR8.6 | The day data location shall be ignored by git in every repository before the first day is written. | rules (Mandated, Forbidden); constraint register SC-01; RAID log R2 |
| FR8.7 | Examples, fixtures, tests, screenshots and documentation shall contain only made-up content. | rules (Mandated); constraint register SC-04 |

### FR9 Page text and languages

| ID | Requirement | Source |
|---|---|---|
| FR9.1 | Every piece of user-facing text on the page (labels, buttons, messages, the copy prompt) shall come from one file per language through a single helper. No component contains copy. | team practices, Code Style; rules (Mandated, Forbidden) |
| FR9.2 | The first version shall ship English only. Adding a language means adding one file and no code change. | Q6 |
| FR9.3 | A text key missing from the language file shall leave its element empty or hidden, never filled with substitute text. | rules (Forbidden) |

## Non-Functional Requirements

| ID | Requirement | Measure (pass/fail) | Source |
|---|---|---|---|
| NFR1 | Privacy of page content | Automated tests show: a request from another machine is not served (FR8.3); a request with a foreign `Origin` is refused (FR8.4); a request with a foreign `Host` is refused (FR8.5); `git check-ignore` reports the data location as ignored in a fresh clone (FR8.6). All four run in CI. | rules (Mandated); constraint register SC-01, SC-02; RAID log R1, R2 |
| NFR2 | Speed on this computer | With both apps running locally and a day of 200 items: the day page shows its content within 1 s of navigation (95th percentile of 20 loads); a state change is stored within 300 ms (95th percentile of 50 changes); the MCP read of that day returns within 300 ms; a change the assistant stores appears on an open page within 2 s (95th percentile of 20 changes). | intent statement, Initiative Trigger; FR3.11 |
| NFR3 | Limits on what is written | Page title up to 200 characters; block title up to 120; item, note and learning text up to 10,000 characters; up to 10 links per item; a web address up to 2,048 characters; up to 50 blocks per page and 200 items per block. A write over any limit is refused as FR2.10 says, and a test covers each limit at, and one past, its value. | RAID log R3; corner checklist |
| NFR4 | Text survives unchanged | Text with emoji, right-to-left script, quotes, backticks, pipes and newlines is stored and read back byte-for-byte (round-trip test through the MCP server and the HTTP interface). | RAID log R3; corner checklist |
| NFR5 | Writes are whole or not at all | A write interrupted at any point (process killed during the write, in a test) leaves the previous stored page readable and unchanged; every write the system confirmed survives a restart. | corner checklist, storage |
| NFR6 | Stored data survives upgrades | A change to how day data is stored keeps a copy of the data as it was, then moves existing days forward on start-up; a test upgrades a sample data set from the previous format and reads every day back. | team practices, Deployment |
| NFR7 | Accessibility | WCAG 2.1 level AA. Text keeps at least 4.5:1 contrast on every surface in both themes, dimmed past items included. The landmarks, heading order, keyboard order and names in the wireframes are met; status is never shown by colour alone; save failures, "Copied" and "Couldn't copy" are announced through a polite live region; touch targets are at least 44 px. Checked by an automated accessibility scan with no violations on S1–S6, plus the keyboard checks in FR5.3. | wireframes, Accessibility; constraint register TC-08 |
| NFR8 | Visual quality | Every screen has a type scale, spacing, visual hierarchy and considered states, in light and dark, at 360 px and at desktop width; checked at the Refined Mockups and Build and Test reviews against the wireframes. | rules (Mandated); constraint register TC-08 |
| NFR9 | Test coverage and checks | At least 80% line coverage in `dayboard-api` and in `dayboard-web`, measured on every pull request; lint, format and type checks pass in CI; a failure blocks the merge. | team practices, Testing Posture, Code Style |
| NFR10 | Ready to move host later | All reading and writing of day data goes through one storage module in the backend; no other module touches the stored files or database. Checked by review and by an import-boundary test. | constraint register TC-03; RAID log R5 |
| NFR11 | Few dependencies | Every runtime dependency in either repository has a one-line reason in the pull request that added it. | constraint register TC-04; team practices, Code Style |
| NFR12 | Public-repository hygiene | No secret, credential, token, key, company name or employer tool appears in either repository; a secret scan runs in CI. | rules (Forbidden); constraint register SC-04 |

## Constraints

- **One person, no user field** (constraint register TC-01): nothing is keyed by user; adding users later must not need a rewrite.
- **Local only, two commands** (TC-02, TC-06): no hosting, no shared space.
- **Movable later** (TC-03): see NFR10.
- **Stack** (TC-04): backend in Python; web app in TanStack Start with TanStack Query, and TanStack Form wherever a form exists. Tooling per the team practices: Ruff, mypy strict and uv for Python; Biome, pnpm and strict TypeScript for the web app.
- **Fixed, generic block kinds** (TC-07): custom kinds are out of scope but not designed out.
- **No sign-in** (SC-03): privacy comes from FR8.3–FR8.6, not from authentication.
- **Public repositories** (SC-04): FR8.7, NFR12.
- **Team way of working** (team practices): test-after per layer; every Unit lands through pull requests the project owner merges.

## Assumptions

| Assumption | Reason |
|---|---|
| The storage choice (one file per day, or a single database file) is made in Domain Design. Nothing here depends on which one wins. | Constraint register TC-05; RAID log R5 carries it to Domain Design |
| How the assistant connects to the MCP server (the transport) is fixed in Contract Design, within FR8.3–FR8.5. | A design decision, not a requirement |
| Supported browsers are current Chrome, Firefox and Safari on desktop; the backend runs on macOS and Linux with a current Python 3 and the web app with a current Node LTS. | One person on their own computer (feasibility Q3, Q5); RAID log A3 |
| A remote picture in Markdown (FR3.8) can tell its host that the page was opened; you accepted this because the host still can't read your pages. | Q7 |
| The assistant you use daily can reach an MCP server on this computer. | RAID log A2; checked at the first end-to-end run |
| The page hash (FR4.9) is enough for simultaneous changes from one person and their assistant; it is not a lock, and two changes checked at the very same instant could both pass until storage adds one. | Constraint register TC-01; your change at the requirements approval |
| Removing a whole day through the assistant (FR2.7) needs no confirmation and can't be undone. If it happens by mistake, the assistant recreates the day. You accepted this risk for this version. | Your decision on review finding R-07 |

## Out of Scope

| Excluded from this version | Source |
|---|---|
| Users, sign-in and authentication | intent statement; scope document |
| Cloud deployment of any kind | intent statement; scope document |
| Custom block kinds added by users | intent statement; scope document |
| A plugin or engine layer | intent statement; scope document |
| Notifications | intent statement; scope document |
| Any integration that reads the person's tools directly | intent statement; scope document |
| Importing today's chat-artifact reports | scope document; constraint register OC-03 |
| Opening the page from another device | scope document; feasibility Q3 |
| Pages for future dates | Q2 |
| Removing a whole day from the page (the assistant can, FR2.7) | Q5 |
| Languages other than English | Q6 |
| Labels the assistant adds to items (withdrawn FR1.14) | your decision on review finding R-14, 2026-10-16 |
| Undo for discarded notes and learnings, and for removed items, blocks and days | wireframes |

## Open Questions

- **Storage choice** (TC-05): decided in Domain Design, with the trade-off written down.
- **MCP transport**: decided in Contract Design, within FR8.3–FR8.5.
- **How live updates reach the page** (FR3.11): you prefer server-sent events (the backend pushes each change to the open page); polling is acceptable. Decided in Contract Design; the update channel obeys the same privacy checks as every other request (FR8.3–FR8.5).
- **Exact copy-prompt wording**: settled in Refined Mockups, within FR6.1–FR6.3.

## Sources

- [desc] The project description (`project-description.json`).
- `ideation/intent-capture/intent-statement.md`: goal, success metrics, product boundary.
- `ideation/scope-definition/scope-document.md` and `intent-backlog.md`: what the first version holds (IB-01 to IB-09).
- `ideation/feasibility/constraint-register.md`, `feasibility-assessment.md`, `raid-log.md`: TC-01 to TC-08, SC-01 to SC-04, OC-03, R1 to R5, A1 to A3.
- `ideation/rough-mockups/wireframes.md` and `user-flow.md`: screens S1 to S6, states and error paths.
- `inception/practices-discovery/team-practices.md`: testing, tooling, deployment and way of working.
- `aidlc/spaces/dayboard/memory/project.md`: the affirmed Mandated and Forbidden rules, cited as "rules".
- `.claude/knowledge/aidlc-product-agent/corner-checklist.md`: the corner sweep behind FR2.10, FR8.2, FR8.5 and NFR3 to NFR5.
- `inception/requirements-analysis/requirements-analysis-questions.md`: Q1 to Q7 and the confirmed summary.
- Your decisions in Refined Mockups (`inception/refined-mockups/refined-mockups-questions.md`, Q1, Q3, Q7), 2026-10-16: meeting times and answers (FR1.6, FR1.13, FR4.1), past meetings (FR4.10) and the change cue (FR3.16); assistant labels (FR1.14) were later withdrawn on review finding R-14.
- Your decisions on the re-check findings R-10 to R-17 (2026-10-16): FR1.13, FR2.3, FR2.12, FR3.6, FR3.11 to FR3.13, FR3.16, FR4.7, FR4.9, FR4.10 and NFR7.
- Your decisions on the fresh-check findings R-18 to R-23 (2026-10-16): FR1.13 (overnight ends, clock edges, malformed times), FR2.3, FR2.5, FR2.6, FR4.2 (meeting fields and answers) and FR4.10 (Past on earlier days, display order, 5-second bound).
- Your decisions on the review findings (R-01 to R-09) and your request at the requirements approval to add live page updates (FR3.11, FR3.13 to FR3.15). Live updates widen the scope document, which did not list them; they replace the "you reload" assumption in `user-flow.md`.

## Assumptions & Open Questions

The Assumptions and Open Questions sections above list them; nothing else is open.
