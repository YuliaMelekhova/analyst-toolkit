---
id: PDR-001
title: The request registry is the state of record, separate from the documentation platform
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-001 — The request registry is the state of record, separate from the documentation platform

---

## Context

A request has a state: submitted, drafted, awaiting review, published, rejected. Three systems are already in the flow and each could hold it. The documentation platform is the obvious candidate, since the output ends up there and it is where people look. Slack is where the request originates and where the conversation continues.

Doing nothing was not available because the pipeline branches on state at three points, and something has to be authoritative when the systems disagree. They will disagree: a page can be edited, a Slack thread can be deleted, and an execution can fail between two writes.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Output lives in the documentation platform | Storing state as a page property |
| The most interesting states have no page yet | A store that exists before publication |
| Rejected and abandoned requests are the useful data | A store that survives non-publication |
| Every extra system is an integration to maintain | Reusing something already in the flow |

---

## Decision

State lives in a dedicated registry. The documentation platform holds published output only; Slack holds conversation only. Neither is authoritative about where a request stands, and neither is consulted to determine it.

---

## Options considered

### Option A — Dedicated registry

**What it is.** A separate database holding one record per request, with lifecycle status, type, reviewer role, timestamps and a pointer to the published page.

**Consequences.**

| Better | Worse |
|---|---|
| Requests that are rejected or abandoned still have a record | An extra system to operate and integrate |
| Throughput, rejection rate and latency are queryable from one table | Every state transition costs an API call |
| State exists before any page does | A third place for state to drift from reality |

**Why chosen.** The states worth measuring are the ones that never produce a page. Any design that creates the record at publication time is blind to exactly the population that would tell you the pipeline is not working.

### Option B — Page properties in the documentation platform

**What it is.** Status stored on the published page, created at the point of drafting rather than approval.

**Consequences.**

| Better | Worse |
|---|---|
| One fewer system | Requires creating pages for requests that may never be approved |
| State sits next to the artifact it describes | Draft pages accumulate in the space, visible and confusing |
| No write-back step needed | Rejected requests either leave orphan pages or vanish entirely |

**Why not chosen.** It forces a choice between polluting the documentation space with unapproved material and losing the record of everything that was not approved. Both are worse than an extra integration.

### Option C — Slack thread as the record

**What it is.** Status tracked by reactions or thread replies on the original message.

**Consequences.**

| Better | Worse |
|---|---|
| Zero additional systems | Messages are edited and deleted; the record is not stable |
| State is visible where the conversation happens | Not queryable in any useful aggregate form |
| No integration work | Retention policies eventually delete the history |

**Why not chosen.** A state of record that can be edited by anyone in the channel and deleted by policy is not a record.

---

## Consequences

**Accepted costs.**

- An additional system in the critical path, with its own credentials, rate limits and outage modes.
- Six registry write nodes in the workflow, each a failure point.
- State can drift between the registry and the documentation platform when a write-back fails.

**What becomes harder.**

- Reading current status from within the documentation platform, which now requires following a link.
- Any bulk operation, since state changes must go through the registry rather than being applied to pages.

**What this closes off.**

- Treating the published page as the artifact of record. It is output, not state.
- Deriving pipeline metrics from page histories, which would have been possible under Option B and is now unnecessary.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| The documentation platform gains a first-class request object with lifecycle states | Platform release notes | Pipeline maintainer |
| Registry maintenance cost exceeds the reporting value it provides | Time spent on registry incidents against use of the reporting | Pipeline maintainer |
| Drift between registry and platform becomes routine rather than exceptional | Write-back failure rate | Pipeline maintainer |

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Rejected and abandoned requests are worth recording | needs revisiting; Option B becomes cheaper and adequate | high |
| Aggregate reporting will actually be done | still holds, but the main benefit goes unrealised | medium |
| The registry is more available than the pipeline needs it to be | still holds; an outage stalls requests rather than losing them | medium |

---

## Not decided here

- Which registry product. The design assumes a queryable store with typed properties and nothing more.
- Retention. Nothing here says how long records are kept or whether they are archived.
- Whether the registry is exposed to requesters directly, or only through Slack notifications.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
