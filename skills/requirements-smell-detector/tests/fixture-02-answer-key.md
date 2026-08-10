# Answer key — fixture-02 (v3)

Not for the agent. This fixture tests **precision**: whether the skill invents
findings on text that is written carefully.

Three defects are planted. They are not the point of the exercise — see
*Reading a run* below, which matters more than the list.

---

## History of this key

**v1** claimed four planted defects in otherwise clean text. The first run
returned eighteen findings. On inspection, roughly fourteen of them were real
defects the author had not intended or noticed: an unreachable completion
condition when every step resolved to the document owner, an undefined expiry
trigger with a race against late responses, a history requirement that did not
cover what an acceptance criterion required. The skill was right and the
fixture was wrong.

**v2** closed those defects and kept four plants. The second run returned twelve
findings — again almost all real, including one defect introduced *by the v1
fix*: an acceptance criterion had been made to reference a p95 NFR, which a
single scenario cannot pass or fail against.

**v3** — this version — removes the plant that turned out not to be a defect and
rewrites the grading section around what three runs actually showed.

**D2 removed.** v1 and v2 claimed a contradiction between withdrawal from
`expired` (R10) and refusal of approval on `expired` (AC-4). There is none.
Withdrawal and approval are different actions; a state can be non-final for one
and final for the other. The skill never reported it, correctly.

---

## Planted defects

| # | Where | Smell | Class | Note |
|---|---|---|---|---|
| D1 | R5 "A reviewer may approve" vs "approver", "assignee" elsewhere | Terminology drift | Should fix | One party under three names. Found in every run, and each run also caught "assignee", which was unintentional. |
| D3 | R11 vs R3 | Missing failure path / unverifiable outcome | Should fix | Reassignment does not state whether the 5-day deadline restarts or carries over. R6 sets a precedent for restarting; R11 follows neither reading. Found in every run. |
| D4 | Throughout | Missing failure path | Should fix | No behaviour for an assignee losing access or leaving while a step is pending. Missed in runs 1 and 2; found in run 3 after the cross-cutting scan was added to the skill. This plant is the reason that section exists. |

---

## Bait — should produce no findings

These are the places a careless reviewer flags. Findings here indicate padding.

| Where | Why it is fine |
|---|---|
| "the routing service …" throughout | The performer is named in the statement. Not smell 1 under the current rule. |
| "5 business days" (R3) | Bounded, with a start point, calendar, timezone and named source. |
| R4 evaluation window | Trigger, interval and the late-response case all stated. |
| R8 skip cases | Last-step skip and all-steps-skipped both resolved. |
| NFR-012, NFR-018 | Value, condition, verification and source all present; the alert gap is explained. |
| "active" | Defined in Context. |
| OQ-014 | Owner, date and default all present. |
| Out of scope table | Complete, with reasons and revisit conditions. |

All three runs left these alone.

---

## Reading a run

**Judge the findings before counting them.** A finding that names a real defect
is correct whether or not it was planted. The key records what the author
intended; the text is what the reviewer reads, and across three attempts those
were never the same thing.

**Adjust the skill only for findings that are wrong on the merits:**

| Defect in the output | What it means |
|---|---|
| A misclassified smell — the named smell does not describe what is missing | The disambiguation rules need tightening |
| An inflated class — *blocking* on something a competent implementer could build | The calibration paragraph is not holding |
| An invented value — a threshold, count or interval that is not in the text | The most serious failure. Fix immediately |
| A defect asserted where the text is complete | Padding. The "do not manufacture findings" constraint is weak |
| An approval or readiness verdict | The no-approval constraint has failed |

**Do not adjust the skill for finding count alone.** The instinct to cap it at a
number assumes the fixture is clean. It was not, twice, and there is no reason
to think a third attempt would be different.

---

## Stopping criterion

A run passes when it contains **no false positives** — not when it stays under
a count.

Secondary signals, in order of importance:

1. No invented values anywhere in the reformulations
2. No readiness verdict
3. Zero or near-zero *blocking* findings on this fixture
4. The bait items above untouched
5. `Cleanest statements` populated and accurate

Runs 2 and 3 met all five. The skill was frozen after run 3.

---

## What this fixture still does not test

- **A mixed artifact** — mostly sound with one badly written section. The
  realistic case, and the hardest to calibrate.
- **Behaviour when a glossary is supplied.** All runs assumed none existed, and
  terminology findings were caveated accordingly. Whether those findings
  correctly disappear when definitions are provided is unverified.
- **A non-English artifact.** The output-language rule has never been exercised
  against a document written in another language.
