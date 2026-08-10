---
id: ADR-000
title: <The decision, stated as a choice made — not as a topic>
status: proposed         # proposed | accepted | superseded | reversed
decided_on: YYYY-MM-DD
decided_by: <role accountable — one role, not a committee>
author: <analyst>
traces:
  affects: []            # BR-xxx, US-xxx, NFR-xxx
  supersedes: null       # ADR-xxx
---

# ADR-000 — <Title>

> An Analysis Decision Record captures a choice made during analysis, not
> during implementation: how a process is bounded, which system owns a concept,
> what a term means, which of two workable specifications is written. It exists
> so that in six months the reasoning is recoverable and the decision can be
> reopened deliberately rather than by accident.
>
> Write one when there was more than one viable option. A decision with only
> one option is a constraint — record it in the BRD instead.
>
> Delete the guidance blockquotes before publishing.

---

## Context

> The situation that forces a choice. Written so that a reader arriving with no
> history understands why doing nothing was not available. State the facts and
> the pressures, not the preferred answer — if the conclusion is obvious from
> this section alone, the alternatives were probably not taken seriously.

<!-- 4–8 sentences. -->

**Forces at play.**

| Force | Pulls towards |
|---|---|
| | |

---

## Decision

> One paragraph, present tense, active voice. "We record the billing ledger as
> the single source of truth for subscription state." Not "it was decided that
> the ledger might be considered authoritative."

---

## Options considered

> Each option gets the same treatment, including the one chosen. An option
> described in one line while the chosen one gets a page was not really
> considered — and a reader can tell.

### Option A — <name>

**What it is.**

**Consequences.**

| Better | Worse |
|---|---|
| | |

**Why not chosen** *(or: why chosen)*.

### Option B — <name>

**What it is.**

**Consequences.**

| Better | Worse |
|---|---|
| | |

**Why not chosen** *(or: why chosen)*.

---

## Consequences

> What is now true because of this choice, including the parts that are worse.
> An ADR listing only benefits is advocacy, not a record.

**Accepted costs.**

- 

**What becomes harder.**

- 

**What this closes off.**

> Decisions foreclose future options. Naming which ones is the difference
> between a considered decision and a lucky one.

- 

---

## What would change this decision

> The most useful section, and the one most often missing. State the specific
> observation, threshold, or event that would make revisiting this correct.
> Without it, the decision either persists past its usefulness or gets reversed
> on someone's intuition.

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| | | |

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| | still holds / needs revisiting / is wrong | high / medium / low |

---

## Not decided here

> Adjacent questions a reader might assume this settles, and does not.

- 

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| YYYY-MM-DD | → proposed | | |
