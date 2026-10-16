# Discovered Rules — Dayboard

Hard constraints stated in the approved records and in the Practices Discovery interview. When the practices are affirmed, every line under Mandated and Forbidden is copied word for word into `aidlc/spaces/dayboard/memory/project.md`, so those two sections contain rule lines only. The source of each rule is listed under Sources.

## Mandated

- ALWAYS keep day data out of git: the folder or file that holds page content is ignored by git in every repository before the first day is written.
- ALWAYS have the Dayboard server answer only requests from this machine, its own web page and the assistant, and refuse requests from any other web origin, for as long as Dayboard has no sign-in.
- ALWAYS use only made-up content in examples, fixtures, tests, screenshots and documentation.
- ALWAYS give every screen a type scale, spacing, visual hierarchy and considered states, in light and dark, usable at phone width.
- ALWAYS keep the web app's user-facing text in one file per language, read through a single helper.

## Forbidden

- NEVER commit page content or a day data file to any Dayboard repository.
- NEVER commit secrets, credentials, tokens or keys to any Dayboard repository.
- NEVER put anything company-specific in a Dayboard repository: no internal names, and no references to an employer's tools or processes.
- NEVER write user-facing copy inside components or other code.

## Sources

- Keeping day data out of git (Mandated 1, Forbidden 1): [desc]; `ideation/feasibility/constraint-register.md` SC-01; `ideation/feasibility/raid-log.md` R2.
- The server answering only this machine, its page and the assistant (Mandated 2): `ideation/feasibility/constraint-register.md` SC-01, SC-02, SC-03; `ideation/feasibility/raid-log.md` R1; `ideation/scope-definition/scope-document.md`, Privacy.
- Made-up content only (Mandated 3): [desc]; `ideation/feasibility/constraint-register.md` SC-04; `ideation/feasibility/raid-log.md` R2.
- Visual bar on every screen (Mandated 4): [desc]; `ideation/feasibility/constraint-register.md` TC-08.
- User-facing text in one file per language, never in code (Mandated 5, Forbidden 4): `practices-discovery-questions.md` Q10, answer B, and the confirmed summary ("This becomes a project rule").
- No secrets and nothing company-specific (Forbidden 2, Forbidden 3): [desc], "What is fixed"; `ideation/feasibility/constraint-register.md` SC-04.
- The privacy guard in the first slice is already a rule in `aidlc/spaces/dayboard/memory/project.md` (Corrections), so it is not repeated here.

## Assumptions & Open Questions

None.
