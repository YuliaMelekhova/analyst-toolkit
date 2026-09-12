# Fixture 04, run 03

## Protocol

Same fixture, same prompt, same skill-liveness check as run 02: confirmed
before this run that the account-level skill carries the 2026-09-08 rule.
File names this time were neutral, `fixture-04-document-a.md` and
`...-document-b.md`; neither transcript references a file name, so this
run's condition-3 result is usable on both twins, unlike run 02's.

Document a is the signed twin (explicitly: *"Документ уже в статусе
'Approved' с подписями"*). Document b is the unsigned twin.

This run tests one thing specifically: whether the 2026-09-08 rule (an
identifier's presence does not decide whether a sentence is in scope) moves
section 9, the fixture's namesake device, which had zero hits across all
four prior transcripts.

## Scoring against the key, all three runs

| Item | Class | R01 signed | R01 unsigned | R02 signed | R02 unsigned | R03 signed | R03 unsigned |
|---|---|---|---|---|---|---|---|
| Section 9: staging rules not agreed | Blocking | Miss | Miss | Miss | Miss | Used as evidence, not its own finding | Own finding, different angle, Should fix |
| Section 9: reconciliation "understood by team" | Should fix | Miss | Miss | Miss | Miss | Miss | Miss |
| Section 9: no threshold, "existing practice" | Should fix | Miss | Miss | Miss | Miss | **Read, certified clean** | **Read, certified clean** |
| Section 9: retail portfolio (non-defect) | - | Miss | Miss | Miss | Miss | Correctly left alone | Correctly left alone |
| R9 vs sections 3/9 | Blocking | Correct | Correct | Correct | Correct | Correct | Correct |
| Stage, circular/deferred | Blocking | Miss | Miss | Miss | Miss, named clean | Reached, tied to R2, Should fix | Miss, named clean |
| R2, process not behaviour | Blocking | Miss | Miss | Reached, Should fix | Miss, named clean | Reached with Stage, Should fix | Miss |
| Coverage, requirement has no AC | Should fix | Miss | Miss | Correct | Correct | Correct | Correct |
| NFR-047 | Blocking | Under | Under | Under | Under | Under | Under |
| AC-1 | Blocking | Under | Under | Under | Under | Under | Under |
| AC-2 | Blocking | Under | Under | Under | Not addressed | Under | Under |
| NFR-052 | Consider | Not addressed | Correct | Correct | Over, Should fix | Over, Should fix | Miss, named clean |
| Section 7, third assumption (compliance by inertia) | Blocking | Excluded | Miss | Miss | Miss | **Found, Consider** | Excluded |
| R8, missing actor | Correct to raise | Raised, flawed reasoning | Miss, named clean | Miss, named clean | Not addressed | Not addressed | Miss, named clean |

Bold marks a first for that item across all six transcripts.

## What moved

**Section 9 is being read now.** Two independent transcripts, both twins,
each pulled a sentence out of section 9 and used it as the subject of its
own finding, not as background for R9. The unsigned twin quoted *"the rules
current at the time of each close"* and asked what binding point that
phrase leaves undefined - a real question the key does not itself raise, and
a correction to it on the same terms fixture 03's key accepted corrections
under. Zero of four prior transcripts did anything like this with section 9's
own sentences.

**The compliance-by-inertia assumption was found for the first time.** The
signed twin's finding 13, on *"would be addressed as it arises,"* is the item
the key calls its sharpest, missed in every one of the four prior transcripts
and excluded from scope entirely in three of them. Found and correctly
reasoned here, at Consider rather than the key's Blocking - the first hit,
still under-called, which is the same shape every other correctly-identified
item in this fixture has taken.

**What the rule did not reach: section 11.** Both twins still exclude Open
questions wholesale, and the reasons given are new phrasings of the same
move: *"itself a record of gaps, not new assertions"* (signed);
*"disclosed-assumption sections rather than requirement statements"*
(unsigned). The 2026-09-08 rule says a cell that is not empty is not the
same as a cell that is correct. Both transcripts route around this by
reclassifying the whole section as a record-of-gaps rather than as a set of
individual cells, which the rule did not anticipate as a category and does
not, on this evidence, close off.

**A new, sharper instance of the certified-without-examination gap.** Both
twins independently name *"no formal adjustment-approval threshold (tracked
via OQ-032)"* clean, the same section 9 item the signed twin's own cleanest
list surfaces by name for the first time, immediately after not testing it.
Being tracked by an open question is being read as being handled by one, the
same reasoning that under-calls R1 against OQ-031 elsewhere in this fixture.
This is not the rule from 2026-09-08 failing; it is the separate,
narrower gap the 2026-09-08 NOTES.md entry already named and did not write a
rule for. Two independent instances in one run is more evidence for writing
one than existed before this run.

**R8 and NFR-052 continue to resist.** R8 was named clean in three of six
transcripts now and correctly raised, if imperfectly reasoned, in exactly
one, the very first. NFR-052 has been correct twice, over-called twice, and
now wrongly certified clean once, across six attempts. Neither looks like
it is trending toward the key's answer as more runs accumulate.

## Pass conditions

1. **Every planted defect found.** Failed, both twins, improved count: signed
   roughly 15 of 28, unsigned roughly 13 of 28, both ahead of every prior
   transcript.
2. **No finding against bait.** Clean, both twins.
3. **No status reference.** Failed, signed, same mechanism as runs 01 and 02,
   sourced from the document's own front matter. Passed, unsigned - and this
   time genuinely scorable, since the file name carried no signal either way.
4. **Section 9 gaps reported as defects.** Partial, first time this fixture
   has produced anything other than a flat failure here. One of three real
   items reached as its own finding (unsigned twin, staging rules, different
   angle than the key's), one read and mis-certified clean on both twins, one
   still fully missed on both, one non-defect correctly left alone on both.
5. **Classes match the key, or a deviation is argued on the merits.**
   Unchanged in kind: the same Blocking items are under-called the same way,
   six for six on NFR-047 now.

## What this run adds

1. A rule can move a section from zero engagement to partial engagement in
   one run, on two independent transcripts, which is a stronger signal than
   either transcript alone. Section 9 went from invisible to read, quoted,
   and individually reasoned about - not solved, but a different kind of
   miss than before.
2. A rule aimed at "identifier does not gate scope" does not automatically
   reach "a whole table does not get exempted by being about gaps rather
   than facts." Both twins found a new way to exclude section 11 that the
   rule's own wording did not name. The fix for section 9 and the fix still
   needed for section 11 are not the same fix, even though both were
   diagnosed under one mechanism on 2026-09-08.
3. The certified-without-examination gap named provisionally in that entry
   now has a second, independent, same-run confirmation, on a different
   statement than the one that first surfaced it. That is close to enough
   evidence to write the rule rather than continue collecting instances of
   its absence.

## Still open after this run

- A rule for section 11 specifically: what stops a table from being excluded
  wholesale on the grounds that it records gaps rather than assertions
- A rule for certified-without-examination, now seen on `R7` and `Stage`
  (run 02) and on the adjustment-threshold limitation (run 03, both twins) -
  three statements, three runs, the same shape each time
- Whether R8 and NFR-052 are simply hard, or whether something about how
  they are worded resists whatever the last several rules have targeted
- A fourth run, if either of the two rules above gets written
