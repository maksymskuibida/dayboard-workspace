# Feasibility Assessment — Dayboard

Inputs: the approved intent statement (`ideation/intent-capture/intent-statement.md`) and the answers in `feasibility-questions.md` (Q1–Q6). No market research ran for this plan.

## Verdict

**Feasible, with low technical risk.** Everything the intent statement asks for is well-understood work for one person who is comfortable with the proposed stack (Q5). Nothing has to integrate with an existing system: the assistant does the investigating and reaches Dayboard through a local connection (Q4), and nothing carries over from today's reports (Q6).

The real risks are not about whether it can be built. They are about keeping sensitive page content private on a server with no sign-in (Q2, Q3), and about the pace of this plan against the hope of having it tomorrow (Q1). Both are in the RAID log (`raid-log.md`).

## Technical Viability by Capability

| Capability (from the intent statement) | Viability | Notes |
|---|---|---|
| A page per day with a title and blocks of fixed kinds; items with text, optional links and a state | Straightforward | A small, fixed data shape. The six block kinds are generic and closed for this version. |
| The page always opens on the latest day on record, today or later | Straightforward | Needs one stated rule for what "today" means; see assumption A1 in the RAID log. |
| Notes for tomorrow, read by a list of dates or from the latest day on record | Straightforward | A read across days. "Latest day on record" also covers the Friday-to-Monday case without a calendar. |
| The assistant lists days, reads a page, writes or updates blocks and items, and reads notes by dates | Straightforward | Your assistant connects to the backend running on this machine (Q4). Every assistant gets the same operations, so no assistant-specific work is needed. |
| A "copy prompt" button that puts the update instruction on the clipboard | Straightforward | The browser can write to the clipboard when the page is opened on this computer (Q3). |
| One command for the backend, one for the web app, on this machine | Straightforward | Each person runs their own copy; there is no shared space (Q4). |
| Interfaces that look current, in light and dark, at phone width | Straightforward, needs design care | Phone width is a layout requirement only; the page is opened on this computer (Q3). |
| Can move to Cloudflare or elsewhere later without a rewrite | Feasible if kept in mind | Depends mostly on the storage choice and on keeping storage behind one boundary. The choice between plain files and a single database file stays open; see the constraint register. |

## Risk Analysis

| Area | Level | Why |
|---|---|---|
| Privacy of page content | **Medium** | The content is sensitive and must be readable only on this machine (Q2), yet the server has no sign-in. Any web page you visit in the same browser could try to reach a server on this machine. This must be closed off by design, not by sign-in. |
| Data in a public repository | **Medium** | The repositories are public, and day data must never be committed (Q2). Data kept in the folder the service runs from needs to be ignored by git from the first run. |
| Pace of this plan | **Medium** | The code is small (Q1), but this plan has many approval steps. Having it tomorrow depends on how quickly those steps close, not on the build. |
| Same item changed by the assistant and by you at once | **Low** | Rare with one person, but a lost change would be silent. A simple rule is enough. |
| Moving to another host later | **Low** | Low if storage sits behind one boundary. The open storage choice is where a rewrite could hide. |
| Stack and skills | **Low** | You are comfortable with both halves of the stack (Q5). |

## Regulatory and Compliance

No regulatory regime applies. One person keeps their own work notes on their own machine, with nothing shared and nothing hosted (Q2, Q3, Q4). The obligation that remains is the one you set: the content is never committed and never readable from outside this machine (Q2).

## Assumptions & Open Questions

- **Storage choice is open.** Plain files (one per day) and a single database file are both viable. Simplicity for the current goal decides, and the trade-off is to be written down when it is decided (intent statement, Initial Scope Signal).
- **"Today" follows this computer's clock and time zone** (RAID log A1).
