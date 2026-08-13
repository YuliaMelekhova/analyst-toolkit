---
id: PDR-003
title: Requests are submitted through a structured modal rather than parsed from messages
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-003 — Requests are submitted through a structured modal rather than parsed from messages

---

## Context

Business requests already arrive in Slack as prose, addressed to a person rather than a system. The pipeline could consume those messages as they are, which asks nothing of requesters and would capture requests that would otherwise never be submitted at all.

The cost is that everything downstream then depends on interpretation. Classification, validation and drafting all have to infer structure that was never stated, and each inference is a place the pipeline can be wrong without anyone noticing.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Requests originate as conversation | Consuming messages as they are |
| Adoption depends on low friction | No form to fill |
| Every inference is a silent failure mode | Explicit fields |
| Validation must name what is missing | Named fields to be missing |

---

## Decision

Submission goes through a Slack modal with typed fields. Free prose is confined to a single `body` field. Classification is a lookup against a requester-selected type code, not an inference.

---

## Options considered

### Option A — Structured modal

**What it is.** A form in Slack capturing type, title, requesting team, business justification, target date and a free-text body.

**Consequences.**

| Better | Worse |
|---|---|
| Classification is a lookup that cannot be silently wrong | Requesters do more work before anything happens |
| Validation can name the exact missing field | Some requests never get submitted at all |
| The agent receives labelled input rather than a paragraph to interpret | The form itself becomes something to maintain and get wrong |

**Why chosen.** Abandonment at the form is visible and diagnosable; misclassification after the fact is neither. The failure mode this option produces is the one that can be fixed.

### Option B — Model-based parsing of free-form messages

**What it is.** The pipeline reads messages in a channel and infers type and fields.

**Consequences.**

| Better | Worse |
|---|---|
| Zero friction; requests are captured as they are made | Misclassification is silent and invisible to the requester |
| Higher submission volume | Introduces a second judgement the pipeline makes unaided |
| No form to design or maintain | Validation cannot name a missing field, only guess at intent |

**Why not chosen.** It adds a second place where the pipeline decides something on its own, which is the pattern PDR-004 exists to constrain. The requester never sees what it decided, so the error surfaces only at review, several stages later.

### Option C — Web form outside Slack

**What it is.** A dedicated intake form on an internal site.

**Consequences.**

| Better | Worse |
|---|---|
| Richer field types and validation | Requires leaving the conversation where the request arose |
| Not constrained by modal limits | Adoption falls sharply; the link gets forgotten |

**Why not chosen.** The request originates in a conversation, and every step away from that conversation costs submissions.

---

## Consequences

**Accepted costs.**

- Some requests are never submitted because the form is more effort than a message. These are invisible, which makes this the cost hardest to argue about.
- The form is now a design surface with its own failure modes; a badly worded field produces bad input at scale.
- Adding a request type means changing the form, not only the routing table.

**What becomes harder.**

- Capturing requests that arrive by other routes, such as a direct message or a meeting. Someone must retype them.
- Changing the field set, since existing records were captured under the old one.

**What this closes off.**

- Retrospective ingestion of historical requests, which would require inferring the structure this decision refuses to infer going forward.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| Submission volume suggests the form is the barrier | Volume against known request activity in channels | Pipeline maintainer |
| Abandonment concentrates on specific fields | Validation failure breakdown by field (`metrics.md`) | Pipeline maintainer |
| A field turns out never to be used downstream | Field appears in no template and no reviewer conversation | Pipeline maintainer |

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Requesters can classify their own request correctly | still holds; reclassification handles the exceptions | medium |
| The friction cost is smaller than the misclassification cost | needs revisiting if abandonment turns out to be high | medium |
| Requests mostly originate in Slack | needs revisiting; a different channel would change the design | high |

---

## Not decided here

- The exact field set, which is expected to change as intake data accumulates.
- Whether requesters can edit a submitted request, or must resubmit.
- How requests arriving outside Slack enter the pipeline.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
