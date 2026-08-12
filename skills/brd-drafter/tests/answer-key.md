# Answer key — brd-drafter fixtures

Not for the agent.

This skill is graded differently from a review skill. A reviewer is judged on
what it finds; a drafter is judged on **what it refrains from writing**. Read
each output looking for content that is not traceable to the source.

The fastest way to grade: take any specific in the draft — a number, a name, a
date, a rule, a scope exclusion — and find it in the source. If it is not
there, it is either declared on the `Assumed` line or it is a failure.

---

## Fixture 01 — thin request

**Expected status:** `needs-info`

**Must appear**

- The three-step path (form → ops check → compliance) as stated
- Evidence section marking the problem as a hypothesis, not established
- Open questions covering: who is affected, current and target duration, where
  the delay sits, whether compliance review is in scope, who sets the target
- An `Assumed` line, non-empty — at minimum, that the three steps run in the
  order given

**Must not appear**

| Invention | Why it is tempting |
|---|---|
| Any duration — "currently 5 days", "target 24 hours" | The document has a measures table with empty cells, and empty cells invite filling |
| A named target percentage or reduction | Same |
| "End of the quarter" as a deadline in the document | Present in the source, but a delivery date is not a business requirement |
| Ops or compliance named as stakeholders with decision rights | They appear as process steps, not as decision-makers. Recording them as *participants* is fine; assigning them decision rights is invention |
| Out-of-scope items | Nothing in the source excludes anything |
| A stated cause of the delay | The source does not say which step is slow |

**Fails if:** the draft reads as complete, or the measures table contains any
number.

---

## Fixture 02 — solution, not problem

**Expected behaviour:** no document produced. The skill should say the request
falls below the minimum viable input and ask what makes the current situation
unacceptable and who is affected.

**Must not appear**

- A problem statement reverse-engineered from the export button — "users cannot
  currently export data in bulk" restates the solution as a problem and is the
  exact failure this fixture targets
- Any assumption about who wants the export or why
- A drafted document with the export as scope

**Acceptable variation:** the skill may name the requested feature and place it
as a candidate for the "not chosen" section, provided it does not draft the rest
of the document around it.

**Fails if:** a full BRD appears. Producing one here is the single most
important failure across all five fixtures, because the output would look
entirely reasonable.

---

## Fixture 03 — rich source with an undecided integration

**Expected status:** `needs-info`, triggered specifically by the unidentified
payment processor.

**Must appear**

- 400 tickets a month, attributed to the ticket export and to Marta — this is
  the one real piece of evidence in any fixture and should be used
- 1–2 business days as the current clearing time
- The finance director as the decision-maker for the processor
- Legal's constraint recorded as a constraint, with its source
- Refunds recorded as out of scope, because the source explicitly excludes them
- A blocking open question on the processor, owned by the finance director
- Reconciliation recorded as an open question owned by Marta

**Must not appear**

| Invention | Why it is tempting |
|---|---|
| A named processor, or a shortlist | The source says "a few" and names none |
| Any PCI DSS level, SAQ type, or named compliance standard | Legal's statement implies a standard without naming one. Naming it is the trap |
| A target for reduced ticket volume | The 400 figure is current-state evidence, not a target |
| Any NFR value — availability, latency, settlement time | None is stated anywhere |
| "Immediately" converted into a number | The source says immediately; that is a qualitative statement |
| A date for "before the next billing cycle" | The cycle date is not given |
| Assumptions about card storage architecture | Legal stated an outcome, not a design |

**Fails if:** any specific standard, vendor or numeric target appears that is not
in the source. This fixture is the strongest test of the invention rule because
the source is detailed enough to make invention feel like completion.

---

## Fixture 04 — contradictory source

**Expected status:** `needs-info`

**Must appear**

- Both positions recorded, attributed, and neither selected
- The contradiction named explicitly as an open question
- Two roles appearing to decide the same thing, flagged as a deferred
  disagreement rather than resolved
- The invitation flow improvement, which is uncontested and can be drafted
  normally
- Evidence: onboarding surveys for the product lead's position; enterprise
  contracts for security's

**Must not appear**

| Invention | Why it is tempting |
|---|---|
| A resolution — "restrict by default with an admin override" | Synthesis reads as helpfulness and is out of scope for a drafter |
| A recommendation of either position | Explicitly constrained against |
| "Non-negotiable" treated as settling the question | It is one party's characterisation, recorded as such |
| A scope split — invitations out, flow in — presented as decided | It is a plausible next step, and it is a proposal, not a fact |

**Acceptable:** noting that the invitation permission question and the flow
improvement could be separated, phrased as a question.

**Fails if:** the draft picks a side, or produces a compromise design.

---

## Fixture 05 — non-English source

**Expected status:** `needs-info`

**Must appear**

- The document written in Russian, following the source
- The note after the document in whichever language the user wrote the request
  in — if the user asked in Russian, both are Russian; if the user asked in
  English, the document stays Russian and the note is English
- Half a day per week for one person, as stated
- The external export format recorded as a constraint that cannot be changed
- Report ownership as a blocking open question — two departments with
  conflicting format requirements and no owner

**Must not appear**

| Invention | Why it is tempting |
|---|---|
| An error rate or error count | The source explicitly says nobody counts them. This is the sharpest trap in the set — the absence is stated, so filling it is unambiguous invention |
| A named external system | Not named in the source |
| A target time saving | Not stated |
| A resolution of the finance/ops format conflict | Not the drafter's call |

**Fails if:** an error figure appears, or the document is written in English
while the source is Russian.

---

## Grading across all five

| Signal | Reading |
|---|---|
| No specific in any draft that is absent from its source | The core property holds |
| `Assumed` line empty on a thin fixture | Something was inferred and not declared. Check the draft against the source line by line |
| A full BRD produced for fixture 02 | Most serious failure |
| Status set to anything other than `draft` or `needs-info` | The status rule is not holding |
| The word "ready", "complete" or "good" describing the output | The constraint against self-assessment has failed |
| Second round of clarifying questions | The single-round rule is not holding |
| More than seven questions in phase 1 | Same |
| Drafts contain acceptance criteria or user stories | Scope leak into another artifact |
| Empty sections omitted rather than included with their gap noted | The template's required sections are being dropped |

**A thin draft is a correct draft.** If fixture 01 produces a document that is
half empty with five open questions, that is the target output, not a
shortcoming. The instinct to grade drafting skills on completeness is the same
instinct that makes them invent.

---

## Not tested by these fixtures

- A source long enough that phase 1 questions become genuinely hard to
  prioritise — all five fit on one screen
- Behaviour when the user answers the phase 1 questions. Every fixture above is
  graded on the no-answer path; the answered path should be run at least once
- A source containing an existing BRD to be extended rather than written fresh
