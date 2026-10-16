# RAID Log — Dayboard

Sources: the approved intent statement (`ideation/intent-capture/intent-statement.md`), the feasibility answers (`feasibility-questions.md`, Q1–Q6), and the constraint register (`constraint-register.md`).

## Risks

| ID | Risk | Likelihood | Impact | Mitigation | Carried to |
|---|---|---|---|---|---|
| R1 | A web page open in your browser sends requests to the server on this machine, which has no sign-in, and reads or changes your pages. | Medium | High: breaks SC-01 | The server listens only on this machine and answers only its own web app and your assistant. Requests from other web origins are refused. | Requirements, NFR Requirements |
| R2 | Day data is committed to a public repository by mistake. | Medium | High: breaks SC-01 | The data location is ignored by git before the first day is written, and examples and fixtures contain only made-up content. | Requirements, Code Generation |
| R3 | The assistant writes content the page can't handle: very long text, markup, or links that aren't links. | Medium | Medium | The server validates what the assistant writes, and the page shows text safely as text. | Requirements |
| R4 | The assistant and you change the same item at the same moment, and one change is silently lost. | Low | Medium | One stated rule decides; for example, the later change wins and the page refreshes to show it. | Requirements |
| R5 | The storage choice makes a later move to another host a rewrite. | Low | Medium | Storage stays behind one boundary, and the trade-off is written down when the choice is made (TC-05). | Domain Design |
| R6 | The number of approval steps in this plan makes "tomorrow" slip. | High | Low: there is no hard deadline (Q1) | Keep each step small, and lighten the remaining plan if you want to. | Scope Definition |

## Assumptions

| ID | Assumption | Basis | Validate by |
|---|---|---|---|
| A1 | "Today" follows this computer's clock and time zone, and the page opens on the latest day on record that is today or later. | One person on one machine (Q3, Q4) | Requirements Analysis |
| A2 | The assistant you use daily can connect to a server running on this machine. | Q4 | The first end-to-end run |
| A3 | This computer has the language runtimes the stack needs (Python, and Node for the web app). | Q5 | The first local run |

## Issues

| ID | Issue | Status |
|---|---|---|
| — | None open. | — |

## Dependencies

| ID | Dependency | Type | Notes |
|---|---|---|---|
| D1 | An AI assistant on this machine that can connect to a local server | External, yours | The first version is proven with the one you use daily (intent statement). |
| D2 | The two code repositories, dayboard-api and dayboard-web, checked out next to this workspace | Internal | The code lives there; this workspace keeps the records. |
| D3 | The storage decision (TC-05) | Decision | Needed before the data shape is built. |
