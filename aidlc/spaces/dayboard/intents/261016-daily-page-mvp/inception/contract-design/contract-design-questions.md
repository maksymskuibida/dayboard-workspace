# Contract Design Questions — Dayboard

Context: this stage writes down the contracts every Unit builds against (`inception/units-generation/unit-of-work.md`): the HTTP interface the web app uses (kept in U1), the MCP tools the assistant uses (kept in U2), and the day-changed stream (kept in U5). The day rules, the page hash and the privacy guard are already decided (`inception/domain-design/decisions.md`). These questions settle the shapes. The stream itself follows the standard server-sent events form: each notice carries its change sequence as the event id, so a reconnecting page resumes from the last one it saw (ADR-007); that needs no question.

## Q1. How does the web app send changes?

A change can be one action (tick a state) or several (the assistant rewrites a block). Every change carries the page hash and is refused as a whole when the page changed since it was read (FR4.9).

**Proposed:** B, because the web app and the assistant then send the same operations, and several of them apply together or not at all.

A. **One request per action**, REST style: for example `PATCH /api/days/{date}/items/{itemId}` to set a state, `DELETE /api/days/{date}/blocks/{blockId}` to remove a block
B. **One change request with a list of operations**: `POST /api/days/{date}/changes` with operations such as `set_state`, `add_item`, `move_item`, `remove_block`, applied together in one transaction; reads stay plain (`GET /api/days`, `GET /api/days/{date}`)
X. Other (please specify)

[Answer]: B

## Q2. How does the page hash travel over HTTP?

Every read returns the page hash and every change carries the hash of its last read (FR4.9).

**Proposed:** A, because it is the standard HTTP way to refuse a change to something that changed since it was read.

A. **Standard headers:** a read returns the hash as `ETag`; a change sends it as `If-Match`; a stale change gets `412 Precondition Failed`
B. **A `hash` field** in every response body and every change body; a stale change gets `409 Conflict`
X. Other (please specify)

[Answer]: A

## Q3. How do assistants reach the MCP server?

The MCP server can run inside the backend, as its own HTTP process, or over stdio (ADR-006, ADR-007). The transport decides what an assistant's settings look like and what the installation guide (U6) explains.

**Proposed:** A.

A. **Both:** Streamable HTTP served by the running backend at `/mcp` (the default the guide shows first), and a stdio command for assistants that prefer to start their own
B. **HTTP only:** every assistant connects to the running backend
C. **stdio only:** every assistant starts its own MCP process
X. Other (please specify)

[Answer]: B — If we need, we'll add second one later

## Q4. How many MCP tools?

The assistant works through tools, and each tool's description costs tokens in every conversation; fewer, broader tools cost less but each does more.

**Proposed:** A, because it mirrors the HTTP contract and keeps the assistant's tool list short.

A. **A few broad tools:** `list_days`, `read_day`, `create_day`, `change_day` (the same operation list as Q1 B), `remove_day`, `read_notes_for_tomorrow`, `read_saved_learnings`
B. **One tool per action:** `add_block`, `add_item`, `set_item_state`, `set_meeting_answer`, `move_item`, `remove_item`, and so on, about 15 tools
X. Other (please specify)

[Answer]: A

## Q5. What does an error look like?

A refused write names the field, the problem and, for a limit, the limit (FR2.10); a stale change says the page changed since it was read (FR4.9). The web app turns errors into its own messages; the assistant reads them as text.

**Proposed:** B, because it is enough for both callers and adds nothing to depend on.

A. **Problem details (RFC 9457):** `application/problem+json` with `type`, `title`, `detail`, plus `field` and `limit` where they apply
B. **One simple shape:** `{"error": {"code": "...", "message": "...", "field": "...", "limit": ...}}`, with a fixed list of codes (`invalid`, `too_long`, `future_date`, `stale`, `not_found`, `exists`), the same in HTTP and MCP
X. Other (please specify)

[Answer]: B — There might be few of them, so we must return a list

## Q6. How are contract changes handled?

Both repositories run on the same computer and are pulled together, and every change reaches `main` through pull requests, with `dayboard-api` first (team practices).

**Proposed:** B, because both sides always move together on one machine, so a version number in every address adds nothing yet.

A. **Versioned addresses:** `/api/v1/...` and an MCP server version; a breaking change opens `v2` beside `v1`
B. **No version in addresses for now:** changes are additive (new fields and operations; callers ignore what they don't know); a breaking change lands in both repositories in the same release, `dayboard-api` first, and is noted in the release notes
X. Other (please specify)

[Answer]: B

## Q7. Which ports and which web origin?

The backend allows browser requests only from the web app's own origin (FR8.4), so the backend must know where the web app runs.

**Proposed:** A.

A. **Fixed defaults, changeable:** backend on `127.0.0.1:8787`, web app on `localhost:5173`; both can be changed in one settings file per repository, and the backend's allowed origin follows the web app's setting
B. **Free ports chosen at start:** each command picks a free port and prints it; the backend learns the web app's origin from a shared file
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

Does this all look correct before I generate the artifact?

- **Changes (Q1):** reads are `GET /api/days` and `GET /api/days/{date}`; every change is `POST /api/days/{date}/changes` with a list of operations applied together in one transaction, all or nothing.
- **Page hash (Q2):** a read returns it as `ETag`; a change sends it as `If-Match`; a stale change gets `412 Precondition Failed`.
- **MCP transport (Q3):** Streamable HTTP only, served by the running backend at `/mcp`; a stdio option can be added later if needed ("If we need, we'll add second one later"). The design still allows it (ADR-006), so nothing else changes; the installation guide (U6) shows the HTTP connection only.
- **MCP tools (Q4):** seven broad tools: `list_days`, `read_day`, `create_day`, `change_day` with the same operation list as HTTP, `remove_day`, `read_notes_for_tomorrow`, `read_saved_learnings`.
- **Errors (Q5):** one simple shape, the same in HTTP and MCP, and always a list, because one change can break several rules at once: `{"errors": [{"code", "message", "field", "limit"}]}` with the codes `invalid`, `too_long`, `future_date`, `stale`, `not_found`, `exists`.
- **Contract changes (Q6):** no version in addresses for now; changes are additive and callers ignore what they don't know; a breaking change lands in both repositories in one release, `dayboard-api` first, and is noted in the release notes.
- **Ports and origin (Q7):** backend on `127.0.0.1:8787`, web app on `localhost:5173`, each changeable in one settings file per repository; the backend's allowed origin follows the web app's setting.
- **Day-changed stream:** `GET /api/events`, standard server-sent events, each notice carrying its change sequence as the event id so a reconnecting page resumes with `Last-Event-ID` (ADR-007, ADR-008).

- Looks correct
- Request changes

[Answer]: Looks correct

