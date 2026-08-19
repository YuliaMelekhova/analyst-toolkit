# Fixture 03 - run record 01

**Date:** 2026-08-18
**Fixture:** `fixture-03-siu-referral.md`
**Skill:** `requirements-smell-detector`
**Commit:** `811187149d070c286e250c19f540c1ff76fefb6b`, branch `main`, clean tree
**Skill parity:** the installed skill and the repository copy of `SKILL.md` are
byte-identical, sha256 `b1cd992aa3945c88431b52f19fb895f6ffd9f8d79a9b293783722bf293f3c4ac`,
12293 bytes
**Outcome:** does not pass. 16 of the key's 20 planted defects found, 4 missed,
all four in the defective section or cross-cutting. Pair B shows saturation as
the key defines it: the defective-section member was missed and named clean
while its careful-section twin was found. Every finding beyond the key's list
falls in the careful half. Six of those additions are real defects the key does
not contain, so this run also corrects the key.

---

## How the run was made

The reviewing context was kept separate from the context that knows this is a
fixture. The run was performed by a subagent with no prior conversation, given
one file and a review request, and nothing else.

| Property | Value |
|---|---|
| Input | one file, copied out of the repository to `inbox/siu-referral.md` |
| Input integrity | sha256 `a7a349d56f58bf51ca7a2ee95a23b62dcc0925e3295c23d56bc59374d92b6c43`, identical to `fixture-03-siu-referral.md`, 190 lines, 8600 bytes |
| Framing given to the reviewer | a colleague sent a requirements document and wants it reviewed before it goes to the delivery team |
| Words not used in the request | fixture, test, key, repository, release, version |
| Path and filename | neutral. `fixture-03-` prefix stripped, no repository path visible |
| Restriction | read that one file only, no listing, searching or looking for related documents |
| Tool calls made by the reviewer | 2, consistent with the skill invocation and one file read |
| Skill invocation | requested explicitly by name, so this run does not test triggering |
| Read before the run | neither `fixture-03-answer-key.md` nor `tests/README.md`. Only filenames, via a directory listing |

The reason for the separation: the context that planned this run knows the
document is a fixture from this repository, knows a key exists and knows that
fixture 01 produced a key correction. None of that is available to a reviewer
receiving a document from a colleague, and all of it disposes a reviewer to hunt.

What this arrangement does not remove: the reviewer was told which skill to use,
and a reviewer told to apply a smell detector is already looking for smells.

---

## Run output, verbatim

Reproduced without edits. The em-dashes below are the skill's own output and are
left in place, on the same grounds that exclude the two smell-detector fixtures
from the repository's em-dash pass: editing a recorded run makes it not a
record. See the NOTES entry for this run.

## Findings

| # | Quote | Smell | Class | Suggested reformulation |
|---|-------|-------|-------|------------------------|
| 1 | "A claim version has at most one open referral." | Terminology drift | Should fix | The state set is never stated in full. Only `under-investigation` and `closed` are named in the requirements, yet "open" carries load in this definition, in R9 and in the story goal. Name the state a referral holds between creation (R4) and being taken (R10), and state which states count as open. |
| 2 | "refers it to the investigations unit where appropriate" | Unbounded quantifier | Should fix | "Where appropriate" and "suspicious characteristics" give no boundary, and R1 restates R2 and R6 more loosely than either. State the referral set once, in one rule, or drop R1 as a narrative summary and let R2 carry the rule. |
| 3 | "A claim scoring above 70" | Untraceable number | Consider | 2 occurrences of a threshold with no origin: "above 70" here and "10 working days" in R12. Both are usable as values; neither can be renegotiated when it proves costly. Record where each came from and who agreed it, as NFR-031 and NFR-034 do. The score's scale and range are also undocumented, so 70 cannot be sanity-checked. |
| 4 | "the investigations unit is notified and the claim is put on hold" | Compound requirement | Should fix | Split. Notification and hold fail independently. Also name who in the unit is notified (the queue, the unit manager, every investigator), and reconcile "the claim is put on hold" with R14, which holds payment only — a claim-level hold and a payment-level hold are different scopes. |
| 5 | "Since every claim has a completed indicator profile by the time it is registered" | Implicit assumption | Should fix | Asserted with no source, and it is the sole justification for the absence of a score-unavailable path. Either cite the US-061 guarantee that makes it true or state the behaviour when it is false. Two further problems ride along: "indicator profile" is a third name for the indicator service output ("a set of named indicators and a score"), and "registered" and "first notification of loss" are used as one moment without saying they are. |
| 6 | "High-value claims are always reviewed by the unit." | Unbounded quantifier | Blocking | "High-value" has no definition anywhere in the document, so the set has no members an implementer can compute, and the rule contradicts R2, which sends a claim scoring 70 or below on without a referral. A high-value claim scoring 40 is covered by both rules with opposite results. State the value threshold, its currency and its source, and state how it composes with the score rule (either rule refers, or both must hold). |
| 7 | "The handler adds a note explaining why the case was raised." | Unverifiable outcome | Should fix | Referrals are created automatically on score (R2), so nothing here says when the note is added, whether it is mandatory, whether its absence blocks anything, or why a handler explains a referral the platform raised. Contrast R11, where the equivalent obligation has a refusal and an error code. This also depends on OQ-021: handler-requested referrals are the only case where "why the case was raised" is the handler's to answer. |
| 8 | "The handler" | Terminology drift | Should fix | 4 names for what may be one role: "claims handler" (story, R17), "The handler" (R7), "The adjuster" (R18), "The case owner" (R19). "Adjuster" is the most consequential — it appears once, grants the reopen permission, and matches none of the roles established in Context (claims handler, investigator, unit manager, claims manager), so the permission cannot be assigned. Fix the role vocabulary against Context and use one name per role. |
| 9 | "stored with the investigation" | Terminology drift | Should fix | Objects drift the same way as roles: "the investigation" and "the case" (R7) against the defined term Referral, which is the only record the document specifies. Either define investigation and case as distinct records or use Referral throughout. |
| 10 | "Referrals are processed quickly." | Subjective quality | Should fix | Cannot pass or fail. NFR-031 bounds creation and queue visibility, not processing, so no existing value covers this. Either add processing time to the NFR table with a value, condition, verification method and source, or delete the sentence — leaving it invites a second, unmeasured interpretation of the same latency. |
| 11 | "A daily CSV export of open referrals is placed on the shared drive" | Solution in disguise | Should fix | Format, medium, destination and frequency are fixed before the need is stated, and the stated need — "so the unit can see what is waiting" — is already met by the queue in R4. State who consumes the export and why the queue does not serve them; if it survives that, state the run time, the destination and the actor. |
| 12 | "records exactly one outcome per referral" | Unverifiable outcome | Should fix | Two defects. First, this does not require the recording investigator to be the owner, while AC-2 sets up "owned by the acting investigator" — say whether a non-owning investigator can record an outcome. Second, "exactly one per referral" contradicts R18, which returns a closed referral to `under-investigation` for a further outcome; state the constraint per open period rather than per referral, or say that reopening supersedes the earlier outcome. |
| 13 | "A rationale of `substantiated` states the material inconsistency the outcome rests on." | Unverifiable outcome | Should fix | Nothing observable to check against. Say whether this is free text, a structured reference to the two conflicting statements of fact the definition names, or a judgement made at review, and state the refusal path if the rationale does not do it — the neighbouring rule has `rationale-required` and this one has nothing. |
| 14 | "no recorded activity for 10 working days" | Terminology drift | Should fix | "Recorded activity" is never defined although the escalation rule turns on it. Enumerate what resets the clock: a history entry, a note, a reassignment under R13, a mere view. AC-3's Given cannot be set up without this, so the criterion is not independently checkable as written. |
| 15 | "becomes accountable for its outcome until it is reassigned" | Unverifiable outcome | Should fix | Accountability has no expression in the system. Say whether escalation changes the owner recorded under R10, whether the state changes, whether the original investigator keeps the referral, and whether the unit manager may record the outcome — R11 grants that to an investigator, and Context says investigators do not decide claims. |
| 16 | "a referral past the limit at the time of evaluation is escalated then" | Missing failure path | Should fix | A referral past the limit stays past it, so every subsequent daily evaluation matches the same condition. State whether escalation fires once per referral, once per owner, or on every evaluation until activity resumes, and whether an already-escalated referral is exempt. |
| 17 | "While a referral is `under-investigation`, payment on the claim is held" | Unverifiable outcome | Should fix | Contradicts R3, which puts the claim on hold when the referral is created — before any investigator takes it. Under R14 alone, a referred but untaken claim can be paid, which defeats the story's stated purpose that "payment is not released while a concern is open". AC-1 tests only the `under-investigation` case, so the gap would pass. State the hold's start point once, and reconcile R3, R14 and AC-1 to it. |
| 18 | "unless the claims manager has declined the claim under R15" | Unverifiable outcome | Blocking | The exception cannot fire when it is evaluated. R15 states the decline is decided on the investigator's recorded outcome, so at the moment the outcome is recorded no decline can yet exist — the hold is always released, and nothing in the document holds payment during the interval between outcome and decline. State what governs payment in that interval: the hold persists past closure pending the claims manager's decision, or it is reinstated on decline, and by whom. |
| 19 | "within the reopening window set by the claims policy clause 6.1" | Unverifiable outcome | Consider | The source is named but no value or unit reaches the reader, unlike R15, which quotes the substance of clause 4.2. Say whether the window runs in calendar or working days, whether it runs from closure or from the outcome, and whether reopening is available once a decline under R15 exists. |
| 20 | "receives a monthly summary of the referrals raised on their claims" | Unverifiable outcome | Should fix | No anchor and no channel: calendar month end or rolling 30 days, in-platform or by email, and what is sent to a recipient with no referrals that month. The recipient is also the undefined "case owner" (finding 8). Nothing here is checkable until those are fixed. |
| 21 | — | Missing failure path | Should fix | Cross-cutting, a dependency does not answer: no behaviour when the indicator service is unavailable, errors, or returns a claim version with no score. R5 asserts this cannot happen but the Out of scope table places only the production of the score in US-061, not its consumption here. The same gap applies to the shared-drive write in R9. |
| 22 | — | Missing failure path | Should fix | Cross-cutting, the action is repeated: the Definitions state the invariant of at most one open referral per claim version, but no requirement says what happens when a second creation is attempted — after a re-score, a replayed event, or a handler request under OQ-021. R10 and R14 both name an error for their refused case; this one names none. |
| 23 | — | Missing failure path | Should fix | Cross-cutting, state changes mid-flight: the score and the referral are scoped to a claim version while the payment hold in R14 is scoped to the claim. Nothing says what happens when a new claim version is created while a referral is open on the previous one — whether the referral carries over, whether a second referral may be raised, or whether the hold follows the claim. |
| 24 | — | Missing failure path | Should fix | Cross-cutting, state changes between read and write: no rule for a reassignment under R13 landing while the previous owner is submitting an outcome under R11, nor for a payment instruction in flight when the hold is released under R16. Which action wins is undefined in both cases. |
| 25 | — | Missing failure path | Should fix | Cross-cutting, the assigned party becomes unavailable: R18 assigns a reopened referral to the investigator who recorded the outcome, with no path when that investigator has left, lost access or been deactivated. R13 covers only a reassignment the unit manager chooses to make, and R12's safety net takes 10 working days. R17 has the same gap for a notified handler no longer on the claim. |
| 26 | — | Missing failure path | Consider | Cross-cutting, configuration changes mid-flight: not stated whether the 70 threshold binds at evaluation time only, and whether the business calendar and published holiday list are read once at referral creation or re-read at each daily evaluation. A holiday added mid-flight moves an in-flight escalation date under R12. |

## Summary

- Statements reviewed: 29 (R1-R20, AC-1 to AC-4, NFR-031, NFR-034, and the three Definitions)
- Findings: 2 blocking, 21 should-fix, 3 consider
- Most frequent smell: Unverifiable outcome, 8 occurrences
- Cleanest statements: R4, R10, R13, R15, R20, AC-1, AC-2, AC-4, NFR-031, NFR-034, and the definitions of Material inconsistency and Working day. R10, R14 and R20 are the model the rest of the document should follow: named actor, named state, explicit refusal with an error code, and a history entry with actor and timestamp. Both NFRs carry a value, a condition, a verification method and a source, and NFR-031's alert threshold is explained rather than asserted. The open questions each name an owner, a deadline and either a default or the reason no default is safe.
- Not reviewed: nothing skipped. Assumptions stated in place of unanswered context questions: reviewed as a pre-approval, ready-for-delivery user story on the strength of "Status: in-review", so gaps are reported as defects rather than as expected first-draft looseness; no glossary or convention outside this file was consulted, as instructed, so terminology findings are judged against the document's own Definitions section and Context; the referenced external documents (US-061, claims policy clauses 4.2 and 6.1, records retention policy v4) were unavailable and their contents taken at face value.

End of verbatim output.

---

## Assessment against the key

### Pass conditions

| Condition | Result |
|---|---|
| 1. Every planted defect is found, in both sections | **Fail.** 16 of 20 found. All four misses are in the defective section or cross-cutting |
| 2. Both members of every pair are found, and classed identically | **Fail.** Pairs A and C pass. Pair B fails: R4 missed, R17 found |
| 3. No finding is raised against a bait item | **Fail as written.** One finding, #13, is raised against the R11 bait. Disputed below |
| 4. Class assignments match the key, or a deviation is argued on the merits | **Partial fail.** Three items the key classes `Blocking` were classed `Should fix` with no deviation stated: R1, R5's implicit assumption, R8 |

### Planted defects, found and missed

Defective section, R1 to R9.

| Key item | Key class | Run | Result |
|---|---|---|---|
| R1 "suspicious characteristics" | Blocking | #2 | Found, folded into a single R1 finding. Class `Should fix` |
| R1 "where appropriate" | Blocking | #2 | Found, same finding. Class `Should fix` |
| R1 evaluation of claims, against Context and Out of scope | Should fix | - | **Missed.** #2 notes only that R1 restates R2 and R6 loosely, not that it assigns work owned by US-061 |
| R2 "70" | Consider | #3 | Found. Class matches. The absent scale is folded in, which the key permits |
| R3 notify and hold | Should fix | #4 | Found. Class matches |
| R3 both clauses, nothing performs them | Should fix | - | **Missed.** #4 asks who is notified, which is the recipient, not the actor |
| R3 "the claim is put on hold" | Should fix | #4 | Found. Class matches |
| R4 queue placement | Should fix | - | **Missed**, and R4 is named among the cleanest statements |
| R5 "Since every claim has a completed indicator profile" | Blocking | #5 | Found. Class `Should fix` |
| R5 registration and first notification conflated | Should fix | #5 | Found, folded. Class matches |
| R6 "High-value" | Blocking | #6 | Found. Class matches |
| R6 "always" | Should fix | #6 | Carried by the smell label only, folded into the `Blocking` finding |
| R6 against R2 | Blocking | #6 | Found. Class matches |
| R7 "case", "raised", "investigation" | Should fix | #9 | Found. Class matches. "raised" is not named as drift |
| R8 "processed quickly" | Blocking | #10 | Found, labelled `Subjective quality` rather than `Unverifiable outcome`. Class `Should fix`. The reasoning reproduces the key's own note about NFR-031 |
| R9 daily CSV export | Should fix | #11 | Found. Class matches |

Careful sections, R10 to R20.

| Key item | Key class | Run | Result |
|---|---|---|---|
| R12 "10 working days" | Consider | #3 | Found, in the same row as R2. Class matches |
| R17 notification on closure | Should fix | #25 | Found. Class matches |
| R18 "adjuster", R19 "case owner" | Should fix | #8 | Found. Class matches |

Cross-cutting.

| Key item | Key class | Run | Result |
|---|---|---|---|
| Acceptance criteria cover nothing from the first section | Should fix | - | **Missed.** AC-1, AC-2 and AC-4 are named among the cleanest statements. #17 observes that AC-1 would pass despite the hold gap and does not generalise from it |

Totals: 16 found, 4 missed. Every miss is upstream, in the defective section or
in the cross-cutting row. No planted defect in the careful sections was missed.

### Pairs

| Pair | Defective side | Careful side | Verdict |
|---|---|---|---|
| A, untraceable number, `Consider` | R2 found | R12 found | Pass. Both in one finding, both at `Consider`, so classed identically by construction |
| B, missing failure path, `Should fix` | R4 **missed** | R17 found | **Fail. Saturation**, exactly as the key defines it |
| C, terminology drift, `Should fix` | R7 found | R18 and R19 found | Pass. Both at `Should fix` |

Pair A passing by construction is worth noting as a limitation of the reading
rather than a result. The run reported R2 and R12 in one row, so the class could
not diverge. The instrument measures class divergence between pair members, and
a run that merges the members makes that measurement unavailable.

Pair B is the load-bearing failure of this run. The missing-failure-path lens was
not absent: #21 applies it to R5 and to the shared-drive write in R9, both in the
defective section, and #25 applies it to R17 in a careful section. The lens was
on, aimed at the same section, and R4 was still missed and then named clean. That
is the plant drowning among its neighbours rather than a technique the skill does
not have.

### Bait

| Bait item | Result |
|---|---|
| NFR-031 | Clean. Cited in #10 as not covering processing, which is the key's own reasoning |
| R11 "material inconsistency" | **Finding raised**, #13. Disputed, see below |
| R15 "reasonable grounds to suspect" | Clean, and named among the cleanest statements |
| R13 "may reassign" | No finding against the permission. #24 raises a concurrency interaction between R13 and R11, which is not this bait's failure mode |
| OQ-022 | Clean |
| "Working day" in R12 and AC-3 | Clean. #14 attacks "recorded activity", a different term that the document does not define |

### Deliberately borderline

| Item | Key's position | Run | Verdict |
|---|---|---|---|
| R16, a decline recorded after closure | Real gap, `Should fix` | #18 at `Blocking`, with an argument | Deviation argued and defensible. The clause cannot be true when it is evaluated, so it is inoperative rather than merely incomplete, and money can leave on a claim heading for decline |
| R19 "monthly summary" | `Consider` at most, silence acceptable | #20 at `Should fix` | Run takes the position the key explicitly rejects, and does not argue against it. Key right |
| R2, the missing score scale | One finding or two, `Consider` | Folded into #3 at `Consider` | Matches |
| R1 "The system" | `Should fix`, folding acceptable | Not raised separately | Acceptable under the key |
| The story statement | Silence expected | No finding | Matches |

### Reading for contamination

The signal the key designates is clean. R12's number is classed at `Consider`,
the same as R2's, with no inflation on the careful-side member.

The distribution is another matter.

- All four misses are in the defective section or the cross-cutting row. The
  careful sections lost nothing: 12 of 16 upstream, 3 of 3 downstream
- Every finding beyond the key's list falls in the careful half. Findings 1, 12
  to 20 and 22 to 26 all sit at or after R10
- Three of the four `Blocking` items in the defective section were classed
  `Should fix`, while the one careful-section item the key classes `Should fix`
  was classed `Blocking`. The severity gradient runs the wrong way across the
  document
- One outright class miss downstream: #20 on R19, at the class the key argues
  against

So this run shows both failure modes at once, in the directions the key predicts
for each. Saturation upstream, where the plants were folded, under-classed and in
one case lost. Mild inflation downstream, where the volume of findings and one
class assignment exceed what the key anticipated. The key treats the two as
alternatives. They are not.

One run supports no claim about how often either happens, which is the key's own
position and is repeated here rather than argued.

### Where the key is wrong

The key invites this: a finding that names a real defect not listed is a
correction to the key. Fixture 01 set the precedent. Six corrections follow, and
three of them land on R12, which matters because R12 is the careful-side member
of pair A and extra findings there would otherwise read as contamination.

**R12 is not complete apart from its number.** The key states that everything
else in R12 is present: actor, trigger, consequence, accountability. Three of the
run's findings against R12 hold up.

- #14. "Recorded activity" is never defined, and the escalation trigger turns on
  it. The document defines Referral, Material inconsistency and Working day, and
  not this. R20 lists what is written to referral history and notes are not among
  them, though R7 introduces them. AC-3's Given cannot be constructed
- #16. R12 settles when inactivity is evaluated and not whether escalation
  repeats. A referral past the limit stays past it, so the condition holds at
  every subsequent daily evaluation. Idempotency is unstated
- #15. "Becomes accountable for its outcome" has no expression in the system.
  Whether escalation changes the owner recorded under R10 is unstated, and
  whether the unit manager may record the outcome is unstated and doubtful, since
  R11 grants that to an investigator and Context says investigators do not decide
  claims. `Consider` fits this better than `Should fix`, but the gap is real

**R11 "exactly one outcome per referral" contradicts R18.** Reopening returns the
referral to `under-investigation` and assigns it to the investigator who recorded
the outcome, who then records a second outcome on the same referral. The key
contains no such item. #12 is right.

**R11 does not require the recording investigator to be the owner.** AC-2 sets up
"owned by the acting investigator", so the criterion assumes a precondition the
requirement does not state. The key records that R10 was completed with
`already-owned`; the matching precondition in R11 was not added. #12 is right.

**R18's reference to claims policy clause 6.1 carries no value.** Bait item 3
justifies R15 precisely because the clause's own test is reproduced inline. By
that standard R18's bare citation is weaker, and neither the unit nor the anchor
of the reopening window reaches the reader. #19 at `Consider` is right, and the
key does not contain it.

**The Definitions do not name the state a referral holds before it is taken.**
`under-investigation` and `closed` are named in the requirements, while "open"
carries load in the Definitions, in R9 and in the story goal, and R4 creates a
referral in an unnamed state. #1 is right.

**Bait item 2 is worded too broadly.** The bait inoculates against the argument
that "material inconsistency" is an undefined judgement call. #13 does not make
that argument: it cites the definition, and names something else, that the third
sentence of R11 is written as a statement of fact rather than as a rule with
enforcement, so a non-empty rationale that fails to state the inconsistency has
no refusal path while an empty one has `rationale-required`. That is a real
defect in the form of the requirement. Two changes follow: the bait should be
narrowed to a finding claiming the term is undefined or unverifiable as a term,
and the run's class should be `Consider` rather than `Should fix`. Counted as
written, this is a false positive; counted on the merits, it is a thin but real
finding at the wrong class.

Of the run's other unlisted findings, #22, #23 and #24 name real gaps the key
does not cover, #21 is right about R5 and R9 while missing the one plant of that
type in the same section, and #26 is a stretch.

### Where the run is wrong

**R4.** The worst of it. Missed and then named among the cleanest statements,
while the same lens was applied twice elsewhere in the same section. A statement
listed as clean is a stronger claim than a statement not mentioned, and it is
wrong here.

**R1's contradiction with Context and Out of scope.** R1 has this system
reviewing claims for suspicious characteristics. Context assigns that to the
indicator service in US-061 and the Out of scope table excludes it. Not raised.

**R3's missing actor.** #4 asks who is notified. Nothing asks what performs the
notification or the hold. Both clauses are passive and nothing in R3 acts.

**Acceptance criteria coverage.** AC-1 to AC-4 exercise R11, R12, R14 and R15.
Referral creation, the threshold and the application of the hold have no
criterion at all. #17 noticed that AC-1 would pass despite the hold gap and drew
no general conclusion, and AC-1, AC-2 and AC-4 were then named clean.

**R19 at `Should fix`.** The key's argument is better. An unspecified delivery
day would be `Should fix` for a control and this is a report, and no decision
depends on its timing.

**R5 at `Should fix`.** The key is right that this is `Blocking`. The whole
statement rests on an unsourced assumption, and if the assumption is false there
is no path for an implementer to write.

**R1 and R8 at `Should fix` without stating the deviation.** Both are defensible
on the merits: R1 can be dropped as narrative once R2 carries the rule, and R8
can be deleted rather than bounded, so neither strictly blocks. Condition 4
allows the deviation and requires it to be argued as one. It was not.

**Three internal contradictions in the run's own summary.** R14 is named among
"the model the rest of the document should follow" while #17 is a finding against
R14. R13 is named among the cleanest statements while #24 is a finding against
the interaction of R13 and R11. R4 is named among the cleanest statements and is
a planted defect. A cleanest-statements list that contradicts the findings table
above it is worse than no list.

**"Not reviewed: nothing skipped."** The count of 29 statements excludes the Out
of scope and Open questions tables, which the run relies on in #21 and #22. They
were read and not counted, which is not nothing skipped.

---

## Similarity to fixture 02

Read after the assessment above was written and fixed, together with
`tests/README.md`, `fixture-02-approval-routing.md` and
`fixture-02-answer-key.md`. The fixture 03 key names this as the fixture's known
weakness: if findings line up suspiciously well with fixture 02, suspect that the
run recognised the shape rather than read the text.

### What lines up

| Fixture 02, established | Fixture 03, this run | Comment |
|---|---|---|
| D1, one party under three names: reviewer, approver, assignee | #8, one party under four names: claims handler, handler, adjuster, case owner. #9, one record under three names: referral, case, investigation | Same defect, same class, transposed onto different vocabulary |
| D4, cross-cutting: no behaviour for an assignee losing access or leaving while a step is pending. Found in fixture 02's run 3 only, after the cross-cutting scan was added to the skill | #25, "the assigned party becomes unavailable", covering R18 and R17 | Same category, and the vehicle by which the pair B careful-side member was found |
| D3, R11 against R3: reassignment does not state whether the deadline restarts or carries over | #14 lists "a reassignment under R13" among the candidates for what resets the inactivity clock. #24 raises the R13 against R11 race | Partial. The same question reaches the same construct, arriving through the undefined term rather than through the reassignment rule |
| Bait: NFR-012 and NFR-018, OQ-014, the Out of scope table, "active" defined in Context, the append-only rule reconciled with retention | The parallel items in fixture 03 all left alone: NFR-031, NFR-034, OQ-022, Out of scope, the Definitions, R20 with NFR-034 | Consistent across four runs and two documents |

### What does not line up

The two documents each carry a statement that fixes when a periodic evaluation
decides, written in almost the same words: fixture 02's R4 for expiry, fixture
03's R12 for escalation. Fixture 02's key lists R4 as bait and all three of its
runs left it alone. This run did not leave R12 alone: #16 attacks it for not
saying whether escalation repeats.

That difference tracks a real difference in the two texts. In fixture 02 expiry
moves the request to `expired`, so the condition stops holding after it fires. In
fixture 03 escalation notifies the unit manager and leaves the referral in
`under-investigation`, so the condition holds at every subsequent evaluation. The
run treated the two differently in the one place where treating them the same
would have been wrong.

A second point in the same direction. Fixture 02's key treats the reassignment
clock question as a genuine planted defect, D3 at `Should fix`. Fixture 03's key
declares R12 and R13 complete. The two keys disagree with each other about the
same construct, and this run sided with the earlier key against the later one.

### The limitation, stated plainly

Similarity of findings does not demonstrate that the run recognised the
structure. The two fixtures are genuinely alike in construction: the same
append-only history, the same non-functional constraints table with the same five
columns and the same explained alert gap, the same out of scope and open
questions tables, a hold or an expiry with a named refusal error, a periodic
evaluation whose timing is pinned, and an internally defined term used as bait.
Two of the three smell types planted in fixture 03 are the smell types planted in
fixture 02, which is the author's choice and not the run's.

Against documents that alike, agreement between runs is what an honest reading
produces. The same evidence supports both explanations and cannot separate them.
Separating them needs a different instrument: a document of the same form with no
defects in it, or the same content with the pairs and the section order shuffled,
so that a run repeating fixture 02's findings would be repeating them somewhere
they do not belong. That is separate work and it has not been done.

This section therefore records a suspicion and not a conclusion. Nothing in it
should be cited as evidence that this run read the text, or that it did not.

---

## What this suggests about the skill

Six items. None fixed here, and the fixture was not edited.

1. **The cross-cutting scan behaves as a checklist satisfied by its first fitting
   statement, rather than as a sweep across all of them.** Findings 21 to 26 read
   as one pass over a fixed category list. #21, "a dependency does not answer",
   names R5 and the shared-drive write in R9 and does not name R4, which is the
   planted case of that category in the same section. The lens was on, aimed at
   the right section, and the plant was missed. The scan needs either a
   requirement to name every statement matching a category, or an explicit
   statement that it checked the others and found nothing.
2. **Class calibration moves with position in the document.** Three of the four
   `Blocking` items in the defective section were classed `Should fix`, while the
   one careful-section item the key classes `Should fix` was classed `Blocking`.
   Nothing instructs the reviewer to assign a class independently of the
   neighbourhood, or to re-check the classes as a set once the pass is complete.
   This is the calibration failure fixture 02's key lists, showing up as a
   gradient rather than as a single inflated finding.
3. **The cleanest-statements list is not reconciled against the findings table.**
   R4, R13 and R14 appear in both. Naming a statement clean is a stronger claim
   than not mentioning it, and R4 is a planted defect. The list should be
   produced by excluding everything named in the findings, and should be checked
   against them before output.
4. **Merging repeated occurrences of one smell into a single finding destroys the
   per-statement record.** #3 covers R2 and R12 in one row. On this fixture that
   made pair A unmeasurable, because two statements reported in one row cannot
   receive different classes. One finding per statement, even when the smell
   repeats, or a required list of every location with its own class.
5. **Nothing asks which requirements have no acceptance criterion.** Each
   criterion was read on its own and #17 even observed that AC-1 would pass
   despite the hold gap, without generalising. Coverage in the other direction,
   from requirements to criteria, is not part of the pass.
6. **The line between an illustrative number and an invented value is not
   drawn.** #6 argues the R2 and R6 contradiction using a claim scoring 40. That
   number is not in the fixture. It is an illustration and not a proposed
   threshold, so it is not the invented value that fixture 02's key calls the most
   serious failure, but the key gives no rule that distinguishes the two, and a
   reader skimming the reformulation column cannot tell which they are looking
   at.

Two things fixture 02's key names as its primary signals held on this run. No
readiness or approval verdict was issued. No value, threshold or interval was
proposed that is not in the text, subject to item 6 above.

One observation about the method rather than the skill. Fixture 01's key was
wrong about R8, fixture 02's key was rewritten twice against its own runs, and
this run corrects fixture 03's key on six points. Three keys, three corrections,
each written by the author of the fixture before any run. The fixture 03 key
predicts this in its own provenance section and calls it the weakest position a
key can be written from. On the evidence so far it is not a weakness that some
keys have.
