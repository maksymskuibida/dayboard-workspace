# Risk and Sequencing Rationale — Dayboard, first version

Why the six Units are built in the order `bolt-plan.md` gives. A Bolt is one build pass over one or more Units, ending in something that runs; here the whole project is one Bolt (Q2), so this file explains the order inside it.

## Heuristic Used

**Flow first, inside the dependency graph:** keep both developers busy at every step, and among the orders that do, prefer the one that makes Dayboard useful soonest (Q1, C). This is closest to value-first sequencing (ship what delivers value earliest, when risk is low), adjusted for two developers working in parallel. No scoring model such as WSJF (weighted shortest job first: value and urgency against size) was used: with one Bolt and a graph that leaves few choices, a score adds nothing.

## The Order and Why

1. **U1 first.** It has no dependencies and every other Unit needs it. It also holds the privacy guard and keeps day data out of git before the first day is written, which your practices require in the first Unit built (team practices, Walking Skeleton; Mandated rules).
2. **U2 and U3 side by side.** Both need only U1. U2 lets the assistant write real days; U3 lets you read and act on them. Together they are the first usable Dayboard.
3. **U5 and U4 side by side.** U5 needs U1, U2 and U3 (live updates read the change log, extend the page, and the end-to-end check drives the MCP server and the web server); U4 needs only U3. Neither needs the other.
4. **U6 last.** Its guide covers starting both parts, connecting the assistant and running U5's end-to-end check, so it needs U1, U2, U3 and U5.

This is a topological order of the graph in `unit-of-work-dependency.md`; nothing deviates from it.

## Risks and How the Order Handles Them

| Risk | Likelihood | Impact | Handling |
|---|---|---|---|
| Live updates across processes miss or duplicate changes (U5) — your main worry (Q5) | Medium | High | Inside U5, the change log, the stream and own-change matching are built and tested first, before the toast and highlight; the end-to-end check covers a change written by a second process |
| Contrast on glass falls below 4.5:1 in one theme (U3) | Medium | Medium | Contrast checks in U3's component tests for light and dark, against the tokens in `design-system-mapping.md` |
| Two writers race on the same day (U1) | Low | High | The page-hash check runs inside the write transaction (ADR-003, ADR-007); a contract test runs two writers |
| The second developer idles while U1 is built | High | Low | Review U1's pull requests and set up CI in both repositories during step 1 (`team-allocation.md`) |
| The assistant cannot reach the MCP server | Closed | — | You confirmed the assistant can reach an MCP server over HTTP (Q4) |

## Sources

- `delivery-planning-questions.md`: Q1 to Q5 and the confirmed summary.
- `inception/units-generation/unit-of-work-dependency.md`: the graph.
- `inception/domain-design/decisions.md`: ADR-003, ADR-007.
- `aidlc/spaces/dayboard/memory/team.md`, `project.md`: Walking Skeleton, Mandated rules.

## Assumptions & Open Questions

None.
