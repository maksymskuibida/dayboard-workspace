# Architecture Decisions — Dayboard, Domain Design

The decisions made while naming Dayboard's building blocks (`components.md`). Each one comes from your answers in `domain-design-questions.md` (Q1 to Q9, and your change request on the summary) and is recorded with its context, consequences, security implications and the alternatives you did not pick. Date: 2026-10-17.

| ADR | Title | Status |
|---|---|---|
| ADR-001 | Days are stored in one SQLite file behind a storage interface | Accepted |
| ADR-002 | Ports and adapters, with Carry-over as its own block | Accepted |
| ADR-003 | A whole day page is one unit of change | Accepted |
| ADR-004 | Day pages owns notes and learnings and checks their states on every save | Accepted |
| ADR-005 | Display-only rules are worked out in the web app | Accepted |
| ADR-006 | A separate request guard in front of every interface | Accepted |
| ADR-007 | Any process may write; Live updates watches storage for changes | Accepted |
| ADR-008 | A day-changed notice carries the date and hash only | Accepted |
| ADR-009 | The web app is split into seven blocks | Accepted |

## ADR-001: Days are stored in one SQLite file behind a storage interface

### Context
The constraint register left storage open (TC-05): plain files, one per day, or a single database file. Simplicity for the current goal decides, a later move to another host must not be a rewrite (TC-03, RAID R5), writes must be whole or not at all (NFR5), only one module may touch the data (NFR10), and dependencies stay few (TC-04). Later, Q9 settled that more than one process may write.

### Decision
All day data lives in one SQLite database file inside the git-ignored data folder, used through the Python standard library's `sqlite3` (no added dependency). It is reached only through the **Storage** block, whose interface speaks in days, not tables, so another store can replace it without touching anything else (your note on Q1: "but behind storage interface, as suggested in C for A").

### Consequences
- **Positive:** transactions make every write whole; the write lock lets the page-hash check run inside the transaction, so two writers can't both succeed from the same read, which the requirements assumed was not yet possible; the schema can later move to a hosted SQL database such as Cloudflare D1; listing days and reading learnings across dates are single queries.
- **Negative:** the data is not readable or editable by hand with a text editor; a backup is a copy of the database, not a folder of readable files.
- **Neutral:** NFR6 upgrades become schema migrations, preceded by a copy of the data made with SQLite's own backup interface while the write lock is held (ADR-007), because a plain file copy is not consistent under write-ahead logging. The database runs with write-ahead logging, so readers never block the writer, and with full durability on commit (NFR5). A writer that finds the lock held waits a short, bounded time, within NFR2's 300 ms, and then refuses with a retryable error instead of hanging; NFR Design confirms the exact wait.
- **Security:** the file sits in the git-ignored data folder (FR8.6, the Mandated rule), so day content can't be committed; it is a local file with the person's own file permissions and is never served directly.

### Alternatives Rejected
- **One JSON file per day (Q1 A):** readable by hand, but whole writes need a temporary-file-and-rename routine, there is no lock across processes, and cross-day reads open one file per day.
- **JSON first, SQLite later (Q1 C):** you kept its storage interface but not its first step; starting with SQLite avoids writing and testing a store that is replaced.

## ADR-002: Ports and adapters, with Carry-over as its own block

### Context
The assistant (through MCP) and the person (through the web page) change the same days with the same rights (FR2.6). The rules include item states, note and learning transitions, schedule times, the page hash, no future pages and the limits. Notes and learnings also drive the only reads that span days (FR2.8, FR2.9, FR2.11).

### Decision
The backend is ports and adapters: **DayPages** holds the day rules (note and learning states included) and the page hash, **CarryOver** holds the cross-day reads and adding notes and learnings (ADR-004), **McpInterface** and **HttpInterface** are thin adapters over both, **Storage** sits behind DayPages, and **LiveUpdates** notifies open pages. The privacy guard is its own block (ADR-006). (Q2 C.)

### Consequences
- **Positive:** one set of rules for both callers, so the assistant and the person can't drift apart; each adapter is tested against the same domain; the cross-day reads stay small and separately testable.
- **Negative:** seven backend blocks for a small product; the adapters duplicate some request shapes.
- **Neutral:** Units Generation decides whether these become one piece of work or several.
- **Security:** no interface holds a rule, so a check can't be skipped by calling through the other interface.

### Alternatives Rejected
- **One Day block (Q2 A):** simpler, but notes, learnings and cross-day reads would grow inside the largest block.
- **One Interfaces block for MCP and HTTP (Q2 B):** fewer blocks, but the two adapters change for different reasons and MCP may run in several processes (ADR-007).

## ADR-003: A whole day page is one unit of change

### Context
Every change carries the hash of the whole page and is refused when the page changed since it was read (FR4.9). Blocks and items have their own lasting identifiers (FR1.12).

### Decision
A day page, its blocks and their items are loaded, checked and saved together inside one storage transaction, and one hash covers all of it. (Q3 A.)

### Consequences
- **Positive:** matches FR4.9 exactly; one save path; easy to reason about and test (the FR4.9 test of a reorder racing a state change).
- **Negative:** two changes to different blocks of the same day, made from the same read, can't both pass; the second is refused and retried. This is intended.
- **Neutral:** a day of 200 items is still small enough to save whole within NFR2.
- **Security:** no partial writes; a refused change stores nothing.

### Alternatives Rejected
- **A hash per block (Q3 B):** fewer refusals, but more rules, a combined page hash, and a weaker guarantee for whole-page changes such as reordering blocks.

## ADR-004: Day pages owns notes and learnings and checks their states on every save

### Context
With CarryOver as its own block (ADR-002) and the whole day as one unit of change (ADR-003), the notes and learnings blocks sit inside the day that the page hash covers. Every entity needs exactly one owner. The architecture review then found that keeping the state rules in CarryOver while both interfaces also change items through DayPages would let a plain item change skip them.

### Decision
**DayPages** owns every block and item, notes and learnings included, and the page hash (Q8 A). It also checks the note and learning states on every save, whichever caller made the change: a new learning is unsaved; unsaved can become saved or discarded; saved can only become discarded; nothing leaves discarded (FR1.7); notes are kept or discarded (FR1.8). **CarryOver** holds no data and no state rules; it answers the cross-day reads and adds notes and learnings, reading and writing only through DayPages. (Your pick on review finding R-02: B.)

### Consequences
- **Positive:** one owner, one hash, one save path and one place for every rule that guards the day; no caller can skip the note and learning rules. A contract test writes a learning state directly through each interface and expects the refusal.
- **Negative:** DayPages grows by the note and learning transitions.
- **Neutral:** "discarded notes and learnings are never returned" stays a read rule of DayPages, because it owns the reads.
- **Security:** none beyond ADR-003.

### Alternatives Rejected
- **CarryOver owns the notes and learnings blocks (Q8 B):** a cleaner split of data, but every save of a day would span two owners and the hash would be computed from both.
- **One Day block (Q8 C):** reverses ADR-002.
- **State rules in CarryOver, with DayPages refusing state changes that don't come through it (R-02 A):** keeps CarryOver larger, but needs a way to tell callers apart and leaves the rule outside the data's owner.

## ADR-005: Display-only rules are worked out in the web app

### Context
Some rules only shape how a day looks: the Past mark within 5 seconds of a meeting's end, by this computer's clock (FR4.10); the schedule order (FR4.10); the done fold (FR3.5). Stored order stays the writer's order (FR1.3).

### Decision
The web app works these out from the stored day and its own clock; the backend returns days exactly as stored, and the assistant sees the stored order. (Q4 A.)

### Consequences
- **Positive:** the Past mark ticks on its own with no extra reads; the backend stays a faithful store; the assistant's view is never reordered under it.
- **Negative:** the display rules live in TypeScript and need their own unit tests in `dayboard-web`.
- **Neutral:** the browser and the backend run on the same computer, so they read the same clock.
- **Security:** none.

### Alternatives Rejected
- **The backend returns a ready-made view (Q4 B):** needs re-reads every few seconds to keep Past current.
- **Both (Q4 C):** two copies of the same rules that can disagree.

## ADR-006: A separate request guard in front of every interface

### Context
The backend answers only this computer (FR8.3), only the web app's origin for browser requests (FR8.4) and only the host names localhost, 127.0.0.1 and ::1 (FR8.5), for HTTP, the stream and possibly MCP. These are Mandated rules for as long as Dayboard has no sign-in.

### Decision
A separate **RequestGuard** block admits every connection before any interface sees it, with its own tests (Q5 A). Because more than one process may run the MCP server (ADR-007), every process that listens on a network port runs its own RequestGuard in front of its listener, with the same tests. An MCP process that speaks stdio listens on no port and needs no guard: it talks only to the program on this computer that started it, so no web page or other machine can reach it. (Your pick on review finding R-01: B.)

### Consequences
- **Positive:** one guard and one set of tests for every listener (NFR1); a new interface or a new listening process is protected by default; both ways an assistant can run the MCP server are covered.
- **Negative:** the guard must know how each transport presents peer address, host and origin, and each listening process must install it.
- **Neutral:** the exact allowed origin is set in Contract Design. A test starts an HTTP MCP process and checks it refuses a foreign host and origin.
- **Security:** this is the privacy boundary of the product; it reads and changes nothing before admitting a request, and refuses with no page content. The stdio case is safe only because it opens no port; a stdio process must never also open a listener without the guard.

### Alternatives Rejected
- **Each interface checks its own requests (Q5 B):** one forgotten check exposes day content.

## ADR-007: Any process may write; Live updates watches storage for changes

### Context
The open page must show a change within 2 seconds, whoever made it (FR3.11), and a "day changed" notice is pushed over server-sent events (FR3.16, Q6). Your change request on the summary pointed out that the design didn't yet say how a write reaches that notice. If every write passes through one process, that process can notify directly; but an assistant may start its own MCP process, and you expect two workers now or later (Q9).

### Decision
Any process may write. Every write goes through **DayPages** and **Storage** in one transaction that checks the page hash and, in the same transaction, records a **DayChange**: the date, its new hash or that it was removed, and a growing sequence number. **LiveUpdates** reads the highest change sequence about every 500 ms, which works from any connection; when it has grown, it reads the changes after the last sequence it handed on and gives a notice for each to every open stream, which **HttpInterface** sends as server-sent events. (Q9 B: "because we may have 2 workers now or in the future"; your pick on review finding R-03: A.) A new stream starts at the latest sequence. A stream that reconnects gets the changes after its last seen sequence; when that sequence is older than the oldest change still kept, the stream tells the page to refresh the list of days and the open day (your pick on review finding R-04: A). The web app records the new hash of every change it made itself and shows no toast or highlight for a notice that matches one (your pick on review finding R-05: A); while one of its own changes to a day is in flight, it holds that day's notices until the answer is in (your pick on re-check finding R-07: A). Because several processes may start against the same data file, a process that has an upgrade to run first takes the write lock; while holding it, it makes the copy of the data with SQLite's own backup interface, which stays consistent under write-ahead logging where a plain file copy would not, and then runs the upgrade in that same locked write transaction. Each process reads the schema version again once it holds the lock, so a process that started at the same time finds the upgrade already done and skips it. A failed upgrade rolls back, leaves the data as it was, keeps the copy and stops the process with a clear message. A process that finds a newer schema than it knows refuses to start (your pick on re-check finding R-08: A; the backup method settles finding R-12; the version re-check and failure handling settle finding R-13).

### Consequences
- **Positive:** works whichever process made the write; no process has to know about another; a reconnecting page catches up from its last sequence or refreshes when it fell behind the kept log (FR3.15); your own saves show no toast (FR3.16); stays well within the 2-second bound.
- **Negative:** a short polling delay (up to about half a second) and a small, steady read of the highest sequence; the change log grows and needs trimming (Functional Design).
- **Neutral:** the polling interval is confirmed in NFR Design.
- **Security:** notices carry dates and hashes only, never page content; the stream passes through RequestGuard like every other request.

### Alternatives Rejected
- **One writer with in-process notices (Q9 A):** instant and simple, but rules out a second writer process, which you expect.
- **A thin MCP relay to the backend (Q9 C):** keeps one writer but adds a forwarding process and a second protocol hop.

## ADR-008: A day-changed notice carries the date and hash only

### Context
The page highlights changed items and shows a toast (FR3.16), and follows days being added or removed (FR3.11, FR3.14).

### Decision
A notice names the date and its new hash, or that the date was removed. The web app re-reads that day and compares old and new to find what to highlight. (Q6 A.)

### Consequences
- **Positive:** nothing private travels twice; a missed notice is repaired by the next read; the notice is the same whatever changed.
- **Negative:** one extra read per change.
- **Neutral:** the stream format is fixed in Contract Design.
- **Security:** a smaller stream carries less content.

### Alternatives Rejected
- **The whole new day in the notice (Q6 B):** no second read, but the full page travels on every change and a missed notice still needs a read.

## ADR-009: The web app is split into seven blocks

### Context
Every user-facing text comes from one language file through one helper (FR9.1); Markdown is rendered safely (FR3.6, FR3.7); the page stays live (FR3.11).

### Decision
**AppShell**, **DayView**, **DayNavigation**, **LiveSync**, **MarkdownText**, **PageText** and one **ApiClient**. (Q7 A.)

### Consequences
- **Positive:** the unsafe-text boundary (MarkdownText) and the live connection (LiveSync) are each in one place; every backend call goes through one client that carries the hash.
- **Negative:** more modules than a small app strictly needs.
- **Neutral:** TanStack Query sits inside ApiClient and LiveSync; the library choice is confirmed in Functional Design.
- **Security:** MarkdownText is the only place untrusted text becomes markup.

### Alternatives Rejected
- **LiveSync and ApiClient as one Data block (Q7 B):** mixes the long-lived connection with plain calls.
- **Three blocks (Q7 C):** navigation, Markdown and text would hide inside Day view and the shell.

## Sources

- `domain-design-questions.md`: Q1 to Q9, your change request and the confirmed summary.
- `inception/requirements-analysis/requirements.md`: FR1 to FR9, NFR1 to NFR12.
- `ideation/feasibility/constraint-register.md`, `raid-log.md`: TC-03 to TC-05, R5.
- `aidlc/spaces/dayboard/memory/project.md`: Mandated rules on day data and the privacy guard.

## Assumptions & Open Questions

- For NFR Design (ADR-001, ADR-007): a longer, retrying lock wait at start-up only, so a process that starts while another runs the backup and upgrade waits instead of giving up after the everyday 300 ms wait.
- For Functional Design (ADR-007): how long the day-change log is kept before trimming.
