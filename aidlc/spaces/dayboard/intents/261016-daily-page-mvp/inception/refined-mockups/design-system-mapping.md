# Design System Mapping — Dayboard, first version

Dayboard has no existing design system or component library. This file defines its tokens and maps every component in `interaction-spec.md` to them. Build: plain CSS with design tokens as CSS custom properties, no UI library (Q8); dependencies stay few (team practices, Code Style). Look: liquid glass, dense (Q2). Light and dark are defined for every colour token; Auto follows the system (FR7.1).

## Colour Tokens

All text pairs below meet 4.5:1 against the surface they sit on, including the solid fallback (NFR7). Values are starting points for Construction; any change must keep the ratios.

| Token | Light | Dark | Used for |
|---|---|---|---|
| `color.bg.field` | gradient `#E9EEF6 → #F4EEF7 → #EAF4F1` | gradient `#0E1320 → #171226 → #0D1B1A` | Page background behind all glass |
| `color.text.primary` | `#121722` | `#EEF1F7` | Titles, item text |
| `color.text.secondary` | `#3C4556` | `#B9C1D0` | Times, block titles |
| `color.text.muted` | `#5B6475` | `#8E97A8` | Past meetings (at full opacity, never faded further, so dimmed text keeps 4.5:1 on its glass, NFR7), "Nothing here", days without a page |
| `color.accent` | `#2F5BEA` | `#7C9BFF` | Focus ring, selected segment, change highlight, links |
| `color.tone.neutral` | bg `#E3E7EE`, text `#2B3240` | bg `#2A3140`, text `#D5DBE6` | "No answer", "Past" |
| `color.tone.positive` | bg `#DDF3E6`, text `#14532D` | bg `#173A27`, text `#A7E8BF` | Yes |
| `color.tone.caution` | bg `#FBEFD5`, text `#6B4500` | bg `#3D2F12`, text `#F5D38A` | Maybe |
| `color.tone.negative` | bg `#FBE1E1`, text `#7F1D1D` | bg `#3F1C1F`, text `#F6B3B3` | No, Remove button |

## Surfaces (glass levels)

| Token | Fill (light / dark) | Blur | Edge | Shadow | Used for |
|---|---|---|---|---|---|
| `surface.glass.1` | white 40% / white 6% | 12 px | top 1 px white 60% / 12% | none | Inline chips for code |
| `surface.glass.2` | white 55% / white 8% | 20 px | top 1 px white 70% / 14% | `0 1px 2px` 6% | Block panels |
| `surface.glass.3` | white 65% / white 10% | 28 px | top 1 px white 80% / 16% | `0 4px 16px` 8% | Header |
| `surface.glass.4` | white 75% / white 14% | 32 px | 1 px white 85% / 18% all round | `0 12px 32px` 16% | Calendar, menus, dialogs, toasts |
| `surface.solid.*` | the same colours, fully opaque | none | the same | the same | Every glass level when the system asks for reduced transparency, or the browser lacks background blur |

Text on glass is checked against the darkest (light theme) and lightest (dark theme) the gradient field can show through that level; where the ratio would drop below 4.5:1 the fill opacity is raised, never the text lightened.

## Type

| Token | Size / line height | Weight | Face | Used for |
|---|---|---|---|---|
| `type.date` | 22 / 28 px | 650 | system sans | The date (`h1`) |
| `type.title` | 15 / 22 px | 500 | system sans | The day's title |
| `type.block` | 12 / 16 px, small caps, 0.04em tracking | 650 | system sans | Block kind label |
| `type.body` | 14 / 20 px | 400 | system sans | Item text, Markdown body |
| `type.meta` | 12 / 16 px | 500 | system sans | Status chips, link labels, toasts |
| `type.mono` | 13 / 20 px | 450 | system monospace | Meeting times, inline code, code blocks |

System sans: `ui-sans-serif, system-ui, -apple-system, "Segoe UI", sans-serif`. System monospace: `ui-monospace, "SF Mono", Menlo, Consolas, monospace`. No web fonts are loaded.

## Spacing, Radius, Size

| Token | Value | Used for |
|---|---|---|
| `space.1` … `space.6` | 4, 8, 12, 16, 24, 32 px | All gaps and padding |
| `radius.chip` | 6 px | Status chips, code chips, segments |
| `radius.panel` | 12 px | Block panels, cards |
| `radius.float` | 16 px | Calendar, dialogs, toasts |
| `size.row` | 32 px desktop, 44 px phone | Item rows |
| `size.target` | 44 px minimum at phone width | Every control (NFR7) |
| `width.page` | up to 760 px, centred | The single column |

## Motion

| Token | Value | Used for |
|---|---|---|
| `motion.fast` | 120 ms ease-out | Segment change, menu open |
| `motion.highlight` | 2000 ms ease-out | Change highlight fade (FR3.16) |
| `motion.toast` | 200 ms in, 200 ms out | Toast enter and exit |
| `motion.shimmer` | 1400 ms loop | Loading placeholders |

With reduced motion asked for, every duration becomes 0 and the shimmer stops; the highlight shows for 2 s without fading.

## Component Mapping

| Component (`interaction-spec.md`) | Surface | Type | Colour / tone | Size |
|---|---|---|---|---|
| Header | `glass.3` | `date`, `title`, `meta` | `text.primary` | `size.target` controls |
| Block panel | `glass.2` | `block`, `body` | `text.primary`, `text.secondary` | `radius.panel` |
| StateSwitch | `glass.1` track | `meta` | selected: `accent` | `size.row` |
| MeetingAnswer | `glass.1` track | `meta` | Yes `tone.positive`, No `tone.negative`, Maybe `tone.caution` | `size.row` |
| Past mark | — | `meta` | `tone.neutral` chip, row text `text.muted` at full opacity | — |
| MoreMenu, Calendar, Dialog | `glass.4` | `body`, `meta` | Remove: `tone.negative` | `radius.float` |
| Toast | `glass.4` | `meta` | `text.primary` | `radius.float` |
| Change highlight | row tint | — | `accent` at 24% fading to 0 | — |
| Markdown code | `glass.1` | `mono` | `text.primary` | `radius.chip` |
| Focus ring | — | — | `accent`, 2 px, 2 px offset | — |

## Language File Keys

Every label named in `mockups.md` and `interaction-spec.md` is a key in the English language file, read through the single helper (FR9.1), for example the done labels per kind (Done, Merged, Closed), the meeting answers (Yes, No, Maybe), "No answer", "Past", the toasts and the dialog text. Construction adds each key with its text; no component holds copy.

## Sources

- `refined-mockups-questions.md`: Q2 and Q8 (liquid glass, plain CSS), Q3 (motion), Q1 and Q7 (state and answer words, answer tones); labels on items were dropped (FR1.14 withdrawn).
- `inception/requirements-analysis/requirements.md`: FR7.1, FR3.16, FR4.10, FR9.1, NFR7, NFR8.
- `inception/practices-discovery/team-practices.md`: Code Style (few dependencies, text in language files).
- `.claude/knowledge/aidlc-design-agent/accessibility-wcag.md`: contrast and focus rules.

## Assumptions & Open Questions

- [assumption] Colour values are starting points; Build and Test checks the contrast ratios against the real rendering.
- [assumption] No web fonts are loaded, to keep dependencies and page weight low; the system faces carry the look.
