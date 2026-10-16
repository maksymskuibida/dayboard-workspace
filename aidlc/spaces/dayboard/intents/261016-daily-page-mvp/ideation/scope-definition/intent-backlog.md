# Intent Backlog — Dayboard, first version

The proto-Units of the first version, prioritized for Delivery Planning. Inputs: `scope-document.md` (this stage), the intent statement (`ideation/intent-capture/intent-statement.md`), the feasibility assessment (`ideation/feasibility/feasibility-assessment.md`) and the constraint register (`ideation/feasibility/constraint-register.md`).

## Prioritization Method

MoSCoW. Every item below is a **Must** for the first cut: your success test needs everything in the description from day one (Q4). Within the Musts, order comes from dependencies, with the privacy guard pulled forward because the content is sensitive from the first real day (constraint register SC-01; feasibility R1, R2). You have no preference on build order (Q5), so this order is a proposal for Delivery Planning.

## Backlog

| ID | Proto-Unit | What it delivers | MoSCoW | Depends on | Source |
|---|---|---|---|---|---|
| IB-01 | Day pages | A page per day, with a title, the six fixed block kinds, and items with text, optional links and a state | Must | — | intent statement |
| IB-02 | Privacy guard | The server answers only this machine, its own page and your assistant, and day data is never committed | Must | — (built with the first slice) | SC-01, SC-02; R1, R2 |
| IB-03 | Assistant access | The assistant lists days, reads a page, creates a page for any date (past ones included), writes and updates blocks and items, and changes states, on any day | Must | IB-01, IB-02 | intent statement, Q1, Q3, your clarification at this stage's review |
| IB-04 | Reading today's page | The page opens on the latest day on record, today or later, in light and dark, usable at phone width | Must | IB-01, IB-02 | intent statement |
| IB-05 | Changing states on the page | You mark items open, done or delayed to tomorrow; delayed items are not moved automatically | Must | IB-04 | intent statement, Q1, Q2 |
| IB-06 | Notes for tomorrow | Written by you on the page or by the assistant on request, shown on the page, and read by the assistant by dates or from the latest day on record | Must | IB-03, IB-04 | intent statement, Q1 |
| IB-07 | Calendar of days | You open any earlier day from a calendar; days with no page are greyed out | Must | IB-04 | Q3 |
| IB-08 | "Copy prompt" button | Copies the instruction for updating today's page to the clipboard | Must | IB-03, IB-04 | intent statement |
| IB-09 | Two-command local run | One command for the backend and one for the web app, each person running their own copy | Must | IB-03, IB-04 | TC-02, TC-06 |

## Proposed Order

1. **First slice:** IB-01 + IB-02 + IB-03 + IB-04. The assistant writes a day and you read it, on a server locked to this machine, end to end.
2. **Daily use:** IB-05 and IB-06, the loop that carries work from one day to the next.
3. **Completeness:** IB-07, IB-08 and IB-09.

## Assumptions & Open Questions

- **Storage choice** (constraint register TC-05) must be settled before IB-01 is built.
- **Order is a proposal.** The final grouping and order are set in Delivery Planning (Q5).
