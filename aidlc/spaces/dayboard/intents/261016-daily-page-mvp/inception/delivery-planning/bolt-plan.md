# Bolt Plan — Dayboard, first version

A Bolt is one build pass over one or more Units of Work, ending in something that runs and can be shown. Dayboard is a small project, so it is built as **one Bolt holding all six Units** (`delivery-planning-questions.md`, Q2). Inside it, each Unit still lands through its own pull requests, one per repository it touches, and is finished on its own (team practices, Way of Working). There is no walking skeleton — no thin end-to-end slice built first — because your team builds each Unit fully (team practices, Walking Skeleton).

## Bolt 1 · Dayboard, first version

**Units:** U1 API core and HTTP, U2 Assistant access, U3 The day page, U4 Moving between days, U5 Live updates and end-to-end check, U6 Installation (`inception/units-generation/unit-of-work.md`). 45 story points (`unit-points.csv`).

**Walking skeleton:** no.

### Order inside the Bolt

The order follows the dependency graph (`unit-of-work-dependency.md`) and keeps both developers busy (Q1):

| Step | Units | Who | Why here |
|---|---|---|---|
| 1 | U1 | One developer | Everything depends on it, and it holds the privacy guard and the git-ignored data folder, which must exist before the first day is written |
| 2 | U2 and U3, side by side | One developer each | Both need only U1; real days written by the assistant (U2) and a page to read them on (U3) make Dayboard useful |
| 3 | U5 and U4, side by side | One developer each | U5 needs U1, U2 and U3; U4 needs U3; neither needs the other |
| 4 | U6 | Whoever is free | Needs U1, U2, U3 and U5, so its guide covers starting, connecting the assistant and the end-to-end check |

A developer may also run two Units at once in separate worktrees, without keeping them in step (Q3), as long as each Unit's prerequisites are merged.

### Inside each Unit

Construction runs **Unit by Unit**: each Unit goes through its design stages, Code Generation and Build and Test before it is finished (Q3). Test-after, one layer at a time (team practices, Testing Posture), in the order listed in `unit-of-work-story-map.md`. In U5, the change log, the stream and own-change matching are built and tested first, before the toast and the highlight, because live updates across processes are the part you worry about most (Q5).

### Definition of Done

**For each Unit**
- Its pull requests are merged into `main` (`dayboard-api` first when it touches both) by the project owner, and the Unit is marked finished with `aidlc unit finish`.
- Its tests pass in CI, with at least 80% line coverage in each repository it touches; lint, format and type checks pass (team practices, Testing Posture and Code Style).
- Its contracts (`inception/contract-design/contract-summary.md`) have contract tests in the providing Unit.
- No secret, no company name and no real page content in any file; examples use made-up content (Forbidden and Mandated rules).

**For the Bolt**
- All six Units are finished.
- The scripted end-to-end check (U5) passes against a running backend and web app, and is the smoke check after pulling `main` (team practices, Deployment).
- Following the installation guide (U6) on a clean setup starts both parts and connects an assistant.
- The first version is tagged (team practices, Deployment).

### Confidence hypothesis

Shipping this Bolt shows that one person and their assistant can run a day on Dayboard instead of a chat-artifact report: the assistant writes today's page through the MCP server, the page shows it within 2 seconds, and the person reads and acts on it all day (intent statement, Success Metrics). The two-week daily-use test then decides whether it replaces the old report.

### Expected demo

On a fresh setup, following U6's guide: start the backend and the web app; connect the assistant; ask it to prepare today's page with the copy prompt; watch the page update without a reload; tick an item, add a note for tomorrow, move to yesterday's page; run the end-to-end check.

## Construction settings

- **Iteration:** Unit by Unit (`unit-major`), as recorded for this piece of work; parallel work happens through separate worktrees and claims, not inside one session.
- **Execution:** serial within a session.
- **Verification command:** asked separately at the end of this stage.

## Sources

- `delivery-planning-questions.md`: Q1 to Q5 and the confirmed summary.
- `inception/units-generation/unit-of-work.md`, `unit-of-work-dependency.md`, `unit-of-work-story-map.md`, `unit-points.csv`: the Units, their graph, stories and points.
- `inception/contract-design/contract-summary.md`: the contracts each Unit builds and tests.
- `aidlc/spaces/dayboard/memory/team.md` and `inception/practices-discovery/team-practices.md`: Way of Working, Walking Skeleton, Testing Posture, Deployment.
- `inception/requirements-analysis/requirements.md`: the done criteria (FR3.11 two-second live updates, FR8 running and privacy, NFR9 coverage) and the success test.
- `inception/refined-mockups/mockups.md`: the screens the expected demo walks through.
- `ideation/intent-capture/intent-statement.md`: the success test.

## Assumptions & Open Questions

None.
