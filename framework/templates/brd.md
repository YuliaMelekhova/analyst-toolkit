---
id: BR-000
title: <One line, written as an outcome, not as a feature>
status: draft            # draft | needs-info | in-review | approved | superseded
owner: <name / role accountable for the business outcome>
author: <analyst>
updated: YYYY-MM-DD
traces:
  decided_in: []         # ADR-xxx
  blocked_by: []         # OQ-xxx
---

# BR-000 — <Title>

> **How to use this template.** Every section below states what it is for and
> what makes it wrong. Delete the guidance blockquotes before publishing.
> Sections marked **required** may not be left empty — if you cannot fill one,
> the document is `needs-info`, not `draft`.

---

## 1. Problem — required

> What is happening today that should not be, or what is not happening that
> should. Describe the situation, not the solution. If this section can be read
> as "we don't have feature X", it is not a problem statement — ask why the
> absence of X matters and write that instead.

<!-- 3–6 sentences. -->

**Evidence.** What tells us this is real: a metric, a support volume, a
specific incident, an interview quote. A problem with no evidence is a
hypothesis, and should be labelled as one.

| Source | What it shows | Date |
|---|---|---|
| | | |

---

## 2. Outcome — required

> What is true after this is delivered, expressed so that a disinterested
> person could check it. "Improved experience" is not checkable. "Plan changes
> complete without contacting support" is.

**Target state.**

**How we will know.**

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| | | | | |

> If the "measured how" column cannot be filled, the measure is decorative.
> Either find a real measurement path or drop the row and say the outcome is
> not measurable yet — that is an honest position, a fake metric is not.

---

## 3. Scope

### In scope — required

> The set of changes this document authorises. Written as capabilities, not as
> screens or endpoints — those belong to the stories.

- 

### Out of scope — required

> The single most useful section in this template. List what a reasonable
> reader would otherwise assume is included, and say plainly that it is not.
> An empty out-of-scope section almost always means the boundary has not been
> thought about yet.

| Not included | Why not | Revisit when |
|---|---|---|
| | | |

---

## 4. Stakeholders — required

| Role | Name | Interest in the outcome | Decides / consulted / informed |
|---|---|---|---|
| | | | |

> "Decides" must name exactly one role per decision area. If two roles decide
> the same thing, the disagreement has not happened yet — it has been deferred.

---

## 5. Current process

> Only if a process exists today. Describe the path a real case takes, including
> the unhappy ones, and name the systems it passes through. Link the as-is
> diagram rather than embedding a long narrative.

**Trigger.**

**Path.**

**Where it breaks.**

| Step | What goes wrong | Frequency | Current workaround |
|---|---|---|---|

---

## 6. Business rules

> Statements that are true regardless of implementation. Each one gets an
> identifier so stories and tests can point at it. Rules that depend on a
> screen or an API shape are not business rules — they are design decisions
> and belong in an ADR.

| Rule | Statement | Source of authority |
|---|---|---|
| BR-000/R1 | | |

---

## 7. Constraints and assumptions

**Constraints** — things that are fixed and not up for negotiation in this scope.

| Constraint | Type | Imposed by |
|---|---|---|
| | legal / technical / commercial / temporal | |

**Assumptions** — things taken as true without proof, which would change the
solution if wrong.

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| | | |

> An assumption with no stated consequence is not being tracked, it is being
> ignored. Assumptions that would invalidate the whole document belong in
> section 9 as open questions instead.

---

## 8. Non-functional requirements

> Do not restate the NFR catalogue here. Reference the identifiers that apply
> to this scope, and note any value that differs from the default.

| NFR | Applies because | Value for this scope |
|---|---|---|
| NFR-000 | | inherits default / overridden: … |

---

## 9. Open questions — required

> Gaps that block drafting or delivery. Every entry has a named owner and a
> date by which an unanswered question becomes a decision by default. A
> question with no owner is a note, not a question.

| ID | Question | Blocks | Owner | Needed by | Default if unanswered |
|---|---|---|---|---|---|
| OQ-000 | | | | | |

---

## 10. Not chosen

> Approaches that were genuinely considered and rejected, with the reason. One
> line each; anything that needed real deliberation gets its own ADR and is
> referenced from here.

| Option | Why not | ADR |
|---|---|---|
| | | |

---

## 11. Downstream artifacts

| Artifact | Identifier | Status |
|---|---|---|
| User story | US-000 | |
| Decision record | ADR-000 | |

---

## Change log

| Date | Change | By |
|---|---|---|
| YYYY-MM-DD | Created | |
