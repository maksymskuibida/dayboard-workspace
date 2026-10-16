# Personas — Dayboard, first version

Two personas, as confirmed in `user-stories-questions.md` (Q1). Both come from the intent statement (`ideation/intent-capture/intent-statement.md`, Target Customer) and the requirements (`inception/requirements-analysis/requirements.md`, Intent Analysis). All example content is made up.

## P1. The person (Dayboard owner)

| Field | Description |
|---|---|
| Role | Runs their own copy of Dayboard on their computer and reads their daily page during the working day. The only human user of this version. |
| Goals | See the day at a glance in a page that is pleasant and quick to read; keep track of what is open, done or delayed; leave notes for tomorrow and keep the learnings worth keeping. |
| Pain points | Today the daily report lives inside one assistant's chat as an artifact: it costs tokens to regenerate, the design is whatever the chat renders, and no other assistant can reach it. |
| Context | Works at a desktop computer; may narrow the window to phone width. Uses one AI assistant daily, possibly others. Page content is sensitive and must stay on this computer. |
| Tech comfort | High: a developer comfortable with both halves of the stack. |
| Frequency | Every working day: opens the page in the morning and keeps it open through the day. |

## P2. The assistant

| Field | Description |
|---|---|
| Role | The person's AI assistant, reaching Dayboard through its MCP server on this computer. Writes most of the page. A non-human persona: its goals come from the person's instructions and the copy prompt. |
| Goals | Prepare today's page from the person's tools, yesterday's notes and delayed items; keep the page current as the day moves; carry kept learnings into its own memory. |
| Pain points | Without Dayboard it keeps its own memory of the report and rewrites it in chat, which costs tokens and can't be shared with other assistants. |
| Context | Any assistant that speaks MCP; no sign-in. Works from what it reads: if the page changed since its last read, its change is refused and it must read again. |
| Frequency | Several times a day: in the morning when the person pastes the copy prompt, and whenever the person asks it to update or delay something. |

## Relationships and Priority

- **P1 is the primary persona.** The success test is the person's daily use for two weeks (intent statement, Success Metrics); when the two personas' needs conflict, the person's reading experience wins (intent statement, Initiative Trigger).
- **P2 acts for P1.** Every assistant story ends in a benefit to the person.
- **Shared data, equal rights.** Both can change the same day; the assistant has the same rights as the person on items, learnings and notes, plus removing a whole day (requirements FR2.6, FR2.7). The page hash keeps one from silently overwriting the other (FR4.9).

## Sources

- `ideation/intent-capture/intent-statement.md`: Target Customer, Success Metrics, Initiative Trigger.
- `inception/requirements-analysis/requirements.md`: Intent Analysis, FR2, FR4.9.
- `user-stories-questions.md`: Q1.

## Assumptions & Open Questions

None.
