---
id: QR-001
title: Definition of Ready
status: draft
owner: <role that enforces this>
updated: YYYY-MM-DD
---

# Definition of Ready

A requirement is *ready* when a developer who was not in the conversation can
start work on it without asking a question, and a tester can decide pass or
fail without asking a second one.

Readiness is a gate, not a score. An item either passes every applicable
criterion or it does not enter the sprint.

---

## Why a gate and not a guideline

A guideline is negotiated at the worst possible moment: when the sprint is
short of capacity and the item is almost ready. The cost of admitting an
unready item is not paid by the person admitting it — it is paid mid-sprint by
whoever hits the ambiguity, usually as a hallway conversation that nobody
records. Making readiness binary moves that cost forward to where it is cheap.

---

## Criteria

### Every item

| # | Criterion | Fails when |
|---|---|---|
| 1 | Has a stable identifier | Referred to by title only, so links break when it is renamed |
| 2 | States the outcome, not the implementation | Phrased as "add a button" with no statement of what becomes possible |
| 3 | Traces to a business requirement | Nothing explains why this is worth doing |
| 4 | Acceptance criteria are independently checkable | A criterion needs its author present to interpret |
| 5 | Unhappy paths are addressed or explicitly ruled out | Only the happy path exists, with no note about the rest |
| 6 | Applicable NFRs are referenced | Nobody knows what "fast enough" means for this item |
| 7 | No blocking open question is unanswered | The item carries an `OQ-` with no answer and no default |
| 8 | Dependencies are named and their state is known | "Depends on the other team" with no named artifact or date |
| 9 | Is small enough to finish inside one iteration | Cannot be demonstrated at the end of the sprint |
| 10 | Someone other than the author has read it | The first reader is the person implementing it |

### Additionally, for items that change an interface

| # | Criterion | Fails when |
|---|---|---|
| 11 | The contract is written down before implementation | The specification will be reverse-engineered from the code |
| 12 | Backward compatibility is stated | Existing consumers find out at release |
| 13 | Error behaviour is specified, not just success | Failure modes are left to the implementer to invent |

### Additionally, for items that touch persisted data

| # | Criterion | Fails when |
|---|---|---|
| 14 | The authoritative source for each field is named | Two systems will disagree and nobody knows which wins |
| 15 | Migration and back-out are addressed | The change is one-way and nobody said so |

---

## Applying it

**Mark criteria not applicable explicitly.** An item that does not touch an
interface skips 11–13, and the skip is recorded. A blank is indistinguishable
from an oversight.

**Failure produces one status, not a debate.** An item failing any criterion is
`needs-info`. Not "ready with a caveat". The status names what is missing and
who owns it.

**The author does not certify their own item.** Criterion 10 exists because the
person who wrote a requirement cannot see what they assumed. This applies with
equal force to a requirement drafted by an agent: generated output is a draft
awaiting review, never a ready item, regardless of how complete it looks.

**Track the misses, not just the gate.** Which criterion fails most often is the
most useful signal this list produces. Consistent failures on 5 point at
elicitation habits; on 8, at how work is sequenced; on 10, at review capacity.

---

## What this is not

- **Not a quality bar for the delivered solution.** That is Definition of Done.
- **Not a documentation checklist.** An item can satisfy every criterion in
  half a page.
- **Not a reason to defer discovery.** If an item repeatedly fails on 3 or 7,
  the problem is upstream and no amount of rewriting will fix it here.
