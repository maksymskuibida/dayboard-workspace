# External Dependency Map — Dayboard, first version

Anything outside the two developers that could hold up the Bolt (a Bolt is one build pass over one or more Units, ending in something that runs; see `bolt-plan.md`).

## Dependencies

None. Dayboard runs only on the developers' own computers and reads no external service, account or data source (constraint register TC-02, TC-06; team practices, Deployment).

## Closed Items

| Item | Was | Now | Source |
|---|---|---|---|
| The assistant can reach an MCP server on this computer (RAID A2) | An open assumption from Feasibility, to be checked at the first end-to-end run | Confirmed: your assistant can reach an MCP server over HTTP; nothing blocks U2 | `delivery-planning-questions.md`, Q4 |

## Tools the Build Relies On

These are the developers' own tools, not outside dependencies: GitHub for pull requests and CI, Python with uv, Node with pnpm, and the AI assistant each developer uses (team practices, Code Style).

## Sources

- `delivery-planning-questions.md`: Q4.
- `ideation/feasibility/raid-log.md`: A2.
- `ideation/feasibility/constraint-register.md`: TC-02, TC-06.

## Assumptions & Open Questions

None.
