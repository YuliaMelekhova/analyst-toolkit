---
id: PDR-008
title: The routing table is duplicated in the workflow and in documentation
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-008 — The routing table is duplicated in the workflow and in documentation

---

## Context

Six request types map to a reviewer role, a template, a parent page, a publication mode and an SLA. That mapping exists as a JavaScript constant inside `ORC 04` and as a table in `routing-matrix.md`.

Duplicating a decision table is normally a defect, and the usual answer is to generate one artifact from the other. The complication is what the table is for. It is not only configuration: it is the statement of who is accountable for reviewing which class of document, and the people who need to challenge it are compliance and finance.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Duplicated data drifts | A single source |
| The table must be reviewable by non-engineers | A prose artifact |
| The table must be what actually executes | The workflow constant |
| Six rows is a small amount of data | Not building tooling |

---

## Decision

Keep both. Divergence between them is a defect in both artifacts, not a documentation lag, and a change to the table is one commit touching both files. Alignment is verified by a check rather than by care.

---

## Options considered

### Option A — Duplicate, with a verification check

**What it is.** Both artifacts maintained by hand, with a script that parses the workflow constant and asserts the documented table matches.

**Consequences.**

| Better | Worse |
|---|---|
| The reviewable artifact is prose, readable by the people who challenge it | Two edits for every change |
| The executing artifact is the workflow, with no indirection | They will drift if the check is not run |
| No build step, no generator to maintain | The check is additional code with its own maintenance |

**Why chosen.** At six rows the synchronisation cost is small and the check makes drift detectable rather than merely regrettable. The alternative single-source options each sacrifice one of the two properties the table needs.

### Option B — Generate the document from the workflow

**What it is.** A generator reads the workflow export and emits the matrix section of `routing-matrix.md`.

**Consequences.**

| Better | Worse |
|---|---|
| Drift becomes impossible | A build step and a generator for six rows |
| One place to edit | Generated prose is thin; the rationale sections still cannot be generated |
| | The document becomes partly machine-owned, discouraging human annotation |

**Why not chosen.** Disproportionate at this size, and it solves only the table while the valuable half of the document is the reasoning attached to each row, which cannot be generated.

### Option C — Load the table from external configuration at runtime

**What it is.** Both the workflow and the document reference a single configuration file.

**Consequences.**

| Better | Worse |
|---|---|
| Genuinely one source | Moves the reviewable artifact somewhere with no review process attached |
| Routing changes without touching the workflow | Adds a runtime dependency and a failure mode |
| | The document still has to restate it to be readable |

**Why not chosen.** It relocates the problem. A configuration file is not more reviewable by compliance than a workflow node, and the document would still duplicate it in prose.

---

## Consequences

**Accepted costs.**

- Two edits per change, and the discipline to make them one commit.
- A verification script that must be maintained and actually run.
- The check currently exists as a session artifact rather than a committed test, which means it does not run. This is a known gap recorded in `NOTES.md`.

**What becomes harder.**

- Adding a routing field, which now means changing the constant, the table, and the check.
- Trusting the documented table without having run the check.

**What this closes off.**

- Treating `routing-matrix.md` as descriptive commentary. It is a normative artifact held to the same standard as the code.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| The table exceeds roughly twenty rows | Row count | Pipeline maintainer |
| A second duplicated table appears | Any new mapping documented twice | Pipeline maintainer |
| A drift incident occurs | Verification check failing, or a reviewer routed wrongly | Pipeline maintainer |

Any one of these makes Option B proportionate.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| The verification check will be run on every change | is wrong today; the check is not committed | low |
| Compliance and finance actually read the documented matrix | needs revisiting; the entire justification rests on this | medium |
| The table stays small | needs revisiting, and the trigger above catches it | high |

---

## Not decided here

- Where the verification check lives or how it is invoked. Currently nowhere, which is the largest open item in this record.
- Whether other duplicated content in the directory warrants the same treatment. The agent instruction is duplicated in `agent-contract.md` on the same reasoning and has no check.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
