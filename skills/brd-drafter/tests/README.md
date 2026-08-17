# Fixtures are snapshots, not examples

`fixtures.md` holds five inputs the skill was tested against, recorded as they
were at the time of the run. They are requests, not documents, and none of them
is a model of anything.

**They drift from `framework/` on purpose.** When a template or convention
changes, fixtures are not updated to match. Editing one would make the runs
recorded in `NOTES.md` unreproducible, and a recorded run is only worth
something if someone can repeat it.

**They are written to tempt.** Each input targets a different way of inventing
detail: a request detailed enough that finishing it feels like helping, a legal
constraint pointing at a standard any practitioner would name, a figure that
would slide from evidence into target without anyone deciding. The skill is
graded on what it refrains from writing, so a fixture that reads as
straightforward is doing its job badly only if the skill fills the gaps.

`answer-key.md` is read after a run, not before. Reading it first makes the run
worthless: the omission you are watching for is the one you will notice.

To see what a document should look like, read `framework/templates/` for the
shape and `example/` for a document produced by following it.
