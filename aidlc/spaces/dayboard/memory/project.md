# Project overrides

## Corrections
- Dayboard runs only on the person's own machine with no cloud account in this phase: skip questions about AWS services, cloud accounts and budget until a hosting decision is made. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:feasibility:d001b79d7b6be65182e3e38000940160b2e96757468dde9a61ed53d79ec912c1 -->
- Dayboard pages hold sensitive content from the first real day: plan the privacy guard (server answers only this machine, its own page and the assistant; day data never committed) into the first slice, not a later one. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:scope-definition:1aa535e999deb16ae1b461de2a251457d84c81b50f579f4efe9501774fff3ef3 -->

## Way of Working
- All three Dayboard repositories follow the workspace's pull-request habit: changes reach `main` through GitHub pull requests that are squash-merged on GitHub; every Unit lands through pull requests (merge strategy: `pr`), never by the workflow merging into `main`. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:practices-discovery:e4e9085b7919a04992f601fc2df0861ff9156d84001410d6e2804e4bc6b1a068 -->
- Hard rules (Mandated and Forbidden) hold only constraints that stay true beyond the current version; limits of this version (one person, no sign-in, local only) stay out of them, and a rule that depends on one says so, as in "for as long as Dayboard has no sign-in". (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:practices-discovery:bcccc874336d0c845f914bf7eb80645d52e22bb5b0ff2e6a8a5ead3a2817be25 -->

- Notes kept at the end of a stage go into the project.md section they belong to (Way of Working, Testing Posture, Code Style, Deployment, Decided, and so on), never under Corrections; Corrections takes an entry only when the person explicitly approves it there. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:requirements-analysis:97d9f1c804afa70ffd1e6b67dc67e0cf20700e749ac268b909736be736c7b981 -->

## Deployment
- Nothing is deployed in this phase: Dayboard runs only on its users' own machines, so merged to `main` counts as released. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:practices-discovery:df2a94b1b5e63ac350682d1f795f0bbf79066a6cd51b3e1c527927b81aca99a9 -->

## Code Style
- Python uses Ruff for both linting and formatting instead of Black: one tool fits the few-dependencies constraint, and Ruff's formatter follows Black's style. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:practices-discovery:d1ba2a4aafe4e7aed8e377cfe8e256b55f949e722495b7637e46dae9a6e94f85 -->

## Testing Posture
- With no walking skeleton, the scripted end-to-end check doubles as the smoke check after pulling `main`. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:practices-discovery:1a6699b36e222a134d7f073a1f290efc321769b4e5220ee748a16f26b491d718 -->

## Forbidden

- NEVER commit page content or a day data file to any Dayboard repository. (affirmed 2026-10-16)

- NEVER commit secrets, credentials, tokens or keys to any Dayboard repository. (affirmed 2026-10-16)

- NEVER put anything company-specific in a Dayboard repository: no internal names, and no references to an employer's tools or processes. (affirmed 2026-10-16)

- NEVER write user-facing copy inside components or other code. (affirmed 2026-10-16)

## Mandated
- ALWAYS keep day data out of git: the folder or file that holds page content is ignored by git in every repository before the first day is written. (affirmed 2026-10-16)
- ALWAYS have the Dayboard server answer only requests from this machine, its own web page and the assistant, and refuse requests from any other web origin, for as long as Dayboard has no sign-in. (affirmed 2026-10-16)
- ALWAYS use only made-up content in examples, fixtures, tests, screenshots and documentation. (affirmed 2026-10-16)
- ALWAYS give every screen a type scale, spacing, visual hierarchy and considered states, in light and dark, usable at phone width. (affirmed 2026-10-16)
- ALWAYS keep the web app's user-facing text in one file per language, read through a single helper. (affirmed 2026-10-16)

## Decided
- Text on Dayboard pages is full Markdown: CommonMark plus the common GitHub extensions, rendered through a safe parser; raw HTML never runs, and only http, https and mailto links are clickable. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:requirements-analysis:d99c8286d9f6a607204c8a7dc6c8130318a21293c8d8e1e3a18bc378c4655ee3 -->
- No page exists for a future date, so the page opens on today or on the latest earlier day; the description's "today or later" reads as today or earlier. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:requirements-analysis:4693f657426d9ad78a90803f99c1463fcf1f6e9257ee0a9fab253d4f0f2a5bc0 -->
- The privacy guard includes a host-name check: the backend refuses a request whose host is not this computer's (localhost, 127.0.0.1 or ::1), so a web page can't reach it by pointing its own domain name at this computer. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:requirements-analysis:f83ef9ff231a5b8359cad8c5bb765be0f0eaa2619426077467d84648b0e84c0d -->
- Simultaneous changes are guarded by a page hash: every read of a page returns its hash, every change carries the hash of its last read and is refused when the page changed since. The open page updates live, with server-sent events preferred and polling acceptable. (learned 2026-10-16) <!-- cid:261016-daily-page-mvp:requirements-analysis:3d64577e722c4313efd6d89f51df5de26e29ff294d7d5e79993ec953119ae470 -->
