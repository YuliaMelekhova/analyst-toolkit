# Fixtures are snapshots, not examples

The files in this directory are inputs a skill was tested against, recorded as
they were at the time of the run. They are not examples of how a document should
look, and copying one is a mistake.

Two consequences follow.

**They drift from the templates on purpose.** When `framework/` changes, fixtures
are not updated to match. Editing a fixture would make the runs recorded in
`NOTES.md` unreproducible, and the whole value of a recorded run is that someone
can repeat it. A fixture carrying a superseded column name or a retired
convention is doing its job.

**Some of them are deliberately bad.** These fixtures exist to be reviewed, and
several statements in them are defective by design, with the defects listed in
the matching answer key. A reader who takes a fixture as a model will copy the
exact problems the skill exists to find.

If you want to see what a document should look like, read
`framework/templates/` for the shape and `example/` for a document produced by
following it.

## What is here

| File | Role |
|---|---|
| `fixture-01-plan-change.md` | Input: a requirements draft with defects throughout |
| `fixture-01-answer-key.md` | The defects planted in it, and what a run should find |
| `fixture-02-approval-routing.md` | Input: a carefully written draft with a few subtle defects |
| `fixture-02-answer-key.md` | The same, for fixture 02 |
| `fixture-03-siu-referral.md` | Input: a mostly sound draft with one badly written section, placed first |
| `fixture-03-answer-key.md` | The same, for fixture 03, plus how to read a run for contamination |
| `fixture-04-ecl-reporting.md` | Input: a complete-looking document carrying an approval apparatus |
| `fixture-04-ecl-reporting-unsigned.md` | The same text with the approval apparatus removed |
| `fixture-04-answer-key.md` | The same, for fixture 04, plus how to read the pair of runs |

Answer keys are read after a run, not before. Reading the key first makes the
run worthless, because the finding you are looking for is the one you will find.

## What each fixture is for

Fixtures 01 and 02 test the skill against the document. Fixture 03 tests the
skill against itself.

- **01** is defective throughout and measures recall: does the skill find
  defects in text that is plainly badly written
- **02** is carefully written and measures precision: does the skill invent
  findings when there is little to find
- **03** is uneven, which is the realistic case and the one neither of the
  others can produce. It measures whether reviewing one section changes the
  review of the next: false findings in careful text that follows bad text, or
  a class assigned by the company a defect keeps rather than by the defect

- **04** is complete-looking and empty. Every section is present, every table is
  filled, and the prose is competent. It measures whether form is accepted in
  place of substance: whether an acknowledged gap counts as a closed one, a
  citation as a source, a defined term as a definition

Fixture 03 carries three matched pairs, each one defect type at one class, with
one member in the badly written section and one in a careful section. The pairs
are the measurement, and they are the reason the fixture cannot be edited
casually: moving a pair member into different company destroys the comparison
it exists to support.

Fixture 04 comes in two copies of one text, differing only in the approval
apparatus: a version number, an approved status, a change history and a table of
signatures. That difference is its second measurement, and a document's status
cannot be shown to soften a review except against a copy without it. Run the
twins separately, by reviewers who cannot see each other's work, and keep them
identical apart from the two differences the answer key declares.
