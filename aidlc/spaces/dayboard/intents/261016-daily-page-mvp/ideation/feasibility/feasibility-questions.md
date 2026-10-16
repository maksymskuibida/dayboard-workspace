# Feasibility & Constraints Questions — Dayboard

Context: the approved intent statement (`ideation/intent-capture/intent-statement.md`) already settles the problem, the single person it serves, success, and the product boundary. These questions cover only what decides whether the first version is feasible and what constrains it.

## Q1. What time do you have for the first version?

This sets how much the first version can hold and which risks matter (a two-week target makes every unknown expensive; an open timeline does not).

A. No deadline; I work on it when I can
B. About two weeks
C. About a month
D. Not yet defined
X. Other (please specify)

[Answer]: X — tomorrow, hopefully, but not required. It has really small scope, what weeks and month are we talking about?

## Q2. How sensitive is what the assistant will write onto your pages?

The pages will hold meeting titles, pull request names, notes and links from your tools, while the repositories are public. This decides how strictly the data must be kept out of the repositories and off the network.

A. Work content: it must never be committed, but plain files on my own disk are fine
B. Sensitive: never committed, and never readable by anything except this machine
C. Not sensitive; I don't mind where it ends up
D. Not yet defined
X. Other (please specify)

[Answer]: B

## Q3. From where will you open the page?

The page has no sign-in. Opening it from your phone over your home network means anyone on that network could also read and change it; opening it only on this computer avoids that.

A. Only on this computer
B. This computer and my phone on the same home network
C. This computer now; the phone later, once sign-in exists
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q4. How does the assistant you use daily connect to local tools?

The assistant reaches Dayboard's data through a local connection. Some assistants start a local tool themselves; others connect to an address on your machine. This decides which connection the first version must offer.

A. It starts a local tool itself
B. It connects to an address on my machine
C. It can do either
D. Not sure
X. Other (please specify)

[Answer]: X — I run local BE and FE. Assistant connects to local BE. There is nobody else who can access it. No others. Anyone else can install it the same way. There is no shared space

## Q5. How familiar are you with the proposed stack?

Your description proposes Python for the backend and TanStack Start (with TanStack Query and Form) for the web app. Familiarity decides which parts carry delivery risk.

A. Comfortable with both
B. Comfortable with Python, new to TanStack Start
C. Comfortable with TanStack Start, new to the Python side
D. New to both; the assistant writes most of the code and I review
X. Other (please specify)

[Answer]: A

## Q6. Should anything from today's chat-artifact reports carry over?

Today the assistant keeps your reports in its own memory. Carrying them over means an import path; starting fresh means the first page is written from scratch.

A. Start fresh; nothing carries over
B. Carry over the notes for tomorrow from the last report only
C. Carry over recent reports as past days
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Time: you'd like the first version tomorrow if possible, but there is no hard deadline; the scope is small (Q1).
- Page content is sensitive: never committed, and never readable by anything except this machine (Q2).
- You open the page only on this computer (Q3).
- You run the backend and the web app locally; your assistant connects to the local backend; nobody else can reach it, and anyone else installs their own copy the same way, with no shared space (Q4).
- You are comfortable with both Python and TanStack Start (Q5).
- Nothing carries over from today's chat-artifact reports; Dayboard starts fresh (Q6).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
