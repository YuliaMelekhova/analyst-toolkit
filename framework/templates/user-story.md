---
id: US-000
title: <What the user can do after this, in a few words>
status: draft            # draft | needs-info | ready | in-progress | done
author: <analyst>
updated: YYYY-MM-DD
traces:
  satisfies: []          # BR-xxx
  constrained_by: []     # NFR-xxx
  decided_in: []         # ADR-xxx
  blocked_by: []         # OQ-xxx
---

# US-000 - <Title>

> Delete the guidance blockquotes before publishing.

## Story

**As a** <specific role, not "user">
**I want** <capability, not solution>
**So that** <the outcome that makes this worth building>

> The "so that" is the only part that can be argued with, which makes it the
> only part that matters. If it restates the "I want" in other words, the value
> has not been articulated - go back to the business requirement and take it
> from there.
>
> "As a user" is almost always too broad. If every role in the system would
> phrase this identically, either the story is infrastructural, or the roles
> have not been separated yet.

---

## Context

> Two or three sentences a developer needs before reading the criteria: what
> exists today, what changes, what stays the same. Not a summary of the BRD -
> only what bears on this slice.

---

## Acceptance criteria

> Written so that someone who did not write the story can decide pass or fail
> without asking a question. Each criterion is independently checkable.

**US-000/AC-1** - <name of the case>

```gherkin
Given <the state the world is in>
  And <any further precondition>
When <the single action taken>
Then <the observable result>
  And <any further observable result>
```

**US-000/AC-2** - <name of the case>

```gherkin
Given
When
Then
```

### Unhappy paths

> A story with only happy paths is half-specified. At minimum, consider:
> invalid input, insufficient permission, the downstream call failing, the
> action repeated twice, and the state changing between the read and the write.
> Cross out the ones that genuinely cannot occur - do not silently omit them.

| Case | Covered by | Or: why it cannot occur |
|---|---|---|
| Invalid or incomplete input | AC- | |
| Actor lacks permission | AC- | |
| Dependency unavailable or times out | AC- | |
| Action submitted twice | AC- | |
| State changed since it was read | AC- | |

---

## Out of scope for this story

> What a reader would reasonably expect to be included and is not. One line
> each, with the story or requirement that will cover it if there is one.


---

## Vertical slice check

> A story is a slice through the system, not a layer of it. If any answer below
> is "another story", this is a task, not a story - either merge it or state
> plainly that it is technical work with no standalone user outcome.

| Question | Answer |
|---|---|
| Can a user observe the change after this alone ships? | |
| Does it include every layer it touches (interface, logic, storage)? | |
| Can it be demonstrated in under two minutes? | |
| Would shipping only this leave the system in a consistent state? | |

---

## Data touched

> Only fields that are new, changed, or newly exposed. Not a schema dump.

| Field | New / changed | Source of truth | Notes |
|---|---|---|---|
| | | | |

---

## Non-functional constraints

| NFR | Why it applies here |
|---|---|
| NFR-000 | |

---

## Open questions

| ID | Question | Owner | Needed by | If unanswered |
|---|---|---|---|---|
| OQ-000 | | | | |

> A story with an unanswered blocking question is `needs-info`, never `ready`.
>
> The last column takes one of two forms: a **default decision** that takes
> effect without further discussion, or, where no default is defensible, an
> **escalation** naming who decides and on what trigger. Neither is not an
> option - a question with no stated consequence for silence will be answered
> by whoever implements it.
>
> Escalation should be rarer here than in a BRD. A story is open for weeks, not
> months, and a question needing someone's authority to settle usually belongs
> to the parent document rather than to the story. If a story carries an
> escalation, check first whether it is really the BRD's question arriving
> late.

---

## Definition of Ready

See `framework/quality-rules/definition-of-ready.md`. This story does not enter
a sprint until every item there is satisfied.
