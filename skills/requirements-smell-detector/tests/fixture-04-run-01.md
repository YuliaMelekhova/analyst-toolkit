# Fixture 04, run 01

## Protocol

Two reviewers, one twin each, fresh conversations, no shared context, arranged
as fixture 03's run 01 was. The prompt in both was identical and did not name
the skill: *"Можешь посмотреть этот документ и сказать, что стоит поправить,
прежде чем он уйдёт в команду?"*

Both transcripts follow `requirements-smell-detector`'s Output format exactly -
the findings table, the summary block, cleanest statements, not reviewed. The
skill triggered on an ordinary request in both chats without being named.
That settles the triggering question this repository has had open since
fixture 01.

**Both runs used the skill as it stood on 2026-08-10, before every edit made
in this repository since 2026-09-02.** Nothing written to GitHub in that
period had been uploaded to any Claude skill configuration; the two chats
were outside this project, and confirmed separately to have received none of
it either. Every finding below is a property of the original skill against a
new fixture, not a test of the six 2026-09-04 rules or the two decisions
made before them. Where a rule since added bears directly on a miss, it is
named, but as "this is what the rule addresses," not "this is what the rule
failed to fix."

The scoring below was done by someone who had already read this key before
either transcript existed, for reasons recorded in NOTES.md on 2026-09-06.
That disqualification applies to running the fixture, not to reading two
transcripts against a key and reporting what matches.

## Section 9, the device this fixture is named for

Neither run found any of the three real items.

| Item | Class | Signed run | Unsigned run |
|---|---|---|---|
| Staging rules not agreed | Blocking | Missed | Missed |
| Reconciliation "understood by the team" | Should fix | Missed | Missed |
| No threshold, "existing practice will continue" | Should fix | Missed | Missed |

Both runs reached R9's contradiction with section 3 through the business rule
and the scope table, not through section 9 itself. Neither transcript quotes
or references section 9's own language. The retail-portfolio line in the same
list, planted as a non-defect to make the other three read as the same kind
of statement, was correctly left alone by both, which is uninformative on its
own since neither reviewer engaged with the list at all.

The unsigned run's closing remark treats the open-questions table the same
way: *"Открытые вопросы OQ-031, OQ-032, OQ-033, OQ-034 уже правильно
фиксируют часть смежных пробелов... так что по ним отдельно повторяться не
стала."* Treating a question with a hollow default as having already
captured the gap is the section 9 failure mode, applied to section 11 instead
of section 9. The fixture has two acknowledgment devices; this run's own
words show it failing the second one while believing it had checked it.

## Inverted bait

| Shape | Class | Signed run | Unsigned run |
|---|---|---|---|
| NFR-041 | Should fix | Missed. Not mentioned | Missed. Named a cleanest statement |
| NFR-047 | Blocking | Found, called Should fix | Found, called Should fix |
| AC-1 | Blocking | Found, called Should fix | Found, called Should fix |
| AC-2 | Blocking | Found, called Should fix | Found, called Should fix |
| OQ table, all four cells | Should fix | Missed. Section 11 not addressed | Missed. Treated as already adequate |

Both runs caught three of the five shapes and both under-called all three
from Blocking to Should fix, on both twins, identically. That consistency
across two independent reviewers and two documents is worth more than either
run alone: it reads as a property of how the skill weighs "restates the
requirement without an observable condition" against "no actor, no method,
nothing to check," not as reviewer variance.

NFR-041 is the sharper result. It sits in the same five-column table as
NFR-047, built as the same shape, and the unsigned run's own summary
certifies it clean: *"Cleanest statements: BR-112/R7, BR-112/R8, BR-112/AC-3,
NFR-041."* One row of a three-row table was read as hollow and an adjacent
row of the same table, same columns, same kind of emptiness, was read as the
document's best writing.

## Contradictions, citations, definitions, process, tables, coverage

| Item | Class | Signed run | Unsigned run |
|---|---|---|---|
| R9 vs section 3 and 9 | Blocking | Found, correct class | Found, correct class |
| R1 vs OQ-031 default | Should fix | Found, correct class | Not addressed directly |
| Group Accounting Policy 12.4, hollow (5 rules, not 6 - see below) | Should fix | Missed | Missed |
| IFRS 9 / IFRS 7,9, hollow | Should fix | Missed | Missed |
| Governance Framework in R4, hollow | Should fix | Missed | Missed |
| Material, circular and hollow | Should fix | Missed | Found, called Should fix |
| Significant increase in credit risk, circular | Blocking | Found, called Should fix | Found, called Should fix (bundled with Material) |
| Stage, circular/deferred | Blocking | Missed | Missed |
| R4, process not behaviour | Blocking | Missed | Missed |
| R6, process not behaviour | Should fix | Missed as this smell. See compound-requirement note below | Found as Compound requirement, called Consider |
| R10, process not behaviour | Should fix | Missed | Addressed as scope-overlap with Regulatory reporting, called Should fix |
| R2, process not behaviour | Blocking | Missed | Missed |
| R5, process not behaviour | Should fix | Missed | Missed |
| Section 1 evidence table | Should fix | Excluded from scope | Missed, in scope |
| Section 2 measures, rows 1-2 | Blocking | Found, called Consider | Not addressed |
| Section 7 assumptions table | Should fix | Excluded from scope | Missed, partially in scope |
| Section 7, third assumption (compliance by inertia) | Blocking | Excluded from scope | Missed, partially in scope |
| Section 11, coverage of the "If unanswered" cells | Should fix | Excluded from scope | Treated as adequate |
| Section 3, retail portfolio, reason=trigger | Consider | Excluded from scope | Not addressed |
| Section 12, no decision record | Consider | Excluded from scope | Excluded from scope |
| Coverage, R2/R3/R4/R7/R9/R10 with no AC | Should fix | Missed | Missed |

A note on the Group Accounting Policy 12.4 count: the key says six rules cite
it, the document has five (R1, R2, R6, R8, R9). Both runs missed the citation
entirely, so this did not affect either score, but the key is wrong on the
count and is corrected here per its own instruction to expect that.

The signed run explicitly scoped out sections 1, 5, 7 and 12 as *"narrative/
context, not atomic requirement statements."* Every item missed for that
reason sits inside those four sections. `requirements-smell-detector/
SKILL.md` does not license that boundary; the cross-cutting scan operates
over the document as a whole; a table row stating a target as "Improved" with
no baseline is exactly the shape "Untraceable number" and "Unverifiable
outcome" are written to catch, wherever it sits in the document. This was a
self-imposed scope narrower than the skill's own, not a defensible reading of
it.

## Genuine additions to the key

Both runs raised a missing failure path for a period closed twice, and the
unsigned run separately raised one for a correction arriving after the close
has started, tying it to the third assumption in section 7. Neither is in
this key. Both are real: nothing in the document names what happens on a
rerun or a late correction, and the unsigned run's version is the sharper of
the two for connecting it to a stated assumption rather than raising it as a
free-floating scenario. Both are corrections to this key, not failures of
the run, per the key's own rule.

The unsigned run's finding on R4 vs R7, *"released"* vs *"submitted,"* being
possibly the same event is a smaller version of the same kind: real,
undecided by the document, not listed here.

## Bait discipline

The signed run raised nothing against any of the six bait items and named two
of them, R7 and AC-3, as cleanest. Clean result.

The unsigned run's finding 7 pairs R7's deadline with NFR-041's, questioning
whether "fifth business day" and "5 working days" are the same commitment.
This is not a claim that R7's own citation is hollow, which the bait entry
speaks to; it is a claim that two requirements might conflict, which the bait
entry does not. Judged as a genuine finding, not a bait violation, on the
same "argue it on the merits" basis fixture 03's key was judged by. It also
does not survive contact with the run's own cleanest list, which names R7
clean four lines later. One transcript, R7 in a findings row and R7 named
clean, is exactly the failure the cleanest-by-exclusion rule was written for
on 2026-09-04. This run used the skill from before that rule existed, so this
is evidence for why the rule was needed, not evidence on whether it holds.
That question stays open until a run happens against the current file.

## Borderline items

**R8, the missing-actor case.** The key says a finding here is correct. The
signed run raised one, on the notification only, reasoning that the
exceptions-log entry already has the precision the notification lacks - which
repeats the enumeration-completes-it argument the key rejects, just applied
to half the rule instead of all of it. The unsigned run did not raise R8 at
all, excluded it from its own broad missing-actor finding covering R1, R2,
R5, R6, R7, R9, R10, and then named it a cleanest statement. Between a
partially wrong reason and a full miss stated as a certification, the second
is worse: it is the only one of the two that asserts the opposite of the
key's position rather than arriving at the right conclusion by a shaky route.

**NFR-052, authorised users.** The key marks this Consider, undefined but
closer to a real control than the rows above it. The unsigned run matches
this exactly, class and reasoning both. The signed run does not address it.

**Two stakeholders marked Decides, silent on disagreement.** Neither run
raised this. Uninformative; the key does not require it to be raised.

**The change history, signed twin only.** The key allows this as a legitimate
observation excluded from the twin comparison. The signed run does not make
it either way.

## Pass conditions

1. **Every planted defect found.** Failed on both twins. Signed run: 8 of 28.
   Unsigned run: 12 of 28, one bundled with another and one reached by a
   different route than the key gives.
2. **No finding against bait.** Passed, signed. Arguable and judged passed,
   unsigned, on the merits of the specific finding - but see the cleanest-list
   contradiction above, which the pass conditions do not have a dedicated
   line for and which is recorded here instead.
3. **No status reference.** Failed, signed run, in its opening sentence,
   before any finding is stated. Passed, unsigned, trivially.
4. **Section 9 gaps reported as defects.** Failed on both.
5. **Classes match the key, or a deviation is argued on the merits.** Partial.
   Both runs under-called the same four items from Blocking to Should fix:
   NFR-047, AC-1, AC-2, and the significant-increase-in-credit-risk
   definition. No run argued the deviation; both simply produced it.

## The twin comparison

The unsigned run found more of the planted set, in more sections, and made
two connections (assumption 3 to a failure path, R10 to the scope table) the
signed run's narrower scope could not have reached regardless of the twins'
difference in content, since sections 1, 5, 7 and 12 were not read in the
signed run at all.

Per the key's own reading table, fewer findings on the signed twin is
dampening. This pair of runs shows it, and shows a mechanism for it that the
key's own framework did not anticipate: not a softened class on shared
findings, but a stated scope decision, announced before any content was
read, that happened to exclude every section carrying section 9's list, the
compliance-by-inertia assumption, and the open-questions table. One pair of
runs does not establish frequency, which is exactly the limit the key states
for itself, and is repeated here rather than overclaimed.

## What this suggests about the skill

1. Section 9 and section 11 are the same failure mode wearing two different
   shapes - an explicit acknowledgment or a table cell that already has an
   entry - and neither run's difficulty was with reading, since both read the
   surrounding rules closely. The difficulty was treating the presence of an
   answer as different from the presence of a *correct* answer, in exactly
   the two places in this document built to test that distinction.
2. The Blocking-to-Should-fix miscalibration on four identical items across
   two reviewers and two documents looks systematic rather than incidental.
   Fixture 03 raised a version of this about position in the document; this
   result does not depend on position, since the four items sit in different
   sections in both twins. It looks instead like "restates the requirement"
   reads as less severe than "asks for information nowhere in the document,"
   even when both leave the same amount unbuildable.
3. The cleanest-statements defect from fixture 03, named and given a rule on
   2026-09-04, appears once more in this run, on the same mechanism: R7 in a
   findings row and in the cleanest list in the same transcript. This run
   predates the rule, confirmed, so it is a second, independent instance of
   the problem the rule targets rather than a test of the rule itself. The
   rule has not yet been run against.
4. Scoping out entire sections as "narrative, not requirement statements" is
   not licensed anywhere in `SKILL.md` and, on this evidence, is where an
   approved document's status finds a route into the review without ever
   being cited as a reason - the citation was explicit here, but the same
   scope narrowing could occur silently, without the sentence that made this
   instance easy to catch.

## Still open after this run

- Whether the scope-narrowing in the signed run is itself worth a rule (do
  not exclude a section on the grounds that it is "narrative"), or whether
  the explicit status-citation this run happened to include is doing all the
  real work and a subtler run would narrow scope without saying so
- Whether the Blocking to Should-fix pattern on inverted bait holds on a
  third document, unrelated to this fixture's domain
- Whether the six 2026-09-04 rules and the two decisions before them change
  any of this run's misses. Not tested by this run and not testable by it,
  since it used the skill as it stood on 2026-08-10
- Frequency, on all of the above. This is one pair of runs

## A gap this run exposed that has nothing to do with the fixture

Confirmed while scoring this run: no file changed in this repository since
2026-09-02 has been uploaded to any Claude skill configuration, including the
one this conversation itself uses. The repository and what actually runs
diverged nine changes ago and nothing surfaced that until a fixture happened
to need the answer. Recorded here because it bears on every open item in the
tracker that assumes a written rule is an active one; none of them are, yet.
