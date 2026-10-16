# Intent Capture Questions — Dayboard

## Sources

- [desc] Initial description: "Dayboard is a new, public, open-source project; this is its first piece of work. Work in space `dayboard`. The code will live in the two child repositories, dayboard-api (Python) and dayboard-web (TanStack Start); this workspace holds the records.\n\nTHE IDEA\nA daily page for one person and their AI assistant. Each day has a page with a title and blocks of fixed kinds: schedule (meetings), open items, pull requests, learnings, waiting on others, and notes for tomorrow. A block holds items; an item has text, optional links, and a state the assistant and the person can change (open, done, delayed to tomorrow). The assistant writes most of the page after investigating the person's tools; the person reads it during the day. The page always opens on the latest day on record, today or later.\n\nNotes for tomorrow belong to each day's page: the assistant fills them on the person's request (\"delay this to tomorrow\"), the page shows them, and the assistant reads them when it prepares the next day, by a list of dates, or the latest day on record when no dates are given (the Friday page when asked on Monday).\n\nAny assistant reaches the same data through an MCP server: list days, read a page, write or update blocks and items, read notes by dates. No authentication. A \"copy prompt\" button on the page puts on the clipboard the instruction the person pastes into their assistant to update today's page through the MCP server.\n\nWHO IT IS FOR\nMe first: today my assistant writes my daily report as a chat artifact and keeps its own memory of it. Dayboard replaces that with a real page, better design, fewer tokens, and it works from any assistant. Later, other people and their own kinds of blocks.\n\nWHAT IS FIXED\n- One person. No users, no user field, no configuration of one; every record belongs to whoever runs it. Users are a later piece of work.\n- Runs locally: one command for the API, one for the web app. Designed so it can run on Cloudflare or anywhere else later without a rewrite, but not now.\n- Public repositories: nothing company-specific, no internal names, no secrets, no references to my employer's tools or processes. Dayboard is project-agnostic; the block kinds are generic.\n- Interfaces must look current: a type scale, spacing, visual hierarchy, considered states, light and dark, phone width.\n\nSTACK PROPOSAL\n- Backend in Python. Frontend in TanStack Start with TanStack Query, and TanStack Form where a form exists. Few dependencies.\n- Storage, two proposals to decide between later with the trade-off written down: plain files in the directory the service runs from, git-ignored, one file per day; or SQLite, one file, whose schema moves to Cloudflare D1 later. Simplicity for the current goal decides.\n\nOUT OF SCOPE FOR NOW, BUT NOT TO BE DESIGNED OUT\nUsers and authentication, cloud deployment, custom block kinds added by users, a plugin or engine layer, notifications, any integration that reads the person's tools directly (the assistant does that)."
- [scope] Workflow-selected scope: `mvp`.

## Q1. How will you know Dayboard has replaced today's chat-artifact report?

Your description names three gains (a real page with better design, fewer tokens, any assistant). This question picks what you will actually check to call the first version a success.

A. I use it every working day for two weeks and never fall back to the chat artifact
B. Preparing a day costs clearly fewer tokens than today (for example, half or less)
C. At least two different assistants have updated the same page through the MCP server
D. All of A, B and C
E. Not yet defined
X. Other (please specify)

[Answer]: A

## Q2. If those gains pull against each other, which one wins?

Some design choices trade one gain for another, for example a richer page shape costs the assistant more tokens to write. Knowing the priority now settles those trade-offs later.

A. Fewer tokens for the assistant to read and write a day
B. A page that is pleasant and quick for you to read during the day
C. Working the same way from any assistant
D. No priority: weigh them case by case
E. Not yet defined
X. Other (please specify)

[Answer]: B

## Q3. Who, besides you, has a stake in this first version?

The repositories are public, and you mention other people later. This decides whose needs the first version must already respect.

A. Only me for this version
B. Me, plus people who read, run or fork the public repositories
C. Me, plus a few people I will ask to try it
D. Not identified
X. Other (please specify)

[Answer]: A

## Q4. Who decides scope and priority, and who influences it?

A. I decide alone
B. I decide; outside people influence it through issues or pull requests
C. Not yet defined
X. Other (please specify)

[Answer]: A

## Q5. Is there any communication or reporting this work must produce?

For example, a public README or changelog written as part of the work, or nothing beyond the records kept in this workspace.

A. None beyond the records in this workspace
B. A public README in each repository explaining what Dayboard is and how to run it
C. A README plus a changelog per release
D. Not applicable
X. Other (please specify)

[Answer]: A

## Q6. Which assistant must the first version be proven with?

The assistant is the second actor on every page. This decides which assistant the first version is checked against, not which ones it may support.

A. One assistant I use daily; others are welcome but not checked
B. Two different assistants, both checked
C. Any MCP-capable assistant, without a specific one being checked
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q7. Beyond the items you already listed as out of scope, what should this first version not do? (select all that apply)

You already excluded users and authentication, cloud deployment, custom block kinds, a plugin layer, notifications, and reading your tools directly. These are further candidates.

A. Adding or editing item text on the page (you change states; the assistant writes the text)
B. Searching or browsing across many past days (beyond opening one day)
C. Export, import or backup tooling
D. Nothing further: the listed exclusions are the whole boundary
E. Not yet defined
X. Other (please specify)

[Answer]: D

## Consolidated Summary Confirmation

- Success: you use Dayboard every working day for two weeks and never fall back to the chat artifact (Q1).
- When gains conflict, a page that is pleasant and quick for you to read during the day wins (Q2).
- Only you have a stake in this first version (Q3).
- You decide scope and priority alone (Q4).
- No communication or reporting beyond the records in this workspace (Q5).
- The first version is proven with one assistant you use daily; others are welcome but not checked (Q6).
- The out-of-scope items in your description are the whole boundary; nothing further is excluded (Q7).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
