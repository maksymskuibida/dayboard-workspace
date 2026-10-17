## Review

**Verdict:** READY
**Reviewer:** aidlc-architecture-reviewer-agent
**Iteration:** 1
**Verdict Dictated:** no
**Date:** 2026-10-17T02:21:35Z

### Findings

| ID | Severity | Location | Finding | Required action | Status |
|---|---|---|---|---|---|
| R-01 | Major | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C4 Operation (add_block, add_item) and C1 Notes (third bullet) | The Notes say that adding a note or learning to a day with no such block is "an add_block and an add_item in one change". But add_item needs an existing block_id, and the id of a block created in the same list is only returned afterwards in ChangeResult.created. No temporary id, client-supplied id or back-reference to an earlier operation exists. The same gap blocks the assistant from building a new block with its items in one all-or-nothing change, and FR4.6 and US-level "add a note" flows cannot be built as written. | Add a way for a later operation to name a block or item created earlier in the same list (a caller-chosen temporary id resolved at apply time, or an items list inside add_block). State how it appears in `created`. Add the example to C1 and C2. | Resolved (reviewer) |
| R-02 | Major | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C6 Web App Slot and Hook (LiveChannelHook) and Contracts table row C6 | The web-side boundary between U3 and U5 (and U4) is thinner than the design needs. `attach(onDayChanged(date, hash, removed), onRefresh)` hands back only the raw notice. components.md and unit-of-work-dependency.md say LiveSync re-reads and diffs the day and reports changed and added items to DayView for highlighting, hosts the toast in AppShell, and refreshes the day list for DayNavigation. None of these is defined: no changed-items callback (item ids), no toast host, and no list-invalidation path to U4, which has no dependency on U5. U3 must be complete on its own and U5 must fill the hook later, but an implementer cannot tell where the diff lives or how highlights and calendar updates are delivered. | Extend C6 with the missing surfaces. Define a changed-items report from the channel to DayView (item ids changed or added), the toast host owned by AppShell, and the day-list refresh signal U4 listens to (for example a shared invalidation function in U3's API client). Or state that U3 re-reads and diffs and drop the diff from LiveSync. | Resolved (reviewer) |
| R-03 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C4 Errors (code list) | ADR-001 and Storage promise that a writer finding the lock held "refuses with a retryable error" after a bounded wait, and a newer-schema or failed-upgrade state stops the process. The error codes (invalid, too_long, future_date, stale, not_found, exists) and the HTTP status list have no code or status for "busy, retry" or "storage unavailable". The assistant cannot tell a retry from a fix, and the HTTP side has only 400/404/412/428. | Add a retryable code (for example `busy`, HTTP 503, and the matching MCP tool error) and an unexpected-failure code. Say that the web app shows "Couldn't save, try again" for them (FR4.2). | Resolved (reviewer) |
| R-04 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C3 Rules (first two bullets) and the `refresh` message | Resuming has two gaps against FR3.15 ("show every change it missed"). A page whose first connection drops before any notice arrives has no `Last-Event-ID`, so the reconnect starts at the latest and misses changes made during the outage. The `refresh` message has no `id:`, so the page's resume point stays at the too-old sequence. The contract also does not say whether a page opens the stream before or after it reads the day, so a change made between its first read and its first connect is never announced. | State that `refresh` carries the current highest sequence as its id. State the page's opening order (connect the stream, then read the day and the list), or send the current sequence once on connect so the page can compare it with the read. | Resolved (reviewer) |
| R-05 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C1 intro paragraph and Contract Ownership Rules (Ports and origin) | The browser sends a CORS preflight for the change POST (JSON body, `If-Match`). The contract says only that responses "allow only the web app's origin" and expose ETag. It does not say that RequestGuard answers OPTIONS or lists the allowed methods and headers, so a correct build can still fail in the browser. "The backend's allowed origin follows the web app's setting" also does not say how two repositories with their own settings files stay in step. | Add the preflight answer (methods GET and POST, headers If-Match and Content-Type, exposed ETag) to C1. Name the backend setting that holds the web origin and the web setting that holds the API address, and say they are set by hand and documented in U6. | Resolved (reviewer) |
| R-06 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C1 paths (`/api/events`) and C4 Day.hash note | `/api/events` is listed in the C1 OpenAPI with no response, although U1 does not implement it (C3, U5 registers it), so U1's contract tests could expect a path U1 does not serve. The page hash is a bare value in the Day body and in stream notices but an ETag on HTTP, which must be a quoted string, so the own-change comparison has no stated normalization. | Remove `/api/events` from C1 or mark it as owned by C3. State that the ETag is the hash in double quotes and that the client compares the unquoted value. | Resolved (reviewer) |
| R-07 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C4 ChangeResult.created and C2 create_day | `created` is keyed by operation index, but create_day takes `blocks` and has no operations list, so what `index` means for it is undefined. | State that for create_day the index is the position in `blocks`, or give create_day its own result shape. | New |
| R-08 | Minor | aidlc/spaces/dayboard/intents/261016-daily-page-mvp/inception/contract-design/contract-summary.md > C6 LiveChannelHook.ownChangeHashes | The API client records the quoted ETag, while `day` notices carry the bare hash. The comparison that suppresses the tab's own notices only works if the client stores or compares the unquoted form. | Add a line that ownChangeHashes holds bare hashes (quotes stripped). | New |

> R-01 Reviewer note: add_block and create_day take items inline (NewBlock, NewItem); ChangeResult.created returns the new block's item ids. No operation needs an id that does not yet exist.

> R-02 Reviewer note: C6 now defines SharedDataCache, reportChangedItems, reportDayRemoved and ToastArea, and states the notice-handling flow. U4 follows through the cache alone, consistent with the Unit dependency (U4 on U1 and U3 only).

> R-03 Reviewer note: Error code busy is defined, with HTTP 503 and Retry-After: 1 in C1, the same code in C2, and a stated one-retry behaviour for the web app.

> R-04 Reviewer note: C3 has a ready event whose id is the latest sequence, and refresh carries an id. The page connects first and reads after ready.

> R-05 Reviewer note: The preflight rules (methods, request headers, 403 for other origins) and the allowed origin as a backend setting are stated.

> R-06 Reviewer note: /api/events is removed from the C1 paths and noted as C3. ETag quoting (quoted header, bare hash in MCP and notices) is stated.

### Validation Tool Results

| Tool | Result | Interpretation |
|---|---|---|
| None run | Not applicable | The stage lists no validation tools for this pass. I checked cross-references by hand against unit-of-work-dependency.md: U4 and U5 stay independent, and the registration points match. |

### Summary

All six prior findings are resolved and the revision is consistent with the Unit dependency graph. Two minor ambiguities remain (create_day result indexing, bare versus quoted hash in the own-change record), and Functional Design can settle them.
