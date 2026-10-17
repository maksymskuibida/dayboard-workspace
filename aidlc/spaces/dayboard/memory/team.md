# Team practices

## Way of Working

- We are two developers, each working in their own clone of the workspace, with `dayboard-api` and `dayboard-web` checked out inside it. (Q11, Q12)
- There are no fixed assignments. Whoever is free first claims the next available Unit with `aidlc unit claim` and builds it end to end, in `dayboard-api` and `dayboard-web` wherever the Unit needs it. (Q12)
- Every change in all three repositories reaches `main` through a GitHub pull request that is squash-merged on GitHub, so each pull request becomes one commit on `main`. `main` is the only long-lived branch in each repository. (Q1)
- Branches are short-lived and named `<kind>/<topic>`, for example `setup/aidlc-workspace-shell`. Each branch lives for one piece of work and is deleted after its merge. (Q1)
- Every Unit lands through pull requests (merge strategy: `pr`). The workflow opens one pull request for each code repository the Unit touches. It never merges into `main` itself and never waits for a merge. (Q1, Q11, confirmed summary)
- When a Unit touches both code repositories, the `dayboard-api` pull request merges first, because the web app consumes the API. (Q1)
- The project owner merges every pull request personally, outside the workflow, and then marks the Unit finished with `aidlc unit finish <unit> [--pr <owner>/<repo>#<n>]`. No pull request approval is required before a Unit is finished. (Q11)
- Ideation and Inception records land in `dayboard-workspace` as one pull request at each natural pause: the end of a phase, or whenever the work is stopped to continue later. (Q2, and the person's clarification of it)
- During Construction, each Unit's records land with that Unit. `aidlc unit pr <unit>` opens the Unit's workspace pull request alongside its code pull requests. The project owner merges it before running `aidlc unit finish <unit>`. (Q2 clarification, Q11)
- A pull request description says what the change is, what is in it and how to check it.

- At every reviewer-backed approval, the findings are shown as a bullet list, each with its ID, severity and one plain sentence, never as a table. Findings already resolved or accepted come first in their own list, each starting with 'resolved' or 'accepted'; the open ones follow in a second list. After the list, the person is asked whether to put the open findings into a question form or continue; with a form, the approval question waits until every finding in it is answered. (learned 2026-10-17) <!-- cid:261016-daily-page-mvp:delivery-planning:282cee5163cf1c5d24151ec60b06900385a541aca348c2e1c53edcf596bae80f -->

- Every revision made after review findings gets a fresh check by the reviewer before the approval question, every time, so nothing is approved that nobody re-checked. (learned 2026-10-17) <!-- cid:261016-daily-page-mvp:delivery-planning:85bfa6e01b3b98bb1cfc880011ff0a1e4d609ee902a22c67f0934f8a5b982630 -->

- One Bolt for planning doesn't merge Units: each Unit still lands through its own pull requests and is finished on its own. (learned 2026-10-17) <!-- cid:261016-daily-page-mvp:delivery-planning:0eb3d9c0ae03318ece04fcbe8b6c2b5e1b6528094949882b409fc0f07328f221 -->

## Walking Skeleton

- We never run a walking-skeleton ceremony for Dayboard. There is no thin end-to-end slice built first and no skeleton checkpoint. (Q3)
- Backlog items are built fully, one after another, in the order Delivery Planning sets. (Q3)
- The privacy guard is still part of the first Unit built: the server answers only this machine, its own web page and the assistant, and day data is ignored by git before the first day is written. (`aidlc/spaces/dayboard/memory/project.md`, Corrections; Q3)

## Testing Posture

- **Methodology**: test-after
- **Methodology evidence**: Chosen in the interview (Q4, answer A): code is written first, then its tests, one layer at a time.
- **Ordering**: Implement each layer of a Unit in turn (storage and day rules, then the MCP and HTTP layer, then the web components), and write and run that layer's tests before starting the next layer.
- Coverage: at least 80% line coverage in both `dayboard-api` and `dayboard-web`, measured on every pull request. The floor is never lowered to make a build pass. (Q5)
- Tooling: `pytest` with `pytest-cov` in `dayboard-api`; Vitest with Testing Library and V8 coverage in `dayboard-web`. (Q4, Q5, Q9)
- Test types: unit tests for the day rules (item states, latest day on record, notes for tomorrow by dates); contract tests that call every MCP tool and HTTP endpoint the assistant and the page use; component tests for the page's states, including light and dark; and one scripted end-to-end check that talks to the MCP server and the web server directly. (Q6)
- No browser test tool for now: the scripted end-to-end check plus the component tests are enough. (Q6)
- The privacy guard has its own tests from the first Unit: a request from another web origin is refused, a request from another machine is refused, and the day data location is ignored by git. (`ideation/feasibility/raid-log.md` R1, R2)
- Test fixtures and examples contain only made-up content. (`ideation/feasibility/raid-log.md` R2; `ideation/feasibility/constraint-register.md` SC-04)
- CI runs on every pull request in both code repositories. A failing test or a coverage drop below the floor blocks the merge. (Q5)

- A verification command that skips a repository not set up yet uses `if … then … fi`, never a `|| not set up` fallback, so a failing check can never count as a pass. (learned 2026-10-17) <!-- cid:261016-daily-page-mvp:delivery-planning:2608668a9b305f272f15c73218b43e6396c555ede5701b0969e29583d4164144 -->

## Deployment

- Nothing is deployed. Dayboard runs only on the developer's own machine: one command starts the backend and one starts the web app. There are no staging or production environments. (Q7; `ideation/feasibility/constraint-register.md` TC-02, TC-06)
- A change merged into `main` counts as released. To pick it up, pull `main`, restart the two commands and run the scripted end-to-end check as the smoke check. (Q7)
- Rolling back means checking out the previous commit and restarting. Day data is never touched by a rollback. (Q7)
- A change to how day data is stored first keeps a copy of the data as it was, then moves existing days forward. (Q7)
- Semantic version tags and GitHub releases start from the first working version. (Q7)
- Hosting on Cloudflare or anywhere else is a later piece of work. Until it is decided, no questions are asked about cloud accounts, services or budget. (`aidlc/spaces/dayboard/memory/project.md`, Corrections)

## Code Style

- Python (`dayboard-api`): Ruff for linting and formatting, mypy in strict mode for types, and uv to manage the environment and dependencies, all configured in `pyproject.toml`. (Q8)
- TypeScript (`dayboard-web`): Biome as the single tool for formatting and linting, pnpm for dependencies, and TypeScript in strict mode. (Q9)
- Naming follows each language's idiom: `snake_case` in Python, `camelCase` for TypeScript values, and `PascalCase` for components and types. There are no project-wide renaming rules. (`org.md` Code Style)
- Lint, format and type checks run in CI on every pull request, and a failure blocks the merge. Before suggesting a style change, read the repository's own Ruff, mypy or Biome configuration. (Q8, Q9; `org.md` Code Style)
- Dependencies stay few: every new runtime dependency has a one-line reason in its pull request. ([desc]; `ideation/feasibility/constraint-register.md` TC-04)
- User-facing text in `dayboard-web` (labels, buttons, empty states, errors) lives in one file per language from the start, English first, and is read through a single helper. Components never contain copy. (Q10)

