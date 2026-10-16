# question-forms

Every AI-DLC questions file becomes a form in one private artifact, **AI-DLC question
forms**, published once per developer from the template in this plugin. The agent writes the
questions as rows, the developer answers in the page, the agent reads one document back and
copies the answers word for word into the file's `[Answer]:` tags. Besides questions with options it
renders text-only questions and board forms (cards sorted into 2–4 boxes, used by `pr-review`). One template, so every form
looks the same on every machine; no HTML is generated per stage.

```
skills/question-forms/SKILL.md           the procedure the agent follows at every questions file
skills/question-forms/question-form.html the template — the data contract is in its header
```

Install for this workspace (from the workspace root):

```
claude plugin marketplace add maksymskuibida/dayboard-workspace --scope project
claude plugin install question-forms@dayboard --scope project
```

A change to the look or the data contract is a PR on the template.
