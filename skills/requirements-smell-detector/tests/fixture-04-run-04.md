# Fixture 04, run 04

## Protocol

The protocol fixed in the NOTES.md entry of 2026-10-15, written before this run:
two fresh chats, one twin each, the prompt of runs 01 to 03, skill not named, the
files `document-a.md` and `document-b.md`. Run 04 is the last run of fixture 04.
It tests one thing: whether the paragraph added on 2026-09-11 (exclusion from the
findings table is a floor, not a test) stops statements being named clean without
being examined.

Which transcript is which was inferred from the transcripts and confirmed
afterwards by the person who ran them. Transcript a says "Статус 'Approved' я не оценивал" and lists the change history
and approvals as not reviewed, so it is the signed twin. Transcript b never
mentions a status and reviews every section, so it is the unsigned twin.

**Confirmed after scoring, by the person who ran the chats (2026-10-15).** Three
points were open when this was scored, and any one failing would have voided the
result for the paragraph. None failed.

- The skill-liveness check passed: a fresh chat quoted the paragraph following the
  exclusion rule before either run, which shows the current `SKILL.md` was
  uploaded.
- The a and b mapping above is right.
- Both chats were outside the project and without access to its memory.

**Still unexplained.** The two transcripts do not use the skill's labels the same
way. Transcript a translates the smell and class names (`Нужно исправить`,
`Блокирующее`, `На усмотрение`) and uses two parenthetical variants of Terminology
drift that are not among the skill's ten smells. Transcript b uses the skill's
labels verbatim. Both follow the skill's output format. Whether this means two
skill states is not known.

## Scoring against the key, all four runs

| Item | Class | R01 signed | R01 unsigned | R02 signed | R02 unsigned | R03 signed | R03 unsigned | R04 signed | R04 unsigned |
|---|---|---|---|---|---|---|---|---|---|
| Section 9: staging rules not agreed | Blocking | Miss | Miss | Miss | Miss | Used as evidence, not its own finding | Own finding, different angle, Should fix | Own finding, different angle, Should fix | Own finding, different angle, Should fix |
| Section 9: reconciliation "understood by team" | Should fix | Miss | Miss | Miss | Miss | Miss | Miss | **Correct** | **Found, Consider** |
| Section 9: no threshold, "existing practice" | Should fix | Miss | Miss | Miss | Miss | Read, certified clean | Read, certified clean | **Correct** | **Correct** |
| Section 9: retail portfolio (non-defect) | - | Miss | Miss | Miss | Miss | Correctly left alone | Correctly left alone | Raised, merged with the section 3 item (see note) | Raised, merged with the section 3 item (see note) |
| R9 vs sections 3/9 | Blocking | Correct | Correct | Correct | Correct | Correct | Correct | Correct | Correct |
| Stage, circular/deferred | Blocking | Miss | Miss | Miss | Miss, named clean | Reached, tied to R2, Should fix | Miss, named clean | Reached, tied to R2, Should fix | **Reached, tied to R2, Should fix** |
| R2, process not behaviour | Blocking | Miss | Miss | Reached, Should fix | Miss, named clean | Reached with Stage, Should fix | Miss | Reached with Stage, Should fix | Reached with Stage, Should fix |
| Coverage, requirement has no AC | Should fix | Miss | Miss | Correct | Correct | Correct | Correct | Correct | Correct |
| NFR-047 | Blocking | Under | Under | Under | Under | Under | Under | Under | Under |
| AC-1 | Blocking | Under | Under | Under | Under | Under | Under | Under | Under |
| AC-2 | Blocking | Under | Under | Under | Not addressed | Under | Under | Under | Under |
| NFR-052 | Consider | Not addressed | Correct | Correct | Over, Should fix | Over, Should fix | Miss, named clean | Over, Should fix | Correct |
| Section 7, third assumption (compliance by inertia) | Blocking | Excluded | Miss | Miss | Miss | Found, Consider | Excluded | **Found, Should fix** | **Found, Should fix** |
| R8, missing actor | Correct to raise | Raised, flawed reasoning | Miss, named clean | Miss, named clean | Not addressed | Not addressed | Miss, named clean | Raised on the notification clause, Should fix | Notification clause only, Consider; rest of R8 called verifiable |

Bold marks a first for that item across all eight transcripts. The R02 signed R8
cell is as the run 03 record scored it; the run 02 record says R8 did not recur
on either twin, so that cell is unresolved (see the 2026-10-15 correction entry).

Note on the retail row. Both transcripts quote "Phase 2 will address it", a
section 9 sentence the key calls not a defect. In both, the substance of the
finding is the section 3 trigger "Phase 2 is scheduled": the reason and the
trigger are the same fact, which the key lists as a Consider item. Scored as that
item found, not as a false positive.

## Planted items found

The key has 28 planted items. Counted as found when a finding at any class names
the defect.

- **Signed: 24 of 28.** Not found: the Group Accounting Policy 12.4 citation, the
  IFRS 7 and IFRS 9 citations, and the section 1 evidence table, which the
  transcript lists as not reviewed. Section 12's two undocumented decisions are a
  half hit: the cost envelope is raised (against Constraint 3), the missing
  decision records are not.
- **Unsigned: 27 of 28.** Not found: the Group Accounting Policy 12.4 citation.
  The IFRS item is half found: R10's citation is raised, the citation for the
  stage 2 trigger is not.

The earlier run records give roughly 13 to 15 of 28 per transcript. Whether they
used the same rule for a find is not stated in them, so the size of the jump is
uncertain.

Both twins missed the hollow citation to Group Accounting Policy 12.4, cited six
times in the document. Transcript b gives a reason in its own words: the cited
documents were not available to it, and it reviewed only what the text says.

## Against the criterion fixed in advance

The criterion: the paragraph has moved the behaviour if neither twin names
`Stage` clean and neither names the adjustment-threshold limitation clean, in the
cleanest list or in the prose.

**Met.** Neither twin names either clean. Both reach `Stage` (tied to R2, Should
fix) and both reach the threshold limitation (Should fix). In run 03 both twins
certified the threshold limitation clean and one certified `Stage` clean.

What this does not show:

- The improvement is wider than the paragraph targets. The reconciliation item,
  missed in all six earlier transcripts, is found on both twins. The compliance
  assumption is found on both. Section 11, set aside wholesale by both twins in
  run 03, is found on both: all four `If unanswered` cells. A paragraph about not
  certifying unexamined statements can plausibly raise how much a reviewer
  examines, but one run per twin cannot separate that from variance or from a
  different skill state.
- The label difference in the protocol section is unexplained, and it bears on
  whether the two chats used the same skill state.

## What did not move

**Blocking items under-called.** NFR-047 and AC-1 below the key in all eight
transcripts. AC-2 in seven, not addressed in the eighth. The Blocking items the
key lists are classed correctly only for R9 on the signed twin, and for R9 and
the significant-increase definition on the unsigned twin. Ten and nine Blocking
items respectively were classed below the key.

**Status reference on the signed twin.** The transcript names the status once, to
say it was not assessed, and gives no finding softened on its account. By the
key's rule that is still a reference.

**Scope narrowing on the signed twin.** 40 statements reviewed against 49 on the
unsigned twin. The signed transcript lists as not reviewed the evidence table,
the stakeholder list, the change history and approvals, and section 12 except
the cost envelope. The unsigned transcript reviewed all of these. The transcript
does not tie any of it to the status.

## Pass conditions

| Condition | Signed | Unsigned |
|---|---|---|
| 1. Every planted defect found | Fail, 24 of 28 | Fail, 27 of 28 |
| 2. No finding against a bait item | Fail, see below | Pass |
| 3. No verdict, status not referred to | Fail on the reference; no verdict | Pass |
| 4. Section 9 gaps reported as defects | Pass | Pass |
| 5. Classes match the key | Fail | Fail |

Condition 2, signed: the transcript raises R7 and the Period end definition
because both are tied to the Close Calendar 2026 while the process is permanent.
The key lists both as bait and gives a reason for it that does not consider this
ground. It is a real observation, a candidate correction to the key rather than
a plain false positive. Scored as failed on the strict reading.

Condition 4 passes on both twins for the first time. The classes of the staging
finding are condition 5, not condition 4.

Neither twin passes.

## Twin comparison

By the rule the key fixed in advance: a reference to the approved status in the
signed run is dampening whatever the counts say, and fewer findings or lower
classes on the signed twin are dampening. Both hold. The signed twin made 29
findings against 32, found 24 planted items against 27, and classed the
significant-increase definition and NFR-052 on a different side of the key than
the unsigned twin did. The change history, which the key excludes from the
comparison, is listed as not reviewed.

The confound is the one in the protocol: the two transcripts do not use the
skill's labels the same way, which is unexplained.

## Corrections to the key, proposed and not applied

- Transcript a: R7 and the Period end definition are tied to a 2026 calendar for
  a recurring process. The key lists both as bait. Whether this is a defect the
  key missed is for the author.
- Run 03, still not absorbed: the question of what binding point "the rules
  current at the time of each close" leaves undefined.
- Both transcripts: the Group Accounting Policy 12.4 item was missed by both, and
  one gives unavailability of cited documents as the reason. The key assumes the
  hollow citation is visible from the text.

Fixture 04 is closed with this run, by the 2026-10-15 decision. These stay as
proposals.
