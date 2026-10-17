# Story Map — Dayboard Units

Every story from `inception/user-stories/stories.md` assigned to the Unit that implements it, with the other Units it also needs. The implementing Unit is the one whose finish makes the story work end to end; the Units it also needs must be built first (`unit-of-work-dependency.md`). `traceability.json` mirrors the "Unit" column.

## Story Assignments

| Story | Title | Unit | Directory | Also needs | Why this Unit |
|---|---|---|---|---|---|
| US1.1 | Prepare today's page | U2 | `u2-assistant-access` | U1 | The assistant creates and writes the page through the MCP tools; U1 holds the rules and storage |
| US1.2 | Carry yesterday over | U2 | `u2-assistant-access` | U1 | The assistant reads notes for tomorrow and delayed items through the MCP tools; CarryOver is in U1 |
| US1.3 | Copy the prompt | U3 | `u3-day-page` | U1 | The Copy prompt button and the first-run view are in the app shell; the first-run view lists days through U1 |
| US2.1 | Read the day | U3 | `u3-day-page` | U1 | The day view renders the page read from U1 |
| US2.2 | See changes without reloading | U5 | `u5-live-updates` | U1, U2, U3 | Live updates and Live sync are built in U5 |
| US3.1 | Track item states on the page | U3 | `u3-day-page` | U1 | The state switches and removals are in the day view; U1 checks and stores them |
| US3.2 | Tick checkboxes in text | U3 | `u3-day-page` | U1 | Markdown text turns ticks into text changes the day view saves through U1 |
| US3.3 | Keep the page current as the assistant | U2 | `u2-assistant-access` | U1 | The assistant changes, reorders and removes through the MCP tools; that the open page then follows is US2.2 (U5) |
| US4.1 | Leave notes and curate learnings | U3 | `u3-day-page` | U1 | Notes and learnings on the page; CarryOver and the state rules are in U1 |
| US4.2 | Take saved learnings | U2 | `u2-assistant-access` | U1 | The assistant reads saved learnings and marks them through the MCP tools |
| US5.1 | Open an earlier day | U4 | `u4-day-navigation` | U1, U3 | The arrows and calendar are in U4 |

## Cross-Cutting Rows

| Concern | Units | How |
|---|---|---|
| Day rules, page hash and storage | U1, used by every other Unit | Every story's writes and reads pass through U1's DayPages and Storage |
| Privacy guard (FR8.3 to FR8.6, NFR1) | U1, plus U2 and U6 for their own listeners | U1 builds the guard and its tests; an HTTP MCP process (U2) installs it; U6's service examples keep it on |
| Installation (FR8.1, team practices, Deployment) | U6 | No story of its own: U6 makes every story usable on a fresh computer |
| Scripted end-to-end check | U5 | Exercises US1.1, US2.1, US2.2 and US3.3 across the MCP server and the web server |

## Order Inside Each Unit

Test-after, one layer at a time (team practices, Testing Posture):

- **U1:** storage and the day rules with their unit tests; then CarryOver; then the request guard with its tests; then the HTTP layer with its contract tests.
- **U2:** the MCP tools over U1, with a contract test per tool; then the tool descriptions.
- **U3:** page text and the API client; then the app shell and its views; then the day view, Markdown text, and their component tests in light and dark.
- **U4:** the arrows; then the calendar and its keyboard behaviour, with component tests.
- **U5:** the backend's live updates and stream with their tests; then Live sync, the toast and highlight with component tests; then the scripted end-to-end check.
- **U6:** the installation instructions; then the launchd and systemd examples; then the assistant connection guide, each checked by following it on a clean setup.

## Coverage Check

- **Every story assigned:** 11 of 11 (US1.1 to US5.1), each to exactly one implementing Unit.
- **Every Unit has rows:** U1 is in every story's "Also needs" and in the cross-cutting rows; U2 has US1.1, US1.2, US3.3 and US4.2; U3 has US1.3, US2.1, US3.1, US3.2 and US4.1; U4 has US5.1; U5 has US2.2; U6 has the installation row, as it carries no story of its own.

## Sources

- `inception/user-stories/stories.md`: US1.1 to US5.1.
- `unit-of-work.md`, `unit-of-work-dependency.md`: the Units and their edges.
- `inception/domain-design/traceability.json`: the components behind each story.

## Assumptions & Open Questions

None.
