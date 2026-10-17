# Contract Summary — Dayboard, first version

The contracts every Unit builds against, from your answers in `contract-design-questions.md` (Q1 to Q7 and the confirmed summary), the Units in `inception/units-generation/unit-of-work.md` and the design in `inception/domain-design/components.md` and `decisions.md`. Each contract lives in the Unit that first builds it (Units Generation, Q3). Every example uses made-up content.

## Contracts

| # | Provider Unit | Consumer | Mechanism | Owner |
|---|---|---|---|---|
| C1 | U1 API core and HTTP | U3 The day page, U4 Moving between days, U5 (web side) | HTTP, JSON, `ETag` / `If-Match` | U1 |
| C2 | U2 Assistant access | External: the person's assistant | MCP over Streamable HTTP at `/mcp` | U2 |
| C3 | U5 Live updates (backend) | U5 Live sync (web) | Server-sent events at `/api/events` | U5 |
| C4 | U1 | C1, C2 and C3 alike | Shared schema: the day, its blocks, items, operations and errors | U1 |
| C5 | U1 | U2, U5 | In-process registration points in the backend | U1 |
| C6 | U3 | U4, U5 | In-process slot and hook in the web app | U3 |

Every HTTP request, the stream and `/mcp` pass through RequestGuard first (ADR-006): only this computer, only `localhost`, `127.0.0.1` or `::1` as host, and for browser requests only the web app's origin.

## C4 · Shared Schema: Day, Blocks, Items, Operations, Errors

One model for all three wire contracts. Field names are `snake_case` on the wire. Limits are NFR3's.

```yaml
# shared-schema
Day:
  date: string            # YYYY-MM-DD, never after today (FR1.1, FR1.10)
  title: string           # up to 200 characters; "" shows no title line (FR1.2)
  hash: string            # page hash; in C1 it travels as ETag, in C2 as this field (FR4.9)
  blocks: [Block]         # stored order (FR1.3); up to 50

Block:
  id: string              # given once, never reused (FR1.12)
  kind: schedule | open_items | pull_requests | waiting_on_others | learnings | notes_for_tomorrow
  title: string           # up to 120 characters, may be ""
  items: [Item]           # stored order; up to 200

Item:
  id: string
  text: string            # Markdown, up to 10,000 characters (FR3.6)
  links: [Link]           # up to 10
  # task kinds (open_items, pull_requests, waiting_on_others):
  state: open | done | later                        # FR1.6
  # schedule:
  all_day: boolean                                  # FR1.13
  start: string | null    # HH:MM, null when all_day
  end: string | null      # HH:MM; earlier than start means the next day
  answer: yes | no | maybe | null                   # null = no answer yet
  # learnings:
  learning_state: unsaved | saved                   # discarded learnings are never returned (FR1.7, FR2.3)
  # notes_for_tomorrow: no state field; discarded notes are never returned (FR1.8)

Link:
  url: string             # up to 2,048 characters; only http, https, mailto are shown as links (FR3.7)
  label: string | null

NewBlock:                 # a block being created, with its first items inline
  kind: Block.kind
  title: string?
  items: [NewItem]?

NewItem:                  # an item being created; the same fields as add_item without block_id
  text: string
  links: [Link]?
  state: string?          # task kinds
  all_day: boolean?       # schedule
  start: string?
  end: string?
  answer: string?

Operation:                # applied in order, all or nothing, in one transaction (Q1, ADR-003)
  # Creating operations take their children inline, so no operation ever needs an id that doesn't exist yet.
  - {op: set_title, title}
  - {op: add_block, kind, title?, items?: [NewItem], after_block_id?}   # after_block_id null = first
  - {op: retitle_block, block_id, title}
  - {op: move_block, block_id, after_block_id}
  - {op: remove_block, block_id}
  - {op: add_item, block_id, text, links?, after_item_id?, state?, all_day?, start?, end?, answer?}
  - {op: change_item, item_id, text?, links?}
  - {op: set_state, item_id, state}                           # task kinds only
  - {op: set_schedule, item_id, all_day, start?, end?}        # making it timed needs start and end
  - {op: set_answer, item_id, answer}                         # yes | no | maybe
  - {op: set_learning, item_id, learning_state}               # saved | discarded; rules in DayPages (ADR-004)
  - {op: discard_note, item_id}
  - {op: toggle_checkbox, item_id, index, checked}            # the index-th task-list marker only (FR3.12)
  - {op: move_item, item_id, to_block_id?, after_item_id}
  - {op: remove_item, item_id}

ChangeResult:
  day: Day                # the day after the change
  created: [{index: integer, id: string, item_ids: [string]?}]   # by operation index: the new block's or item's id, and a new block's item ids in order

Errors:                   # always a list: one change can break several rules (Q5)
  errors:
    - code: invalid | too_long | future_date | stale | not_found | exists | busy   # busy: retry shortly (ADR-001)
      message: string     # plain English, safe to show to the assistant
      field: string | null   # e.g. "operations[2].start"
      limit: integer | null  # for too_long and count limits
```

## C1 · HTTP Interface (U1)

Served by the backend at `http://127.0.0.1:8787` by default. Reads are plain; every change is one request with a list of operations (Q1). The page hash travels in standard headers (Q2). Responses allow only the web app's origin (`http://localhost:5173` by default, following the web app's setting, Q7) and expose `ETag`.

```yaml
openapi: 3.1.0
info:
  title: Dayboard HTTP interface
  version: "0"            # no version in addresses (Q6); changes are additive
servers:
  - url: http://127.0.0.1:8787
paths:
  /api/days:
    get:
      summary: List the dates that have a page, oldest first (FR2.2, FR5)
      parameters:
        - {name: from, in: query, schema: {type: string, format: date}}
        - {name: to, in: query, schema: {type: string, format: date}}
      responses:
        "200":
          content:
            application/json:
              schema:
                type: object
                properties:
                  today: {type: string, format: date}     # this computer's local date (FR1.9)
                  dates: {type: array, items: {type: string, format: date}}
  /api/days/{date}:
    parameters:
      - {name: date, in: path, required: true, schema: {type: string}}
    get:
      summary: Read one day
      responses:
        "200":
          headers: {ETag: {schema: {type: string}}}
          content: {application/json: {schema: {$ref: "#/components/schemas/Day"}}}
        "404": {description: "No page for this date, or not a valid date (code not_found or invalid)", content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
  /api/days/{date}/changes:
    parameters:
      - {name: date, in: path, required: true, schema: {type: string}}
    post:
      summary: Apply a list of operations to one day, all or nothing (FR4, FR3.12)
      parameters:
        - {name: If-Match, in: header, required: true, schema: {type: string}}
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [operations]
              properties:
                operations: {type: array, minItems: 1, items: {$ref: "#/components/schemas/Operation"}}
      responses:
        "200":
          headers: {ETag: {schema: {type: string}}}
          content: {application/json: {schema: {$ref: "#/components/schemas/ChangeResult"}}}
        "400": {description: "One or more operations break a rule (invalid, too_long, future_date)", content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
        "404": {description: "No page for this date, or an id that doesn't exist", content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
        "412": {description: "The page changed since it was read (code stale); nothing stored", content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
        "428": {description: "If-Match missing (code invalid, field If-Match)", content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
        "503": {description: "Another writer holds the lock longer than the short wait (code busy); nothing stored", headers: {Retry-After: {schema: {type: integer, const: 1}}}, content: {application/json: {schema: {$ref: "#/components/schemas/Errors"}}}}
components:
  schemas:
    Day: {description: "C4 Day"}
    Operation: {description: "C4 Operation"}
    ChangeResult: {description: "C4 ChangeResult"}
    Errors: {description: "C4 Errors"}
```

Notes:
- The web app never creates or removes a whole day: there is no `POST /api/days` or `DELETE /api/days/{date}` on HTTP (FR4.8; only the assistant creates and removes days, through C2).
- Adding a note or a learning when the day has no such block is one `add_block` with the note or learning in its `items` (FR4.6).
- `/api/events` is served on the same backend but belongs to C3 (U5), not to this contract.
- **ETag and hash:** the `ETag` header is the page hash in double quotes (`"9f2c41"`); `If-Match` is compared with the current hash after removing the quotes. The MCP tools (C2) use the bare hash.
- **Busy:** on `503` with code `busy`, the web app retries the same change once after `Retry-After`, then shows "Couldn't save, try again" (FR4.2).
- **Cross-origin rules:** the backend answers a preflight (`OPTIONS`) only when `Origin` is its allowed web origin, allowing the methods `GET` and `POST` and the request headers `If-Match` and `Content-Type`, and exposing `ETag`; any other origin gets `403` (FR8.4). The allowed origin is a setting in the backend's own settings file, default `http://localhost:5173`; the installation guide (U6) says to change it together with the web app's port (Q7).
- A request refused by RequestGuard gets `403` with no body content about any day.

## C2 · MCP Tools (U2)

Streamable HTTP served by the running backend at `http://127.0.0.1:8787/mcp` (Q3). A stdio option can be added later if needed; the design already allows it (ADR-006). Seven tools (Q4). MCP has no headers, so the page hash travels as a field, bare (no quotes). A `busy` error tells the assistant to retry the same call shortly. Each tool's description teaches the text rules, including headings at level 3 or smaller in item text (FR2.12). Errors come back as a tool error whose text is the C4 `errors` list as JSON.

```yaml
# mcp-tools
server: {name: dayboard, transport: streamable-http, path: /mcp}
tools:
  - name: list_days
    input: {from?: date, to?: date}
    output: {today: date, dates: [date]}                     # FR2.2
  - name: read_day
    input: {date: date}
    output: Day | {no_page: true, date: date}                # FR2.3; unsaved learnings marked unsaved (FR2.11)
  - name: create_day
    input: {date: date, title?: string, blocks?: [NewBlock]}        # today or earlier only (FR2.4, FR1.10); blocks and items inline
    output: Day                                              # the new day exactly as stored: what the database holds after the write, in its stored order, every block and item with its id; error code exists when the date has a page
  - name: change_day
    input: {date: date, hash: string, operations: [Operation]}      # same operations as C1 (FR2.5, FR2.6)
    output: ChangeResult                                     # error code stale when the hash is old (FR4.9)
  - name: remove_day
    input: {date: date}                                      # no confirmation, no undo (FR2.7)
    output: {removed: date}
  - name: read_notes_for_tomorrow
    input: {dates?: [date]}
    output:                                                  # FR2.8, FR2.9
      by_date: [{date: date, notes: [Item] | null}]          # null = no page for that date
      chosen_dates: [date]                                   # when no dates given: latest day before today, plus today if it has notes
  - name: read_saved_learnings
    input: {from: date, to: date}
    output: {learnings: [{date: date, item: Item}]}          # saved and not discarded only (FR2.11)
```

## C3 · Day-Changed Stream (U5)

`GET /api/events` on the backend, standard server-sent events (ADR-007, ADR-008). Each notice carries no page content: only the date, its hash and whether it was removed.

```yaml
asyncapi: 3.0.0
info: {title: Dayboard day-changed stream, version: "0"}
channels:
  events:
    address: /api/events
    messages:
      ready:
        summary: First event on every connection; its id is the latest change sequence, so a dropped first connection can resume
        payload: {type: object, properties: {}}
        # SSE framing: "id: <latest sequence>", "event: ready", "data: {}"
      day:
        summary: A day changed or was removed
        payload:
          type: object
          properties:
            date: {type: string, format: date}
            hash: {type: string, nullable: true}   # null when removed
            removed: {type: boolean}
        # SSE framing: "id: <sequence>", "event: day", "data: <payload JSON>"
      refresh:
        summary: The page fell behind the kept change log; re-read the list of days and the open day
        payload: {type: object, properties: {}}
        # SSE framing: "id: <latest sequence>", "event: refresh", "data: {}"
operations:
  receive:
    action: receive
    channel: {$ref: "#/channels/events"}
```

Rules:
- A new connection starts at the latest change: its first event is `ready`, whose id is the latest sequence (ADR-007).
- A reconnecting page sends `Last-Event-ID: <sequence>`; it gets every change after it, or one `refresh` (with the latest sequence as its id) when that sequence is older than the oldest change kept.
- The page **connects to the stream first and reads the day after `ready`**, so no change can fall between the read and the connection.
- A comment line is sent every 15 seconds so a dropped connection is noticed.
- The web app ignores a `day` notice whose date and hash match a change it made itself, and holds a day's notices while one of its own changes to that day is in flight (ADR-007).

## C5 · Backend Registration Points (U1)

In-process, inside the backend started by U1's command (`unit-of-work-dependency.md`, Mounting at Start-Up).

```yaml
# python-interface
register_mcp(app) -> None          # U2 mounts its MCP server at /mcp behind RequestGuard
register_events(app) -> None       # U5 mounts /api/events behind RequestGuard
Storage.highest_sequence() -> int
Storage.changes_after(sequence: int) -> list[DayChange] | TooOld   # TooOld when older than the oldest kept
```

## C6 · Web App Slot and Hook (U3)

In-process, inside the web app.

```yaml
# typescript-interface
HeaderNavigationSlot: component placed in the header next to the date      # filled by U4
SharedDataCache:                        # the web app's one data cache, owned by the API client (U3)
  markDayStale(date): void              # the day view re-reads that day on its own
  markDaysListStale(): void             # navigation (U4) and the calendar re-read the list of days on its own
LiveChannelHook:                        # U5's Live sync attaches here
  attach(): void                        # called once by the app shell at start-up
  ownChangeHashes(date): string[]       # bare hashes: the API client removes the ETag's quotes before recording, so they match notices (ADR-007)
  isChangeInFlight(date): boolean
  reportChangedItems(date, itemIds: string[]): void   # the day view highlights these (FR3.16)
  reportDayRemoved(date): void          # the day view switches to "No page for this day" (FR3.14)
ToastArea: a place in the app shell where Live sync shows "This day was updated" (FR3.16)
```

On each `day` notice that isn't this tab's own change, Live sync marks the list of days stale and, for the open day, re-reads it, compares old and new, reports the changed items and shows the toast; on a removal of the open day it reports the removal; on `refresh` it marks both the list of days and the open day stale. Navigation follows through the shared cache alone, so it needs no link to Live sync (`unit-of-work-dependency.md`).

## Contract Ownership Rules

- **Owner:** each contract belongs to the Unit in the Contracts table; changes to it land in that Unit's repository first, through a pull request.
- **Additive changes are safe:** new fields, new operations and new tools may be added at any time; every caller ignores fields and notices it doesn't know (Q6).
- **Breaking changes** (removing or renaming a field, an operation or a tool, or changing a meaning) land in both repositories in the same release, `dayboard-api` first, and are written in the release notes (Q6; team practices, Way of Working).
- **Contract tests:** every HTTP endpoint, every MCP tool and the stream have contract tests in the providing Unit (team practices, Testing Posture).
- **Ports and origin** (Q7): backend `127.0.0.1:8787` and web app `localhost:5173` by default, each changeable in one settings file per repository; the backend's allowed origin follows the web app's setting.

## Open Questions

| Contract | Question | Blocks |
|---|---|---|
| C4 | The exact form of identifiers (for example a short random string) and of the page hash | U1 (Functional Design) |
| C3 | How long the change log is kept before trimming, which decides when a page gets `refresh` | U5 (Functional Design) |

## Sources

- `contract-design-questions.md`: Q1 to Q7 and the confirmed summary.
- `inception/units-generation/unit-of-work.md`, `unit-of-work-dependency.md`: the Units, the registration points and where each contract lives.
- `inception/domain-design/components.md`, `decisions.md`: the components, ADR-003 (one unit of change), ADR-004 (state rules), ADR-006 (request guard), ADR-007 (change log and stream), ADR-008 (small notices).
- `inception/requirements-analysis/requirements.md`: FR1 to FR9, NFR3 (limits).

## Assumptions & Open Questions

- [assumption] The 15-second keep-alive on the stream is short enough to notice a dropped connection without noticeable load; NFR Design confirms it.
- The two open questions above go to Functional Design.
