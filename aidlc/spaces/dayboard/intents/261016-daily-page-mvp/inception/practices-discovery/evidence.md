# Evidence — Practices Discovery

Greenfield run with the lead agent only: collaborators are off for the `mvp` plan, so no support review ran and there are no contribution files. Reverse engineering was skipped because the project is greenfield, so none of its artifacts (code-structure, technology-stack, dependencies, code-quality-assessment, architecture, business-overview) exist to consume. The practices come from the `org.md` defaults, adapted to this project in the lead draft, and settled by the interview in `practices-discovery-questions.md`.

## What the lead inspected

| Source | What it showed |
|---|---|
| `aidlc/spaces/dayboard/memory/org.md` | The five default sections (Way of Working, Walking Skeleton, Testing Posture, Deployment, Code Style), used as the starting suggestions. |
| `aidlc/spaces/dayboard/memory/team.md` | Holds only its title: no practices had been affirmed, so this is a first run, not a re-run. |
| `aidlc/spaces/dayboard/memory/project.md` | Two learned rules: no cloud questions in this phase; privacy guard in the first slice. |
| `aidlc-state.md` | Greenfield, scope `mvp`, Test Strategy Standard, Collaborators off, Construction Checkpoints enabled, unit-major, serial. Operation stages are skipped; CI Pipeline (3.7) runs. |
| `.claude/scopes/aidlc-mvp.md` | `skeleton: on` by default for this scope; the interview switched it off (Q3). |
| `ideation/scope-definition/scope-document.md`, `ideation/scope-definition/intent-backlog.md` | Backlog IB-01 to IB-09 with the privacy guard in the first slice; two-command local run. |
| `ideation/feasibility/constraint-register.md`, `ideation/feasibility/feasibility-assessment.md`, `ideation/feasibility/raid-log.md` | TC-01 to TC-08, SC-01 to SC-04, R1 (other web pages reaching the local server), R2 (day data committed by mistake). |
| `.gitignore`, `README.md` (workspace) | `dayboard-api/` and `dayboard-web/` are ignored by the workspace and checked out inside it; rules land through pull requests. |
| `dayboard-api/README.md`, `dayboard-web/README.md` | One-line descriptions only; no code, no build files, no CI, no linter or formatter configuration, no `.gitignore`, no licence file. |
| `.claude/tools/aidlc-unit.ts` (installed AI-DLC tools, workspace commit `7b1a420`) | `aidlc unit claim`, `aidlc unit land --strategy merge\|pr`, `aidlc unit pr` and `aidlc unit finish <unit> [--pr <owner>/<repo>#<n>]` are available. |

## Git commands and findings

| Command | Finding |
|---|---|
| `git -C . remote -v` | `origin` is `github.com/<owner>/dayboard-workspace` (public GitHub). |
| `git -C dayboard-api remote -v`, `git -C dayboard-web remote -v` | Same GitHub owner: `dayboard-api`, `dayboard-web`. |
| `git -C . branch -a` and the same in both code repositories | Each repository has only `main` locally and `origin/main` remotely. |
| `git -C . log -5 --format='%h parents=%p \| %s%n%b'` | Pull requests #1 to #3 each landed as a single-parent commit whose title ends in the pull request number and whose body is the pull request description: a GitHub squash merge. #1 names its source branch `setup/aidlc-workspace-shell`. |
| `git -C dayboard-api log -5`, `git -C dayboard-web log -5` | One "Initial commit" each. |
| `git -C dayboard-api status --short --ignored`, `git -C dayboard-api ls-files`, `git -C dayboard-web ls-files` | Only `README.md` is tracked; an untracked `.DS_Store` sits in `dayboard-api`. |
| `git -C . log -3 --format='%H %s'` | Workspace `HEAD` is now `7b1a42006c8cc1a8746aa62f0f329ecfa82a58f9` (#4), used for the timestamp. |
| `gh auth status` | The GitHub CLI is signed in with `repo` scope, which `aidlc unit pr` and `aidlc unit finish` use to open and check pull requests. |

## What the lead inferred in the draft

- Pull requests squash-merged into `main` were the existing habit in the workspace repository; the interview confirmed it for all three repositories (Q1).
- Branch names follow `<kind>/<topic>`, from the one branch name on record; confirmed with Q1.
- With no deployment target, "released" can only mean "merged to `main`"; confirmed with Q7.
- Rules that hold for this version only (one person with no user field, no sign-in, running only on this machine) were kept out of `discovered-rules.md` so they do not bind later work. The privacy rule is worded "for as long as Dayboard has no sign-in" for the same reason.

## Interview decisions

The person answered Q1–Q12 and confirmed the Consolidated Summary Confirmation with "Looks correct".

| Question | Decision |
|---|---|
| Q1 How changes reach `main` | A: a pull request for every change in all three repositories, squash-merged into `main`, short-lived `<kind>/<topic>` branches; the `dayboard-api` pull request merges first when a Unit touches both code repositories. |
| Q2 When records are committed | A: one pull request at each natural pause, the end of a phase or when the work is stopped. Clarified by the person while the approval was open, in their words: "i meant ideation and inception. unit records land as the engine says, per unit". So Ideation and Inception records land at natural pauses. During Construction, each Unit's records land in its own workspace pull request, opened by `aidlc unit pr <unit>` and merged before `aidlc unit finish <unit>`. |
| Q3 Walking skeleton | D: "D — no skeleton, switch it off". Backlog items are built fully one after another; the privacy guard is still in the first Unit (project rule). |
| Q4 When tests are written | A: after the code, layer by layer (storage and day rules, then MCP and HTTP, then web components), each layer's tests run before the next layer. |
| Q5 Coverage | A: at least 80% line coverage in both `dayboard-api` and `dayboard-web`. |
| Q6 Browser tests | A: no browser tool for now; a scripted end-to-end check plus component tests. |
| Q7 Release and rollback | B: merged to `main` is released; pull, restart, run the smoke check; roll back to the previous commit with day data untouched; stored-data changes keep a copy, then migrate forward; semantic version tags and GitHub releases from the first working version. |
| Q8 Python tools | A: Ruff for linting and formatting, mypy strict, uv. |
| Q9 TypeScript tools | B: Biome for formatting and linting, pnpm, strict TypeScript. |
| Q10 User-facing text | B: one file per language from the start (English first), read through a single helper; becomes a project rule. |
| Q11 Who reviews and merges | X, in the person's words: "I will merge all pull requests, workflow shouldn't care about that. I will just call aidlc unit finish <unit-name> when it's done(verify you have everything you need for it)" |
| Q12 How the work is split | X, in the person's words: "Each developer takes a Unit and does it, no specific assignments. Whoever is free first claims the next available unit" |

Consequences recorded in `team-practices.md`:

- Every Unit lands through pull requests (merge strategy `pr`). The workflow opens the pull requests and never merges into `main` or waits for a merge. The person merges them outside the workflow, then runs `aidlc unit finish <unit> [--pr <owner>/<repo>#<n>]`. No pull request approval is needed first.
- Two developers, each in their own clone of the workspace, claim the next available Unit with `aidlc unit claim`.

## What `aidlc unit finish` needs (Q11 check)

Read from `.claude/tools/aidlc-unit.ts` at workspace commit `7b1a420`:

- Before the merge gate, `aidlc unit pr <unit> --repo <name>` opens a pull request in each code repository the Unit touches and records it.
- `aidlc unit land <unit> --strategy pr` folds the Unit's record onto the Unit's own branch and does not merge into `main`.
- `aidlc unit pr <unit>` (no `--repo`) then opens the Unit's pull request in `dayboard-workspace`, holding its record under `construction/<unit>/`, the clone's audit shard and the Unit's merged row.
- `aidlc unit finish <unit>` runs in the clone that built the Unit. It checks through the GitHub CLI that the Unit's workspace pull request is merged; `--pr <owner>/<repo>#<n>` names it when none is on record. It then closes that clone's Unit scope.

## Open points

None. Everything the lead draft left open was settled in the interview: merge style and order (Q1), when records land (Q2), walking skeleton (Q3), test order, coverage and browser tests (Q4–Q6), release and versioning (Q7), the Python type checker and package manager (Q8), TypeScript tooling (Q9), user-facing text (Q10), merging (Q11) and how the work is split (Q12).

For later stages, outside the five practice areas: the code repositories are public but have no licence file and no `.gitignore` yet.

## Sources

- [desc] The project description in `aidlc-state.md` (Project).
- `practices-discovery-questions.md`: Q1–Q12 and the confirmed summary.
- Every path and command listed in the tables above.

## Assumptions & Open Questions

None.
