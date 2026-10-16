# Constraint Register — Dayboard

Sources: the approved intent statement (`ideation/intent-capture/intent-statement.md`, its fixed constraints and product boundary) and the feasibility answers (`feasibility-questions.md`, Q1–Q6).

## Technical Constraints

| ID | Constraint | Source | Impact |
|---|---|---|---|
| TC-01 | One person. No users, no user field, and no configuration of one; every record belongs to whoever runs it. | intent statement | Nothing is keyed by user; adding users later must not need a rewrite. |
| TC-02 | Runs locally: one command starts the backend, one starts the web app. | intent statement | Setup has to stay at two commands. |
| TC-03 | Must be able to move to Cloudflare or another host later without a rewrite, but is not deployed anywhere now. | intent statement | Storage and environment details sit behind one boundary. |
| TC-04 | Backend in Python; web app in TanStack Start with TanStack Query, plus TanStack Form wherever a form exists; few dependencies. | intent statement (stack proposal) | Each added dependency needs a reason. |
| TC-05 | Storage is open: plain files in the folder the service runs from (git-ignored, one file per day) or a single database file whose schema can later move to Cloudflare D1. Simplicity for the current goal decides, and the trade-off is written down. | intent statement (stack proposal) | To be decided in a later stage, before the data shape is built. |
| TC-06 | The assistant connects to the backend running on this machine. Each person runs their own copy; there is no shared space. | Q4 | Only a local connection is needed. Nothing is hosted or shared. |
| TC-07 | Block kinds are fixed and generic: schedule, open items, pull requests, learnings, waiting on others, notes for tomorrow. | intent statement | Custom kinds are out of scope but must not be designed out. |
| TC-08 | Interfaces look current: a type scale, spacing, visual hierarchy, considered states, light and dark, phone width. | intent statement | This is the visual quality bar for every screen. |

## Security and Privacy Constraints

| ID | Constraint | Source | Impact |
|---|---|---|---|
| SC-01 | Page content is sensitive. It is never committed, and nothing but this machine can read it. | Q2 | Day data is ignored by git from the first run, and the server answers only this machine. |
| SC-02 | The page is opened only on this computer. | Q3 | The server can refuse anything that doesn't come from this machine. |
| SC-03 | No sign-in in this version. Authentication is out of scope but must not be designed out. | intent statement | Privacy comes from where the server listens and whom it answers, not from sign-in. |
| SC-04 | Public repositories: nothing company-specific, no internal names, no secrets, no references to an employer's tools or processes. | intent statement | Applies to code, examples, fixtures and documentation. |

## Organizational Constraints

| ID | Constraint | Source | Impact |
|---|---|---|---|
| OC-01 | One developer, who is comfortable with both halves of the stack, decides scope alone. | Q5, intent statement | No team coordination is needed. |
| OC-02 | You'd like it tomorrow if possible, with no hard deadline. | Q1 | Every step should stay small, and approvals are the pacing item. |
| OC-03 | Starts fresh: nothing is imported from today's chat-artifact reports. | Q6 | No import or migration path is needed. |

## Regulatory Constraints

| ID | Constraint | Source | Impact |
|---|---|---|---|
| RC-01 | None identified. One person keeps their own notes on their own machine, with nothing hosted or shared. | Q2, Q3, Q4 | No compliance work is needed for this version. |
