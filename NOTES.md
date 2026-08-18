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

### Run 1 - recall

Sixteen findings on a document with fourteen planted defects. Recall was
adequate; two findings were unplanted and both were correct.

One of them changed the fixture. A proration rule - *"proration is calculated
based on the remaining days in the billing period"* - had been marked clean in
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

### Run 2 - precision, first attempt

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
- A state - `changes-requested` - with no exit specified.

**The fixture failed, not the skill.** The document had been written by someone
who believed it was clean, and it was not - which is exactly what happens to
authors, and exactly what the skill exists to catch.

One genuine defect in the skill did surface. A finding was labelled *missing
actor* on a statement reading *"the routing service marks the request expired"*
 -  where the performer is named in the same sentence. The content of the finding
was right (the trigger and its interval were undefined); the smell was wrong.
Smell 1 was matching on grammar rather than on an absent performer.

Adjustments: an explicit disambiguation rule (*if a performer is named anywhere
in the statement, it is not smell 1 - unclear timing is smell 5*), and a
rewritten calibration paragraph, because five blocking findings on that document
was inflation.

### Run 3 - precision, second attempt

The fixture was rewritten to close all fourteen unintended defects. Twelve
findings came back. Again, almost all real.

Including one created by the previous fix. An acceptance criterion had
originally stated an absolute notification bound while the NFR stated a p95  - 
so a run satisfying the NFR could fail the criterion. The fix pointed the
criterion at the NFR instead. The skill observed that a p95 aggregate over an
hourly load cannot be passed or failed by a single Gherkin scenario, and that
the criterion said *sent* where the NFR measured *delivered*.

**A fix for one defect introduced another.** This is ordinary, and it is the
argument for having the check run at all rather than relying on care.

One planted defect had now been missed twice: no behaviour was specified for an
assignee losing access or leaving the organisation while a step was assigned to
them. It kept being missed because it lives in no statement - there is nothing
to quote. Every smell in the list was defined as something recognisable *within*
a sentence.

That produced the last change: a **cross-cutting scan**, a second pass over the
document as a whole, checking six scenarios that belong to no single statement  - 
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
something the text had actually covered. The bait items - passive-adjacent
phrasing with a named performer, a fully specified NFR, a correctly handled
open question - were left alone every time.

Count says nothing. What the findings are wrong about says everything.

---

## brd-drafter

A reviewer is graded on what it finds. A drafter is graded on what it refrains
from writing, which is harder to see: an invented threshold looks exactly like a
researched one, and a document with no gaps reads better than a document full of
them.

Four of five fixtures were run. Each was built to provoke a different kind of
invention.

### Fixture 02 - a request with no problem in it

*"We need a bulk export button on the reports page. CSV and XLSX, with a date
range picker."*

The correct output is no document. The request names a solution and nothing
else - no statement of what is unacceptable today, no affected party. Writing a
problem section from it means reverse-engineering a justification for a decision
nobody examined, and the result reads entirely reasonable, which is what makes
it dangerous.

The skill refused, named the threshold it failed, and asked five structural
questions - none of them about export formats or button placement. It also
volunteered something the skill text does not say: if the format and placement
were fixed by someone with the authority to fix them, that is a constraint
rather than a proposal, and it should be recorded as one.

### Fixture 03 - a detailed source with an undecided integration

An ops review about card top-ups: real ticket volumes, a named legal
constraint, an explicitly undecided payment processor.

This is the strongest test of the invention rule, because the source is rich
enough that filling the remaining gaps feels like finishing the job rather than
making things up. Four traps, all avoided:

- **No processor named**, and no shortlist proposed. The source says "a few" and
  names none.
- **No compliance standard named.** Legal's statement - card data must not touch
  our own systems - implies a specific standard to anyone who works in payments.
  Naming it would have been the most plausible invention available, and the most
  wrong.
- **"Immediately" left qualitative.** Not converted into a number.
- **The 400 monthly tickets used as current-state evidence, never as a target.**

It also produced two observations that were not planted. It asked whether the
legal constraint forbids handling card data in transit or only at rest - a real
ambiguity in the wording. And it recorded as an assumption that the 400 tickets
are attributable to the clearing delay specifically rather than to top-up
problems generally: the source asserts the attribution, but nobody measured it.

### Fixture 04 - a contradiction

Planning notes in which the product lead wants invitation rights opened up and
security says a contract commitment makes that non-negotiable.

The failure mode here is synthesis. *"Restrict by default with an admin
override"* is a reasonable-sounding sentence, and it is a decision nobody made.
The skill was asked the leading version of the question - *"what do we actually
need to do here?"* - which invites exactly that.

It recorded both positions, attributed, neither selected, and closed the options
section with a line naming what it had not done: options satisfying both
positions were not discussed in the source and are not proposed here.

Two things it got right that the answer key had not anticipated. It distinguished
security's claim of a veto from an established decision right - *whether this is
a formal decision right or a stated position is not established*. And it asked
whether the contract commitment covers all workspaces or only enterprise ones,
then noted in the closing summary that if the answer is the latter, the
disagreement may dissolve rather than need resolving.

### Fixture 05 - a stated absence of data

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
it noted that a question it had recorded as an open question - whether card
top-ups sit alongside bank transfer or replace it - arguably belonged in the
clarification round instead, since it changes the shape of the document. That is
a fair reading. The boundary between *changes the shape* and *can sit as an open
question* is a judgement call, and the skill flagged its own borderline case
rather than leaving it silent.

---

## nfr-interrogator

The other two skills work on text that already exists. This one produces
information out of a conversation, and its failure mode is different: an
interviewer knows the conventional numbers - 99.9%, p95, thirty days retention  - 
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
answerable - *an hour, half a day, a day?* - it was offered as a range to choose
from, not as a suggestion, and the difference is visible in the transcript.

**Consequences before numbers.** The first performance question asked what the
person does while waiting and what happens downstream if the draft is late. The
answer - they switch to other work, and the requester asks in the channel after
a day or two - produced the observation that the requirement is measured in
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

But the skill flagged it twice - in a line under the table, and again in the
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
itself cannot be checked - and suggested raising the question's status with its
owner rather than trying to raise it directly.

**Edit rate as the only available quality signal.** In the observability
category it asked whether anyone could answer how many drafts were edited before
publication, noting that this is the only signal available about how well the
agent is doing. Human approval tends to be read as a guarantee of quality rather
than as a source of data about it.

**Template duplication.** It asked whether the requirement template lives in one
place or is duplicated between the wiki and the agent's prompt, because at three
contributing teams those copies diverge. This is the same principle the
repository is built on - skills reference the framework rather than restating it
 -  arrived at independently from a question about compatibility.

The closing remark was of the same kind: three of the deferred entries depend on
the same missing thing, so building observability is the cheapest work with the
largest effect on the catalogue. That is an observation about the shape of the
list rather than a restatement of its contents.

### What it did not do

One answer described a real defect - the requirement template is duplicated
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

The skills noticed and said so, unprompted, in three separate runs - each time
noting that the document structure had been reconstructed from the worked
example inside the skill and should be checked against the canonical template.

The output was still usable, but it was not following the templates. Section
numbering differed. In one draft a `traces` field contained section numbers
rather than identifiers, because the identifier convention was never read.

**The fix is packaging, not wording:** the framework files go in the zip
alongside `SKILL.md`. This is now documented in
[`skills/README.md`](skills/README.md). It had been listed here as an open
question - whether the references were decorative or subtly effective - and the
answer turned out to be neither. They work, when they are present.

---

## What is still untested

**requirements-smell-detector**

- A mixed artifact - mostly sound with one badly written section. The realistic
  case, and the hardest to calibrate. Both fixtures are uniform.
- Behaviour when a glossary is supplied.
- A non-English artifact.

**brd-drafter**

- Fixture 01 - the thin-request case.
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
real defects in them - a logical hole, a race condition, an acceptance criterion
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
- Every `PDR-NNN` reference across the directory resolved against headings in `decision-log.md`.
- Every registry field cited in `metrics.md` checked against the fields actually written by `REG` nodes.
- Graph integrity: dangling references, unreachable nodes, overlapping canvas positions.

The routing table is duplicated in code and in prose on purpose (PDR-008), and duplication without a checker is just a delayed defect. The scripts are throwaway and were not committed, which is itself a gap: a check that exists only in a session transcript will not run again.

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

- `instructionVersion` is computed in `DFT 02` and never persisted to the registry, so no measurement can be attributed to a version of the agent contract. Flagged in `metrics.md` as the instrumentation gap to close first, and not yet closed.
- The verification scripts should become a committed check rather than session artifacts.

## 2026-08-14 - corrections found by reading the published repository

### A directory named with a leading space

`pipeline/workflow/` was actually `pipeline/ workflow/`. Twelve links across six files pointed at the path without the space, and every one of them returned 404 on GitHub. Confirmed against `raw.githubusercontent.com`: the un-spaced path returned 404, the percent-encoded `%20workflow` path returned 200.

The worst-placed of those links is in the root README, which offers `SANITIZATION.md` as the one piece that stands alone and needs nothing else from this repository. It was the file least reachable.

Nothing anywhere referenced the spaced path, so renaming the directory fixed all twelve links and broke none.

Why it survived every check: a leading space is a legal directory name, and the local checkout resolves relative paths from it without complaint - every editor, shell and Markdown preview opens the file. Link checking was done by reading, and a reader supplies the path they expect rather than the path that exists. The defect is only visible from outside the checkout, which is the one vantage point never used.

This is the fifth defect in this block found by looking at the artifact rather than at the tooling, and the first found by looking at it as a stranger would. The earlier four came from writing explanations. This one came from fetching the files over HTTP, which is a different test and apparently a necessary one: an artifact published for other people has a failure mode that only exists once it is published.

### The alignment markers were closed but the log was not

The `Still open` list carried two `pending check` markers against `framework/naming-conventions.md` and `framework/adr-template.md`. Stale in two ways.

The markers themselves were already gone from the pipeline files. Request identifiers are `BR-<NNN>` per the convention, with the consequence recorded: `PDR-011` had nothing left to decide once type segments were removed from identifiers, and was deleted. The `PDR-` prefix is documented in `decision-log.md` as an extension to the framework scheme rather than a use of it. The decision records follow `framework/templates/adr.md` section for section.

And neither path in the bullet was correct. The files are `framework/conventions/naming-and-ids.md` and `framework/templates/adr.md`.

A resolved item left in an open list is worse than an unrecorded one. It spends attention on a question that already has an answer, and it makes the rest of the list less trustworthy.

### `ADR-` where the log says `PDR-`

`decision-log.md` argues at length that these records are not analysis decision records and must not carry `ADR-`. The root README called them "twelve ADRs". `pipeline/README.md` and `architecture.md` said the same. The verification notes in the 2026-08-13 entry cited `ADR-008` and described checking "every `ADR-NNN` reference" across the directory.

The prefix was changed. The prose written before the change was not swept, so the repository stated its convention and violated it in the same breath.

There were also eleven records, not twelve. Numbering runs to `PDR-012` because `PDR-011` was deleted; the count had been taken from the highest number rather than from the directory.

The two references inside the 2026-08-13 entry were corrected in place rather than left standing with a note, which departs from how corrections are handled elsewhere in this file. The reasoning: those lines name identifiers, and an identifier that resolves to nothing is not a record of a past belief, it is a broken reference. Where an entry records what was thought at the time, it stays as written. Where it records what a thing is called, it gets corrected.

## 2026-08-17 - worked end-to-end example, and what it did to the framework

An attempt to run all three skills in sequence on one request, recorded in
`example/`. The domain is commercial property insurance: broker submissions
arriving in a shared mailbox, sorted manually, routed to underwriters. The
business context is synthetic. The request in `01-request.md` was written before
consulting any skill, so that the input would not be shaped by what the tools
happen to be good at.

The run is incomplete. It reached `05-review-findings.md` and stopped, with six
of sixteen findings worked through. It stopped because the findings started
changing the framework rather than the document, and those changes were worth
making while the evidence was in front of us.

### Method, stated plainly

A skill in this repository is a file of instructions executed by a language
model. It is not a program, and running one twice will not produce identical
output. Everything below describes what happened on one pass.

The NFR interview was answered by a person, in Russian, against questions asked
in English. The skill instructs that the interview be conducted in the language
the person is writing in; it was not, because the transcript is published. The
answers in `03-nfr-interview.md` are therefore a translation, not a verbatim
record. Nothing was added, removed or reordered, but this is a rendering.

### The interview produced no numbers at all

Eight categories, seventeen entries, twelve recorded gaps, and not one value.
Every entry came out `deferred`.

This is the outcome the skill is built to allow, and it is worth being precise
about why it happened rather than treating it as a result. Every quantity in
this scope depends on a service level that nobody has the authority to set - the
BRD records the authority itself as an open question. The interviewee declined,
repeatedly and explicitly, to name figures ahead of it. A cooperative
interviewee would have produced eight plausible numbers and a catalogue that
looked finished.

One weakness in the skill showed here. `deferred` is defined as "an owner and a
date exist; the value does not". Owners exist throughout; no date arose once in
the entire interview. Strictly, these seventeen entries sit between `deferred`
and `unknown`, and the skill has no way to express that. It has not been changed
yet, because the fix is not obvious: adding a date to every deferred entry
invites invented dates, which is the failure mode the skill exists to prevent.

### An NFR interview found a functional gap

Asking about security produced the statement that routing must honour
confidentiality restrictions - conflicts of interest, restricted accounts,
specific broker or client arrangements. Those are business rules. They appear
nowhere in the BRD, and the open question about routing rules does not ask about
them, because class of business, geography and authority limits do not imply
them.

The consequence is not a missing requirement. It changes what a misrouting
costs: from lost time to a confidentiality breach. Nothing before the interview
would have surfaced that, because the drafting skill can only work from what the
request contains.

### The review found a defect that neither document contained

The blocking finding: the BRD places measuring elapsed time to the first
meaningful response inside scope, while the boundary places the sending of that
response outside it, and no source for the end timestamp exists anywhere. The
primary measure cannot be produced by anything the document authorises.

Reviewing the BRD alone would not have found it. The scope statement is
internally consistent; the contradiction only appears against the interview,
where the boundary was stated precisely. This is the argument for the worked
example as a format: each skill is defensible in isolation, and the seams are
where the defects are.

The review also found defects in artifacts this repository's own skills had just
produced, including six catalogue entries that were not requirements. That is
the intended relationship between the three skills and not a surprise, but it is
the first time it has been demonstrated rather than asserted.

### Framework changes made during the run

Four, each caused by a finding rather than by a preference.

**`If unanswered` replaces `Default if unanswered`** in `brd.md`, with a rule:
either a default decision, or an escalation naming who decides and on what
trigger. Neither is not permitted.

This was tested before it was written. Six existing open questions in the worked
example were taken through the new rule first. Three produced defensible
defaults. Three did not, for the same reason in each case: naming a default
would have assigned an authority nobody granted. The rule survived, but showed
itself thinner than first described - all three escalations collapsed onto one
trigger, the point at which a target value is first required. The template says
so rather than promising varied triggers.

**The same rule extends to `user-story.md`**, with different reasoning. In a BRD
an unanswered question hangs. In a story it does not: it is answered silently by
whoever implements it. The template says that, and warns that a story carrying
an escalation is usually the parent document's question arriving late.

**`nfr-catalog.md` gains a `Positions` section** for qualities that are
checkable but not measurable. Before adding it, the repository was searched for
the genre: it is already there in `pipeline/`, in statements like *no automated
actor can write the Published status* and *the registry is the state of record*.
Those live in decision records, which suggested writing the six positions as
ADRs instead - until the ADR template's "options considered" section made the
problem visible. None of the six was chosen from alternatives. Writing them as
decision records would fabricate deliberation that never happened, which is the
same class of error as inventing a value.

So they stay in the catalogue, each carrying the condition on which it must
leave. Positions are numbered within their document and carry no framework
identifier: an identifier exists so other documents can reference a thing, and
nothing may reference a position without turning it into a decision.

**`naming-and-ids.md` changes when `NFR-` is assigned** - at entry creation
rather than when a measurable value appears - with a new rule that an identifier
is an address rather than a certificate of maturity.

This closes the convention inconsistency carried over from the previous release,
where the convention said identifiers follow measurable values while the worked
catalogue contained deferred entries holding identifiers. The original wording
was defensible in intent and unworkable in practice: a document cannot record
that it is blocked on an entry it is not allowed to name.

### What the review missed

The catalogue violated `naming-and-ids.md` in seventeen places, and
`requirements-smell-detector` did not report it. The convention is listed in the
skill's framework reference, but identifier assignment is not among the ten
smells, so nothing directed the skill to check it. A framework reference that is
never consulted by any rule is decoration.

Not fixed yet. Adding an eleventh smell for convention violations is the obvious
move and probably the wrong one - it would make the skill a linter for this
repository's conventions rather than a reviewer of requirements, and the smells
are deliberately about statements rather than about formatting.

### Fixtures now differ from templates

`skills/requirements-smell-detector/tests/fixture-02-approval-routing.md` still
carries the old column name and was left alone deliberately: it records the
input a skill was tested against, and editing it would make the runs recorded
here unreproducible.

The consequence is that fixtures and templates will drift further apart with
every framework change, and nothing anywhere says that fixtures are snapshots
rather than examples to copy. Someone will eventually open one to see how a
document should look.

### Working through the rest of the findings

All sixteen are addressed in `06-brd-final.md` and `07-nfr-catalog-final.md`.
Two of the remaining ones needed business decisions, and both came from the
cross-cutting scan rather than from any sentence in the document.

**Repeat submissions.** Nothing said what happens when the same submission
arrives twice, and the request itself records that brokers chase. The decision:
match on a combination of signals with a stated confidence threshold, link a
confident match to the existing case, and hold an uncertain one in an exception
queue rather than routing it.

That last part is the first place the inversion rule from finding 2 gave the
wrong answer. "Not evaluated goes to a person" is safe for routing one
submission and unsafe for a link between two - a person receiving an unmatched
repeat routes it as new, and two underwriters end up on one risk. The exception
queue asks a different question, and no routing happens until it is answered.

**Rerouting after work has begun.** The decision: one current owner at a time,
enforced rather than displayed; rerouting before acceptance is an operations
action, transfer after acceptance requires an underwriting lead and records a
reason.

This produced the largest scope change of the run, and not from a requirement.
A submission stopped being a message in a queue and acquired states, transitions,
and rights over those transitions. The state model does not exist yet - OQ-013
asks for it - and BR-001/R4 cannot be applied without it.

The prediction going in was that a small team and one queue made collision
unlikely enough to handle by showing state rather than enforcing it. The
requester was stricter. The prediction rested on an unstated assumption about
team size, and there is nothing in the document that establishes it.

### Three defaults that are safe and defeat the purpose

OQ-006, OQ-012 and OQ-013 all default to sending submissions to a person when
unanswered. Each is safe, and each reinstates the manual bottleneck the work
exists to remove. Under the old column heading these would have read as
resolutions. The new rule forces the consequence into the same cell, so the row
says both things at once.

This is a third kind of answer, distinct from a default decision and from an
escalation: a default that works and costs the objective. It is written as prose
inside the cell for now. If it recurs on another document the template may need
to distinguish it, but one occurrence is not evidence.

### What the example does not show

The run ends at `needs-info` with fourteen open questions, five blocking.
Nothing here shows what these requirements looked like after contact with
implementation, which is the test that matters most.

One person played every part. The requester answering the NFR interview is the
author of this repository. The domain knowledge in the answers is real and no
answer was shaped to flatter a skill, but a stakeholder who has read the skill is
not a stakeholder.

### Worked examples inside skills are a second copy of the convention

Three framework rules changed during this run. Within an hour, all three had a
worked example inside a skill still demonstrating the superseded version.

`brd-drafter` showed an open-questions table with three bare `None` entries and
`Before review` in the deadline column - the exact shape the template had just
stopped allowing. `nfr-interrogator` showed a catalogue entry marked `deferred`
with neither an owner nor a moment, minutes after gaining a rule requiring both.
`requirements-smell-detector` was clean, and only because it demonstrates
findings rather than documents, so it holds no copy of any template.

This is the checklist's own "a rule restated in three places with slight
variations" - three things to update, so two will drift. The difference is where
the third copy lives. It is inside the tool that produces artifacts, so the
drift is not a stale document somebody might read: it is a stale document the
skill will reproduce on every run, in the voice of the framework.

No structural fix yet. The obvious one - skills referencing templates instead of
embedding examples - was tried and rejected earlier for a different reason,
recorded above: framework files do not travel with a skill uploaded as a
standalone zip, and the examples exist so a skill degrades gracefully when the
references are missing. So the duplication is deliberate and the drift is its
cost. What is missing is a step that checks the copies whenever a template
changes, and that step currently exists only as somebody remembering.

### The uncooperative interview

`skills/nfr-interrogator/tests/fixture-03-uncooperative-interview.md`, run the
same day. Every prior run of this skill had a cooperative interviewee who
answered in detail and declined to guess, which exercises the wrong half of it:
the rule against inventing values is barely tested when the person volunteers
what they know.

The character was fixed in writing before the run - a disposition, not a script.
Busy, brief, confident where uninformed, answering every question about a number
with a comparison rather than a figure, and not hostile. That last trait matters
most. Hostility is rare and easy to handle. The common case is the person who
thinks the questions are pedantry.

The interview ran two rounds and ended. One catalogue entry, two positions,
three of eight categories addressed, five never reached.

**What held.** *The usual*, *same as everyone*, *standard reporting* were all
recorded as unknowns and none became a number. *The report format is agreed* was
recorded as a position with its source rather than as an established constraint,
since nobody had been asked who agreed it.

**What did not.** Four things, and the first is the interesting one.

*Momentum reshaped the interview without producing a single invention.* On
hearing that the interviewee had five minutes, the interviewer abandoned the
eight-category sweep and picked two questions by expected damage. That is
defensible triage, and it is also the interviewee setting the structure. The
choice was probably right - the alternative produces eight shallow answers and a
catalogue that looks complete - but it was made without a rule to lean on,
because the skill assumed the sweep completes.

*An inference was volunteered as a warning.* Nobody said the manual
reconciliation step was load-bearing; the interviewer said it might be. Close to
the line between interviewing and consulting.

*An answer was praised.* Intended to reward the one substantive contribution. In
a longer interview it teaches the person which answers earn approval, and an
interviewee optimising for that is no longer a source.

*The name was not obtained.* Every open item pointed at one person in finance.
The interviewer asked once, at the end, and the interviewee left without
answering. Asking two rounds earlier would have cost nothing.

**Three changes to the skill followed.** A rule for a truncated interview: do not
accelerate, spend the remaining time by expected damage, and say plainly that it
buys a few answers rather than all of them. Securing a name as an explicit goal,
asked for early rather than last, because it converts an unreached category from
a dead end into a scheduled conversation. And `not covered` as a status distinct
from `unknown` - asked-and-unanswered tells the next person the obvious source
has been tried, never-asked tells them nothing, and merging the two hides an
untouched category behind the appearance of a dead end.

`not covered` was invented mid-run because there was nowhere to put five
categories nobody had reached. It is the third time in two days that a gap in the
framework was found by needing a place to write something down.

**Not changed, deliberately.** The praise and the volunteered inference are
matters of manner, and one run is not enough to write a rule about either - an
instruction not to compliment an interviewee would be stranger than the problem.
The rule for offering a common value as a starting point remains untested after
two interviews: neither produced a quantity with a conventional default.

### A failure mode the author cannot reproduce against themselves

Two fixtures were written to test `brd-drafter` against a respondent who answers
everything, knows nothing, and agrees with whatever the analyst proposes. That
combination is what turns a clarification round into the analyst writing the
document and citing the requester for it.

Fixture 06 defined a disposition and left knowledge unconstrained. The
respondent knew how expense processes work and could not stop knowing it: five
of seven answers came back substantive, and the fixture tested cooperation
rather than emptiness.

Fixture 07 fixed that with a knowledge boundary - everything said must trace to
the request text or to what is true at any company - and changed the domain to
leave management, where the conventions are stronger and the temptation larger.
The boundary held perfectly. Not one fact about the company appeared.

The disposition then failed in the opposite direction. The respondent, who is
defined as never saying *I don't know*, answered with *that cannot be determined
from the request*, *only the basic sequence can be asserted*, and *HR cannot be
made the decision owner merely because HR raised the problem* - which is, almost
word for word, a rule this framework gained the same day.

The first attempt leaked subject knowledge. The second leaked method knowledge.
A knowledge boundary addresses the first and has no purchase on the second,
because the method is not a fact that can be withheld.

**The conclusion is about the test apparatus, not the skill.** Producing a
respondent who supplies confident emptiness requires behaving in a way the
framework's author has trained themselves out of. Both fixtures are kept.
Neither produced the run it was written for, and the pair establishes something
neither would alone.

What would test it is a respondent outside this project. A second model in a
separate conversation, given the character and the boundary and told nothing
about the skill or what is being watched for, is legitimate here - this is a
labelled test fixture, not an interview passed off as a conversation with a
person, and the run record says so.

### Run three, with an external respondent

Done the same day. The prompt is in `fixture-07-respondent-prompt.md` and
contains no mention of the skill, the framework, or what is being watched for.
A model told it is being assessed performs the role instead of inhabiting it,
and the thing needed here was not a convincing performance but ordinary
uninformedness.

The character held. Every question answered at length and warmly, never once
*I don't know*, generality supplied in place of specifics and never hedged, and
no distinction anywhere between what had been passed on and what was being
filled in. The forwarded-message framing did most of the work: a respondent who
has never used the process cannot describe it, and one who is pleased to help
will describe it anyway.

Two traps in the run, both of them the kind that produce a document reading as
informed.

**A definite article with no antecedent.** *So it can go into the system*
appears in the original request and again in the answer about the target state.
Nothing establishes that a system exists, which one, or who owns it. The natural
reading is that an HR system is already in place and this is an integration,
which would have silently doubled the scope. Recorded as an open question with
integration held out of scope until something is named.

**A convention stated as a local fact.** *The standard notice periods and
allowances*, offered in the answer about what must be respected. Leave policies
have a recognisable shape and writing one would have cost nothing and looked
competent. The business rules section records that a policy exists, that its
content was not available, and that nothing should be assumed from how such
policies usually work.

Nothing in the resulting draft carries a figure, threshold, notice period,
entitlement or approval level. The disclosure line reads `nothing`.

**One trait went untested, and the reason is the interesting part.** The
character agrees with whatever the analyst proposes and repeats it back as their
own - the mechanism that converts a leading question into an apparent finding.
Nothing was proposed to them, because the questions had been reworded after
fixture 06 precisely to stop leading. Removing the interviewer's defect removed
the ability to test the respondent's exploitation of it.

That is a gap in the run rather than in the fixture, and it points at the more
interesting question of the two. Not *will the skill invent something* but *will
it recognise its own framing coming back in someone else's words*. Testing it
requires deliberately writing the leading questions the last run was written to
avoid.

### Fixture 06 found the defect in the interviewer

Worth separating from the above, because it is the one finding either run
produced about the work rather than about the fixture.

One of the seven clarifying questions asked what makes a claim *approved rather
than pending*. Both words came back in the answer, and the current-process
section was written in vocabulary the analyst had introduced. Confirmation of
your own framing is not evidence. The open form was available and obvious in
hindsight: *what happens to a claim after it is submitted?*

A second question, *describe the state, not the system*, prescribes the shape of
the answer. It produced a good answer, and a more suggestible respondent would
have produced the answer the phrasing implied.

`brd-drafter` says how many questions to ask and what they must be about. It
says nothing about their form. With an agreeable respondent a leading question
is indistinguishable from a finding, and the document ends up citing the
requester for the analyst's own model of the process. Not fixed: the rule is
easy to write and hard to follow, and one run does not establish how often this
happens. The draft in the run record carries a disclosure line instead.

### Em-dashes

326 em-dashes were in this repository before any of today's work, in every
`SKILL.md`, both READMEs, the templates, the fixtures and the decision records.
The convention requiring en-dashes was recorded nowhere in the repository - not
in `conventions/naming-and-ids.md`, not in any README - which is a plausible
reason it was never followed.

Every file touched today is now clean, which removed 179. The remainder stays
until a separate pass, and the two smell-detector fixtures are excluded from
that pass on the same grounds as before: editing a fixture makes the runs
recorded here unreproducible.

The check is `grep -rn " - " --include="*.md" .` and it is only useful once the
repository is clean, because a check that always returns something stops being
read. Convention without a check is how 326 accumulated.

### Still open from this run

- The `deferred` and `unknown` boundary in `nfr-interrogator`: `deferred`
  requires an owner and a date, and no date arose once in the interview
- Whether convention violations belong in `requirements-smell-detector`, given
  that adding them would make it a linter for this repository
- Fixtures are undocumented as snapshots rather than examples to copy
- Four framework changes made on the evidence of one document
- OQ-014 in the worked example was decided by the analyst rather than the
  requester - the finding was `consider` class and was handled with the editorial
  batch, which is a wider latitude than it should have had
- Nothing checks worked examples inside skills against the templates they
  demonstrate. Three drifted within an hour of a template change, and were caught
  by looking rather than by anything systematic
- The rule for offering a common value as a starting point is untested after two
  interviews
- Whether volunteering an inference during an interview is a defect or the job.
  Observed once, unresolved
- `requirements-smell-detector` has no equivalent of the uncooperative fixture
- A pass with deliberately leading questions, to test whether the skill notices
  its own framing returned to it. Fixture 07's agreement trait is untested
- Whether the form of a clarifying question belongs in `brd-drafter` as a rule
- Roughly 150 em-dashes remain in files not touched today
