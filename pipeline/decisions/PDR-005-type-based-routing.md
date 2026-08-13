---
id: PDR-005
title: Routing depends on request type alone, not on amount, seniority or declared urgency
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-005 — Routing depends on request type alone, not on amount, seniority or declared urgency

---

## Context

Routing by financial impact is the conventional design, and the request types already carry impact implicitly: a fee change moves revenue, a reporting request does not. Adding a monetary threshold would be straightforward and would look like prudent governance.

The same is true of the other obvious inputs. Seniority is available from Slack, and a priority field is one line of form configuration.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Larger financial impact plausibly warrants more scrutiny | Amount-based routing |
| Reviewer time is scarce | Any rule that filters what reaches them |
| Every routing input is an input requesters can control | Inputs that are hard to manipulate |
| Approval thresholds have a known behavioural response | Avoiding gradients entirely |

---

## Decision

Routing depends on the request type code and nothing else. No route consults a monetary amount, the requester's seniority, or a self-declared priority. Triage within a reviewer's own queue is left to the reviewer.

---

## Options considered

### Option A — Type only

**What it is.** A flat lookup from type code to reviewer role, template, publication mode and SLA.

**Consequences.**

| Better | Worse |
|---|---|
| No gradient to sit below, so no incentive to split requests | A trivial change gets the same reviewer as a significant one |
| The matrix is small enough to be argued about in one sitting | Reviewer load is not shaped by impact |
| Every input is chosen from a fixed set, not supplied as a number | Occasionally wasteful use of senior attention |

**Why chosen.** The waste is visible and bounded, and the reviewer can triage inside their queue with better information than the pipeline has. The alternative introduces a manipulable gradient in exchange for savings that accrue mostly to the requests that needed the least review anyway.

### Option B — Amount bands per type

**What it is.** Within each type, a threshold routes larger requests to a more senior reviewer or adds a secondary approval.

**Consequences.**

| Better | Worse |
|---|---|
| Scrutiny scales with stated impact | The amount is self-reported and unverified at intake |
| Matches how financial approval usually works | Creates an incentive to submit two requests instead of one |
| Defensible to an auditor at first glance | Doubles the size of the routing matrix |

**Why not chosen.** Approval thresholds reliably produce requests that sit just below them. This is not a hypothetical risk; it is the documented behavioural response to every spending limit ever set. A threshold on a self-reported field is worse still.

### Option C — Model-assessed risk scoring

**What it is.** A model scores each request and routing follows the score.

**Consequences.**

| Better | Worse |
|---|---|
| Not directly manipulable by a single field | The pipeline makes a second silent judgement |
| Could catch impact the type code misses | The score has no accountable author |
| Adapts without matrix changes | Routing becomes unexplainable to the people it routes to |

**Why not chosen.** It reintroduces exactly what PDR-004 constrains, and it makes the routing matrix unreviewable, which defeats the reason PDR-002 chose a legible tool.

---

## Consequences

**Accepted costs.**

- Senior reviewers see requests that did not need them, on every type.
- The SLA is uniform within a type regardless of how urgent an individual request is.
- No mechanism exists to fast-track anything, including things that genuinely warrant it.

**What becomes harder.**

- Demonstrating proportionate governance to an auditor who expects to see thresholds.
- Reducing reviewer load without either reducing volume or adding reviewers.

**What this closes off.**

- Any routing input supplied as a free number by the requester.
- Priority as a concept the pipeline understands. Urgency is expressed only through the SLA attached to a type, set by the people who bear the consequences.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| One type is dominated by requests all reviewers agree are trivial | Reviewer feedback plus decision distribution by type | Reviewer roles |
| A verifiable, non-self-reported field becomes available that separates them | New system integration exposing it | Pipeline maintainer |
| An audit obligation requires threshold-based approval | Compliance requirement | Compliance Reviewer |

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Requesters would respond to thresholds by splitting requests | needs revisiting; the main objection weakens | high |
| Reviewers can triage their own queue effectively | still holds, but the waste is larger than assumed | medium |
| Type is a good enough proxy for the kind of scrutiny needed | needs revisiting; would suggest the type set is wrong | medium |

---

## Not decided here

- How reviewers prioritise within their own queue. Deliberately theirs.
- Whether the type set is correct. Adding or removing types is governed by `routing-matrix.md`.
- Whether SLA values are right. They were set per type and are not derived from anything.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
