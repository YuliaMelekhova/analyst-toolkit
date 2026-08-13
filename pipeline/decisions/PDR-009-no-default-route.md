---
id: PDR-009
title: An unrecognised request type fails the execution rather than taking a default route
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-009 — An unrecognised request type fails the execution rather than taking a default route

---

## Context

`ORC 04` resolves a routing rule by type code. Validation in `ORC 02` should prevent unknown codes reaching it, so an unknown code at this point means something else is wrong: a form change not reflected in the routing table, a type added on one side only, or a payload arriving by an unexpected path.

Every branch needs a behaviour for the unmatched case. The three available shapes are a general queue, a fallback reviewer, or a failure.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| An unmatched request has already revealed a defect | Failing loudly |
| Failing loses the request, which the requester submitted in good faith | Catching it somewhere |
| Queues without owners grow unattended | Not creating one |
| A fallback makes the defect survivable and therefore invisible | Failing loudly |

---

## Decision

No default branch. `ORC 04` throws on an unresolved type code and the execution halts. The cost falls on the pipeline maintainer, who can act on it.

---

## Options considered

### Option A — Throw on unmatched

**What it is.** An explicit error naming the unrecognised code, with no fallback path.

**Consequences.**

| Better | Worse |
|---|---|
| The underlying defect gets fixed rather than absorbed | The request is lost and must be resubmitted |
| Impossible to accumulate a silent backlog | Requires monitoring to notice, which this reference lacks |
| The failure names the exact cause | A single form change can break submissions for one type entirely |

**Why chosen.** An unmatched type is not a request that needs somewhere to go; it is a symptom that two parts of the pipeline disagree. Routing it somewhere resolves the symptom and leaves the disagreement.

### Option B — General queue

**What it is.** Unmatched requests route to a catch-all channel for someone to triage.

**Consequences.**

| Better | Worse |
|---|---|
| No request is lost | The queue has no owner, so it grows |
| Triage can reclassify and resubmit | Nobody is told the queue exists until it is large |
| Degrades gracefully | The defect survives indefinitely because its consequences are absorbed |

**Why not chosen.** Unowned queues are where problems go to become invisible. The graceful degradation is precisely what prevents the fix.

### Option C — Route to the strictest reviewer

**What it is.** Unmatched types default to Compliance Reviewer on the assumption that over-reviewing is safer than under-reviewing.

**Consequences.**

| Better | Worse |
|---|---|
| Nothing unreviewed reaches publication | Penalises compliance for a classification defect |
| Requires no new destination | Trains everyone that unknown types are compliance's problem |
| Fails safe in the risk sense | The reviewer receives a request with no template and no knowledge pack |

**Why not chosen.** It fails safe in one dimension by making another role absorb a defect they did not cause and cannot fix, and the draft they receive was generated against no template anyway.

---

## Consequences

**Accepted costs.**

- A requester whose submission hits this loses the request with no explanation, since no error workflow exists.
- A mismatch between the form and the routing table takes the affected type entirely offline rather than degrading it.

**What becomes harder.**

- Introducing a new type gradually. The form and the table must change together or submissions fail.
- Operating without monitoring, since the failure is only visible in the execution log.

**What this closes off.**

- Any staging area for requests awaiting a routing decision. If a type exists in the form it must have a rule.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| Types are added frequently enough that a staging route becomes worthwhile | Rate of type additions | Pipeline maintainer |
| Unmatched failures occur in production at all | Execution failures at `ORC 04` | Pipeline maintainer |

The second trigger is not an argument for a default route. It is an argument for finding out why validation let the code through.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Validation makes unmatched types rare | needs revisiting; frequent failures would need a softer path | high |
| Someone monitors execution failures | is wrong today; nothing surfaces them | low |
| The requester can resubmit once the defect is fixed | still holds, assuming they are told, which they are not | medium |

---

## Not decided here

- How the requester is notified. Currently they are not, which is a gap listed in `not-automated.md`.
- Whether validation and routing should share a single type definition, which would make this class of mismatch impossible.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
