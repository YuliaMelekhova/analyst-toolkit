# Fixture 04, run 02

## Protocol, and an error in it

Same arrangement as run 01: two fresh conversations, one twin each, identical
prompt, skill not named. This time the skill itself was the variable under
test, confirmed uploaded and live before either transcript was produced.

The files handed over for this run were named
`fixture-04-ecl-reporting-signed.md` and `fixture-04-ecl-reporting-unsigned.md`
- named that way when first prepared for run 01 and reused unchanged for this
one. The unsigned twin's transcript opens by citing the filename directly:
*"судя по названию файла 'unsigned' - это версия перед подписанием."* That is
not a property of the document or the skill; it is a leak introduced in how
the material was prepared, handing the reviewer exactly the label the twins
are built to withhold. The signed twin's equivalent framing cites the
document's own front matter, `status: Approved`, which is legitimate content
and scored normally. The unsigned twin's condition-3 result this run is not
usable as clean evidence either way and is marked as such below. File names
for any further run will not contain either word.

## What the skill version was

All three skills, including this one, were confirmed live with the current
`SKILL.md` before this run: a fresh chat was asked to quote a section that
only exists in the post-2026-09-04 text and returned it verbatim. This run
tests the six 2026-09-04 rules and the two decisions before them. Run 01 did
not.

## Scoring against the key, both runs side by side

| Item | Class | Run 01 signed | Run 01 unsigned | Run 02 signed | Run 02 unsigned |
|---|---|---|---|---|---|
| Section 9, staging rules not agreed | Blocking | Missed | Missed | Missed | Missed |
| Section 9, reconciliation "understood by team" | Should fix | Missed | Missed | Missed | Missed |
| Section 9, no threshold, "existing practice" | Should fix | Missed | Missed | Missed | Missed |
| R9 vs sections 3/9 | Blocking | Correct | Correct | Correct | Correct |
| R1 vs OQ-031 default | Should fix | Correct | Not addressed | Correct, and critiques the fallback itself | Under-called, Consider |
| Group Accounting Policy 12.4, hollow | Should fix | Missed | Missed | Missed | Missed |
| IFRS citations, hollow | Should fix | Missed | Missed | Missed | Missed |
| Governance Framework in R4, hollow | Should fix | Missed | Missed | Missed | Missed |
| Material, circular | Should fix | Missed | Correct | Under-called, Consider | Correct |
| Significant increase in credit risk, circular | Blocking | Under-called | Under-called | Under-called | **Correct**, first time |
| Stage, circular/deferred | Blocking | Missed | Missed | Missed | Missed, and named cleanest |
| R4, process not behaviour | Blocking | Missed | Missed | Reached via compound-requirement, Should fix | Reached via compound-requirement, Should fix |
| R6, process not behaviour | Should fix | Missed | Reached via compound-requirement, Consider | **Correct**, tied to OQ-032/034 | Reached via compound-requirement, Should fix |
| R10, process not behaviour | Should fix | Missed | Reached via scope-overlap, Should fix | **Correct** | Missed |
| R2, process not behaviour | Blocking | Missed | Missed | Reached, Should fix, tied to Known limitations | Missed, and named cleanest |
| R5, process not behaviour | Should fix | Missed | Missed | Missed | Reached via missing-actor list, Should fix |
| Section 1 evidence table | Should fix | Excluded | Missed | Excluded | Excluded |
| Section 2 measures, rows 1-2 | Blocking | Under-called, Consider | Not addressed | Under-called, Should fix | Under-called, Should fix |
| Section 7 assumptions table | Should fix | Excluded | Partial | Excluded (cost envelope only) | Excluded (cross-ref only) |
| Section 7, third assumption, compliance by inertia | Blocking | Excluded | Missed | Missed | Missed |
| Section 11, "If unanswered" cells | Should fix | Excluded | Treated as adequate | Excluded | Excluded |
| Coverage, requirement has no AC | Should fix | Missed | Missed | **Correct** | **Correct** |
| AC-1 | Blocking | Under-called | Under-called | Under-called | Under-called |
| AC-2 | Blocking | Under-called | Under-called | Under-called | Not separately addressed |
| NFR-041 | Should fix | Missed, named cleanest | Missed, named cleanest | Under-called, Consider | **Correct** |
| NFR-047 | Blocking | Under-called | Under-called | Under-called | Under-called |
| NFR-052 | Consider | Not addressed | **Correct** | **Correct** | Over-called, Should fix |

Bold marks the first correct-class hit for that item across all four
transcripts.

## What changed

**The coverage rule works.** Both run 02 transcripts independently flag a
requirement with no acceptance criterion, at the class the key gives. Neither
run 01 transcript found this at all. This is the cleanest before/after result
in either run: a rule added on 2026-09-04 for exactly this gap, absent in
both pre-fix transcripts, present in both post-fix ones.

**The cleanest-list self-contradiction did not recur, but a related error
did.** No statement in either run 02 transcript appears in both the findings
table and the cleanest list, unlike run 01's unsigned twin, where R7 did
both. The 2026-09-04 rule requires the list to be produced by removing every
identifier already in the findings table, and on this evidence it is being
followed: nothing found is also named clean. What the rule does not reach is
a statement never examined at all. Run 02's unsigned twin names both `Stage`
and `R2` cleanest; both use the same deferred-to-nonexistent-rules
construction as "significant increase in credit risk," which the same
transcript correctly calls Blocking two rows earlier in the same document.
The rule fixes internal consistency between two lists. It does not fix
confident certification of a statement that was simply never read against
the ten smells. That is a different, narrower defect than the one named on
2026-09-04, and this run is the first evidence it exists on its own.

**The Blocking to Should-fix pattern mostly held, with one exception.** AC-1,
AC-2 and NFR-047 were under-called in all four transcripts, run 01 and run
02, signed and unsigned, without exception. "Significant increase in credit
risk" was under-called in three of four and correctly called Blocking in the
fourth (run 02, unsigned). One correct call after three misses is not enough
to say the calibration rule from 2026-09-04 is working on this item;
it is enough to say it is not obviously not working, which is the most this
sample size supports.

**Scope-narrowing on the signed twin repeated almost verbatim.** Run 02's
signed twin excludes the same sections, on close to the same words, as run
01's: *"context and governance sections rather than testable statements."*
No rule from 2026-09-04 addressed this, and none was expected to. Named here
to confirm the prediction rather than to report a surprise.

**Section 9 stayed fully missed, on both twins, both runs.** Four independent
reviewers, two skill versions, zero hits on the fixture's namesake device.
This is now the single most consistent result across the whole exercise.

## Borderline items, updated

**R8.** Run 01's unsigned twin named it cleanest without addressing its
missing actor. Run 02 repeats this on neither twin directly, but the pattern
recurs one document earlier: run 02's unsigned twin names `R2` and `Stage`
cleanest by the same mechanism, not examined, then certified. The specific
statement changed; the failure shape did not.

**NFR-052.** Correct in run 01 (unsigned), correct in run 02 (signed), and
over-called to Should fix in run 02 (unsigned) where the key gives Consider.
Three runs, two right, one over-cautious in the direction opposite every
other miscalibration in this fixture. Worth noting as the only item where the
error runs the other way.

## Pass conditions

1. **Every planted defect found.** Failed, all four transcripts. Run 02
   found more than run 01 on both twins: signed run 01, 8 of 28; signed run
   02, roughly 13 of 28; unsigned run 01, 12 of 28; unsigned run 02, roughly
   14 of 28, with two items reached through a different, defensible route
   than the key gives.
2. **No finding against bait.** Clean on both run 02 transcripts. The R7/
   NFR-041 cross-reference finding recurs in run 02's unsigned twin, judged
   the same way it was in run 01: a claim about consistency between two
   requirements, not a claim against R7's own construction.
3. **No status reference.** Failed, run 02 signed, same mechanism as run 01,
   sourced from the document's own front matter. Not scorable, run 02
   unsigned, for the filename reason above.
4. **Section 9 gaps reported as defects.** Failed, all four transcripts.
5. **Classes match the key, or a deviation is argued on the merits.**
   Materially unchanged from run 01, with the coverage item now consistently
   correct and one Blocking item correctly called for the first time.

## What this run adds

1. A rule can be confirmed working within one run, when the fixture plants a
   gap the rule specifically targets and both transcripts independently find
   it. The coverage rule is the clean case of this in the whole exercise so
   far.
2. A rule can also be confirmed doing exactly what it says and nothing more.
   The cleanest-by-exclusion rule stops a statement from appearing in two
   contradictory places at once; it was never going to stop a statement from
   being certified without being read, because that was not the defect it
   was written against. The distinction matters for deciding what to fix
   next: not a stronger version of the same rule, but a different one.
3. Section 9 is now the strongest single result in this repository's testing
   of this skill. Every other finding here has at least one run bucking the
   pattern. Section 9 has none, across two skill versions and four
   independent reviewers.
4. File preparation is part of the instrument. This run's clean half (the
   coverage rule holding) and its compromised half (the filename leak) came
   from the same batch of material, prepared by the same process, and only
   one half was checked before use.

## Still open after this run

- Whether a third and fourth run, on documents whose filenames do not name
  their own condition, would confirm the unsigned twin's condition-3 result
  either way
- Whether the scope-narrowing on the signed twin is worth a rule now, having
  recurred once with no rule addressed to it, or whether two occurrences is
  still one occurrence for frequency purposes
- Whether "certified clean without being examined" is common enough across
  the two skill versions to be worth a rule of its own, distinct from the
  cleanest-by-exclusion rule already written
- Section 9. Not a new question, but the one this repository has now spent
  the most evidence on without moving
