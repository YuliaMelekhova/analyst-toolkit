---
id: PDR-002
title: n8n orchestrates the pipeline rather than application code
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-002 — n8n orchestrates the pipeline rather than application code

---

## Context

The pipeline is six integrations, one classification lookup and one decision point. That is well within what a small service in any language could do, deployed and monitored alongside everything else.

The complicating factor is who needs to understand it. The people who argue about routing are compliance and finance, not backend engineers. A routing table they cannot inspect is a routing table they cannot challenge, and a routing decision nobody challenges is one nobody owns.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Stakeholders who challenge routing are non-engineers | A visual, inspectable flow |
| Routing changes are frequent and low-risk | Configuration over deployment |
| Business logic benefits from tests and types | Application code |
| The pipeline is integration glue, not computation | A tool built for integration glue |

---

## Decision

n8n orchestrates the flow, and the workflow export is the deployable artifact. Business logic that cannot be expressed as node configuration is confined to Code nodes and mirrored in documentation where it constitutes policy.

---

## Options considered

### Option A — n8n workflow

**What it is.** The flow as a canvas of typed nodes with a JSON export under version control.

**Consequences.**

| Better | Worse |
|---|---|
| Legible to the stakeholders who challenge routing | Workflow JSON diffs poorly in code review |
| Changing a reviewer channel needs no deploy | Testing is largely manual |
| Retry, error and wait semantics come built in | No type checking across node boundaries |
| Integration credentials are managed outside the artifact | Logic drifts into Code nodes, untyped and untested |

**Why chosen.** The tool matches the shape of the problem while the problem stays integration glue with one decision point. The costs are real and are accepted with a named mitigation for the largest of them (PDR-008).

### Option B — Custom service

**What it is.** A small application in a conventional language, deployed through the normal pipeline.

**Consequences.**

| Better | Worse |
|---|---|
| Unit tests, types, reviewable diffs | Routing becomes opaque to compliance and finance |
| Logic lives in one place with one set of conventions | Every routing change is a deploy |
| Easier to reason about concurrency and idempotency | Retry, wait and webhook handling must be built |

**Why not chosen.** Better engineering properties, worse against the constraint that actually binds: the people who need to argue with the routing matrix cannot read it.

### Option C — Managed integration platform

**What it is.** A hosted iPaaS with similar visual semantics.

**Consequences.**

| Better | Worse |
|---|---|
| No infrastructure to operate | Less control over where request data is processed |
| Comparable legibility | Vendor-specific export formats, harder to publish or reuse |

**Why not chosen.** Data residency and processing location matter in a payments context, and the reduced control is not offset by anything the self-hosted option lacks.

---

## Consequences

**Accepted costs.**

- Review of a routing change means reading a JSON diff or opening the canvas, neither of which is a good review surface.
- No automated test suite. Behaviour is verified by execution against sample payloads.
- Code node contents are outside every static check the repository otherwise applies.

**What becomes harder.**

- Refactoring, since renaming a node breaks expression references that nothing validates.
- Reasoning about concurrency. The identifier sequence problem in `REG 02` is a direct consequence.
- Onboarding an engineer who expects the logic to be in a repository rather than in a canvas.

**What this closes off.**

- Treating the workflow as testable software. It is configuration with embedded logic, and pretending otherwise would produce a test suite nobody maintains.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| Code node logic exceeds roughly a screen per node | Reading the export | Pipeline maintainer |
| A routing change requires an engineer to explain the diff | Review conversations on routing commits | Pipeline maintainer |
| The flow stops fitting on one canvas without scrolling past comprehension | Anyone new taking more than a few minutes to trace it | Pipeline maintainer |

The failure mode here is gradual. Nobody notices the day the canvas stops being legible, so the triggers are deliberately mechanical rather than judgement calls.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Non-engineer stakeholders will actually inspect the flow | needs revisiting; the main benefit is theoretical | medium |
| The pipeline stays integration glue rather than growing computation | needs revisiting, and the triggers above are how we would know | medium |
| Manual verification is sufficient at this change frequency | still holds, but risk rises with every added type | low |

---

## Not decided here

- Whether the workflow export is the source of truth or an artifact of the running instance. Currently ambiguous, and the ambiguity is a known weakness.
- How workflow changes are reviewed. No process is defined.
- Self-hosted versus n8n cloud. This decision is about the tool, not the deployment.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
