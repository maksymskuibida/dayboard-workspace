# Wireframes — Dayboard, first version

Low-fidelity structure only: boxes, labels and behaviour, no visual style. The confirmed answers are in `rough-mockups-questions.md` (Q1–Q9). What the page must hold comes from the scope document (`ideation/scope-definition/scope-document.md`) and the intent backlog (`ideation/scope-definition/intent-backlog.md`). Who it is for, and how success is checked, come from the intent statement (`ideation/intent-capture/intent-statement.md`). All example content is made up.

## Information Architecture

- **Home** (`/`): redirects to the latest day on record. When no day exists yet, it shows the first-run placeholder (S4) (Q6).
- **Day** (`/<date>`, for example `/2026-10-16`): one page per day, with the date in the address (Q6).
  - **Header:** product name, the copy-prompt button, the theme switch, previous and next arrows, the date (which opens the calendar), and the day's title (Q5).
  - **Blocks:** only the blocks the assistant wrote, in its order. A kind can repeat, each block with its own title (Q2, Q8, Q9).
  - **Items:** text and optional links. Task-like kinds (schedule, open items, pull requests, waiting on others) have a three-way state switch (Q4), and their done items fold into an "N done" line (Q3). Learnings have Save and Discard instead (below).
  - **Notes for tomorrow:** shown like any block, with its own add-note field. A note has no state switch. It has a Discard button that dismisses it after a confirmation popup. When the day has no notes block, a standing "Add a note for tomorrow" field sits at the end of the day and creates the block on first use. When the block exists, the standing field isn't shown.
  - **Learnings:** the assistant can create a learnings block. Each learning has Save and Discard buttons. A saved learning keeps only Discard, and Discard asks for confirmation, as it does for notes. The assistant reads only learnings that were saved and not discarded. When the day has no learnings block, a standing "Add a learning" field sits at the end of the day and creates the block on first use.
- **Calendar:** opens from the date. Days without a page keep their numbers but are greyed out and skipped by the arrow keys (Q5).
- **Theme:** a three-way icon switch in the header: Light, Dark, Auto. Auto is the default and follows the system setting.

There is one layout at every width: a single column of stacked blocks (Q1). The tone is dense, with tight rows and small type steps. It is still a designed interface, not a raw list (Q7).

## S1. Day page, populated (wide window)

```
+----------------------------------------------------------------+
| Dayboard                    [ Copy prompt ]   ( Lt | Dk | A )  |
|                                                                |
| [<]  Fri 16 Oct 2026 v  [>]                                    |
| Ship the sync retry and clear the review queue                 |
+----------------------------------------------------------------+
| SCHEDULE  Today                                                |
|  09:30  Team standup                    ( Open |Done| Later )  |
|  13:00  Design review: calendar view    ( Open |Done| Later )  |
|  > 1 done                                                      |
+----------------------------------------------------------------+
| OPEN ITEMS  Sync service                                       |
|  Add retry with backoff to the sync job ( Open |Done| Later )  |
|    link: issue 118                                             |
|  Write the migration note               ( Open |Done| Later )  |
|  > 2 done                                                      |
+----------------------------------------------------------------+
| OPEN ITEMS  Docs                                               |
|  Update the install steps               ( Open |Done| Later )  |
+----------------------------------------------------------------+
| PULL REQUESTS  Waiting for review                              |
|  #42 Add retry to sync job              ( Open |Done| Later )  |
|    link: pull request 42                                       |
+----------------------------------------------------------------+
| WAITING ON OTHERS                                              |
|  Answer on the API limits from Sam    ( Open |Closed| Later )  |
+----------------------------------------------------------------+
| LEARNINGS                                                      |
|  Backoff needs jitter or retries line up    [ Save ][Discard]  |
|  Saved: The parser test is flaky on CI only         [Discard]  |
|  [ Add a learning                                   ] [ Add ]  |
+----------------------------------------------------------------+
| NOTES FOR TOMORROW                                             |
|  Start with the flaky test in the parser            [Discard]  |
|  [ Add a note                                       ] [ Add ]  |
+----------------------------------------------------------------+
```

<!-- Text fallback: A single column. The header has the product name, a Copy prompt button, a three-way Light, Dark, Auto theme switch, previous and next arrows around the date (which opens a calendar), and the day's title. Below it, blocks stack in the assistant's order: schedule, two open-items blocks with their own titles, pull requests, waiting on others (whose done label reads Closed), learnings with Save and Discard buttons per learning, and notes for tomorrow with an add-note field. Task items have a three-way switch, and done items fold into a "N done" line. -->

- **Block heading:** the kind label (small caps) plus the block's own title. Two blocks of one kind are told apart by their titles (Q9).
- **State switch (task kinds):** three segments, Open, the done state, and Later (delayed to tomorrow). The done segment's label fits the kind: "Done" for most kinds, "Closed" for waiting on others (Q4).
- **"N done" line:** it opens in place to show the done items, dimmed, each with its switch so it can be reopened (Q3).
- **Delayed items:** they stay in their block with the Later segment set. Nothing moves to the next day by itself (scope Q2).
- **Learnings:** each learning shows Save and Discard. A saved learning is marked "Saved" and keeps only its Discard button, so it can still be discarded but can't go back to unsaved. Discard opens the same confirmation popup as notes ("Discard this learning?" with Discard and Cancel). Confirming removes the learning for you and for the assistant: the assistant reads only learnings that were saved and not discarded. The add field adds a learning to this block.
- **Notes:** each note has a Discard button. It opens a small confirmation popup ("Discard this note?" with Discard and Cancel). Confirming removes the note. Notes have no state switch.
- **One way to discard:** notes and learnings both use the confirmation popup. There is no undo.
- **Links:** shown under the item text as short labels. They open in a new tab.
- **Accessibility:** h1 is the date, and h2 is each block (kind plus title). Landmarks are header and main. Keyboard entry starts at the Copy prompt button, then the theme switch, the arrows, the date, and the blocks in order. Each state switch is a radio group moved with the arrow keys. Save and Discard are buttons named after their learning.

## S1b. A day without notes or learnings blocks (end of the page)

```
+----------------------------------------------------------------+
| PULL REQUESTS  Waiting for review                              |
|  #42 Add retry to sync job              ( Open |Done| Later )  |
+----------------------------------------------------------------+
|  [ Add a note for tomorrow                          ] [ Add ]  |
|  [ Add a learning                                   ] [ Add ]  |
+----------------------------------------------------------------+
```

<!-- Text fallback: When the assistant wrote no notes-for-tomorrow block and no learnings block, two standing fields sit at the end of the day: Add a note for tomorrow, and Add a learning. Each one creates its block on first use. -->

- **Standing fields:** each appears only while its block is missing. The first entry creates the block at the end of the day, and from then on the block's own field is used.
- **Accessibility:** each field has a visible label. Enter adds the entry, an empty entry is not added, and a failed save keeps the typed text and shows "Couldn't save, try again".

## S2. Calendar open

```
+----------------------------------------------------------------+
| [<]  Fri 16 Oct 2026 ^  [>]                                    |
|      +--------------------------------------+                  |
|      |  [<]      October 2026      [>]      |                  |
|      |  Mo  Tu  We  Th  Fr  Sa  Su          |                  |
|      |              1   2  -3  -4           |                  |
|      |   5   6   7   8   9 -10 -11          |                  |
|      |  12  13  14  15 [16]-17 -18          |                  |
|      | -19 -20 -21 -22 -23 -24 -25          |                  |
|      |  key: -n = no page (greyed out)      |                  |
|      |  [ Go to latest day ]                |                  |
|      +--------------------------------------+                  |
+----------------------------------------------------------------+
```

<!-- Text fallback: Clicking the date opens a month calendar under it. Every date keeps its number. Dates without a page are greyed out (shown here with a minus sign) and can't be chosen, and the current day is bracketed. Month arrows move between months, and a Go to latest day button jumps to the latest day on record. -->

- **Greyed-out days:** they keep their numbers, are greyed out, and can't be chosen. Arrow keys skip over them (scope Q3).
- **Day arrows:** previous and next go to the nearest days that have a page. At the first day on record the previous arrow is disabled, with the accessible name "No earlier day". At the latest day the next arrow is disabled, with "No later day" (Q5).
- **Phone width:** the calendar opens as a full-width panel under the header, with the same grid and a close button.
- **Accessibility:** the calendar is a dialog with a grid, labelled by the month. Arrow keys move between days that have a page, Enter opens a day, and Escape closes it and returns focus to the date.

## S3. Day page at phone width

```
+------------------------------------+
| Dayboard          [cp] ( Lt|Dk|A ) |
| [<] Fri 16 Oct 2026 v [>]          |
| Ship the sync retry and clear the  |
| review queue                       |
+------------------------------------+
| SCHEDULE  Today                    |
|  09:30  Team standup               |
|  ( Open | Done | Later )           |
|  13:00  Design review: calendar    |
|         view                       |
|  ( Open | Done | Later )           |
|  > 1 done                          |
+------------------------------------+
| OPEN ITEMS  Sync service           |
|  Add retry with backoff to the     |
|  sync job                          |
|  ( Open | Done | Later )           |
+------------------------------------+
```

<!-- Text fallback: At phone width the same single column holds. Long item text wraps, and the state switch drops onto its own line under the item. In the header, Copy prompt becomes an icon button, so the name, that icon and the theme switch fit on one line, with the date controls on the next. -->

- **Header at phone width:** Copy prompt becomes an icon button (`[cp]` in the drawing) with the accessible name "Copy prompt". The name, that icon and the three theme icons fit on one line, each at least 44 px.
- **Layout:** the page has the same structure. The switch moves under the item text so it never squeezes the text, and every touch target is at least 44 px (Q1).
- **Accessibility:** the heading order and landmarks are as in S1. Nothing scrolls sideways.

## S4. First run: no days yet (home page)

```
+----------------------------------------------------------------+
| Dayboard                                      ( Lt | Dk | A )  |
+----------------------------------------------------------------+
|                                                                |
|   No days yet                                                  |
|   Your assistant writes your first page. Copy the prompt and   |
|   paste it into your assistant to prepare today.               |
|                                                                |
|   [ Copy prompt ]                                              |
|                                                                |
+----------------------------------------------------------------+
```

<!-- Text fallback: When no day exists, the home page shows a placeholder titled No days yet, which explains that the assistant writes the first page, with a Copy prompt button and the theme switch in the header. -->

- **When it shows:** only on the home page, and only when no day exists. Otherwise the home page goes straight to the latest day on record (Q6).
- **Accessibility:** h1 is "No days yet". Landmarks are header and main. Keyboard entry starts at the Copy prompt button.

## S5. A date with no page (address opened directly)

```
+----------------------------------------------------------------+
| Dayboard                    [ Copy prompt ]   ( Lt | Dk | A )  |
| [<]  Sat 17 Oct 2026 v  [>]                                    |
+----------------------------------------------------------------+
|   No page for this day                                         |
|   [ Go to latest day ]                                         |
+----------------------------------------------------------------+
```

<!-- Text fallback: Opening the address of a date with no page shows a short No page for this day message, with the date controls still in place and a Go to latest day button. The arrows go to the nearest days that have a page. -->

- **Behaviour:** a typed or bookmarked address shows this message, not an error. The arrows go to the nearest earlier and later days that have a page, and are disabled when there is none.
- **Accessibility:** h1 is the date. The message is plain text, and focus starts at the Go to latest day button.

## S6. States

```
+----------------------------------------------------------------+
| Loading:   header in place; each block a grey placeholder row  |
| Saving:    the switch shows the new state at once; reverts     |
|            with "Couldn't save, try again" if the save fails   |
| Offline:   "Can't reach Dayboard on this computer. Is the      |
|            backend running?" with [ Try again ]                |
| Day failed: "Couldn't load this day." with [ Try again ];      |
|            the header and date controls stay usable            |
| Partial:   a block that can't be shown reads "This block       |
|            can't be shown"; the other blocks show as normal;   |
|            a link that isn't a link shows as plain text        |
| Empty block: a block with no items shows "Nothing here"        |
| Copied:    the Copy prompt button reads "Copied" for 2 s       |
| Copy refused: "Couldn't copy. Select the prompt below." with   |
|            the prompt shown as selectable text                 |
+----------------------------------------------------------------+
```

<!-- Text fallback: Loading shows the header with grey placeholder rows. A state change shows immediately and reverts with a message if saving fails. If the backend can't be reached, a message asks whether it is running and offers Try again. If one day fails to load while the backend is up, Couldn't load this day appears with Try again. A block that can't be shown says so while the rest of the day shows normally, and a broken link shows as plain text. A block with no items says Nothing here. After copying, the button reads Copied for two seconds. If the browser refuses the copy, the prompt is shown as selectable text. -->

- **Accessibility:** save failures, "Copied" and "Couldn't copy" are announced politely through a live region. Errors are shown as text with an icon, never by colour alone.

## Assumptions & Open Questions

- **Done-state labels still to decide** in Refined Mockups, for schedule, open items and pull requests. Waiting on others uses "Closed" (Q4). Learnings use Save and Discard, and notes for tomorrow use Discard with a confirmation, instead of the switch.
- **A learning you add yourself** gets the same Save and Discard buttons as one the assistant writes, so the assistant reads every saved learning the same way.
- **The copy prompt always targets today**, even when you are looking at an earlier day.
- **Adding notes or learnings on an earlier day** works the same as on today.
