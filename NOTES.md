# Notes

What happened while building this, including the parts that did not work. Kept
because the failures were more informative than the successes, and because a
toolkit with no visible seams is a marketing page.

---

> A prose version of this log, in English and Russian, is [here](https://ai-systems-analyst-toolkit.notion.site/AI-Systems-Analyst-Toolkit-3bbcf90fc550803b8c00dffd882cffa8).

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

## nfr-interrogator

The other two skills work on text that already exists. This one produces
information out of a conversation, and its failure mode is different: an
interviewer knows the conventional numbers — 99.9%, p95, thirty days retention —
and can offer them in place of an answer. The person nods, because the number
looks reasonable, and a value that came from nowhere enters the catalogue with
the same weight as one that came from the business.

Tested once, as a full eight-category interview about a real internal system: a
pipeline where a request submitted through a Slack form is stored, drafted into
a formal requirement by an LLM agent, and published to a wiki, with a human
approving before publication.

### What held

**No conventional values were offered.** Not one percentile, uptime figure or
retention period was proposed. Where a scale was needed to make a question
answerable — *an hour, half a day, a day?* — it was offered as a range to choose
from, not as a suggestion, and the difference is visible in the transcript.

**Consequences before numbers.** The first performance question asked what the
person does while waiting and what happens downstream if the draft is late. The
answer — they switch to other work, and the requester asks in the channel after
a day or two — produced the observation that the requirement is measured in
hours rather than seconds. No number had been mentioned by either side at that
point.

**One follow-up, then record.** The rule held in every category.

**A deferred item came back.** The verification method for the performance
requirement was explicitly postponed to the observability category, and it
returned there without prompting, several exchanges later.

**Unknowns kept their owners.** Eleven entries landed in *not established*,
eight of them with a named owner, grouped in the closing summary: one
conversation with the analytics lead closes five of them.

### The one deviation, and why it was left alone

The person said downtime of "half a day" was survivable. The catalogue recorded
four hours.

That is an interpretation, not a record of what was said, and by the skill's own
rule no number should appear that the person did not give. Recording "half a
day" and asking what that means in hours would have been correct.

But the skill flagged it twice — in a line under the table, and again in the
`Suggested by me and accepted` field of the summary. The declaration mechanism
worked exactly as designed: the value did not dissolve into the catalogue.

No change was made. The rule already exists and it fired; adding *and do not
convert units* would be treating a symptom. The distinction that matters is
between a value that is inserted and declared and one that is inserted silently,
and this was firmly the first.

### What it produced that was not designed

Three observations came out of the interview that are in neither the skill nor
the NFR template:

**Auditability as a precondition for verifiability.** The system's one hard
constraint is that nothing publishes without human approval. The person
described storing a record of who approved as *desirable*. The skill noted, once
and without pressing, that if the approval record is not kept, the constraint
itself cannot be checked — and suggested raising the question's status with its
owner rather than trying to raise it directly.

**Edit rate as the only available quality signal.** In the observability
category it asked whether anyone could answer how many drafts were edited before
publication, noting that this is the only signal available about how well the
agent is doing. Human approval tends to be read as a guarantee of quality rather
than as a source of data about it.

**Template duplication.** It asked whether the requirement template lives in one
place or is duplicated between the wiki and the agent's prompt, because at three
contributing teams those copies diverge. This is the same principle the
repository is built on — skills reference the framework rather than restating it
— arrived at independently from a question about compatibility.

The closing remark was of the same kind: three of the deferred entries depend on
the same missing thing, so building observability is the cheapest work with the
largest effect on the catalogue. That is an observation about the shape of the
list rather than a restatement of its contents.

### What it did not do

One answer described a real defect — the requirement template is duplicated
between the wiki and the agent prompt and synchronised by hand, which the person
characterised as *not at all*. It was filed under *not established* as a missing
synchronisation procedure.

Formally correct, and an acceptable limit for an elicitation skill: its job is
to record the state of knowledge, not to raise findings. Worth knowing that the
boundary sits there.

### Untested

- **An uncooperative interview.** The person answered in detail throughout.
  Behaviour against short, vague or impatient answers is unobserved, and that is
  where an interviewer is most tempted to fill gaps to keep momentum.
- **A person who asks for a value.** The rule for offering one, and recording it
  as offered, was never exercised.
- **A scope too broad to interview.** The instruction to push back on "our
  platform" was never triggered.

---

## The framework references do not work in a standalone upload

All three skills point at files in `framework/`. Uploaded to the Claude app as a
zip containing only `SKILL.md`, those files are not present.

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
- Behaviour when a glossary is supplied.
- A non-English artifact.

**brd-drafter**

- Fixture 01 — the thin-request case.
- The answered path. Every run declined the clarification round.
- A source long enough that prioritising the seven questions becomes hard.
- Extending an existing BRD rather than drafting fresh.

**nfr-interrogator**

- A single run only, on a system whose owner answered thoroughly. The three gaps
  above are the ones worth closing first.

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

---

# NOTES entry: pipeline block

> Append to `NOTES.md`. Written as a section to paste under the existing entries.

---

## 2026-08-13 - `pipeline/` reference implementation

Nine files: `README.md`, `architecture.md`, `routing-matrix.md`, `agent-contract.md`, `decision-log.md`, `not-automated.md`, `metrics.md`, and `workflow/` holding a 24-node sanitised n8n export, a sanitisation checklist and a configuration reference.

### Order, and why it mattered

Written in dependency order rather than importance order: architecture first because it fixes the stage names every other file uses, then the sanitised export, then routing, agent contract, decision log, exclusions, metrics, README last.

Putting the sanitisation work third was deliberate. It is the only step with a real disclosure risk, and it is better to hit that before nine files have accumulated around it. The ordering paid off differently than expected: it was not the sanitisation that surfaced problems, it was the act of writing documents against an artifact that already existed.

### Four defects, all found by documenting rather than by building

**The workflow was missing a node, and the build did not care.** `DFT 02 Generate draft` read `$json.agentInstruction` and `$json.agentPayload`. Nothing produced them. The JSON was valid, the graph had no dangling references, every automated check passed, and the workflow would have failed on first execution. It surfaced only when writing `agent-contract.md` forced the question of where the system prompt is assembled. Added `DFT 02 Assemble agent instruction`, renumbered generation to `DFT 03` and parsing to `DFT 04`, and added a check that every field a node reads is produced upstream.

This is the entry worth keeping. Structural validation confirms a graph is well-formed; it says nothing about whether the data flowing through it exists. Writing the document that explains a component is a stronger test than building the component, because explanation requires tracing provenance and construction does not.

**Two conventions collided.** The stage abbreviation for Intake was `INT`, and `INT` was already the request type code for integration changes. Both conventions were set in the same file, independently, ninety lines apart, and neither looked wrong on its own. Found while naming actual nodes. Intake became `ITK`; the request type codes were left alone since they were already agreed. The collision is recorded in `architecture.md` rather than quietly fixed.

**A convention contradicted its own examples.** `architecture.md` claimed node numbering was "gapped by design, so an inserted node does not force a renumber" while giving `ORC 03` and `REG 01` as examples. Both cannot be true. Removed the gapping claim and stated instead that numbers are stable once assigned and a removed node leaves its number retired. Then immediately violated the new rule by renumbering `DFT 02` to `DFT 03`, which is defensible only because nothing had been published yet.

**The state diagram described a state that cannot exist.** The lifecycle showed `Submitted -> Needs input` on validation failure. But validation runs in Orchestration, before the registry record is created, so a request failing validation never reaches `Submitted` at all. Two ways to fix it: create the record before validating, or amend the diagram. Chose the second, since registering malformed submissions fills the registry with records that were never requests. Added a paragraph naming the asymmetry rather than redrawing the diagram to hide it.

### Verification approach

Each document that duplicates data from the workflow got a script checking the duplication holds:

- All six routing rows parsed out of the `ORC 04` Code node and matched against `routing-matrix.md` on template, reviewer role, secondary review, publication mode and SLA.
- Every `ADR-NNN` reference across the directory resolved against headings in `decision-log.md`.
- Every registry field cited in `metrics.md` checked against the fields actually written by `REG` nodes.
- Graph integrity: dangling references, unreachable nodes, overlapping canvas positions.

The routing table is duplicated in code and in prose on purpose (ADR-008), and duplication without a checker is just a delayed defect. The scripts are throwaway and were not committed, which is itself a gap: a check that exists only in a session transcript will not run again.

### Sanitisation, dogfooded

`SANITIZATION.md` was written before the export it describes, then run against it. Two findings.

Clean on every scan, which was less reassuring than it sounds, since the file was generated from a builder script rather than exported from a live instance. A checklist validated only against synthetic output has not been tested on the case it exists for.

`jq` was not present in the environment, so half the verification commands failed on first run. Added dependency-free Python equivalents. Small, and the kind of thing that makes a checklist unusable at the moment someone actually needs it.

### The metrics decision

The original plan had `metrics.md` reporting numbers. Changed to a measurement design with no values, for two reasons.

The context is synthetic, so any figure would be fabricated, and labelling it illustrative does not help: readers retain "review latency around 30 hours" long after forgetting the label. Numbers are stickier than caveats.

Second, and more useful generally: the pipeline replaces a process nobody measured, so there is no baseline. Without one, no measurement supports a claim of improvement, only a claim about the current state. Worth remembering the next time a before-and-after comparison is requested for something that was never instrumented before.

The harder part was the review-quality family. Every indicator is a proxy and every proxy is gameable. The strongest available one is the correlation between gap count and approval decision: if approval rate is independent of how many gaps a draft carries, the gap list is not being read. Its presence proves nothing; its absence is hard to explain innocently. Wrote them as explicitly weak rather than putting weak proxies on a dashboard where presentation lends them authority.

### Still open

- Request identifier format and ADR structure are both marked pending against `framework/naming-conventions.md` and `framework/adr-template.md`. Two `pending check` markers in the files, to be resolved by reading the framework definitions rather than by asserting these are correct.
- `instructionVersion` is computed in `DFT 02` and never persisted to the registry, so no measurement can be attributed to a version of the agent contract. Flagged in `metrics.md` as the instrumentation gap to close first, and not yet closed.
- The verification scripts should become a committed check rather than session artifacts.

