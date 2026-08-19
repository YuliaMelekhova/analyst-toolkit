# Answer key - fixture-03 (v2)

Not for the agent. Read after a run, never before.

This fixture tests something neither of its companions can: whether reviewing
one part of a document changes the review of the next. Fixture 01 is defective
throughout and tests recall. Fixture 02 is carefully written and tests
precision. Both are uniform, and uniform quality is the artificial case.

The document is mostly sound with one badly written section, the shape named in
`fixture-02-answer-key.md` under what it does not test. The defective section is
placed first, so that everything measurable happens downstream of it.

---

## History of this key

**v1** was written before any run, by the author of the fixture. It claimed 20
planted defects, three matched pairs and six bait items, and its provenance
section called that the weakest position a key can be written from.

**v2** is this version, written after run 01
([`fixture-03-run-01.md`](fixture-03-run-01.md), commit `8111871`). The run
corrected v1 on six points, three of them on one statement. One pair is retired
as an instrument, one bait item is narrowed, one borderline position is
reversed, and a contradiction with `fixture-02-answer-key.md` is resolved
against this key.

### The hazard in revising a key against the run that tested it

A key edited to match a run stops being an independent test of the next one. The
protection is that every change below is argued from the text of the fixture and
would stand if a different run had raised it, and that each says plainly what
would make it wrong.

One change alters how run 01 itself would score. Bait item 2 is narrowed, and
under v2 the finding that failed pass condition 3 would not be a false positive.
The numbers in the run record are not restated. They were scored against v1,
the run record says so, and rescoring a recorded run against a key revised
afterwards produces a result that never happened.

### What v2 changes

| Change | Source | What would make it wrong |
|---|---|---|
| R12 gains three defects, and the claim that it is complete apart from its number is withdrawn | Run 01, findings 14, 15, 16 | If "recorded activity" were defined anywhere, if escalation were stated to fire once, or if accountability had any expression in the system. None is the case |
| Pair A retired as an instrument | Consequence of the R12 correction, and of merged reporting in run 01 | If R12 turned out to be clean after all |
| R11 gains two defects | Run 01, finding 12 | If R18's reopening did not produce a second outcome on the same referral, or if AC-2 did not assume ownership |
| R18's citation of clause 6.1 gains a defect | Run 01, finding 19 | If bait item 3 did not justify R15 precisely by the clause's test being reproduced inline. The same standard cannot excuse R18 |
| Definitions gain a defect: the state before a referral is taken is unnamed | Run 01, finding 1 | If R4 named the state it creates a referral in |
| Bait item 2 narrowed | Run 01, finding 13 | If the run had argued the term is undefined. It did not; it argued the third sentence of R11 states a fact rather than a rule with enforcement |
| R16 moves from `should fix` to `blocking` | Run 01, finding 18, argued | If a decline could exist at the moment the outcome is recorded. Under R15 it cannot, so the exception never fires |
| The reassignment clock is resolved against this key | Run 01, and `fixture-02-answer-key.md` D3 | If reassignment demonstrably reset or did not reset the inactivity clock. Nothing in the document says which |

---

## Layout

| Part | Statements | Quality |
|---|---|---|
| Indicator evaluation and referral | R1 to R9 | Defective by design |
| Handling by the investigations unit | R10 to R15 | Careful |
| Closing a referral and returning the claim | R16 to R20 | Careful |
| Acceptance criteria, NFRs, out of scope, open questions | - | Careful |

The word careful now means intended to be careful. Run 01 established six
defects in these sections that were not put there.

## The instrument

Three matched pairs were built. Each pair is one smell type at one intended
class, worded differently and on different subject matter, with one member in
the defective section and one in a careful section. Nothing about a pair is
supposed to differ in kind except its neighbourhood, so a difference in how the
two members are treated is attributable to position.

| Pair | Smell | Class | In the defective section | In a careful section | Status |
|---|---|---|---|---|---|
| A | Untraceable number | Consider | R2, the score threshold of 70: no source, no basis, and no scale defined anywhere in the document | R12, the 10 working day inactivity limit | **Retired in v2** |
| B | Missing failure path | Should fix | R4, placement on the unit queue: no behaviour if placement fails or the queue is unavailable | R17, notification of the claims handler on closure: no behaviour if the handler has left, no longer holds the claim, or the notification fails | Intact. Failed in run 01 |
| C | Terminology drift | Should fix | R7, which calls the referral a *case* that is *raised* and stored with an *investigation*, against the definition of *referral* | R18 and R19, which call the claims handler the *adjuster* and the *case owner*, and speak of referrals *raised* | Intact |

**Why pair A is retired.** The pair required each member to carry exactly one
defect, so that only the neighbourhood differed. v1 asserted that R12 was
complete apart from its number. It is not: it carries three further defects, and
that makes the careful-side member the messiest statement in the careful half.
The pair never held content constant, so no reading of it supports a conclusion
about position. Run 01 also reported both members in one row, which would have
made the comparison unavailable on its own.

Retirement does not remove R2 and R12 from the planted defects. They are still
scored under pass condition 1. It removes them from pass condition 2, and no
future run may be read for position through them.

**The instrument is now two pairs.** Pair B has a recorded result and later runs
can be compared against it. Pair C is intact and unused. Repairing R12 is not
available: the fixture is frozen by a recorded run, and moving a pair member
into different company is the one edit this fixture cannot survive. A
replacement clean careful-side number belongs in a future fixture.

## Planted defects

Items marked **v2** were not planted. They were established by run 01 and are
now part of the key, on the precedent of fixture 01, where the run classed R8
blocking and the key was wrong.

### Defective section, R1 to R9

| Where | Smell | Class | Note |
|---|---|---|---|
| R1 "suspicious characteristics" | Subjective quality | Blocking | Nothing observable to check |
| R1 "where appropriate" | Unbounded condition | Blocking | The referral condition is left entirely open, then contradicted by R2 which states one |
| R1 evaluation of claims | Contradiction | Should fix | R1 has this system reviewing claims for characteristics. Context assigns that to the indicator service in US-061, and Out of scope excludes it |
| R2 "70" | Untraceable number | Consider | Pair A, retired. Value usable; source, basis and scale absent |
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
| Definitions, the state before a referral is taken | Undefined term | Should fix | **v2.** `under-investigation` and `closed` are named in the requirements. "Open" carries load in the Definitions, in R9 and in the story goal, and R4 creates a referral in a state with no name |
| R11 "exactly one outcome per referral" | Contradiction | Should fix | **v2.** R18 returns a closed referral to `under-investigation` for a further outcome on the same referral |
| R11, the recording investigator and the owner | Missing precondition | Should fix | **v2.** AC-2 sets up "owned by the acting investigator". R11 does not require it. R10 gained `already-owned`; the matching rule in R11 was never added |
| R12 "10 working days" | Untraceable number | Consider | Pair A, retired. Source and basis absent |
| R12 "recorded activity" | Undefined term | Should fix | **v2.** The escalation trigger turns on it and it is defined nowhere. R20 lists what history records and notes are not among them, though R7 introduces them. AC-3's Given cannot be constructed |
| R12, repeated escalation | Missing failure path | Should fix | **v2.** A referral past the limit stays past it, so the condition holds at every subsequent daily evaluation. Whether escalation fires once, once per owner, or every time is unstated |
| R12 "becomes accountable for its outcome" | Unverifiable outcome | Consider | **v2.** No expression in the system. Whether escalation changes the owner recorded under R10 is unstated, and whether the unit manager may record an outcome is unstated and doubtful, since R11 grants that to an investigator and Context says investigators do not decide claims |
| R17 notification on closure | Missing failure path | Should fix | **Pair B.** Stands out because R11, R14 and the amended R10 all name their refusals |
| R18 "adjuster", R19 "case owner" and "raised" | Terminology drift | Should fix | **Pair C.** R17 says *claims handler* four statements earlier |
| R18, the reopening window | Untraceable number | Consider | **v2.** Clause 6.1 is named and neither the value nor the anchor reaches the reader. Bait item 3 justifies R15 by the clause's own test being reproduced inline, and R18 does not meet that standard |

### Cross-cutting

| Finding | Class | Note |
|---|---|---|
| Acceptance criteria cover nothing from the first section | Should fix | AC-1 to AC-4 exercise R11, R12, R14 and R15. Referral creation, the threshold and the application of the hold have no criterion |

## Bait - should produce no findings

| Item | Why it looks like a defect | Why it is not |
|---|---|---|
| NFR-031 | A number in a requirements document | Value, percentile, condition, verification method and source are all present, and the alert sits below the requirement with the reason stated |
| R11 "material inconsistency" **as a term** | Reads as a judgment call | Defined in Definitions with a test: it changes the amount payable or the decision to pay, or it is not material. **Narrowed in v2:** this item covers only a finding claiming the term is undefined or unverifiable as a term. A finding about the form of R11's third sentence, that it states a fact rather than a rule with enforcement, is a real defect and is listed above |
| R15 "reasonable grounds to suspect" | The canonical unverifiable phrase | Quoted from claims policy clause 4.2, with the clause's own test reproduced inline |
| R13 "may reassign" | Optional behaviour | A permission granted to one named role, with a defined effect and a history entry |
| OQ-022 | An open question with no default | Under the current template an escalation naming who decides and when is a correct answer. The absence of a default is argued |
| "Working day" in R12 and AC-3 | An undefined unit | Defined in Definitions against a named calendar. Note that "recorded activity" in the same statement is a real defect, and a finding against it is not a finding against this bait |

A finding against any of these is a false positive.

## Deliberately borderline

The key states its position and the argument it rejected. Class drift cannot be
detected against a key that is vague about class.

| Item | Key's position | Argument rejected |
|---|---|---|
| R16, a decline recorded after closure | **Blocking, changed in v2.** The exception cannot fire when it is evaluated: R15 decides the decline on the recorded outcome, so at the moment of recording no decline exists. The clause is inoperative and money can leave on a claim heading for decline | That it is merely incomplete. v1 held that position and run 01 argued against it on the merits |
| R19 "monthly summary" | **Consider at most, and silence is acceptable.** No decision depends on its timing. Held in v2: run 01 classed it `should fix` and did not argue against the key | That an unspecified delivery day is Should fix. It would be, for a control. This is a report |
| R2, the missing score scale | **One finding or two, either is right. Consider.** | That the absent scale is Blocking on its own. Without the threshold it would be inert |
| R1 "The system" | **Should fix, but folding it into the R1 finding is not a miss** | That it is a separate Blocking actor defect. Context names the actors; R1 is loose rather than empty |
| The story statement, "claims carrying fraud indicators" | **Silence expected.** A finding here is tolerated and not counted either way | That stories are requirements. They are not, and reviewing them as such generates noise |

## Pass conditions

A run passes when all of the following hold.

1. Every planted defect is found, in both the defective and the careful sections
2. Both members of pairs B and C are found, and classed identically
3. No finding is raised against a bait item
4. Class assignments match this key, or a deviation is argued on the merits

**Merged reporting makes condition 2 unmeasurable, not passed.** A run that
reports both members of a pair in one row cannot give them different classes, so
identical classing is an artifact of the output format. Record the pair as
unmeasurable and say so. v1 lacked this rule, and run 01's pair A was recorded
as a pass by construction, which is a weaker statement than it looks.

## Reading a run for contamination

The layout puts the defective section first, so the careful sections are the
downstream half and that is where the effect shows.

**Contamination.** Findings raised against bait items in the careful sections,
or class inflation on a careful-side pair member.

v2 weakens this reading, and the weakening is real rather than a caveat. v1
treated a high volume of findings in the careful half as a contamination signal.
Six defects have since been established there. Extra findings downstream may now
be correct rather than inflated, and only the bait items and the class of pair
B's and C's careful-side members remain as clean signals.

**Saturation.** A pair member missed in the defective section while its twin is
found in a careful section. That is the opposite failure: the plant drowned
among its neighbours. Run 01 showed exactly this on pair B, with the relevant
lens demonstrably active in the same section.

**Both at once.** v1 wrote the two as alternatives. They are not. Run 01 showed
saturation upstream and a severity gradient running down the page, in one pass.

**Neither.** Pairs found and classed identically, bait untouched. Contamination
is not demonstrated by that run. It is not disproved either, and one run
supports no claim about frequency.

**Judge findings before counting them.** A finding that names a real defect not
listed here is a correction to this key, not a failure of the run. Fixture 01
established this: R8 was listed as clean, the run classed it blocking, and the
key was wrong. Adjust the skill only for findings that are wrong on the merits.

## Where this key and fixture 02's key disagreed

`fixture-02-answer-key.md` lists D3 at `should fix`: R11 against R3, whether
reassignment restarts the deadline or carries it over. v1 of this key declared
the same construct complete in R12 and R13.

**Resolved against this key.** Reassignment under R13 writes to referral
history, R12 turns on "recorded activity", and nothing says whether a history
entry counts. The gap fixture 02 planted deliberately is present here by
accident. It is recorded above as a v2 addition to R12.

Run 01 sided with the earlier key before either was read, because its comparison
was written after its assessment was fixed.

Two keys stating opposite things about one construct is not a scoring problem.
It is the same failure the repository already recorded for worked examples
inside skills against the templates they demonstrate: two copies of one piece of
knowledge, and nothing that compares them. Nothing compares keys either. This
section exists because a run found the contradiction, not because anything
looked for it.

## Provenance of this key

v1 was written by the author of the fixture, before any run, which its own text
called the weakest position a key can be written from. The correction arrived on
the first run, on six points, three of them on one statement, and one of them
destroyed a third of the instrument. That is a sharper result than the
prediction.

Six statements in the fixture were changed after the document was drafted and
before v1 was written: R2, R4 and R7 were reduced to one defect each so the
pairs would be readable, R10 and R12 were completed so that a concurrency gap
and an undefined escalation mechanism would not sit as unplanned noise in the
careful half, and R18 was trimmed for the same reason as R7. R12 is the
instructive one. It was edited specifically to be clean, by the person who then
certified it clean, and it was not.

## What this fixture does not test

- **Adaptation.** A defect placed after a long careful run, where the risk is a
  missed finding rather than an invented one. The defective section is first,
  so this layout cannot show it. A reversed-order run is a separate item
- **Frequency.** One run can show that contamination exists. It cannot show how
  often, and no claim about how often should be written from it
- **A clean careful-side untraceable number.** Pair A was the instrument for
  that and it is retired. The fixture can no longer offer one, and a future
  fixture has to
- **The author's response to findings.** A different instrument, and a
  different fixture
- **Author neutrality.** See provenance above
- **Triggering.** Run 01 named the skill in the request, as every run so far has

## One thing it does test that was not planned

`fixture-02-answer-key.md` lists behaviour with a supplied glossary as
untested. This document has a Definitions section, and three bait items depend
on it. Run 01 left the defined terms alone and still caught R7, R18 and R19,
and its attack on "recorded activity" was aimed at a term the document does not
define. That gap is partly closed. Partly, because the glossary here is
internal to the document rather than supplied separately.

## Known weakness of the fixture

The document reuses the structural furniture of fixture 02: append-only history,
the same non-functional constraints table shape, the same out of scope and open
questions tables, a payment or approval hold with a refusal error. A run may
recognise the shape rather than read the text.

Run 01 examined this and could not settle it, which is the correct outcome
rather than a failure of that run. Findings did line up with fixture 02 on D1,
D4 and partly D3, and against documents this alike, agreement is what an honest
reading also produces. Separating reading from recognition needs an instrument
that does not exist: a document of the same form with no defects in it, or this
fixture with its sections and pairs shuffled, so that repeating fixture 02's
findings would mean repeating them somewhere they do not belong.
