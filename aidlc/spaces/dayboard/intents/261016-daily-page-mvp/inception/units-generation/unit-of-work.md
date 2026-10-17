# Units of Work — Dayboard, first version

The 14 components from Domain Design (`inception/domain-design/components.md`) grouped into six Units, cut by layer (`units-generation-questions.md`, Q1, Q2, Q6, Q7, and your request for an installation Unit). One developer claims a Unit and builds it end to end, in `dayboard-api` and `dayboard-web` wherever it needs (team practices, Way of Working). This file says what each Unit holds; `unit-of-work-dependency.md` says what depends on what; Delivery Planning decides the order. Story points are in `unit-points.csv` in the intent's folder.

## Unit Index

| Unit ID | Directory | Name | Kind | Repositories | Complexity | Story points |
|---|---|---|---|---|---|---|
| U1 | `u1-api-core` | API core and HTTP | `service` | `dayboard-api` | L | 13 |
| U2 | `u2-assistant-access` | Assistant access | `service` | `dayboard-api` | M | 5 |
| U3 | `u3-day-page` | The day page | `ui` | `dayboard-web` | L | 13 |
| U4 | `u4-day-navigation` | Moving between days | `ui` | `dayboard-web` | S | 3 |
| U5 | `u5-live-updates` | Live updates and end-to-end check | — (spans both) | both | M | 8 |
| U6 | `u6-installation` | Installation | `packaging` | both | S | 3 |

Total: 45 story points.

## U1 · API core and HTTP (`u1-api-core`)

**Components:** Storage, DayPages, CarryOver, RequestGuard, HttpInterface.

**Owns and delivers**
- The SQLite data file in the git-ignored data folder, the storage interface, write-ahead logging, the bounded lock wait, the day-change log, the schema version, the start-up copy and upgrade (ADR-001, ADR-007).
- Every day rule: dates and "no future pages", block kinds, item states, note and learning states, schedule times, identifiers, limits, the page hash and the refusal of stale changes (ADR-003, ADR-004).
- The cross-day reads and adding notes and learnings (CarryOver).
- The privacy guard for every listening process (ADR-006), with its own tests: another machine, a foreign origin and a foreign host are refused, and the data folder is ignored by git (NFR1).
- The HTTP interface the web app uses: list days, read a day, change a day, add notes and learnings, remove items and blocks.
- The command that starts the backend (FR8.1) and a first run with no data (FR8.2).
- Two registration points in the backend process: one where an MCP interface registers behind RequestGuard (used by U2), and one where a day-changed stream registers on the HTTP interface (used by U5). U1 owns both and the start command that runs them (`unit-of-work-dependency.md`, Mounting at Start-Up).

**Deployment:** standalone, the backend process started by one command on this computer.

**Notes and constraints**
- Holds the privacy guard and keeps day data out of git before the first day is written, so whichever Unit is built first after it inherits both (team practices, Walking Skeleton; Mandated rules).
- Test-after per layer: storage and day rules first, then the HTTP layer (team practices, Testing Posture). Contract tests cover every HTTP endpoint.
- The HTTP contract is written down by Contract Design before this Unit starts and lives here (Q3).
- Complete and testable without U2 and U5: with nothing registered, the backend serves the HTTP interface alone.

## U2 · Assistant access (`u2-assistant-access`)

**Components:** McpInterface.

**Owns and delivers**
- The MCP server and every assistant tool: list days, read a page, create, change, reorder and remove days, blocks and items, set states and meeting answers, read notes for tomorrow by dates or the latest day, read saved learnings across dates (FR2.1 to FR2.11).
- The tool descriptions that teach the assistant the text rules, headings at level 3 or smaller (FR2.12).
- Running inside the backend behind its guard, as a separate HTTP process with its own guard, or over stdio (ADR-006, ADR-007).

**Deployment:** embedded in the backend process; may also run as its own process.

**Notes and constraints**
- Calls DayPages and CarryOver from U1; holds no rules of its own.
- Registers McpInterface into U1's MCP registration point, so the backend's one start command serves it; a separate MCP process uses the same registration behind its own RequestGuard.
- Contract tests call every MCP tool (team practices, Testing Posture). The MCP contract lives here (Q3).

## U3 · The day page (`u3-day-page`)

**Components:** AppShell, DayView, MarkdownText, PageText, ApiClient.

**Owns and delivers**
- The web app's frame: header, theme switch, Copy prompt, routes for the home address and each day, the first-run, offline and day-failed views (FR3.1 to FR3.3, FR6, FR7).
- The day itself: blocks in stored order, schedule order, the Past mark, the done fold, state switches and meeting answers, notes and learnings, adds, removals and confirmations (FR3.4 to FR3.10, FR3.12, FR4).
- Safe Markdown rendering and checkbox ticks (FR3.6 to FR3.9, FR3.12).
- The language file and its single helper (FR9).
- The API client, which carries the page hash and records this tab's own change hashes (ADR-007).
- The command that starts the web app (FR8.1), and the liquid-glass look from the Refined Mockups canvas.

**Deployment:** standalone, the web app started by one command on this computer.

**Notes and constraints**
- Component tests for every state of the page, light and dark (team practices, Testing Posture).
- The own-change hash record is built here even though Live sync (U5) is its reader.
- Exposes a slot in the header for day navigation (used by U4) and a hook where a live channel attaches to the app shell and reports changed items to the day view (used by U5). Complete and testable without U4 and U5: the page shows the open day with no arrows or calendar, and updates on reload.

## U4 · Moving between days (`u4-day-navigation`)

**Components:** DayNavigation.

**Owns and delivers**
- The previous and next arrows, the date button and the month calendar, by mouse, touch and keyboard, and as a full-width panel at phone width (FR5.1 to FR5.4).

**Deployment:** part of the web app.

**Notes and constraints**
- Fills U3's header slot and uses U3's API client to list days.

## U5 · Live updates and end-to-end check (`u5-live-updates`)

**Components:** LiveUpdates (backend), LiveSync (web app).

**Owns and delivers**
- In the backend: polling the change log, turning day changes into notices, the server-sent events stream, catch-up after a reconnect and the refresh after a long gap (ADR-007, ADR-008).
- In the web app: the live connection, reconnecting, re-reading and diffing a changed day, the highlight, the "This day was updated" toast, telling this tab's own changes apart, and switching to "No page for this day" when the open day is removed (FR3.11, FR3.13 to FR3.16).
- The scripted end-to-end check that talks to the MCP server and the web server, used as the smoke check after every merge (team practices, Testing Posture and Deployment).

**Deployment:** spans the backend process and the web app.

**Notes and constraints**
- Two pull requests, `dayboard-api` first (team practices, Way of Working). The stream contract lives here (Q3).
- Registers the day-changed stream into U1's stream registration point, and attaches Live sync to U3's live-channel hook.
- No single kind: it carries a backend service part and a web part, so it keeps the full construction design matrix.

## U6 · Installation (`u6-installation`)

**Components:** none; it packages U1, U2 and U3, and documents U5's end-to-end check.

**Owns and delivers**
- Installation instructions for both repositories: what to install, the commands to install and start the backend and the web app, and how to stop and update them (team practices, Deployment).
- A launchd service example for macOS and a systemd service file example for Linux, so the backend and the web app can start on their own.
- How to connect an assistant to the MCP server, over HTTP to the running backend or as a stdio process (ADR-006).
- How to pick up a new version: pull `main`, restart, run the end-to-end check; and how to roll back (team practices, Deployment).

**Deployment:** documentation and example files in both repositories.

**Notes and constraints**
- Examples use made-up paths and names only (Mandated rules). Nothing is company-specific.
- The service examples bind to this computer only and never weaken the privacy guard.

## Sources

- `inception/domain-design/components.md` and `decisions.md`: the 14 components and ADR-001 to ADR-009.
- `inception/requirements-analysis/requirements.md`: FR1 to FR9, NFR1 to NFR12.
- `inception/user-stories/stories.md`: US1.1 to US5.1 (see `unit-of-work-story-map.md`).
- `inception/practices-discovery/team-practices.md` and `aidlc/spaces/dayboard/memory/team.md`: Way of Working, Walking Skeleton, Testing Posture, Deployment.
- `units-generation-questions.md`: Q1 to Q7, your request for an installation Unit and story points, and the confirmed summary.

## Assumptions & Open Questions

- [assumption] The story points are AI estimates on a 1, 2, 3, 5, 8, 13 scale relative to each other; change any row in `unit-points.csv` and the panel follows.
