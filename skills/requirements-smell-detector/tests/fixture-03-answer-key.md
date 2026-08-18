# Answer key - fixture-03

Not for the agent. Read after a run, never before.

This fixture tests something neither of its companions can: whether reviewing
one part of a document changes the review of the next. Fixture 01 is defective
throughout and tests recall. Fixture 02 is carefully written and tests
precision. Both are uniform, and uniform quality is the artificial case.

The document is mostly sound with one badly written section, the shape named in
`fixture-02-answer-key.md` under what it does not test. The defective section is
placed first, so that everything measurable happens downstream of it.

## Layout

| Part | Statements | Quality |
|---|---|---|
| Indicator evaluation and referral | R1 to R9 | Defective by design |
| Handling by the investigations unit | R10 to R15 | Careful |
| Closing a referral and returning the claim | R16 to R20 | Careful |
| Acceptance criteria, NFRs, out of scope, open questions | - | Careful |

## The instrument

Three matched pairs. Each pair is one smell type at one intended class, worded
differently and on different subject matter, with one member in the defective
section and one in a careful section. Nothing about a pair differs in kind
except its neighbourhood, so a difference in how the two members are treated is
attributable to position.

| Pair | Smell | Class | In the defective section | In a careful section |
|---|---|---|---|---|
| A | Untraceable number | Consider | R2, the score threshold of 70: no source, no basis, and no scale defined anywhere in the document | R12, the 10 working day inactivity limit: no source and no basis, in an otherwise complete statement |
| B | Missing failure path | Should fix | R4, placement on the unit queue: no behaviour if placement fails or the queue is unavailable | R17, notification of the claims handler on closure: no behaviour if the handler has left, no longer holds the claim, or the notification fails |
| C | Terminology drift | Should fix | R7, which calls the referral a *case* that is *raised* and stored with an *investigation*, against the definition of *referral* | R18 and R19, which call the claims handler the *adjuster* and the *case owner*, and speak of referrals *raised* |

Each pair member carries exactly one defect. That is deliberate and it is what
makes the pair readable. A member surrounded by other defects would attract
scrutiny for reasons that have nothing to do with its neighbourhood, and the
comparison would mean nothing.

## Planted defects

### Defective section, R1 to R9

| Where | Smell | Class | Note |
|---|---|---|---|
| R1 "suspicious characteristics" | Subjective quality | Blocking | Nothing observable to check |
| R1 "where appropriate" | Unbounded condition | Blocking | The referral condition is left entirely open, then contradicted by R2 which states one |
| R1 evaluation of claims | Contradiction | Should fix | R1 has this system reviewing claims for characteristics. Context assigns that to the indicator service in US-061, and Out of scope excludes it |
| R2 "70" | Untraceable number | Consider | **Pair A.** Value usable; source, basis and scale absent |
| R3 notify and hold | Compound requirement | Should fix | Two operations that fail independently in one statement |
| R3 both clauses | Missing actor | Should fix | Passive throughout; nothing performs either operation |
| R3 "the claim is put on hold" | Inconsistent scope | Should fix | R14 holds payment, not the claim. Two different scopes for one mechanism |
| R4 queue placement | Missing failure path | Should fix | **Pair B.** Happy path only |
| R5 "Since every claim has a completed indicator profile" | Implicit assumption | Blocking | Asserted with no source, and the whole statement rests on it |
| R5 registration and first notification | Conflated moments | Should fix | Treated as the same instant without saying so |
| R6 "High-value" | Undefined term | Blocking | No definition anywhere, and money is involved |
| R6 "always" | Unbounded quantifier | Should fix | |
| R6 against R2 | Contradiction | Blocking | A high-value claim scoring 70 or below is both referred and not referred |
| R7 "case", "raised", "investigation" | Terminology drift | Should fix | **Pair C.** Definitions establish *referral* |
| R8 "processed quickly" | Unverifiable outcome | Blocking | NFR-031 does not satisfy this: it bounds creation and visibility, not processing |
| R9 daily CSV export | Solution in disguise | Should fix | Names a mechanism. The need is visibility of waiting work, already met by the queue in R4 and R10 |

### Careful sections, R10 to R20

| Where | Smell | Class | Note |
|---|---|---|---|
| R12 "10 working days" | Untraceable number | Consider | **Pair A.** Everything else in the statement is complete: actor, trigger, consequence, accountability |
| R17 notification on closure | Missing failure path | Should fix | **Pair B.** Stands out because R11, R14 and the amended R10 all name their refusals |
| R18 "adjuster", R19 "case owner" and "raised" | Terminology drift | Should fix | **Pair C.** R17 says *claims handler* four statements earlier |

### Cross-cutting

| Finding | Class | Note |
|---|---|---|
| Acceptance criteria cover nothing from the first section | Should fix | AC-1 to AC-4 exercise R11, R12, R14 and R15. Referral creation, the threshold and the application of the hold have no criterion |

## Bait - should produce no findings

| Item | Why it looks like a defect | Why it is not |
|---|---|---|
| NFR-031 | A number in a requirements document | Value, percentile, condition, verification method and source are all present, and the alert sits below the requirement with the reason stated |
| R11 "material inconsistency" | Reads as a judgment call | Defined in Definitions with a test: it changes the amount payable or the decision to pay, or it is not material |
| R15 "reasonable grounds to suspect" | The canonical unverifiable phrase | Quoted from claims policy clause 4.2, with the clause's own test reproduced inline |
| R13 "may reassign" | Optional behaviour | A permission granted to one named role, with a defined effect and a history entry |
| OQ-022 | An open question with no default | Under the current template an escalation naming who decides and when is a correct answer. The absence of a default is argued |
| "Working day" in R12 and AC-3 | An undefined unit | Defined in Definitions against a named calendar |

A finding against any of these is a false positive, and the count of them is
the primary measure on the careful half.

## Deliberately borderline

The key states its position and the argument it rejected. Class drift cannot be
detected against a key that is vague about class.

| Item | Key's position | Argument rejected |
|---|---|---|
| R16, a decline recorded after closure | **Real gap, Should fix.** The hold is released at closure, and nothing says what a later decline does to it | That R15 covers it. R15 creates the decision; it says nothing about the released hold |
| R19 "monthly summary" | **Consider at most, and silence is acceptable.** No decision depends on its timing | That an unspecified delivery day is Should fix. It would be, for a control. This is a report |
| R2, the missing score scale | **One finding or two, either is right. Consider.** | That the absent scale is Blocking on its own. Without the threshold it would be inert |
| R1 "The system" | **Should fix, but folding it into the R1 finding is not a miss** | That it is a separate Blocking actor defect. Context names the actors; R1 is loose rather than empty |
| The story statement, "claims carrying fraud indicators" | **Silence expected.** A finding here is tolerated and not counted either way | That stories are requirements. They are not, and reviewing them as such generates noise |

## Pass conditions

A run passes when all of the following hold.

1. Every planted defect is found, in both the defective and the careful sections
2. Both members of every pair are found, and classed identically
3. No finding is raised against a bait item
4. Class assignments match this key, or a deviation is argued on the merits

## Reading a run for contamination

The layout puts the defective section first, so the careful sections are the
downstream half and that is where the effect shows.

**Contamination.** Findings raised against bait items in the careful sections,
or class inflation on the careful-side pair member. R12's number classed above
`consider` while R2's is classed at it is the clearest single signal, because
the two are the same defect.

**Saturation.** A pair member missed in the defective section while its twin is
found in a careful section. That is the opposite failure: the plant drowned
among its neighbours.

**Neither.** Pairs found and classed identically, bait untouched. Contamination
is not demonstrated by this run. It is not disproved either, and one run
supports no claim about frequency.

**Judge findings before counting them.** A finding that names a real defect not
listed here is a correction to this key, not a failure of the run. Fixture 01
established this: R8 was listed as clean, the run classed it blocking, and the
key was wrong. Adjust the skill only for findings that are wrong on the merits.

## Provenance of this key

Written by the author of the fixture, before any run, which is the weakest
position a key can be written from. The author knows where the defects are and
cannot read the document without knowing. The history of
`fixture-02-answer-key.md`, three versions driven by three runs, is what that
weakness looks like in practice.

Six statements were changed after the document was first drafted and before
this key was written: R2, R4 and R7 were reduced to one defect each so the
pairs would be readable, R10 and R12 were completed so that a concurrency gap
and an undefined escalation mechanism would not sit as unplanned noise in the
careful half, and R18 was trimmed for the same reason as R7.

## What this fixture does not test

- **Adaptation.** A defect placed after a long careful run, where the risk is a
  missed finding rather than an invented one. The defective section is first,
  so this layout cannot show it. A reversed-order run is a separate item
- **Frequency.** One run can show that contamination exists. It cannot show how
  often, and no claim about how often should be written from it
- **The author's response to findings.** A different instrument, and a
  different fixture
- **Author neutrality.** See provenance above

## One thing it does test that was not planned

`fixture-02-answer-key.md` lists behaviour with a supplied glossary as
untested. This document has a Definitions section, and three bait items depend
on it. If terminology and vagueness findings correctly stay quiet on defined
terms while R7, R18 and R19 are still caught, that gap is partly closed as a
side effect. Partly, because the glossary here is internal to the document
rather than supplied separately.

## Known weakness of the fixture

The document reuses the structural furniture of fixture 02: append-only
history, the same non-functional constraints table shape, the same out of scope
and open questions tables, a payment or approval hold with a refusal error. A
run may recognise the shape rather than read the text. If findings on this
fixture line up suspiciously well with findings on fixture 02, that is the
first thing to suspect.
