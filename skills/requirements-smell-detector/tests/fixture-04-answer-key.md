# Answer key - fixture-04

Not for the agent. Read after a run, never before.

This fixture tests whether form is accepted in place of substance. Fixture 01 is
visibly bad, fixture 02 is genuinely careful, fixture 03 is uneven. None of them
is dressed as careful while being empty, and that is the case this one covers.

The document is a business requirements document with every section of
`framework/templates/brd.md` present and every table filled. The prose is
competent, no requirement uses the words *quickly* or *where appropriate*, and a
reader skimming the headings should conclude the document is in good order. The
emptiness is structural.

## The twins

Two files with the same requirements text.

| File | Carries |
|---|---|
| `fixture-04-ecl-reporting.md` | Version number, approved status, owner, change history, approvals table |
| `fixture-04-ecl-reporting-unsigned.md` | None of it |

**They differ in two places and nowhere else.** The twenty-line block in the
header, and one sentence at the head of section 9: the signed twin reads
*Accepted at sign-off and recorded here rather than left implicit*, the unsigned
twin reads *Recorded here rather than left implicit*. That sentence is approval
apparatus in prose rather than in a table. Leaving it in the unsigned twin would
have made that document refer to an approval it does not show, and a run
flagging the inconsistency would produce a difference between the twins that has
nothing to do with the effect being measured.

Everything from section 1 to section 12 is otherwise byte-identical, and the
unsigned twin was produced from the signed one by script rather than by hand.

## The instrument: inverted bait

Four shapes were bait in fixtures 02 and 03, and were left alone in every run of
both documents. This fixture puts the same shapes in the same places and hollows
them out.

| Shape | Bait in | Here |
|---|---|---|
| Five-column non-functional constraints table | Fixture 02 NFR-012 and NFR-018, fixture 03 NFR-031 and NFR-034 | Section 8, all three rows |
| Gherkin acceptance criteria | Fixture 02 AC-1 to AC-4, fixture 03 AC-1 to AC-4 | AC-1 and AC-2 |
| Open questions table with an `If unanswered` column | Fixture 02 OQ-014, fixture 03 OQ-021 and OQ-022 | Section 11, all four rows |
| An internally defined term | Fixture 03 *material inconsistency*, *working day* | Section 4, three of five entries |

The five-column constraints table is a deliberate departure from the BRD
template, which specifies three columns. Shape identity with the earlier bait is
the instrument; using the template's own table would test nothing the other
fixtures do not.

**If the skill reads, it finds these. If the skill recognises the shape, it
passes over them.** This is the question run 01 of fixture 03 could not settle,
and its similarity section named the two instruments that would settle it as
non-existent. This is a third and cheaper one: the same form with the defects
placed where the bait used to be.

Its limit, recorded before the run rather than after. Finding these items is
evidence of reading. Missing them is consistent with recognition and also with
their being genuinely hard, and one run separates neither.

## Planted defects

### The acknowledged gaps, section 9

The device this fixture is named for. An acknowledgement closes nothing.

| Item | Class | Note |
|---|---|---|
| Staging rules not agreed, *the process will follow the rules current at the time of each close* | Blocking | The calculation basis may differ between two closes with nothing controlling the change. R2 defers to these rules and Stage is defined by them |
| Reconciliation out of scope, *the reconciling items are understood by the team* | Should fix | The knowledge is in one place and it is not the document. Contradicts R9 |
| No threshold for post-model adjustments, *existing practice will continue* | Should fix | The same gap appears in R6, in the third assumption, and in OQ-032 and OQ-034, and is unresolved in all five places |
| Retail portfolio not covered | - | Not a defect. Consistent with the out of scope table, and it is here to make the other three look like the same kind of statement |

A run that treats any of the first three as handled because they are
acknowledged has failed this fixture in the way it was built to catch.

### Contradictions

| Item | Class | Note |
|---|---|---|
| R9 requires reconciliation to the general ledger; section 3 places it out of scope and section 9 repeats that | Blocking | A requirement that the scope forbids |
| R1 covers *all in-scope portfolios*; OQ-031 asks which portfolios are in scope and defaults to *as per current practice* | Should fix | Current practice is the process being replaced |

### Hollow citations

| Item | Class | Note |
|---|---|---|
| *Group Accounting Policy 12.4* as the source of authority for six unrelated rules | Should fix | Cited six times, reproduces nothing. Compare R7, which carries its source's content into the sentence |
| *IFRS 9* as the authority for the stage 2 migration trigger, *IFRS 7, IFRS 9* for R10 | Should fix | A standard named as the authority for a specific rule, with nothing of the rule in it |
| *Governance Framework* in R4 | Should fix | Named, never quoted, and it is the control that releases the pack |
| *Material*, defined as material where its omission would be material to the reader, citing Group Materiality Standard clause 3.1 | Should fix | The citation reproduces a test that is itself vacuous. This is the middle of the scale fixture 03 established by accident: R15 reproduced a usable test and was left alone, R18's bare citation was attacked. Secondary: the term is defined and then never used anywhere in the document |

### Circular and deferred definitions

| Item | Class | Note |
|---|---|---|
| *Significant increase in credit risk*: an increase in credit risk since initial recognition that is significant | Blocking | Restates the term. It is the trigger for R3 and it decides which exposures move to stage 2 |
| *Stage*: classification in accordance with the staging rules | Blocking | Defers to an artifact that does not exist and that section 9 says is not agreed |

### Requirements that describe process rather than behaviour

| Item | Class | Note |
|---|---|---|
| R4, *reviewed and approved in line with the governance framework* | Blocking | No actor, no observable outcome, no refusal path, and it is the release control. OQ-033 asks who signs off in one absence case and answers *Team to decide* |
| R6, *documented and approved before they are applied* | Should fix | No actor, no threshold, no form of documentation |
| R10, *in accordance with the applicable standards* | Should fix | The set is unbounded and nothing is checkable |
| R2, *in accordance with the staging rules* | Blocking | Defers to the undefined artifact. Listed here as well as under definitions because the rule and the definition fail independently |
| R5, *sourced from the group data warehouse* | Should fix | Rests on the first assumption in section 7, which is asserted with no source |

### The hollow tables

| Item | Class | Note |
|---|---|---|
| Section 1 evidence table, all three rows | Should fix | The `What it shows` column restates that a problem exists. No frequency, no quantity, no specific failure |
| Section 2 measures table, rows 1 and 2 | Blocking | Current is *not currently measured*, target is *improved* and *reduced*, measured how is *via the new process*. No baseline, no target, no method, so the outcome cannot be evaluated at any point |
| Section 7 assumptions table, all three rows | Should fix | `If it turns out false` and `How we would find out` are filled with non-answers: *would need to be revisited*, *during implementation*, *through review* |
| Section 7, third assumption: *current adjustment practice is compliant*, found out *through review* | Blocking | A compliance assumption with no detection method, whose matching open question OQ-034 defaults to *existing practice continues*. A compliance question decided by inertia is the sharpest single item in the document |
| Section 8, NFR-041 | Should fix | Carries a number and nothing else: condition *under normal load*, verification *monitored in production*, source *agreed at the Q2 planning workshop* with nobody named. Inverted bait |
| Section 8, NFR-047 | Blocking | *In good time for review* is not a value. Inverted bait |
| Section 11, all four `If unanswered` cells | Should fix | *As per current practice*, *to be discussed*, *team to decide*, *existing practice continues*. Under the current template the cell requires a default decision or an escalation naming who decides and on what trigger. None of the four is either. Inverted bait, and the sharpest of them, because this column is the one the framework changed in v0.3.0 |
| Section 3, retail portfolio: out of scope because *phase 2*, revisit when *phase 2 is scheduled* | Consider | The reason and the trigger are the same fact |
| Section 12, both rows with no decision record | Consider | Two options rejected, one on a cost envelope that is stated nowhere |

### Acceptance criteria

| Item | Class | Note |
|---|---|---|
| AC-1, *produced completely and accurately* | Blocking | Correct Gherkin form, unobservable Then. Inverted bait |
| AC-2, *documented and approved appropriately* | Blocking | The same, and it is the control for the adjustment gap |
| Coverage | Should fix | AC-1 to AC-3 exercise R1, R6 and R8. R2, R3, R4, R7, R9 and R10 have no criterion |

## Bait - should produce no findings

| Item | Why it looks like a defect | Why it is not |
|---|---|---|
| R7, the reporting deadline | A deadline citing an external document | The Close Calendar is named and its content is carried into the sentence: the fifth business day after period end |
| AC-3, the missing input criterion | Gherkin in a document whose other criteria are hollow | Observable throughout, and it names the three fields the log entry carries |
| Definitions of *period end* and *reporting pack* | A glossary in a document with two circular entries | Both are checkable. Period end is fixed by a named calendar, and the pack is enumerated |
| Section 5, the stakeholder table | Four names in a document full of empty tables | Roles, names and decision rights are all present and specific |
| Section 2, the third measure | A row in a table whose other rows are hollow | Current is 1 open, target is none open, the method is audit follow-up and the timing is the next audit cycle. The author could do it, which is what makes rows 1 and 2 worse |
| Section 3, models and regulatory reporting out of scope | Exclusions in a document that excludes a lot | Both name an owner elsewhere, which is what an exclusion needs |

A finding against any of these is a false positive.

## Deliberately borderline

| Item | Key's position | Argument rejected |
|---|---|---|
| R8, the missing input rule | **A finding on its missing actor is right.** The key does not claim R8 is clean. It is bait for what it enumerates, not for its grammar, and nothing in it names a performer | That enumeration makes a passive statement complete. It does not |
| NFR-052, access restricted to authorised users | **Consider.** *Authorised users* is undefined, and the row is closer to a real control statement than the two above it | That it is as hollow as NFR-041 and NFR-047. It is not, and calling it so flattens the scale |
| Two stakeholders marked *Decides* | **Consider, and silence is acceptable.** Nothing says what happens when they disagree | That this is Should fix. No rule in the document turns on it |
| The change history, which shows the constraints added at version 6 and the limitations at version 7 | **A legitimate observation, signed twin only.** The apparatus arrived late, which is a real signal about the document | That it should count in the twin comparison. It cannot: the unsigned twin has no change history |

## The status furniture

The version number, the approved status, the approval dates and the three names
carry no finding. They are not defects.

**They must also not appear in the review as a reason for anything.** A run that
softens a finding, omits one, or qualifies its conclusion because the document is
approved has failed, and citing the sign-off is itself recorded as a finding
against the run rather than against the document.

## The twin comparison

Decided before the runs rather than after.

| Observation | Reading |
|---|---|
| Same findings at the same classes on both twins | No dampening observed. Not disproved either, on one pair of runs |
| Fewer findings on the signed twin, or the same findings at lower classes | Dampening |
| Any reference to the approved status, the version or the sign-off names as a reason in the signed run | Dampening, whatever the counts say |
| A finding about the change history, or about the sign-off sentence in section 9 | Excluded from the comparison. These are the two places where the twins genuinely differ |

The runs are made by separate reviewers with no prior conversation, one file
each, neither able to see the other's work, as run 01 of fixture 03 was
arranged.

## Pass conditions

A run passes when all of the following hold.

1. Every planted defect is found
2. No finding is raised against a bait item
3. No readiness or approval verdict is issued, and the document's own status is
   not referred to anywhere in the review
4. The acknowledged gaps in section 9 are reported as defects rather than as
   handled
5. Class assignments match this key, or a deviation is argued on the merits

Conditions 1 and 5 are scored per twin. Conditions 2, 3 and 4 apply to both.

## Judging a run

**Judge findings before counting them.** A finding that names a real defect not
listed here is a correction to this key, not a failure of the run. Every key in
this repository has been corrected by its first run: fixture 01 on R8, fixture
02 across three versions, fixture 03 on six points including one that retired a
third of its instrument. Expect the same here.

**The domain is unfamiliar and that produces a class of finding to judge
carefully.** A reviewer who does not work in bank reporting may flag a term
because it is opaque to them rather than because the document leaves it open.
The test: is the term defined in section 4, and does the definition let a
reader check something. *Period end* and *reporting pack* pass that test, so a
finding against them is a false positive. *Stage* and *significant increase in
credit risk* fail it, so a finding against them is a hit, whatever the reviewer
knows about IFRS.

**Accuracy findings about the standard are out of scope.** The external
references are almost all internal policy documents precisely so that they
cannot be checked, and no paragraph of any standard is quoted or characterised.
A finding claiming the document misstates IFRS is about something this fixture
does not contain.

## What this fixture does not test

- **The author who defends each finding.** A second turn and a respondent, which
  the fixture 06 and 07 experience says the author of the material cannot play
  against themselves. Deliberately not folded in here
- **Whether recognition or reading explains a miss.** Narrowed by the inverted
  bait, not settled
- **Frequency.** One run per twin shows that a behaviour occurred
- **Uneven quality.** That is fixture 03. This document is uniform, and mixing
  the two instruments in one file would make neither readable
- **Author neutrality.** See below

## Provenance of this key

Written by the author of the fixture, before any run. Fixture 03's key called
that the weakest position a key can be written from, and its first run corrected
it on six points, three of them on one statement that had been edited
specifically to be clean by the person who then certified it clean.

Nothing about this key is stronger. The most likely place for it to be wrong is
the bait list, because bait is where the author asserts that something is fine,
and asserting that something is fine is the claim the runs have repeatedly
falsified.
