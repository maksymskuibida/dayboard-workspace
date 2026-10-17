# Team Allocation — Dayboard, first version

Who builds which part of the one Bolt (a Bolt is one build pass over one or more Units, ending in something that runs; see `bolt-plan.md`). Team Formation did not run for this piece of work, so the team is the one your practices describe (`aidlc/spaces/dayboard/memory/team.md`, Way of Working).

## The Team

Two developers, each in their own clone of the workspace with `dayboard-api` and `dayboard-web` inside it, each working with an AI assistant (the AI-DLC developer agent writes the code; the developer directs and reviews it). There are no fixed assignments: whoever is free first claims the next available Unit with `aidlc unit claim` and builds it end to end, in both code repositories where the Unit needs it. The project owner merges every pull request and marks each Unit finished.

## Units by Step

| Step | Unit | Taken by | Story points |
|---|---|---|---|
| 1 | U1 API core and HTTP | The first developer free | 13 |
| 2 | U2 Assistant access | One developer | 5 |
| 2 | U3 The day page | The other developer | 13 |
| 3 | U5 Live updates and end-to-end check | One developer | 8 |
| 3 | U4 Moving between days | The other developer | 3 |
| 4 | U6 Installation | Whoever is free | 3 |

While U1 is being built, the second developer has no Unit to claim, because every other Unit depends on U1. That time can go to reviewing U1's pull requests as they open, or to setting up both code repositories' CI.

A developer may hold two Units at once in separate worktrees (`delivery-planning-questions.md`, Q3), each claimed separately.

## Sources

- `aidlc/spaces/dayboard/memory/team.md`: Way of Working.
- `bolt-plan.md`, `delivery-planning-questions.md`: the order and Q3.
- `unit-points.csv`: story points per Unit.

## Assumptions & Open Questions

None.
