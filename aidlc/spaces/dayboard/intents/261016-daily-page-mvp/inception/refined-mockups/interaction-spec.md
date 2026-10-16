# Interaction Specification — Dayboard, first version

Component specifications for the screens in `mockups.md`, in the shape of `.claude/knowledge/aidlc-design-agent/component-spec-template.md`. Breakpoints: phone below 768 px, desktop from 768 px (the page is one column at every width, so there is no separate tablet layout). Every visible text comes from the English language file through the single text helper (FR9.1); a missing key leaves the element empty (FR9.3). Token names refer to `design-system-mapping.md`.

## Header

| Field | Value |
|---|---|
| Component | Header |
| Description | Pinned glass bar with the product name, Copy prompt, theme switch, day navigation and the day's title |
| Category | navigation |

| State | Description | Trigger |
|---|---|---|
| default | Full header on glass level 3 | page load |
| scrolled | Stays pinned; the edge highlight strengthens | page scrolled |
| no-day | Date controls hidden (first run, S4) | no day on record |

| Breakpoint | Behaviour |
|---|---|
| phone | Name, Copy prompt icon button and theme switch on one line; date controls and title below |
| desktop | Name left, Copy prompt and theme switch right; date controls and title below |

| Requirement | Implementation |
|---|---|
| ARIA role | `header` landmark (banner); the date is the page's `h1` |
| Keyboard | Tab order: Copy prompt, theme switch, previous arrow, date, next arrow |
| Label | Copy prompt icon button named "Copy prompt"; arrows named "Previous day" / "Next day", or "No earlier day" / "No later day" when disabled |
| Contrast | 4.5:1 text on glass level 3 in both themes, solid fallback included |
| Screen reader | The title is read after the date |
| Focus management | Focus never moves on live updates (FR3.13) |

## State Switch (Open / done / Later)

| Field | Value |
|---|---|
| Component | StateSwitch |
| Description | Three-way control for open items, pull requests and waiting on others (FR4.1) |
| Category | input |

| State | Description | Trigger |
|---|---|---|
| default | Current state selected | load |
| hover | Segment tint rises | mouseover |
| focus | 2 px focus ring on the group | Tab |
| saving | New segment selected at once; no spinner | change |
| error | Returns to stored state; toast "Couldn't save, try again" | save failed |
| stale | Returns; toast "This changed, try again"; page refreshes | page hash refused (FR4.9) |

| Prop | Type | Required | Default | Description |
|---|---|---|---|---|
| value | "open" \| "done" \| "later" | yes | — | Stored state |
| kind | "open_items" \| "pull_requests" \| "waiting" | yes | — | Picks the done label: Done, Merged, Closed |
| itemId | string | yes | — | Stable item identifier (FR1.12) |
| onChange | (value) => void | yes | — | Sends the change with the page hash |

| Breakpoint | Behaviour |
|---|---|
| phone | Full width under the item text, 44 px tall segments |
| desktop | At the row's end, 28 px tall segments |

| Requirement | Implementation |
|---|---|
| ARIA role | `radiogroup` with three `radio` segments, named after the item |
| Keyboard | Arrow keys move and select; Tab leaves the group |
| Label | `aria-label` "State of: <item title>" |
| Contrast | Selected segment 3:1 against its neighbours, plus a check mark |
| Screen reader | Errors announced through the polite live region |
| Focus management | Focus stays on the switch after a revert |

## Meeting Answer

| Field | Value |
|---|---|
| Component | MeetingAnswer |
| Description | Yes / No / Maybe control for a schedule item (FR1.13, FR4.1) |
| Category | input |

| State | Description | Trigger |
|---|---|---|
| unanswered | No segment selected; a "No answer" chip after the title | answer is none |
| default | Answer selected | load |
| past | Row text in `color.text.muted` at full opacity with a "Past" chip; the control stays usable | the meeting's own end has passed: a timed meeting's end (the next day for an overnight one), an all-day meeting's day (FR4.10) |
| saving / error / stale | As the state switch | change |

| Prop | Type | Required | Default | Description |
|---|---|---|---|---|
| value | "yes" \| "no" \| "maybe" \| null | yes | null | Stored answer |
| allDay | boolean | yes | false | All-day meeting: shows "All day", has no times (FR1.13) |
| start, end | "HH:MM" \| null | when timed | null | Local times; an end earlier than the start is on the next day and shows as "→ HH:MM" |
| itemId | string | yes | — | Stable identifier |

| Breakpoint | Behaviour |
|---|---|
| phone | Under the title (and the Past chip), 44 px segments |
| desktop | At the row's end |

| Requirement | Implementation |
|---|---|
| ARIA role | `radiogroup`, three `radio` segments; none checked when unanswered |
| Keyboard | Arrow keys; when unanswered, the first arrow press selects Yes |
| Label | "Your answer for: <meeting title>, <start> to <end>", or "…, all day" for an all-day meeting |
| Screen reader | "Past" is part of the row's accessible name, not only the dimming |
| Focus management | The past mark appearing never moves focus |

The past check runs on the page at least every 5 seconds and on every live update, using this computer's clock as it reads now (FR1.9, FR4.10), so the mark appears within 5 seconds of the end, also after a clock or daylight-saving change. A local time that doesn't exist or occurs twice on a daylight-saving night counts as its first valid instant (FR1.13).

Order in a schedule block: all-day meetings first, then timed meetings by start time, ties in stored order; the stored order of the block otherwise doesn't change the display (FR4.10).

## Learning Actions and Note Discard

| State | Description | Trigger |
|---|---|---|
| unsaved | [Save] [Discard] | new learning |
| saved | "✓ Saved" mark and [Discard] | Save pressed, by you or the assistant |
| confirming | Confirmation dialog (S7) | Discard pressed |

- Buttons are named after the item: "Save learning: <text start>", "Discard note: <text start>".
- A saved learning never goes back to unsaved (FR1.7).

## Add Field (note or learning)

| State | Description | Trigger |
|---|---|---|
| default | Labelled input with [Add] | load |
| standing | Shown at the end of the day while the block is missing; creates it on first use (FR4.6) | no block |
| error | Text kept; toast "Couldn't save, try again" | save failed |
| stale | Text kept; toast "This changed, try again" | page hash refused |

- Enter adds; an empty or whitespace-only entry does nothing (FR4.7). The label is visible above the input, never a placeholder only.

## More Menu ("…") and Confirmation

| Field | Value |
|---|---|
| Component | MoreMenu |
| Description | Per-item and per-block menu holding Remove (Q5, FR4.8) |
| Category | navigation |

| State | Description | Trigger |
|---|---|---|
| closed | "…" button, visible at all times | load |
| open | Floating glass menu under the button | click, Enter, Space |
| confirming | Dialog naming the item or block and its item count | Remove chosen |

| Requirement | Implementation |
|---|---|
| ARIA role | Button with `aria-haspopup="menu"`; `menu` with `menuitem`s; the confirmation is a modal `dialog` |
| Keyboard | Arrow keys in the menu, Escape closes and returns focus to "…"; in the dialog focus starts on Cancel and is trapped |
| Label | "More for: <item or block title>" |
| Focus management | After Remove, focus moves to the next item, or to the block heading |

## Day Navigation and Calendar

As in the rough wireframes (FR5.1 to FR5.4): previous and next arrows go to the nearest days with a page; the calendar is a modal `dialog` with a `grid`, labelled by the month; days without a page are dimmed with a dot mark and are skipped by the arrow keys; Escape closes and returns focus to the date. A live update that adds or removes a day updates the grid without closing it (FR3.13).

## Markdown Text

| Element | Rendering |
|---|---|
| Headings | Rendered at their level, as written (Q4); the MCP server tells the assistant to keep them to "###" or smaller (FR2.12) |
| Inline code, code blocks | Monospace face on a faint glass chip / panel; code blocks scroll sideways inside the item only |
| Links | Only http, https and mailto clickable; open in a new tab with `rel="noopener noreferrer"` and an ↗ mark (FR3.7, FR3.9) |
| Pictures | Load from their address (FR3.8), capped at the item width and 240 px high, with their description as alt text |
| Task lists | Checkboxes are real; ticking one changes only that marker and saves straight away (FR3.12); named "Check: <task text>" |
| Raw HTML | Shown as text or dropped, never run (FR3.7) |

## Toast

| Field | Value |
|---|---|
| Component | Toast |
| Description | Short floating message for live updates and save results (Q3, FR3.16). A live update not made in this tab, removals included, raises "This day was updated"; changes made in this tab raise none |
| Category | feedback |

| State | Description | Trigger |
|---|---|---|
| shown | Floating glass pill | message raised |
| merged | A further live update within 5 s of the last keeps the one toast and restarts its 5 s | update during a shown "This day was updated" toast |
| dismissing | Fades out (instant with reduced motion) | 5 s elapsed or × pressed |

| Breakpoint | Behaviour |
|---|---|
| phone | Bottom centre, above the safe area |
| desktop | Top right, under the header |

| Requirement | Implementation |
|---|---|
| ARIA role | Text mirrored into one polite `status` live region |
| Keyboard | × button reachable; the toast never takes focus |
| Timing | 5 s, paused while hovered or focused |

## Change Highlight

- A changed or added item, from a change not made in this tab, gets the accent tint behind its row, fading from 24% to 0% over 2 s (`motion.highlight`). With reduced motion asked for, the tint shows for 2 s and disappears without fading (FR3.16).
- A removed item has nothing left to highlight; the toast still shows. If the removed item or block held the field being typed in, or an open confirmation, the typed text and the confirmation go with it, with no message about the dropped text (FR3.13).
- Changes made in this tab get no highlight and no toast.

## Sources

- `mockups.md`: screens S1 to S7.
- `inception/requirements-analysis/requirements.md`: FR1.7, FR1.12, FR1.13 (FR1.14 withdrawn), FR2.12, FR3.6 to FR3.16, FR4.1 to FR4.10, FR5, FR6, FR7, FR9.
- `ideation/rough-mockups/wireframes.md`: keyboard order, accessibility notes.
- `refined-mockups-questions.md`: Q1 to Q9.
- `.claude/knowledge/aidlc-design-agent/component-spec-template.md`: specification format.

## Assumptions & Open Questions

- [assumption] Toasts pause while hovered or focused, so a reader is never cut off.
