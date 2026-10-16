# Refined Mockups — Dayboard, first version

Annotated mid-to-high fidelity wireframes, refined from the rough wireframes (`ideation/rough-mockups/wireframes.md`) and the user flow (`ideation/rough-mockups/user-flow.md`) with the decisions in `refined-mockups-questions.md` (Q1 to Q9), the approved stories (`inception/user-stories/stories.md`) and requirements (`inception/requirements-analysis/requirements.md`). They are written the standard way, as text, by your choice (Q9). Components are specified in `interaction-spec.md`, tokens in `design-system-mapping.md`, and checks in `accessibility-checklist.md`. All content is made up.

## Visual Direction

**Liquid glass, dense** (Q2, Q8). The page background is a soft, slowly shifting gradient field. Everything you read sits on glass: translucent surfaces with a background blur, a thin bright edge on top, and a faint shadow below. There are three glass levels (`design-system-mapping.md`, Surfaces):

- **Header glass** (level 3, strongest blur) stays pinned at the top while the page scrolls.
- **Block glass** (level 2): one panel per block, with 12 px corners.
- **Floating glass** (level 4): the calendar, the "…" menus, confirmations and toasts.

Rows stay tight: 32 px minimum row height on desktop, 44 px touch targets at phone width. Type steps are small: body 14 px, block heading 12 px small caps, date 22 px. When the system asks for reduced transparency, every glass level turns into a solid surface of the same colour, and the layout doesn't change. Light and dark are both glass; Auto follows the system (FR7.1).

Legend for the drawings: `░` marks a glass surface, `( a | b | c )` a segmented control, `[ … ]` a button, `‹chip›` a status chip drawn by the page ("Past", "No answer"), `~` dimmed text. Items carry no assistant-written labels in this version (FR1.14 withdrawn).

## S1. Day page, populated (wide window)

```
 page background: gradient field
┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐  header glass, pinned
░ Dayboard                              [⧉ Copy prompt]  ( ☀ | ☾ | A ) ░
░ [‹]  Fri 16 Oct 2026 ▾  [›]                                          ░
░ Ship the sync retry and clear the review queue                       ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ SCHEDULE  Today ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐  block glass
░  All day      Quarterly planning day                ( Yes|No|Maybe ) ░
░ ~09:30–09:45  Team standup  ‹Past›                  ( Yes|No|Maybe ) ░
░  11:00–11:30  Design review: calendar view          ( Yes|No|Maybe ) ░
░  22:00 → 01:00  Release window          [no answer] ( Yes|No|Maybe ) ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ OPEN ITEMS  Sync service ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐
░  Add retry with backoff to the sync job        ( Open|Done|Later ) […]
░    issue 118 ↗                                                       ░
░  Write the migration note                      ( Open|Done|Later ) […]
░  › 2 done                                                            ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ PULL REQUESTS  Waiting for review ░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐
░  #42 Add retry to `sync-job`                  ( Open|Merged|Later ) […]
░    pull request 42 ↗                                                 ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ WAITING ON OTHERS ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐
░  Answer on the API limits                      ( Open|Closed|Later ) […]
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ LEARNINGS ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐
░  Backoff needs jitter or retries line up        [Save] [Discard]     ░
░  ✓ Saved  The parser test is flaky on CI only          [Discard]     ░
░  [ Add a learning                                     ] [Add]        ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘

┌░ NOTES FOR TOMORROW ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ […] ░┐
░  Start with the flaky test in the parser               [Discard]     ░
░  [ Add a note                                         ] [Add]        ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
```

<!-- Text fallback: A pinned glass header holds the product name, a Copy prompt button, the Light, Dark, Auto theme switch, the previous and next arrows around the date (which opens the calendar), and the day's title. Below it, each block is its own glass panel with a "…" menu in its heading. The schedule lists all-day meetings first, reading All day, then timed meetings by start time, each with a time range, title and a Yes, No, Maybe control; a meeting running past midnight shows its end as an arrow and the next day's time; a meeting whose own end has passed is dimmed and marked Past. Open items, pull requests and waiting on others have a three-way state switch whose middle segment reads Done, Merged or Closed, plus a "…" menu per item; done items fold into an "N done" line. Learnings have Save and Discard, saved ones show a Saved mark; notes have Discard. Both have an add field. -->

**What changed from the rough wireframes**

- **Meetings (Q7, FR1.13, FR4.1):** each schedule row is the time, the title, then a Yes / No / Maybe control in place of the Open / Done / Later switch. With no answer yet, no segment is selected and a "No answer" chip shows. The time is set in the monospace face so ranges line up.
- **All-day and overnight (FR1.13, FR4.10):** an all-day meeting reads "All day" in place of times. A meeting whose end is earlier than its start ends the next day and shows its end as "→ 01:00". All-day meetings come first, then timed ones by start time, ties in stored order; reordering the block through the assistant doesn't change this.
- **Past meetings (Q7, FR4.10, NFR7):** once a meeting's own end has passed, its text turns to `color.text.muted` at full opacity (no transparency, so it keeps 4.5:1 on its glass) and it gets a "Past" chip. A timed meeting turns Past when its end passes (for an overnight one, on the next day), an all-day one when its day ends, also on earlier days. The mark follows this computer's clock as it reads now and appears within 5 seconds, without a reload. The answer control stays usable.
- **No labels (FR1.14 withdrawn):** items show no assistant-written labels in this version; the only chips are the page's own "Past" and "No answer".
- **Done labels (Q1, FR4.1):** "Done" for open items, "Merged" for pull requests, "Closed" for waiting on others.
- **"…" menus (Q5, FR4.8):** one at the end of each item row and one in each block heading. The item menu holds Remove item. The block menu holds Remove block. Each opens a confirmation (S7).
- **Markdown (Q4, FR3.6, FR2.12):** item text renders as written; the MCP server tells the assistant to keep headings inside items to "###" or smaller, so the date and block headings stay on top. Inline code uses the monospace face on a faint glass chip, links open in a new tab with an ↗ mark, and pictures load inline, capped at the item width and 240 px high.
- **Folded done items:** "› 2 done" opens in place. Done rows show dimmed, each with its switch.

## S2. Calendar open

```
░ [‹]  Fri 16 Oct 2026 ▴  [›]
     ┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐  floating glass
     ░  [‹]      October 2026      [›]    ░
     ░  Mo  Tu  We  Th  Fr  Sa  Su        ░
     ░              1   2  ~3  ~4         ░
     ░   5   6   7   8   9 ~10 ~11        ░
     ░  12  13  14  15 (16)~17 ~18        ░
     ░ ~19 ~20 ~21 ~22 ~23 ~24 ~25        ░
     ░  [ Go to latest day ]        [×]   ░
     └░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
```

<!-- Text fallback: The calendar is a floating glass panel under the date. Month arrows change the month; dates without a page are dimmed and can't be chosen; the open day is ringed; a Go to latest day button and a close button sit at the bottom. -->

- **Behaviour unchanged** from the rough wireframes (FR5.2 to FR5.4). Days without a page use the muted colour plus a strike-through dot below the number, so they're not told by colour alone.
- **Live updates (FR3.11):** a day the assistant adds while the calendar is open becomes selectable at once; the calendar stays open (FR3.13).
- **Phone width:** a full-width floating panel under the header, with the same grid.

## S3. Day page at phone width (360 px)

```
┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐
░ Dayboard        [⧉] ( ☀|☾|A )   ░
░ [‹] Fri 16 Oct 2026 ▾ [›]        ░
░ Ship the sync retry and clear    ░
░ the review queue                 ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
┌░ SCHEDULE  Today ░░░░░░░░░ […] ░┐
░ ~09:30–09:45  Team standup       ░
░ ‹Past›                           ░
░ ( Yes | No | Maybe )             ░
░  11:00–11:30  Design review:     ░
░  calendar view                   ░
░ ( Yes | No | Maybe )             ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
┌░ OPEN ITEMS  Sync service ░ […] ░┐
░  Add retry with backoff to the   ░
░  sync job                  […]   ░
░ ( Open | Done | Later )          ░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
```

<!-- Text fallback: At 360 px the same single column holds. The header shows the name, the Copy prompt icon button and the theme switch on one line, with the date controls and the title below. The Past chip sits under the title, and each control moves to its own line under the item text. Every touch target is at least 44 px. -->

- Controls drop under the text, the Past chip wraps under the title, nothing scrolls sideways (FR7.2), and Copy prompt becomes the ⧉ icon button named "Copy prompt" (FR7.3).

## S4. First run: no days yet

```
┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐
░ Dayboard                                    ( ☀ | ☾ | A )░
└░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
        ┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐
        ░  No days yet                             ░
        ░  Your assistant writes your first page.  ░
        ░  Copy the prompt and paste it into your  ░
        ░  assistant to prepare today.             ░
        ░            [⧉ Copy prompt]               ░
        └░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
```

<!-- Text fallback: With no days, the home page shows a centred glass card titled No days yet, explaining that the assistant writes the first page, with a Copy prompt button. -->

## S5. A date with no page

As in the rough wireframes: the header with the date controls, then a glass card reading "No page for this day" with **Go to latest day**. The same view shows for an address that isn't a date (FR3.3), and when the open day is removed by the assistant (FR3.14).

## S6. States

| State | What it looks like | Source |
|---|---|---|
| Loading | Header in place; each block a glass panel with two shimmering placeholder rows (static with reduced motion) | FR7.4 |
| Saving | The control shows the new value at once; on failure it returns and a toast reads "Couldn't save, try again" | FR4.2 |
| Changed underneath | The control returns, the current page loads in place, and a toast reads "This changed, try again" | FR4.9 |
| Updated elsewhere | A change not made in this tab (by the assistant or from another tab): each changed or added item glows with the accent tint fading over 2 s; a toast "This day was updated" shows for 5 s, top right on wide, bottom at phone width, announced politely. Changes within 5 s of each other share one toast; a removal shows the toast too, with nothing left to highlight. Changes made in this tab show neither | FR3.16, Q3 |
| Removed while typing | When the item or block being typed in, or named in an open confirmation, is removed elsewhere, it disappears with the typed text and the confirmation closes; there is no message about the dropped text, only the usual toast | FR3.13 |
| Offline | A glass banner under the header: "Can't reach Dayboard on this computer. Is the backend running?" with [Try again]; reconnects on its own | FR7.5, FR3.15 |
| Day failed | A glass card: "Couldn't load this day." with [Try again]; header and date controls stay usable | FR7.5 |
| Partial | A block that can't be shown reads "This block can't be shown"; a link that isn't one shows as plain text | FR7.6, FR3.7 |
| Empty block | "Nothing here" in the muted colour | FR3.4 |
| Copied | The Copy prompt button reads "Copied" for 2 s | FR6.4 |
| Copy refused | A floating glass panel: "Couldn't copy. Select the prompt below." with the prompt as selectable text | FR6.4 |

## S7. Confirmations

```
        ┌░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┐  floating glass, dimmed backdrop
        ░  Remove "Sync service"?              ░
        ░  The block and its 4 items go for    ░
        ░  good. There is no undo.             ░
        ░             [Cancel]  [Remove]       ░
        └░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░┘
```

<!-- Text fallback: A floating glass dialog over a dimmed page names what is removed and how many items go with it, says there is no undo, and offers Cancel and Remove. -->

- **One dialog pattern** for removing an item or a block (FR4.8) and for discarding a note or a learning (FR4.3, FR4.4). It names the thing, and the destructive button uses the negative tone. Focus starts on Cancel.

## Story Coverage

| Story | Screens |
|---|---|
| US1.3 Copy the prompt | S1 header, S4, S6 Copied / Copy refused |
| US2.1 Read the day | S1, S3, S5, S6 |
| US2.2 See changes without reloading | S6 Updated elsewhere, Removed while typing, Offline; S2 live days |
| US3.1 Track item states | S1 switches and answers, S7, S6 Saving / Changed underneath |
| US3.2 Tick checkboxes in text | S1 Markdown items (`interaction-spec.md`, Markdown text) |
| US4.1 Notes and learnings | S1 learnings and notes blocks, S7 |
| US5.1 Open an earlier day | S1 arrows, S2 |
| US1.1, US1.2, US3.3, US4.2 | The assistant's stories have no screen of their own; their results show in S1 |

## Sources

- `ideation/rough-mockups/wireframes.md`, `ideation/rough-mockups/user-flow.md`: layout, screens S1 to S6, states.
- `inception/requirements-analysis/requirements.md`: FR1 to FR9, NFR7, NFR8.
- `inception/user-stories/stories.md`: the stories each screen serves.
- `inception/practices-discovery/team-practices.md`: page text in one file per language.
- `refined-mockups-questions.md`: Q1 to Q9 and the confirmed summary.

## Assumptions & Open Questions

- [assumption] The "No answer" chip on an unanswered meeting is shown so an empty control isn't mistaken for a broken one.
- Headings written inside item text keep their level (Q4); the MCP server's tool description keeps them to "###" or smaller (FR2.12), so the page's heading order holds as long as the assistant follows it.
