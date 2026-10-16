# Intent Statement — Dayboard

## Problem Statement

Today one person's AI assistant writes their daily report as a chat artifact and keeps its own memory of it, so the report lives inside one assistant's conversation rather than on a page of its own. [desc]

Dayboard replaces that with a real daily page shared by the person and their assistant: each day has a page with a title and blocks of fixed, generic kinds (schedule, open items, pull requests, learnings, waiting on others, notes for tomorrow), each block holds items, and each item has text, optional links and a state both of them can change (open, done, delayed to tomorrow). [desc]

The assistant writes most of the page after investigating the person's tools; the person reads it during the day, and the page always opens on the latest day on record, today or later. [desc]

Notes for tomorrow belong to each day's page: the assistant fills them when the person asks, the page shows them, and the assistant reads them when it prepares the next day, by a list of dates or, when no dates are given, from the latest day on record. [desc]

## Target Customer

The first and only customer of this version is the person who runs Dayboard for themselves; no one else has a stake in this first version. [desc] [Q3]

The person's AI assistant is the second actor on every page: it writes and updates the page, and the person reads it and changes item states. [desc]

Other people with their own kinds of blocks are a later audience, not part of this version. [desc] [Q3]

## Success Metrics

- **Replacement in daily use:** the person uses Dayboard every working day for two consecutive weeks and does not fall back to the chat-artifact report on any of those days. [Q1]
- **Proof assistant:** that use is carried out with one assistant the person uses daily; other assistants are welcome but are not part of the check. [Q6]
- **Day of use:** a working day on which the assistant prepares that day's page and the person reads it during the day. [Q1]
- **Fallback:** asking an assistant for the old chat-artifact report on any of those days. [Q1]
- **Unmeasured gains:** fewer tokens and access from any assistant are goals of this version but are not measured in it; the daily-use check above is the only success measure. [Q1] [Q6]

## Initiative Trigger

The current report is a chat artifact the assistant keeps in its own memory; the person wants a real page with better design, that costs fewer tokens, and that works from any assistant. [desc]

When those gains conflict, a page that is pleasant and quick for the person to read during the day takes priority. [Q2]

## Initial Scope Signal

**Workflow-selected scope:** `mvp`, workflow-selected. [scope]

**User-confirmed product boundary:** the first version leaves out users and authentication, cloud deployment, custom block kinds added by users, a plugin or engine layer, notifications, and any integration that reads the person's tools directly; these are out of scope now but must not be designed out. [desc]

Nothing beyond that list is excluded from the first version. [Q7]

Fixed constraints from the request: one person with no user field or user configuration, runs locally, public repositories with nothing company-specific, and interfaces that look current in light and dark and at phone width. [desc]

## Assumptions & Open Questions

None.
