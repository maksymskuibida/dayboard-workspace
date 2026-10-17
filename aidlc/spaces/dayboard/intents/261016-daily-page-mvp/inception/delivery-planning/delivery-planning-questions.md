# Delivery Planning Questions — Dayboard

Context: this stage decides the order in which the six Units (`inception/units-generation/unit-of-work.md`) are built. Each piece of that order is a Bolt: one build pass over one or more Units, ending in something that runs and can be shown. The dependency graph (`unit-of-work-dependency.md`) already fixes what must come before what: U1 first; U2 and U3 after U1, side by side; U4 after U3; U5 after U1, U2 and U3; U6 after U1, U2, U3 and U5. Your team practices already settle a few things: no walking skeleton (no thin end-to-end slice first), the privacy guard in the first Unit built, two developers who each claim the next free Unit, and pull requests with `dayboard-api` first. These questions settle the rest.

## Q1. Which order inside what the graph allows?

After U1, the graph leaves a choice: U2 and U3 can go in either order or side by side, and so can U4, U5 and U6 once their prerequisites are done.

**Proposed:** C, because two developers can take U2 and U3 at once, and real days written by the assistant are the first thing that makes Dayboard useful.

A. **Value first:** U1, then U2 (the assistant writes real days), then U3 (you read them), then U5, U4, U6
B. **Risk first:** U1, then U3 and U5 as early as possible, because live updates across processes and the liquid-glass page are the least certain parts; U2, U4, U6 after
C. **Both developers busy:** U1; then U2 and U3 side by side, one developer each; then U5 and U4 side by side; U6 last, when its guide can cover everything
X. Other (please specify)

[Answer]: C

## Q2. How big is one Bolt?

**Proposed:** A, because your team claims and finishes one Unit at a time, with its own pull requests.

A. **One Unit per Bolt:** six Bolts, one per Unit
B. **Bundle the small ones:** U4 goes with U3, and U6 with U5, so four Bolts
X. Other (please specify)

[Answer]: X — We have one PR per unit, but all units is one Bolt, I guess, overall, this is a small project, I believe one bolt is enough

## Q3. How are the Construction stages walked?

Each Unit goes through Construction's stages: Functional Design, NFR Requirements, NFR Design, Code Generation, Build and Test, CI Pipeline (Infrastructure Design where it applies).

**Proposed:** A, because a developer who claims a Unit takes it from design to merged code in one go.

A. **Unit by Unit:** each Unit runs all its stages to the end before that developer takes the next Unit
B. **Stage by stage:** every Unit goes through Functional Design, then every Unit through the next stage, and so on
X. Other (please specify)

[Answer]: X — Unit by Unit, but, developer may use worktrees and therefore run 2 Units in parallel, not synchronized

## Q4. Does anything outside the two of you hold up a Bolt?

Dayboard runs only on your own computers and reads no external service. One assumption from Feasibility is still open: that the assistant you use daily can reach an MCP server over HTTP on this computer (RAID A2).

**Proposed:** A.

A. **Only that assumption:** checked as soon as U2 is built, by connecting your assistant to it; if it fails, the stdio option (Contract Design Q3) is added in U2
B. **Nothing:** no outside dependency at all
X. Other (please specify)

[Answer]: B — Assistant can. I don't know why it is still open

## Q5. What worries you most, so it is tackled early? (select all that apply)

**Proposed:** none chosen for you; the plan puts the matching checks first in each Bolt you pick.

A. **Live updates across processes** (U5): the change log, the stream and own-change matching
B. **The liquid-glass page** (U3): readable contrast on glass in light and dark, and the dense layout at phone width
C. **The assistant's tools** (U2): whether seven broad tools and their descriptions are enough for the assistant to write good pages
D. **Writes under load** (U1): two writers, the lock and the page hash
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Order (Q1):** U1 first; then U2 and U3 side by side, one developer each; then U5 and U4 side by side; U6 last, when its guide can cover everything.
- **Bolt size (Q2):** the whole project is one Bolt holding all six Units, because it is a small project; each Unit still lands through its own pull requests (one per repository it touches) and is finished on its own.
- **How stages are walked (Q3):** Unit by Unit; a developer may also run two Units at once in separate worktrees, without keeping them in step.
- **Outside dependencies (Q4):** none. You confirmed the assistant can reach an MCP server over HTTP, so the Feasibility assumption (RAID A2) is closed and recorded as confirmed in the external-dependency map.
- **Worry to tackle early (Q5):** live updates across processes (U5): the change log, the stream and own-change matching are built and tested first inside U5, before its toast and highlight.
- **Who builds:** the two developers, each with an AI assistant, claiming the next free Unit; no fixed assignment (team practices).

- Looks correct
- Request changes

[Answer]: Looks correct
