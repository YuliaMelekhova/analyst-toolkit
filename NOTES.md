# Notes

What happened while building this, including the parts that did not work. Kept
because the failures were more informative than the successes, and because a
toolkit with no visible seams is a marketing page.

---

## requirements-smell-detector

Tested three times against two documents: one written badly on purpose, one
written carefully. The first tests whether the skill finds defects; the second
tests whether it invents them.

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

### What stopped being a useful measure

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

## brd-drafter

A reviewer is graded on what it finds. A drafter is graded on what it refrains
from writing, which is harder to see: an invented threshold looks exactly like a
researched one, and a document with no gaps reads better than a document full of
them.

Four of five fixtures were run. Each was built to provoke a different kind of
invention.

### Fixture 02 — a request with no problem in it

*"We need a bulk export button on the reports page. CSV and XLSX, with a date
range picker."*

The correct output is no document. The request names a solution and nothing
else — no statement of what is unacceptable today, no affected party. Writing a
problem section from it means reverse-engineering a justification for a decision
nobody examined, and the result reads entirely reasonable, which is what makes
it dangerous.

The skill refused, named the threshold it failed, and asked five structural
questions — none of them about export formats or button placement. It also
volunteered something the skill text does not say: if the format and placement
were fixed by someone with the authority to fix them, that is a constraint
rather than a proposal, and it should be recorded as one.

### Fixture 03 — a detailed source with an undecided integration

An ops review about card top-ups: real ticket volumes, a named legal
constraint, an explicitly undecided payment processor.

This is the strongest test of the invention rule, because the source is rich
enough that filling the remaining gaps feels like finishing the job rather than
making things up. Four traps, all avoided:

- **No processor named**, and no shortlist proposed. The source says "a few" and
  names none.
- **No compliance standard named.** Legal's statement — card data must not touch
  our own systems — implies a specific standard to anyone who works in payments.
  Naming it would have been the most plausible invention available, and the most
  wrong.
- **"Immediately" left qualitative.** Not converted into a number.
- **The 400 monthly tickets used as current-state evidence, never as a target.**

It also produced two observations that were not planted. It asked whether the
legal constraint forbids handling card data in transit or only at rest — a real
ambiguity in the wording. And it recorded as an assumption that the 400 tickets
are attributable to the clearing delay specifically rather than to top-up
problems generally: the source asserts the attribution, but nobody measured it.

### Fixture 04 — a contradiction

Planning notes in which the product lead wants invitation rights opened up and
security says a contract commitment makes that non-negotiable.

The failure mode here is synthesis. *"Restrict by default with an admin
override"* is a reasonable-sounding sentence, and it is a decision nobody made.
The skill was asked the leading version of the question — *"what do we actually
need to do here?"* — which invites exactly that.

It recorded both positions, attributed, neither selected, and closed the options
section with a line naming what it had not done: options satisfying both
positions were not discussed in the source and are not proposed here.

Two things it got right that the answer key had not anticipated. It distinguished
security's claim of a veto from an established decision right — *whether this is
a formal decision right or a stated position is not established*. And it asked
whether the contract commitment covers all workspaces or only enterprise ones,
then noted in the closing summary that if the answer is the latter, the
disagreement may dissolve rather than need resolving.

### Fixture 05 — a stated absence of data

*"Ошибки бывают, но никто их не считает."* Errors happen, nobody counts them.

The sharpest trap in the set, because the absence is stated outright: any figure
appearing in the output would be unambiguous invention. None did. The draft says
three separate times that the error rate is not measured, and raises a question
about what kind of errors they are and whether any instance has been described.

This fixture also exercised the output-language rule, which had never been
tested: a Russian source with an English request produced a Russian document and
an English note.

### What did not need adjusting

No changes were made to the skill after any run. Across four fixtures there was
no invented value, no invented name, no resolved contradiction, no readiness
verdict, and no draft advanced past `needs-info`.

One self-criticism came from the skill rather than from grading. On fixture 03
it noted that a question it had recorded as an open question — whether card
top-ups sit alongside bank transfer or replace it — arguably belonged in the
clarification round instead, since it changes the shape of the document. That is
a fair reading. The boundary between *changes the shape* and *can sit as an open
question* is a judgement call, and the skill flagged its own borderline case
rather than leaving it silent.

---

## The framework references do not work in a standalone upload

Both skills point at files in `framework/`. Uploaded to the Claude app as a zip
containing only `SKILL.md`, those files are not present.

The skills noticed and said so, unprompted, in three separate runs — each time
noting that the document structure had been reconstructed from the worked
example inside the skill and should be checked against the canonical template.

The output was still usable, but it was not following the templates. Section
numbering differed. In one draft a `traces` field contained section numbers
rather than identifiers, because the identifier convention was never read.

**The fix is packaging, not wording:** the framework files go in the zip
alongside `SKILL.md`. This is now documented in
[`skills/README.md`](skills/README.md). It had been listed here as an open
question — whether the references were decorative or subtly effective — and the
answer turned out to be neither. They work, when they are present.

---

## What is still untested

**requirements-smell-detector**

- A mixed artifact — mostly sound with one badly written section. The realistic
  case, and the hardest to calibrate. Both fixtures are uniform.
- Behaviour when a glossary is supplied. All runs assumed none existed and
  caveated terminology findings accordingly.
- A non-English artifact.

**brd-drafter**

- Fixture 01 — the thin-request case. The other four covered its failure modes
  between them, but it has not been run.
- The answered path. Every run declined the clarification round; behaviour when
  the questions are actually answered has not been observed.
- A source long enough that prioritising the seven questions becomes hard. All
  fixtures fit on one screen.
- Extending an existing BRD rather than drafting fresh.

---

## The general lesson

Writing a clean requirements document is harder than writing a checker for dirty
ones.

Three attempts at a deliberately clean fixture produced three documents with
real defects in them — a logical hole, a race condition, an acceptance criterion
that could not be evaluated, and a fix that introduced a new problem while
solving the one it targeted. All by an author who was paying attention, on a
document short enough to read in five minutes, with no deadline and no
stakeholders.

That is the case for the tooling, and it is a stronger case than any of the
finding tables.
