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

Answer keys are read after a run, not before. Reading the key first makes the
run worthless, because the finding you are looking for is the one you will find.
