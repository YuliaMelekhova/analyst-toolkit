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
| [`requirements-smell-detector`](requirements-smell-detector) | Reviews requirements for ambiguity and unverifiable statements, and returns a findings table | A requirement, story, acceptance criterion, BRD section or specification |

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

On your own computer, create a folder named exactly as the skill:

```
requirements-smell-detector/
└── SKILL.md
```

Two things people get wrong here:

- The file must be named `SKILL.md` — capitals, that exact name
- The folder name must match the `name:` field at the top of `SKILL.md`

**Step 3 — Zip the folder.**

Right-click the **folder** and compress it. Not the file — the folder. When you
open the resulting archive you should see the folder inside it, and `SKILL.md`
inside that:

```
requirements-smell-detector.zip
└── requirements-smell-detector/
    └── SKILL.md
```

If you see `SKILL.md` sitting at the top of the archive, you zipped the file
instead of the folder. Start again.

**Step 4 — Upload.**

Settings → **Customize** → **Skills** → add a skill → choose your zip.

**Step 5 — Check it took.**

Start a new conversation, attach a requirements document, and ask a normal
question without naming the skill:

> Can you review these requirements before I send them to the team?

If the response comes back as a findings table with smells and classes, the
skill loaded. If it comes back as ordinary commentary, see
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

**Step 2 — Copy the skill folder into place.**

```bash
mkdir -p ~/.claude/skills
cp -r analyst-toolkit/skills/requirements-smell-detector ~/.claude/skills/
```

Replace `~` with your project folder plus `.claude/skills` if you want it
scoped to one project instead.

**Step 3 — Use it.** Open Claude Code in a folder containing the document you
want reviewed, and ask in plain language:

```
Review requirements.md — is anything in there ambiguous?
```

**Why this method is worth the setup.** The skill can read the
[`framework`](../framework) files it references, rather than working from the
descriptions inside `SKILL.md` alone. If you want that, copy the framework
folder alongside:

```bash
cp -r analyst-toolkit/framework ~/.claude/skills/requirements-smell-detector/
```

---

### C. No installation

For a one-off, or when you cannot install anything.

**Step 1.** Open `SKILL.md`, select everything, copy.

**Step 2.** Start a new conversation and paste it, with one line above it:

> Follow the instructions below for this conversation.
>
> *[paste SKILL.md here]*

**Step 3.** In the same message or the next one, paste or attach the
requirements you want reviewed.

**What this does not test.** The skill will behave correctly, but you have told
it to — so this proves nothing about whether the description triggers on its
own. Fine for using the skill, not for evaluating it.

---

## Using the smell detector

### What to give it

Anything written as a requirement: a full story, a single acceptance criterion,
a section of a BRD, an API description in prose. It works on fragments — you do
not need a complete document.

It is worth telling it two things, because they change the review:

- **What the artifact is** — a BRD tolerates open questions that a
  ready-for-sprint story does not
- **How far along it is** — a first draft is read for gaps, a pre-approval
  artifact for defects

If you say nothing, it will assume something and tell you what it assumed.

### How to phrase the request

Any of these work:

> Review these requirements before I hand them over.

> Is anything here ambiguous?

> This is a first draft of a BRD section — what needs work?

> Check the acceptance criteria in this story.

You do not need to say "use the requirements-smell-detector skill". If you have
to, the description is not matching and should be fixed.

### What comes back

A findings table and a summary:

| Column | Meaning |
|---|---|
| Quote | The exact text at issue. A dash means the finding is about something absent from the whole document, not a specific sentence |
| Smell | Which of the ten defect types it is |
| Class | Blocking, should-fix, or consider |
| Suggested reformulation | What is missing and what would resolve it |

Read the classes carefully — they carry most of the information:

- **Blocking** — cannot be built as written. Should be rare, and zero on a
  well-written document
- **Should fix** — buildable, but two developers would build it differently
- **Consider** — a note; decline it freely

The summary names the **cleanest statements** as well as the defects. That line
is worth reading: it tells you what to preserve when you rewrite.

### What it will not do

Deliberate limits, not gaps:

| It will not | Why |
|---|---|
| Rewrite your requirements | A corrected version gets accepted wholesale, and the review stops teaching you anything |
| Fill in a missing number | An invented threshold becomes a real commitment the moment someone reads it |
| Write your missing failure paths | Naming the gap is the review; filling it is the writing |
| Judge scope, priority or business value | Different review, different reader |
| Tell you the requirements are ready | That is a human decision against [`definition-of-ready.md`](../framework/quality-rules/definition-of-ready.md) |

If you want the gaps filled rather than named, that is a different task — ask
for it separately, after you have read the findings.

---

## When it does not trigger

The skill loaded but did nothing, or you got ordinary commentary instead of a
findings table.

| Check | Fix |
|---|---|
| Is code execution on? | Settings → Capabilities |
| Does the folder name match the `name:` field? | Rename the folder |
| Was the folder zipped, or the file? | Re-zip the folder |
| Did you ask about *requirements*? | The description triggers on requirements review, not on general document feedback |
| Is the text actually a requirement? | Prose that is not a requirement is explicitly out of the skill's scope |

If everything checks out and it still does not trigger, the `description` field
is the thing to change — not the body of the skill. That field is the only part
the assistant reads when deciding whether to load it.

---

## Testing a skill

Each skill folder has a `tests/` directory with fixtures and answer keys. They
exist because a skill that has never been run against a known-bad document is a
guess.

Two kinds:

- **Recall fixtures** — deliberately bad text with many planted defects. Tests
  whether the skill finds them
- **Precision fixtures** — carefully written text with two or three subtle
  defects. Tests whether the skill invents findings that are not there

Precision matters more. A reviewer who flags everything gets ignored within a
week.

**How to read a run.** Judge the findings on their merits before comparing
against the key. A finding that names a real defect is correct whether or not
it was planted — and in practice, writing a genuinely clean fixture is harder
than it sounds. See [`NOTES.md`](../NOTES.md) for what happened when we tried.

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
