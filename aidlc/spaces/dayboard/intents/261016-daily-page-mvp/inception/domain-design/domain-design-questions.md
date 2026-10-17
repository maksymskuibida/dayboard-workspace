# Domain Design Questions — Dayboard

Context: this stage names the building blocks of Dayboard (the code you write, not the things it runs on) and which block owns which data. The requirements (`inception/requirements-analysis/requirements.md`) already fix the behaviour, and the stack is fixed too: a Python backend (`dayboard-api`) and a TanStack Start web app (`dayboard-web`). How the blocks are grouped into pieces of work is decided in Units Generation, and the wire formats in Contract Design. These questions settle the choices left open: storage, where the day rules live, the live-update path, the privacy guard and the web app's structure.

## Q1. Where are the days stored?

The constraint register left this open (TC-05): simplicity for now decides, and the choice must not make a later move to another host a rewrite (TC-03, RAID R5). Either way, only one storage module touches the data (NFR10), the folder is ignored by git (FR8.6), and a write is whole or not at all (NFR5).

A. One JSON file per day (`2026-10-16.json`) in a git-ignored data folder, written to a temporary file and then renamed into place. Easy to read and back up by hand; listing days reads the folder; a learnings read across a date range opens one file per day
B. One SQLite database file in the git-ignored data folder, using the Python standard library's `sqlite3` (no added dependency). Transactions make writes whole and add a real lock behind the page hash (FR4.9); the schema can later move to a hosted SQL database such as Cloudflare D1
C. Start with A, behind a storage interface written so B can replace it without touching anything else
X. Other (please specify)

[Answer]: B — but behind storage interface, as suggested in C for A

## Q2. How are the backend's building blocks split?

The assistant (through the MCP server) and the page (through the HTTP interface) change the same days with the same rules (FR2.6: same rights as the person). The day rules include item states, learning and note transitions, schedule times, the page hash, "no future pages" and the limits.

A. Ports and adapters: one **Day** block owns every rule and the page hash; the **MCP interface** and the **HTTP interface** are thin adapters that translate requests and call it; **Storage** sits behind it; a **Live updates** block tells open pages when a day changed. The privacy guard is its own block (Q5)
B. Fewer blocks: one **Interfaces** block holding both MCP and HTTP, plus **Day** and **Storage**; live updates live inside Interfaces
C. Finer blocks: split **Day** into **Day pages** (pages, blocks, task items, schedule) and **Carry-over** (notes for tomorrow and learnings, with the cross-day reads FR2.8, FR2.9, FR2.11), on top of A
X. Other (please specify)

[Answer]: C

## Q3. What is one unit of change for a day?

Every change carries the hash of the whole page and is refused when the page changed since it was read (FR4.9). Blocks and items have their own identifiers that never change (FR1.12).

A. The whole day page is one unit: page, its blocks and their items are read, checked and saved together, and the hash covers all of it. Simple and matches FR4.9; two changes to different blocks of the same day can't both pass from the same read, which is intended
B. Each block is its own unit with its own hash, so changes to different blocks don't refuse each other; the page hash becomes a combination of block hashes. Fewer refusals, more rules
X. Other (please specify)

[Answer]: A

## Q4. Where are the display-only rules worked out?

Some rules only shape how a day looks: a meeting is "Past" once its end passes, by this computer's clock, within 5 seconds and without a reload (FR4.10); a schedule shows all-day meetings first, then by start time (FR4.10); done items fold into "N done" (FR3.5). The stored order stays the order the writer set (FR1.3).

A. The web app works them out from the stored day and its own clock, so the "Past" mark can tick on its own; the backend returns days exactly as stored and the assistant sees the stored order
B. The backend returns a ready-made view: each meeting with a `past` flag and in display order; the page re-reads every few seconds to keep "Past" current
C. Both: the backend owns the rules as a shared definition and returns the flags, and the web app recomputes "Past" locally between reads
X. Other (please specify)

[Answer]: A

## Q5. Where does the privacy guard sit?

The backend answers only this computer (FR8.3), only the web app's own origin for browser requests (FR8.4), and only the host names `localhost`, `127.0.0.1` and `::1` (FR8.5). It applies to the HTTP interface, the live-update channel and, depending on the transport chosen in Contract Design, the MCP server.

A. A separate **Request guard** block that every incoming connection passes before any interface sees it, with its own tests (NFR1)
B. Each interface checks its own requests, sharing one small helper
X. Other (please specify)

[Answer]: A

## Q6. What does a "day changed" notice carry to the open page?

The open page updates within 2 seconds when the assistant or another tab changes the day (FR3.11), shows a toast and highlights changed items (FR3.16), and follows days being added or removed (FR3.11, FR3.14). The channel itself (you prefer server-sent events) is decided in Contract Design.

A. A small notice: which date changed and its new hash (or that it was removed); the page re-reads that day, then compares old and new to find what to highlight. Nothing private travels twice, and a missed notice is fixed by the next read
B. The whole new day in the notice, so the page needs no second read
X. Other (please specify)

[Answer]: A

## Q7. How is the web app split?

Every user-facing text comes from one language file through a single helper (FR9.1), and Markdown is rendered safely (FR3.6, FR3.7).

A. **App shell** (header, theme switch, Copy prompt), **Day view** (blocks, items, state switches, meeting answers, notes and learnings, remove and confirmations), **Day navigation** (arrows and calendar), **Live sync** (the update channel, toast and highlight), **Markdown text** (safe rendering and checkbox ticks), **Page text** (the language helper) and one **API client** that every part uses to reach the backend
B. The same, but Live sync and the API client are one **Data** block
C. Fewer: **App shell**, **Day view** (including navigation and Markdown) and **Data**, with Page text inside the shell
X. Other (please specify)

[Answer]: A

## Q8. Follow-up: who owns notes and learnings, now that Carry-over is its own block?

You chose a separate **Carry-over** block for notes and learnings (Q2, C) and the whole day as one unit of change, covered by one page hash (Q3, A). Notes and learnings are blocks on a day's page, so they sit inside that one unit. Every piece of data needs exactly one owning block, so this decides which block owns them.

**Proposed:** A, because it keeps one owner, one hash and one save path for a day, while Carry-over still holds its own rules.

A. **Day pages** owns every block and item, notes and learnings included, and the page hash. **Carry-over** owns the rules for notes and learnings (unsaved, saved, discarded) and the cross-day reads (notes for tomorrow, learnings across dates), and reaches the data only through Day pages
B. **Carry-over** owns the notes and learnings blocks themselves; Day pages asks Carry-over for them whenever it reads, hashes or saves a day. A cleaner split of data, but every save of a day spans two blocks
C. Go back to Q2 A: one **Day** block holds everything
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Storage (Q1):** one SQLite database file in the git-ignored data folder, through the standard library's `sqlite3`, behind a storage interface so the storage can be replaced without touching anything else.
- **Backend blocks (Q2, Q8):** ports and adapters. **Day pages** owns every page, block and item (notes and learnings included) and the page hash. **Carry-over** owns the note and learning rules and the cross-day reads, and reaches the data only through Day pages. **MCP interface** and **HTTP interface** are thin adapters. **Storage** sits behind Day pages. **Live updates** tells open pages when a day changed.
- **Unit of change (Q3):** the whole day page is read, checked and saved together, and one hash covers all of it.
- **Display rules (Q4):** the web app works out Past, the schedule display order and the done fold from the stored day and its own clock. The backend returns days exactly as stored.
- **Privacy guard (Q5):** a separate **Request guard** block that every incoming connection passes before any interface sees it, with its own tests.
- **Day-changed notice (Q6):** a small notice with the date and its new hash, or that it was removed. The page re-reads the day and compares old and new to find what to highlight.
- **How a change reaches the open page (Q9, after your change request):** any process may write, because there may be two workers now or later. Every write goes through **Day pages** and **Storage** in one database transaction that also checks the page hash, so two writers can't both win from the same read. In that same transaction, Storage records which day changed and its new hash, or that the day was removed. **Live updates** checks the database's change counter about every 500 ms; when it moves, Live updates reads which days changed since its last check and pushes a notice for each one to every open page over the **HTTP interface**'s server-sent events stream. This works whichever process made the write, the delay stays within the 2-second target (FR3.11), and Live updates reads only through Storage (NFR10).
- **Web app (Q7):** App shell, Day view, Day navigation, Live sync, Markdown text, Page text and one API client.

- Looks correct
- Request changes

[Answer]: Looks correct

## Requested Changes Feedback

What should change?

[Answer]: I don't see in application design anything about how one place writes the data and live updates know about the update and send it via SSE

## Q9. Follow-up: how does a change reach the open page?

Every write, from the assistant or from the page, has to end as a "day changed" notice that the backend pushes to open pages over server-sent events (FR3.11, Q6). How that works depends on whether every write passes through one backend process. The requirements say one command starts the backend with both its HTTP interface and the MCP server (FR8.1). If the assistant instead started its own MCP process (a stdio server, one per assistant), that process would write to the same SQLite file without the backend knowing.

**Proposed:** A, because one writer keeps the page hash, the lock and the change notice in one place, and FR8.1 already puts the MCP server inside the backend.

A. **One writer.** The backend process is the only thing that writes. The MCP server runs inside it and the assistant connects to it over HTTP on this computer (transport details stay in Contract Design). After **Day pages** saves a day, it tells **Live updates** in the same process ("2026-10-16 changed, new hash", or "removed"). Live updates pushes that notice to every open page over the **HTTP interface**'s server-sent events stream. Instant and simple; the assistant must connect to the running backend instead of starting its own MCP process
B. **Any process may write.** **Live updates** watches the database for changes (SQLite's change counter, checked about every 500 ms) and pushes a notice for each changed day. Works even when the assistant starts its own MCP process, but adds polling and up to half a second of delay
C. **One writer with a thin relay.** The assistant may start a small MCP process, but it holds no rules and forwards every call to the running backend's API, so writes still happen only in the backend, as in A
X. Other (please specify)

[Answer]: B — because we may have 2 workers now or in the future
