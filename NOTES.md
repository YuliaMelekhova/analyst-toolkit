# Notes

What happened while building this, including the parts that did not work. Kept
because the failures were more informative than the successes, and because a
toolkit with no visible seams is a marketing page.

---

## The skill was tested three times. The fixtures failed twice.

`requirements-smell-detector` was built against two test documents: one written
badly on purpose, one written carefully. The first tests whether the skill finds
defects; the second tests whether it invents them.

The second is the one that matters. A reviewer who flags everything is
negotiated with rather than listened to, and gets ignored within a week.

### Run 1 — recall

Sixteen findings on a document with fourteen planted defects. Recall was
adequate; two findings were unplanted and both were correct.

One of them changed the fixture. A proration rule — *"proration is calculated
based on the remaining days in the billing period"* — had been marked clean in
the answer key. The skill classed it blocking: two implementations of that
sentence produce different amounts, because rounding, timezone, day boundary and
whether the change day counts are all unstated. For a money calculation that is
a real defect. **The answer key was less strict than the skill, and the answer
key was wrong.**

Two adjustments followed. The recognition clue for *unverifiable outcome* was
widened to cover rules stated loosely enough that two implementations would
differ. A summary line naming the *cleanest statements* was added, because the
run produced one unprompted and it was useful: a review that only reports
defects gives the author no signal about what to preserve.

### Run 2 — precision, first attempt

Eighteen findings on a document intended to contain four subtle defects and
nothing else.

By the grading table, that was a failure: anything above nine meant the skill
was padding. Reading the findings on their merits gave a different answer.
Roughly fourteen were real defects the author had not intended and had not
noticed:

- A skip rule that made the completion condition unreachable. If every step in
  an approval sequence resolved to the document owner, every step was skipped,
  no approver was ever assigned, and the rule that completes the request could
  never fire.
- An expiry mechanism with no stated trigger, leaving a race between a response
  submitted after the deadline and the check that marks the request expired.
- A history requirement listing the fields to record, none of which was the
  field an acceptance criterion required.
- A state — `changes-requested` — with no exit specified.

**The fixture failed, not the skill.** The document had been written by someone
who believed it was clean, and it was not — which is exactly what happens to
authors, and exactly what the skill exists to catch.

One genuine defect in the skill did surface. A finding was labelled *missing
actor* on a statement reading *"the routing service marks the request expired"*
— where the performer is named in the same sentence. The content of the finding
was right (the trigger and its interval were undefined); the smell was wrong.
Smell 1 was matching on grammar rather than on an absent performer.

Adjustments: an explicit disambiguation rule (*if a performer is named anywhere
in the statement, it is not smell 1 — unclear timing is smell 5*), and a
rewritten calibration paragraph, because five blocking findings on that document
was inflation.

### Run 3 — precision, second attempt

The fixture was rewritten to close all fourteen unintended defects. Twelve
findings came back. Again, almost all real.

Including one created by the previous fix. An acceptance criterion had
originally stated an absolute notification bound while the NFR stated a p95 —
so a run satisfying the NFR could fail the criterion. The fix pointed the
criterion at the NFR instead. The skill observed that a p95 aggregate over an
hourly load cannot be passed or failed by a single Gherkin scenario, and that
the criterion said *sent* where the NFR measured *delivered*.

**A fix for one defect introduced another.** This is ordinary, and it is the
argument for having the check run at all rather than relying on care.

One planted defect had now been missed twice: no behaviour was specified for an
assignee losing access or leaving the organisation while a step was assigned to
them. It kept being missed because it lives in no statement — there is nothing
to quote. Every smell in the list was defined as something recognisable *within*
a sentence.

That produced the last change: a **cross-cutting scan**, a second pass over the
document as a whole, checking six scenarios that belong to no single statement —
the assigned party becoming unavailable, the action repeating, state changing
between read and write, a dependency not answering, configuration changing
mid-flight, and the empty or boundary case.

Run 3 found it.

---

## What stopped being a useful measure

The original grading tables scored runs by finding count: under six, correct;
over nine, over-strict.

That assumes the fixture is clean. It was not, twice, on documents written
specifically to be clean. There is no reason to believe a third attempt would
have been different.

The criterion that replaced it: **a run passes when it contains no false
positives.** Across three runs there were none. Not one finding pointed at
something the text had actually covered. The bait items — passive-adjacent
phrasing with a named performer, a fully specified NFR, a correctly handled
open question — were left alone every time.

Count says nothing. What the findings are wrong about says everything.

---

## What did not need adjusting

Three behaviours held from the first version onwards, and they are the ones that
mattered most:

**No invented values.** Every reformulation asked for the missing number rather
than proposing one. *"State the percentile and the load condition"*, never
*"suggest p95 = 2s"*. This was the failure mode most worth preventing, because
a plausible invented threshold becomes a real commitment the moment somebody
reads it.

**No readiness verdict.** Every run opened by declining to say whether the
document was ready, and pointing at the definition of ready as a human decision.
This was not incidental — the constraint exists because a tool that certifies
its own output removes the only check on it.

**No rewriting.** Reformulations stayed inside findings. A corrected version
gets accepted wholesale, and then the review has taught the author nothing.

---

## What is still untested

- **A mixed artifact** — mostly sound with one badly written section. The
  realistic case, and the hardest to calibrate. Both fixtures are uniform.
- **Behaviour when a glossary is supplied.** All runs assumed none existed and
  caveated terminology findings accordingly. Whether those findings correctly
  disappear when definitions are provided has never been checked.
- **A non-English artifact.** The output-language rule has not been exercised.
- **Whether the framework references do anything.** The skill points at files in
  `framework/`. In the app, uploaded as a standalone zip, those files are not
  present — and the output did not visibly suffer. Either the references are
  decorative, or their effect is subtle. Unresolved.

---

## The general lesson

Writing a clean requirements document is harder than writing a checker for
dirty ones.

Three attempts at a deliberately clean fixture produced three documents with
real defects in them — a logical hole, a race condition, an acceptance criterion
that could not be evaluated, and a fix that introduced a new problem while
solving the one it targeted. All by an author who was paying attention, on a
document short enough to read in five minutes, with no deadline and no
stakeholders.

That is the case for the tooling, and it is a stronger case than any of the
finding tables.
