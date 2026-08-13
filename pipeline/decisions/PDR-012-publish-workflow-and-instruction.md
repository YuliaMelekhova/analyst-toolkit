---
id: PDR-012
title: The workflow and the agent instruction are published in full
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-012 — The workflow and the agent instruction are published in full

---

## Context

This directory publishes the flow structure, the routing logic and the complete agent instruction including its negative constraints. A reader who wanted a draft that evades those constraints now knows exactly what they are and how the untrusted input block is delimited.

The alternative framings are available: publish the architecture and withhold the instruction, or publish nothing and describe the pattern in prose.

The question underneath is what the pipeline's controls actually are. If any of them depend on an attacker not knowing the prompt, publication weakens the system. If none do, publication costs nothing and the fact that it costs nothing is itself worth demonstrating.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| The design is only reusable if it is inspectable | Full publication |
| Disclosure helps anyone trying to work around the constraints | Withholding the instruction |
| Controls that depend on secrecy are weak controls | Testing that assumption by publishing |
| Sanitisation removes credentials but not design knowledge | Being explicit about what remains disclosed |

---

## Decision

Publish in full, after sanitisation for credentials, identifiers and business context per `workflow/SANITIZATION.md`. The agent instruction is reproduced in `agent-contract.md` including its negative constraints and its known weaknesses.

---

## Options considered

### Option A — Publish everything, sanitised

**What it is.** Architecture, routing, workflow export and full agent instruction, with credentials, identifiers and business context removed.

**Consequences.**

| Better | Worse |
|---|---|
| The design is reusable rather than merely described | Anyone can see how the constraints are phrased |
| Demonstrates that no control depends on prompt secrecy | Node parameter shapes reveal API surfaces and versions in use |
| The known weaknesses are stated where a reader can evaluate them | The delimiter convention is disclosed |

**Why chosen.** The controls are that the agent holds no credentials and cannot write, and that a human decides. Both hold with the instruction fully disclosed. Nothing is weakened by publication, which is the property worth demonstrating.

### Option B — Publish the architecture, withhold the instruction

**What it is.** Full design record, with the agent instruction described rather than reproduced.

**Consequences.**

| Better | Worse |
|---|---|
| Less material for anyone probing the constraints | Implies the instruction is a control, which is misleading |
| Still communicates the pattern | The most reusable artifact is the one withheld |
| | A reader cannot evaluate the untrusted input handling they are told about |

**Why not chosen.** Withholding it would signal that prompt confidentiality is load-bearing here. It is not, and implying otherwise misdirects a reader about where the safety sits.

### Option C — Describe the pattern without artifacts

**What it is.** Prose only, no export, no instruction, no routing table.

**Consequences.**

| Better | Worse |
|---|---|
| No disclosure at all | Nothing reusable |
| No sanitisation risk | Claims about the design cannot be checked |

**Why not chosen.** An unverifiable description of a careful design is indistinguishable from an unverifiable description of a careless one.

---

## Consequences

**Accepted costs.**

- The workflow structure, error handling and integration order are disclosed. This would be unacceptable for a system with a security-by-obscurity assumption, and is acceptable here because there is none.
- Node parameter shapes narrow the API version window an outsider would probe against.
- The negative constraints and the delimiter convention are known to anyone wanting to work around them.

**What becomes harder.**

- Adding any future control that would depend on non-disclosure. Such a control would now need a different mechanism or a different publication decision.
- Changing the sanitisation posture retroactively, since published material stays published.

**What this closes off.**

- Treating the prompt as a security boundary anywhere in this design, permanently.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| A control is added that depends on non-disclosure | Design review of any new control | Pipeline maintainer |
| Disclosed material turns out to enable an attack the human gate does not stop | An incident, or a reported weakness | Pipeline maintainer |

If the first trigger fires, the control is the thing to reconsider before the publication decision is.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| No control depends on the instruction being secret | is wrong; publication would need reversing, which is impossible | high |
| The human gate stops what prompt hygiene misses | still holds, but its limits become more important | medium |
| Sanitisation removed everything identifying | needs verifying against a real export, not a generated one | medium |

The last assumption is not yet tested. The sanitised export in this directory was produced by a builder script rather than exported from a live instance, so the checklist has not been run against the case it exists for.

---

## Not decided here

- Whether a real deployment of this pipeline should publish its own workflow. That depends on its controls, not on this one's.
- What licence applies to the published material. Governed by the repository root.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
