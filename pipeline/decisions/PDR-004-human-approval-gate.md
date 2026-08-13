---
id: PDR-004
title: Human approval is a structural gate with no automated bypass
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []            # reference implementation - no project artifacts
  supersedes: null
---

# PDR-004 — Human approval is a structural gate with no automated bypass

---

## Context

The pipeline drafts documents that will be read as internal reference material in a payments business. Generation is cheap and getting cheaper; review is not, and does not get cheaper as models improve. Every increase in drafting capacity therefore widens the gap between what the pipeline produces and what anyone has time to check.

The pressure to add an automated path to publication is permanent, and it does not arrive as a proposal to remove review. It arrives as reasonable local optimisations: approve low-risk types automatically, approve when the agent reports no gaps, approve after the SLA expires so requests stop stalling. Each is defensible on its own and each removes the same control.

Doing nothing was not available, because the absence of an explicit decision is itself a decision: a pipeline with no stated position on this acquires bypasses incrementally, each justified by the queue depth on the day it was added.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Generation capacity outpaces review capacity | Automating approval for some subset |
| Documents acquire authority once published | Keeping every publication human-decided |
| Stalled queues are visible and uncomfortable | Timeouts, escalation, anything that clears them |
| Reviewer time is the scarce resource | Reserving it for what actually needs judgement |
| A gate that is only procedural gets bypassed under load | Making the constraint structural rather than a policy |

---

## Decision

No automated actor can write the `Published` status. The transition is reachable only from `In review`, and only through a human decision. There is no timeout that approves, no request type that skips review, and no confidence signal from the agent that shortens the path.

The constraint is enforced structurally rather than by policy. The drafting agent holds no credentials and cannot reach the registry or the documentation platform; every write is performed by n8n on its behalf. `REV 02` halts execution indefinitely awaiting a decision. `REV 03` has no fallback output, so an unrecognised decision value fails the execution rather than defaulting to any status.

---

## Options considered

### Option A — Structural gate with no bypass

**What it is.** Publication requires a human decision in all cases. The agent is architecturally incapable of writing the status, rather than instructed not to.

**Consequences.**

| Better | Worse |
|---|---|
| Throughput cannot outrun review capacity | Throughput does not improve as models improve |
| Bypasses cannot be added incrementally without an architecture change | Requests stall when reviewers are unavailable, with no workaround |
| The control holds even if the agent's instruction is compromised | Executions stay open for long periods, at operational cost in n8n |
| Capacity shortages stay visible as queue depth | The visible queue creates ongoing pressure to reverse this decision |

**Why chosen.** The failure this pipeline exists to prevent is not slow documentation, it is fluent documentation that nobody checked. Every alternative trades the second failure for the first while presenting itself as solving the first. A structural constraint also survives the conditions under which a procedural one fails: the load spike, the deadline, the new maintainer who does not know why the rule existed.

### Option B — SLA timeout with automatic approval

**What it is.** A request undecided after its SLA is published automatically, with the timeout recorded on the record.

**Consequences.**

| Better | Worse |
|---|---|
| Queues never stall; elapsed time becomes predictable | Publishes exactly the requests reviewers found hardest |
| Removes the pressure that a growing queue creates | Turns a capacity shortage into published output, hiding it |
| Simple to implement and to explain | Reviewer non-response becomes indistinguishable from approval |

**Why not chosen.** The subset that breaches SLA is not random. It is disproportionately the requests a reviewer opened, found difficult, and set aside. Approving that subset automatically is the worst available selection rule, and it is the one that feels most like operational hygiene.

### Option C — Automatic approval when the agent reports no gaps

**What it is.** Drafts with an empty `gapList` publish without review; drafts with gaps are routed to a reviewer.

**Consequences.**

| Better | Worse |
|---|---|
| Reviewer attention concentrates where the agent flagged uncertainty | Makes the gap list an incentive target for the agent |
| Plausible-sounding basis: the agent said it had everything | Rewards the drafting behaviour that reports fewest gaps |
| Reduces queue volume immediately | A confidently wrong draft is precisely the one with no gaps |

**Why not chosen.** It inverts the relationship the gap list is supposed to have with review. The agent's honesty about what it could not fill is the mechanism that makes drafts reviewable, and attaching a reward to a low gap count corrupts the one signal the reviewer relies on.

### Option D — Exempt low-risk request types

**What it is.** `RPT` requests, being reconciliation and reporting, publish on generation. Other types keep the gate.

**Consequences.**

| Better | Worse |
|---|---|
| Reviewer load falls on the type least likely to need judgement | The requester chooses the classification |
| Matches the intuition that not everything needs the same scrutiny | Creates immediate pressure to classify requests as `RPT` |
| Keeps the gate where risk is concentrated | Establishes that exemption is possible, making the next one easier |

**Why not chosen.** The exemption is defined on a field the requester controls. Any category that skips review becomes the category everything gets filed under, and the argument for the second exemption is always the argument that succeeded for the first.

---

## Consequences

**Accepted costs.**

- Throughput is bounded by reviewer attention and does not improve as generation improves.
- Requests stall visibly when reviewers are unavailable. This is intended: a stalled queue is a capacity problem stated honestly, and an auto-approving queue is the same problem hidden.
- n8n executions remain open for extended periods, with the operational cost that implies.
- The pipeline is a harder sell than one advertising end-to-end automation, because its main claim is about what it refuses to do.

**What becomes harder.**

- Scaling submission volume, which now requires a corresponding conversation about reviewer capacity rather than an infrastructure change.
- Demonstrating value through automation metrics, since the meaningful ones are about review quality and are all proxies (`metrics.md`).
- Any future integration expecting the pipeline to complete without human input.

**What this closes off.**

- Straight-through processing for any request type, permanently, not merely for now.
- Using the agent's own confidence as an input to any control decision anywhere in the flow.
- Deriving approval from inaction. Silence never means yes in this design, which forecloses every timeout-based mechanism including escalation-then-approve variants.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| A class of requests is identified whose documents carry no downstream authority at all | Post-publication citation or edit patterns showing a type nobody ever references | Pipeline maintainer |
| Review is found not to be happening in practice, making the gate ceremonial | Approval rate statistically independent of gap count (`metrics.md`) | Pipeline maintainer |
| Reviewer capacity conversation concludes that the queue is structurally unservable | SLA breach rate sustained near total across all types | Reviewer roles |

Throughput alone is not a trigger. A backlog is the expected operating condition of this design, not evidence against it.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Published documents acquire authority and get cited later | needs revisiting; the cost of an unreviewed page would be low | high |
| Reviewers read at least some of what they approve | still holds, but the gate becomes ceremonial and the real problem is elsewhere | medium |
| Review capacity can be increased if the shortage is made visible | still holds; the alternative to visibility is not automation but a worse decision made blind | medium |
| Model output cannot be trusted to self-assess completeness | still holds; self-assessment would remain the wrong basis for a control | high |

---

## Not decided here

- Whether reviewers are individually authenticated. Attribution is role-level; see PDR-010.
- Whether a `RISK` draft is promoted to current status. That is a separate manual step; see PDR-006.
- How reviewer capacity is allocated or prioritised. The pipeline records SLA and enforces nothing.
- Whether generation volume should be limited to match review capacity. Currently unlimited, and arguably should not be.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
