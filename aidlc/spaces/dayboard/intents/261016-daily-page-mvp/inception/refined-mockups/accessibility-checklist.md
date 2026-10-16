# Accessibility Checklist — Dayboard, first version

Target: WCAG 2.1 level AA (requirements NFR7). Each row is checked by the automated scan named in AC2.1.16, by the component tests (team practices, Testing Posture), or by hand at Build and Test. Screens refer to `mockups.md`.

## Perceivable

| Check | Where | How it is met | Checked by |
|---|---|---|---|
| Text contrast at least 4.5:1, large text 3:1 | All screens, both themes, glass and solid surfaces, past meeting rows included | Tokens in `design-system-mapping.md`; glass fill raised where needed; past rows use `text.muted` at full opacity, never faded (FR4.10) | Scan + hand check on S1 light and dark, with a past row |
| Controls and focus ring at least 3:1 | Switches, answers, status chips, focus ring | `accent` and tone tokens | Scan |
| Nothing told by colour alone | Past meetings, meeting answers, unavailable calendar days, errors | "Past" chip, answer words on the segments, dot mark on unavailable days, toast text with icon | Component tests |
| Pictures have text alternatives | Markdown pictures | Description becomes alt text; empty description gives `alt=""` | Component test |
| Reduced transparency | All glass | Solid surfaces under `prefers-reduced-transparency` | Hand check |
| Content reflows at 360 px and 400% zoom | S1, S2, S3 | Single column, controls wrap under text | Hand check |

## Operable

| Check | Where | How it is met | Checked by |
|---|---|---|---|
| Everything works by keyboard | All controls | Tab order as in `interaction-spec.md`, Header | Component tests |
| Visible focus | All controls | 2 px `accent` ring, never removed | Scan |
| Radio groups move with arrow keys | State switch, meeting answer, theme switch | `radiogroup` pattern | Component tests (AC3.1.6) |
| Dialogs trap focus and close with Escape | Calendar, confirmations, menus | Focus returns to the opener | Component tests (AC5.1.4) |
| Touch targets at least 44 px | Phone width | `size.target` | Scan at 360 px |
| No timing traps | Toasts | 5 s, paused on hover or focus, never take focus | Component test |
| Motion can be turned off | Highlight, toasts, shimmer | `prefers-reduced-motion` sets durations to 0 | Component test |
| Live updates never move focus or close an open panel | All screens | FR3.13; the one exception is a removal of the item or block in use, which drops the typed text and closes its confirmation (AC2.2.8) | Component tests (AC2.2.2, AC2.2.8) |
| The Past mark appears without moving focus | S1 schedule | Checked every 5 s and on each live update; never takes focus | Component test (AC2.1.17) |

## Understandable

| Check | Where | How it is met | Checked by |
|---|---|---|---|
| Page language declared | Document | `lang` from the language file in use | Scan |
| Inputs have visible labels | Add a note, Add a learning | Label above the field | Scan |
| Errors say what happened and what to do | Save failures, changed underneath, offline | "Couldn't save, try again", "This changed, try again", offline banner | Component tests |
| Destructive actions confirmed | Remove item or block, discard note or learning | One dialog that names the thing; focus starts on Cancel | Component tests |

## Robust

| Check | Where | How it is met | Checked by |
|---|---|---|---|
| Landmarks | All screens | `header`, `main`; dialogs as `dialog` | Scan |
| Heading order | S1 | Date `h1`, blocks `h2`. Headings inside items keep their level as written (Q4); the MCP server's tool description keeps them to "###" or smaller (FR2.12) | Scan; contract test on the tool description (AC1.1.15) |
| Status messages announced | Toasts ("This day was updated", merged within 5 s), save results | One polite `status` live region | Component test (AC2.2.6) |
| Names on icon buttons | Copy prompt (phone), "…", arrows | `aria-label` from the language file | Scan |

## Sources

- `inception/requirements-analysis/requirements.md`: NFR7, FR2.12, FR3.13, FR3.16, FR4.10, FR5.3.
- `inception/user-stories/stories.md`: AC1.1.15, AC2.1.16, AC2.1.17, AC2.2.2, AC2.2.6, AC2.2.8, AC3.1.6, AC5.1.4.
- `ideation/rough-mockups/wireframes.md`: landmarks, heading order, keyboard order.
- `mockups.md`, `interaction-spec.md`, `design-system-mapping.md`.
- `.claude/knowledge/aidlc-design-agent/accessibility-wcag.md`.

## Assumptions & Open Questions

- [assumption] The assistant follows the MCP server's heading guidance (FR2.12); a "#" heading it writes anyway still renders as written and the scan may report it on that page.
- [assumption] `prefers-reduced-transparency` support varies by browser; where it is missing, a solid fallback also applies when background blur is unsupported.
