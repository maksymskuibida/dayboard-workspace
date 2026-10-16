# Practices Discovery Questions — Dayboard

Context: Dayboard has no code yet, so these questions settle how you want to work before anything is built. The release engineer drafted a suggested answer for each one in `inception/practices-discovery/team-practices.md`. That draft starts from the framework defaults, adapted to Dayboard: you work alone, there are three public repositories (the records workspace, `dayboard-api` and `dayboard-web`), and everything runs only on this machine. In each question, option A is that suggestion. Your answers become the standing practices every later stage follows.

## Q1. How should changes reach `main`?

The workspace history shows every change arriving as a GitHub pull request that was squash-merged (#1, #2, #3). The two code repositories have one commit each, so they show no habit yet. This decides how each built piece of work lands in the code repositories.

A. A pull request for every change in all three repositories, squash-merged into `main` (one commit per pull request), short-lived `<kind>/<topic>` branches, and you merge your own pull requests. When a piece touches both code repositories, the `dayboard-api` pull request merges first.
B. Commit straight to `main` in the two code repositories while you work alone, and keep pull requests for the records workspace only
C. Pull requests everywhere, merged with a merge commit, so each piece's individual commits stay in the history
D. Pull requests everywhere, rebase-merged: a straight history that still shows every commit
X. Other (please specify)

[Answer]: A

## Q2. When should the workflow records (requirements, designs, decisions) be committed to the workspace repository?

The Ideation records landed together in one pull request (#3). This decides how often you see a records pull request.

A. One pull request at each natural pause: the end of a phase, or whenever you stop the work to continue later
B. One pull request after every approved stage
C. Together with the code: each code pull request has a matching records pull request
X. Other (please specify)

[Answer]: A

## Q3. Build a thin end-to-end slice first? A walking skeleton is a minimal version that runs the whole way through, built first to prove the pieces connect before the real features go in.

The privacy guard (the server answers only this machine, its own page and the assistant, and day data is never committed) is part of the first slice in every option below, because your project rule requires it.

A. Yes: the assistant writes today's page through the MCP server, the web page opens on it and shows it, and the privacy guard is on. One command proves it, and you run it and approve it before any other piece starts.
B. Yes, but backend only: the assistant writes and reads a day through the MCP server with no web page yet; the page comes in the second piece
C. Yes, as in A, and you can also change an item's state on the page, so the daily loop is closed from the start
D. No: build the backlog items fully, one after another
X. Other (please specify)

[Answer]: D — no skeleton, switch it off

## Q4. When are tests written?

This sets the order of work inside every piece that gets built.

A. After the code: build each layer (storage and day rules, then the MCP and HTTP layer, then the web components), then write and run that layer's tests before the next layer
B. Test first (test-driven) for the backend's day rules, tests after the code for the web app
C. Acceptance scenarios first: the Given/When/Then criteria from the user stories are automated before the code, with unit tests written after
D. Test first everywhere
X. Other (please specify)

[Answer]: A

## Q5. What test coverage must each code repository keep?

The framework default for this kind of first version is at least 80% of lines covered, checked on every pull request; a drop below the floor blocks the merge, and the floor is never lowered to make a build pass.

A. At least 80% line coverage in both `dayboard-api` and `dayboard-web`
B. At least 90% for `dayboard-api` and 70% for `dayboard-web`
C. A different floor (say which in Other)
X. Other (please specify)

[Answer]: A

## Q6. Should the web page get real browser tests?

Without them, the end-to-end check is a scripted check that talks to the MCP server and the web server directly. A browser tool such as Playwright clicks through the real page in light and dark and at phone width, at the cost of one more development dependency.

A. No browser tool for now: the scripted end-to-end check plus component tests are enough
B. Yes: Playwright for the main flows (open the latest day, change an item's state, copy the prompt)
C. Later: add browser tests once the page has more than one screen
X. Other (please specify)

[Answer]: A

## Q7. How is a new version released and rolled back?

Dayboard runs only on this machine and is not deployed anywhere. The suggestion is that a change merged into `main` counts as released: you pull `main`, restart the two commands, and run the skeleton's check as a smoke test. Rolling back means checking out the previous commit; day data is never touched. A change to how day data is stored keeps a copy of the old data, then moves existing days forward.

A. As described: no version tags or GitHub releases yet
B. As described, plus semantic version tags and GitHub releases from the first working version
C. As described, plus one command (or Docker Compose) that starts both apps in place of the two commands. This changes the "one command for the API, one for the web app" constraint (TC-02).
X. Other (please specify)

[Answer]: B

## Q8. Which tools check Python code style in `dayboard-api`?

The checks run in CI on every pull request and a failure blocks the merge.

A. Ruff for linting and formatting (its formatter follows Black's style), mypy in strict mode for types, and uv to manage the environment and dependencies
B. Ruff for linting and formatting, pyright for types, uv for dependencies
C. Black for formatting and Ruff for linting (the framework default), mypy for types, pip with a requirements file
D. Ruff only, with no type checker
X. Other (please specify)

[Answer]: A

## Q9. Which tools check TypeScript code style in `dayboard-web`?

TypeScript runs in strict mode with any of these.

A. Prettier for formatting and ESLint for linting, with pnpm
B. Biome as one tool for both formatting and linting, with pnpm
C. Whatever the TanStack Start scaffolder sets up, unchanged
X. Other (please specify)

[Answer]: B

## Q10. Where does the page's user-facing text (labels, buttons, empty states, errors) live?

Your standing instruction is that user-facing copy is never written into the code, but Dayboard has no translation source yet. This decides where every label lives from the first slice on, and whether that becomes a Dayboard rule.

A. One English strings file in `dayboard-web`, read through a single helper; components never contain copy. This becomes a project rule.
B. One file per language from the start (English first, others added later), read through a single helper. This becomes a project rule.
C. The backend serves the strings, so the assistant and the page share one source
X. Other (please specify)

[Answer]: B

## Q11. Who reviews and merges pull requests now that two developers work on Dayboard?

Follow-up: Dayboard will be built by two developers, each working in their own clone of the workspace. The answer to Q1 assumed one person merging their own pull requests, so this settles how review works between the two of you.

A. Each pull request is reviewed and merged by the other developer
B. Each developer merges their own pull requests; the other is told when it lands
C. The other developer reviews only pull requests that change what the web app and the API share (the MCP and HTTP interface); everything else is merged by its author
X. Other (please specify)

[Answer]: X — I will merge all pull requests, workflow shouldn't care about that. I will just call aidlc unit finish <unit-name> when it's done(verify you have everything you need for it)

## Q12. How do the two developers split the work?

Follow-up: this decides whether a piece of work belongs to one person across both code repositories, or each person owns one repository. Which pieces go to whom is planned later, in Delivery Planning.

A. By piece of work: each developer takes whole pieces and builds them end to end in their own clone, in both `dayboard-api` and `dayboard-web` where the piece needs it
B. By repository: one developer owns `dayboard-api`, the other owns `dayboard-web`
C. Decide piece by piece in Delivery Planning
X. Other (please specify)

[Answer]: X — Each developer takes a Unit and does it, no specific assignments. Whoever is free first claims the next available unit

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Way of working:** a pull request for every change in all three repositories, squash-merged into `main`; short-lived `<kind>/<topic>` branches; the `dayboard-api` pull request merges first when both code repositories change. Each Unit lands as pull requests, never as a direct merge by the workflow.
- **Team:** two developers, each in their own clone of the workspace. No fixed assignments: whoever is free first claims the next available Unit and builds it.
- **Merging:** you merge all pull requests yourself; the workflow neither merges nor waits for a merge. You mark a Unit finished when it's done (Q11: `aidlc unit finish <unit-name>`).
- **Records:** committed to the workspace in one pull request at each natural pause (end of a phase, or when the work is stopped).
- **First slice:** no walking skeleton; it is switched off, and the backlog items are built fully one after another. The privacy guard still goes into the first piece built (project rule).
- **Test order:** tests after the code, layer by layer (storage and day rules, then MCP and HTTP, then web components), each layer's tests run before the next layer.
- **Coverage:** at least 80% line coverage in both `dayboard-api` and `dayboard-web`, never lowered to pass a build.
- **Browser tests:** none for now; a scripted end-to-end check plus component tests.
- **Release:** merged to `main` is released; pull, restart, run the smoke check; roll back by checking out the previous commit, day data untouched; semantic version tags and GitHub releases from the first working version.
- **Python tools:** Ruff for lint and format, mypy strict, uv for environment and dependencies.
- **TypeScript tools:** Biome for format and lint, pnpm, strict TypeScript.
- **User-facing text:** one file per language from the start (English first), read through a single helper; components never contain copy. This becomes a project rule.

- Looks correct
- Request changes

[Answer]: Looks correct
