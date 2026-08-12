# Skills

Agent skills that apply the conventions in [`../framework`](../framework) to
requirements work. Each skill is a folder containing a `SKILL.md` file — plain
Markdown, no code.

This page assumes you have never used a skill before. If you have, skip to
[Installing](#installing).

---

## What a skill is

A skill is a set of instructions an AI assistant loads when it recognises a
matching task. It changes *how* the assistant does something, not *what* it can
do.

Two consequences worth understanding before you start:

**A skill loads by description, not by name.** Each `SKILL.md` opens with a
description of when it applies. The assistant reads it, matches it against what
you asked, and loads the skill if it fits. You do not have to name the skill —
and if you find yourself having to, the description needs work rather than your
prompt.

**A skill is not a guarantee.** It steers behaviour; it does not enforce it.
Output still needs reading. That is why every skill here refuses to certify its
own work.

---

## Available skills

| Skill | What it does | Give it |
|---|---|---|
| [`brd-drafter`](brd-drafter) | Turns an informal business request into a structured BRD draft, flagging gaps rather than filling them | A short request, a ticket, meeting notes, a few sentences describing a need |
| [`nfr-interrogator`](nfr-interrogator) | Interviews you across eight quality categories and assembles a catalogue, recording unknowns rather than filling them | A system or capability that needs quality attributes established |
| [`requirements-smell-detector`](requirements-smell-detector) | Reviews requirements for ambiguity and unverifiable statements, and returns a findings table | A requirement, story, acceptance criterion, BRD section or specification |

They are listed in the order you would typically use them: draft, establish
quality attributes, review.

---

## Installing

Three ways to use these. Pick one:

| Method | Effort | Best for |
|---|---|---|
| [A. Upload to the Claude app](#a-upload-to-the-claude-app) | ~5 min once | Most people. Works on web, desktop and mobile afterwards |
| [B. Claude Code](#b-claude-code) | Requires Claude Code installed | Working alongside a repository |
| [C. Paste into a conversation](#c-no-installation) | 30 seconds | Trying it once, or if the other two are unavailable |

---

### A. Upload to the Claude app

**Step 1 — Turn on code execution.**

Open Settings → **Capabilities** and switch on **Code execution and file
creation**. Skills do not run without it.

On Team or Enterprise plans this lives in Organization settings and is
controlled by an owner — if the toggle is missing, ask them.

**Step 2 — Build the folder.**

On your own computer, create a folder named exactly as the skill, and put the
framework files it references inside it:

```
brd-drafter/
├── SKILL.md
└── framework/
    ├── conventions/naming-and-ids.md
    ├── templates/brd.md
    ├── templates/nfr-catalog.md
    └── quality-rules/review-checklist.md
```

**The framework folder is not optional.** Each skill points at those files
rather than restating their contents — that is what keeps the templates and the
agent instructions from drifting apart. Uploaded without them, a skill still
works, but it reconstructs the document structure from the worked example
inside `SKILL.md`, and the result will not match your templates. Section order,
identifier schemes and required fields all come from the framework.

You can tell this has happened: the skill says so, usually in a closing note
about the referenced files being absent.

Which framework files each skill needs is listed under *Framework reference* in
its `SKILL.md`. Copying the whole `framework/` folder is simpler and does no
harm.

Two things people get wrong here:

- The file must be named `SKILL.md` — capitals, that exact name
- The folder name must match the `name:` field at the top of `SKILL.md`

**Step 3 — Zip the folder.**

Right-click the **folder** and compress it. Not the file — the folder. When you
open the resulting archive you should see the folder inside it, and its contents
inside that:

```
brd-drafter.zip
└── brd-drafter/
    ├── SKILL.md
    └── framework/
```

If you see `SKILL.md` sitting at the top of the archive, you zipped the file
instead of the folder. Start again.

**Step 4 — Upload.**

Settings → **Customize** → **Skills** → add a skill → choose your zip.

Repeat for each skill you want. They are independent — installing one does not
require the others.

**Step 5 — Check it took.**

Start a new conversation and ask a normal question without naming the skill:

> Write this up as a BRD: [paste a short request]

> I need to work out non-functional requirements for [a system]. Can you help?

> Can you review these requirements before I send them to the team?

If the response comes back in the skill's output format, it loaded. If it comes
back as ordinary commentary, see
[When it does not trigger](#when-it-does-not-trigger).

---

### B. Claude Code

Claude Code is a command-line tool that runs in a folder on your computer. It
reads skills from two places:

- `.claude/skills/` inside the folder you are working in — available in that
  project only
- `~/.claude/skills/` in your home folder — available everywhere

**Step 1 — Get the files.** Either clone this repository, or download it as a
zip from the GitHub page (Code → Download ZIP) and unpack it.

**Step 2 — Copy the skill folders into place.**

```bash
mkdir -p ~/.claude/skills
for s in brd-drafter nfr-interrogator requirements-smell-detector; do
  cp -r analyst-toolkit/skills/$s ~/.claude/skills/
  cp -r analyst-toolkit/framework ~/.claude/skills/$s/
done
```

Replace `~` with your project folder plus `.claude/skills` if you want them
scoped to one project instead. The second copy is the framework — without it
the skills have nothing to reference.

**Step 3 — Use it.** Open Claude Code in a folder containing the document you
want worked on, and ask in plain language:

```
Review requirements.md — is anything in there ambiguous?
```

---

### C. No installation

For a one-off, or when you cannot install anything.

**Step 1.** Open `SKILL.md`, select everything, copy.

**Step 2.** Start a new conversation and paste it, with one line above it:

> Follow the instructions below for this conversation.
>
> *[paste SKILL.md here]*

**Step 3.** In the same message or the next one, paste the material you want
worked on. If the output should follow your templates, paste those too.

**What this does not test.** The skill will behave correctly, but you have told
it to — so this proves nothing about whether the description triggers on its
own. Fine for using the skill, not for evaluating it.

---

## Using the skills

### brd-drafter

**Give it** an informal request: a few sentences, a ticket, notes from a
meeting. It does not need to be well organised — that is the point.

**Expect questions first.** One round of up to seven questions about things that
change the shape of the document, then it drafts. You can decline and tell it to
draft anyway; the unanswered questions become open questions in the output.

**You get back** a document plus a short note recording what it was drafted
from, why the status is what it is, what the blocking gaps are, and — the line
worth reading first — what it **assumed**. Anything inferred rather than stated
appears there.

**The status will be `draft` or `needs-info`, never anything further.**
`in-review` and `approved` are reached by a person.

**A thin draft is a correct draft.** A three-sentence request produces a
document that is half empty with a long list of open questions. That is the
output working, not failing — the gaps are the deliverable, and the document is
the container for them.

If a request names only a solution — *"add an export button"* — the skill will
refuse to draft and ask what makes the current situation unacceptable. Writing a
problem statement backwards from a requested feature is how a document ends up
justifying a decision nobody examined.

### nfr-interrogator

**This one asks rather than reads.** Set aside time: eight categories, one at a
time, two or three questions each. A full interview runs fifteen to twenty
exchanges. You can stop early and ask it to assemble what it has.

**Have the person with the answers present.** The skill will not supply values,
so an interview conducted by someone guessing on the business's behalf produces
a catalogue of guesses with a plausible shape.

**"I don't know" is a real answer** — it will ask who does, and record the
category as unestablished with an owner. That is more useful than a number
nobody stands behind.

**You get back** three tables — established, not established, not applicable —
plus a summary. Two things there are worth reading closely:

- **`Suggested by me and accepted`** — any value that originated in the
  conversation rather than from you, even after you agreed to it. Whoever reads
  the catalogue next needs to know which numbers came from the business
- **Entry statuses.** A value with no verification method is `deferred`, not
  `agreed`, however firm the number. A requirement nobody checks is
  documentation

**Expect fewer than eight categories to be established.** Six with values, two
with owners and no values, one marked not applicable is a realistic and useful
result.

### requirements-smell-detector

**Give it** anything written as a requirement: a full story, a single acceptance
criterion, a section of a BRD, an API description in prose. It works on
fragments.

Worth telling it two things, because they change the review:

- **What the artifact is** — a BRD tolerates open questions that a
  ready-for-sprint story does not
- **How far along it is** — a first draft is read for gaps, a pre-approval
  artifact for defects

**You get back** a findings table — quote, smell, class, suggested
reformulation — and a summary. A dash in the quote column means the finding is
about something absent from the whole document rather than a specific sentence.

Read the classes carefully:

- **Blocking** — cannot be built as written. Rare, and zero on a well-written
  document
- **Should fix** — buildable, but two developers would build it differently
- **Consider** — a note; decline it freely

The summary names the **cleanest statements** as well as the defects. That line
tells you what to preserve when you rewrite.

---

## What these skills will not do

Deliberate limits, not gaps:

| They will not | Why |
|---|---|
| Invent a number, threshold or target | A plausible invented value becomes a real commitment the moment somebody reads it |
| Offer a conventional value as though it were yours | 99.9% and p95 are defaults, not answers. If one is suggested, the summary records that it was |
| Name a system, vendor or standard that is not in the source | Same problem, harder to spot |
| Fill in a missing failure path | Naming the gap is the work; filling it is a different task |
| Rewrite your requirements wholesale | A corrected version gets accepted as-is, and the review teaches you nothing |
| Resolve a contradiction between stakeholders | Recording both positions is analysis; picking one is a decision, and it is not theirs |
| Judge scope, priority, effort or business value | Different review, different reader |
| Tell you anything is ready | A human decision against [`definition-of-ready.md`](../framework/quality-rules/definition-of-ready.md) |

If you want the gaps filled rather than named, ask for that separately, after
reading what is missing.

---

## When it does not trigger

The skill loaded but did nothing, or you got ordinary commentary instead of the
expected output.

| Check | Fix |
|---|---|
| Is code execution on? | Settings → Capabilities |
| Does the folder name match the `name:` field? | Rename the folder |
| Was the folder zipped, or the file? | Re-zip the folder |
| Did you ask about the thing the skill covers? | The descriptions are narrow on purpose |
| Is the input the right kind? | Prose that is neither a requirement nor a request is explicitly out of scope |

If everything checks out and it still does not trigger, the `description` field
is what to change — not the body of the skill. That field is the only part the
assistant reads when deciding whether to load it.

**If the output ignores your templates**, the framework files are missing from
the upload. See [step 2](#a-upload-to-the-claude-app).

---

## Testing a skill

Each skill folder has a `tests/` directory with fixtures and answer keys. They
exist because a skill that has never been run against known input is a guess
about its own behaviour.

The three are graded on different properties:

- **A reviewer is judged on what it finds** — and, more importantly, on what it
  leaves alone. Fixtures: one deliberately bad document, one carefully written
  one
- **A drafter is judged on what it refrains from writing.** Fixtures: five short
  requests, each provoking a different kind of invention — a request with no
  problem in it, a source naming an unidentified external system, a
  contradiction, a stated absence of data
- **An interviewer is judged on the conversation**, which no fixture captures.
  Run it against a system you actually know and watch for a number you did not
  say

The fastest way to grade a draft or a catalogue: take any specific in it — a
number, a name, a date, a rule — and find it in the source or in what you said.
If it is not there, it should be on the `Assumed` or `Suggested by me` line. If
it is on neither, that is the failure these skills exist to prevent.

**How to read a run.** Judge the findings on their merits before comparing them
against the key. A finding that names a real defect is correct whether or not it
was planted — and in practice, writing a genuinely clean fixture is harder than
it sounds. See [`NOTES.md`](../NOTES.md) for what happened when we tried.

---

## Writing your own

The skills here follow a fixed structure. If you adapt one, keep these parts:

| Section | Purpose |
|---|---|
| `name`, `description` | The description decides when the skill loads. It is the highest-leverage text in the file |
| Role | Who the assistant is being, and what it is not responsible for |
| Task | The single job, stated once |
| Framework reference | Points at the standards rather than restating them, so updating the standard updates the skill |
| Output format | Exact shape, with a worked example |
| Constraints | What not to do. Usually the part that decides whether the output is usable |
| Output language | Which language to answer in |

The two that carry the most weight in practice are **constraints** and the
**worked example**. The example sets the density and tone of everything the
skill produces; without one, output drifts between terse and exhaustive run to
run.

One pattern worth copying: give the skill somewhere to declare what it supplied.
The drafter has an `Assumed` line, the interviewer a `Suggested by me and
accepted` line. Both fired in testing, and in each case a value that would
otherwise have disappeared into the output stayed visible instead.
