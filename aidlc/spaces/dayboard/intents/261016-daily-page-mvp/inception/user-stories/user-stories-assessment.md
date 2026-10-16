# User Stories Assessment — Dayboard, first version

## Decision

**Execute.**

## Rationale

Dayboard is a user-facing product with two actors on every page: the person, who reads the page and works with it during the day, and their AI assistant, which writes most of it through the MCP server (`inception/requirements-analysis/requirements.md`, Intent Analysis). The requirements describe behaviour from the system's side; stories restate it from each actor's side, with acceptance criteria that Construction can test directly.

## Factors Considered

| Factor | Finding | Source |
|---|---|---|
| Project type | A new product, greenfield, in two code repositories | requirements.md, Intent Analysis |
| User-facing scope | A web page with six screens and states (S1–S6), and an MCP interface the assistant uses | `ideation/rough-mockups/wireframes.md`; requirements.md FR2, FR3 |
| Actors | The person and their assistant, with different rights and goals on the same data | requirements.md FR2.6, FR4 |
| Business logic | Item states per block kind, notes read by dates, the page hash on every change, live updates | requirements.md FR1, FR2.9, FR3.11, FR4.9 |
| Work split | Two developers each claim whole Units; independently testable stories help cut Units cleanly | `inception/practices-discovery/team-practices.md`, Way of Working |

## Where Stories Add the Most Value

- **The daily loop end to end:** the assistant prepares today from yesterday's notes, the person reads and acts, and notes carry to the next day.
- **Rules where two actors meet:** item states, learnings curation, and a change refused because the page changed underneath it.
- **The page's states:** first run, a date with no page, loading, offline, a failed save, live updates arriving while you type.
- **Acceptance criteria for the privacy guard**, written from the person's side so they are tested from the first Unit.

## Sources

- `inception/requirements-analysis/requirements.md`
- `inception/practices-discovery/team-practices.md`
- `ideation/rough-mockups/wireframes.md`, `ideation/rough-mockups/user-flow.md`

## Assumptions & Open Questions

None.
