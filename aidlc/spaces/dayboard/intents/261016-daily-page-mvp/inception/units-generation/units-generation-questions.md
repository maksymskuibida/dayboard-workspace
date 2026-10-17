# Units Generation Questions — Dayboard

Context: this stage groups the 14 components from Domain Design (`inception/domain-design/components.md`) into Units: pieces of work one developer claims and builds end to end, in `dayboard-api` and `dayboard-web` wherever the Unit needs it (team practices, Way of Working). It only says what depends on what; Delivery Planning decides the order. The team builds each Unit fully (no walking skeleton), and the privacy guard must be part of the first Unit built (team practices, Walking Skeleton).

## Q1. How are the Units cut?

Two developers each claim the next free Unit and build it end to end (team practices). The cut decides how often a Unit spans both repositories and how much one Unit waits on another.

**Proposed:** C, because the day rules and storage are what every feature needs, and after that each feature can be built and shown on its own.

A. **By feature, end to end:** each Unit is one capability across both repositories (for example "read the day", "change states", "notes and learnings", "live updates", "navigation"), and each brings the backend parts it needs
B. **By layer:** a backend core Unit, an MCP Unit, an HTTP Unit, and web app Units, each in one repository
C. **Foundation, then features:** one foundation Unit (Storage, Day pages and the Request guard, with the day rules and their tests), then feature Units that each span both repositories
X. Other (please specify)

[Answer]: B

## Q2. How many Units?

There are 11 stories and 14 components. Fewer Units mean less coordination; more Units mean the two of you can work in parallel more often and each pull request stays small.

**Proposed:** B.

A. **Coarse:** 3 to 4 Units
B. **Medium:** 5 to 7 Units
C. **Fine:** 8 or more Units, roughly one per story
X. Other (please specify)

[Answer]: X — 4 to 5. First must be api core and http layer, then web units and mcp in parallel, one or few finalzying, if needed

## Q3. Where do the contracts between backend and web app live?

Contract Design (next stage) fixes the HTTP endpoints, the MCP tools and the live-update stream. Both repositories build against them.

**Proposed:** B, because the contracts change with the features that use them and a separate Unit would hold nothing runnable.

A. A separate **contracts** Unit (kind `spec`) that every feature Unit depends on
B. Each contract lives in the Unit that first builds it; Contract Design still writes them all down before any Unit starts
X. Other (please specify)

[Answer]: B

## Q4. Do the assistant's tools get their own Unit?

The MCP interface is how the assistant writes the page (US1.1, US1.2, US3.3, US4.2). It uses the same Day pages and Carry-over as the web app.

**Proposed:** A, because the assistant's side can be built and tested without the web app and is the first thing that puts real days into Dayboard.

A. Yes: one **assistant access** Unit (the MCP interface and its tools, including notes for tomorrow and saved learnings)
B. No: each feature Unit adds both its MCP tools and its web screens
X. Other (please specify)

[Answer]: A

## Q5. Can independent Units be built at the same time?

Two developers work in parallel. Units with no dependency between them could be built side by side; this stage only marks which ones are independent, and Delivery Planning decides the order.

**Proposed:** A.

A. Yes: mark every pair of Units with no dependency as buildable in parallel
B. No: treat the Units as one chain, built one after another
X. Other (please specify)

[Answer]: A

## Q6. Follow-up: what goes into the finalizing Unit?

You chose Units by layer, 4 to 5 of them: first the API core and HTTP layer, then the web Units and the MCP Unit in parallel, then "one or few finalizing, if needed" (Q1, Q2). Live updates span both sides (Live updates and the server-sent events stream in the backend, Live sync in the web app), and the team's scripted end-to-end check needs every part in place.

**Proposed:** A, because live updates need the backend and the web app together, and the end-to-end check is the smoke check after every merge (team practices).

A. One finalizing Unit: **live updates on both sides** (Live updates, the stream, Live sync with the toast and highlight) plus the **scripted end-to-end check** that talks to the MCP server and the web server
B. One finalizing Unit with only the **scripted end-to-end check**; the backend's Live updates goes into the API core Unit and Live sync into a web Unit
C. No finalizing Unit: live updates are split as in B, and the end-to-end check is written by whichever Unit finishes last
X. Other (please specify)

[Answer]: A

## Q7. Follow-up: how many web Units?

The web app has seven components (App shell, Day view, Day navigation, Live sync, Markdown text, Page text, API client). With 4 to 5 Units in total (Q2), there is room for one or two web Units beside the API core, the MCP Unit and a finalizing Unit.

**Proposed:** A if live updates go to the finalizing Unit (Q6 A): 4 Units in total.

A. **One web Unit:** the whole page (shell, day view, navigation, Markdown, page text, API client)
B. **Two web Units:** "the day" (shell, day view, Markdown, page text, API client) and "moving between days" (navigation and calendar), buildable in parallel once the first exists
X. Other (please specify)

[Answer]: B

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Cut and count (Q1, Q2, Q6, Q7, and your request after the first confirmation):** Units by layer, 6 in total:
  - **U1 API core and HTTP** (`dayboard-api`, kind `service`): Storage, Day pages, Carry-over, Request guard and the HTTP interface, with the day rules and the privacy-guard tests.
  - **U2 Assistant access** (`dayboard-api`, kind `service`): the MCP interface and its tools, including notes for tomorrow and saved learnings (Q4).
  - **U3 The day page** (`dayboard-web`, kind `ui`): App shell, Day view, Markdown text, Page text and the API client.
  - **U4 Moving between days** (`dayboard-web`, kind `ui`): Day navigation, the arrows and the calendar.
  - **U5 Live updates and end-to-end check** (both repositories, no single kind): Live updates and the stream in the backend, Live sync with the toast and highlight in the web app, and the scripted end-to-end check against the MCP server and the web server.
  - **U6 Installation** (both repositories, kind `packaging`): installation instructions and the commands to install and start the backend and the web app, a launchd service example for macOS and a systemd service file example for Linux, and how to connect an assistant to the MCP server.
- **Dependencies (Q2, Q5):** U2 and U3 depend on U1 and can be built in parallel; U4 depends on U3; U5 depends on U1, U2 and U3; U6 depends on U1, U2 and U3, the parts it installs and starts, and can be built alongside U4 and U5. Units with no dependency between them are marked as buildable in parallel; Delivery Planning sets the order.
- **Contracts (Q3):** each contract lives in the Unit that first builds it; Contract Design writes them all down before any Unit starts.
- **Story points (your request, for the project panel's build progress; set by me, as there is no Jira):** U1 13, U2 5, U3 13, U4 3, U5 8, U6 3, 45 in total, written to `unit-points.csv` in the intent's folder with source `ai`. Complexity in `unit-of-work.md` matches: U1 L, U2 M, U3 L, U4 S, U5 M, U6 S.
- **Plan approval (stage Step 4):** "Looks correct" also approves this decomposition plan, including each Unit's kind.

- Looks correct
- Request changes

[Answer]: Looks correct

## Requested Changes Feedback

What should change?

[Answer]: We beed one more unit. Someone needs to build installation inctruction and command  launchd service and systemd service file example, etc. So, it's described how to install
