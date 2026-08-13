---
id: PDR-006
title: Fraud rule requests publish as draft regardless of approval
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Compliance Reviewer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-006 — Fraud rule requests publish as draft regardless of approval

---

## Context

A fraud rule threshold adjustment is documented before, or in parallel with, the threshold actually changing in the fraud system. The pipeline has no visibility into whether the change was implemented, tested, or agreed with the risk owner.

If an approved `RISK` document publishes as current documentation, it reads as settled policy. Someone will later cite it as the authority for a control, and the document does not carry the caveat into the conversation where it is used. The gap between "we approved this description" and "this control exists" is invisible to the reader.

`RISK` requests also tend to follow an incident, which is the condition under which people move fastest and check least.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Documentation is written before implementation | Marking the difference explicitly |
| Published pages get cited as authority | Withholding current status until it is true |
| These requests are urgent, following incidents | Publishing immediately |
| An extra manual step will sometimes be forgotten | Automation, or accepting the omission |

---

## Decision

`RISK` uses `draft-only` publication mode. On approval the page is created with draft status. Promotion to current status is a separate manual step outside this pipeline. Approving a `RISK` request authorises the record, not the activation of a threshold.

---

## Options considered

### Option A — Draft-only publication

**What it is.** The page exists, is attributable and is linked from the registry, but carries draft status until someone promotes it.

**Consequences.**

| Better | Worse |
|---|---|
| A described control is never mistaken for an implemented one | Accurate documentation can sit in draft, harder to find |
| The record exists immediately and is attributable | An extra manual step outside the pipeline's visibility |
| Approval and implementation are visibly different acts | Promotion will sometimes be forgotten |

**Why chosen.** Both failure modes are real and they are not symmetric. An implemented control that is under-documented is a gap someone can discover. A documented control that was never implemented is a false assurance that actively misleads.

### Option B — Publish as current with a pending banner

**What it is.** Normal publication, with a visible notice that implementation is outstanding.

**Consequences.**

| Better | Worse |
|---|---|
| The document is findable immediately | Banners are read once and then not read |
| No extra step to forget | The banner is not carried when the page is cited or excerpted |
| Simple to implement | Removing the banner is the same forgotten step, with worse consequences |

**Why not chosen.** It relies on a reader noticing a notice, and the failure is silent. Draft status is enforced by the platform rather than by attention.

### Option C — Implementation-confirmed status in the pipeline

**What it is.** The pipeline waits for confirmation that the threshold changed, then publishes as current.

**Consequences.**

| Better | Worse |
|---|---|
| The published state would be accurate by construction | Requires observing system state the pipeline has no access to |
| No manual promotion step | Confirmation would be self-reported, which is the problem restated |

**Why not chosen.** The pipeline cannot observe the fraud system. Accepting a human confirmation that implementation happened is a claim the pipeline cannot verify, and automating an unverifiable claim is worse than leaving the step manual and visible.

---

## Consequences

**Accepted costs.**

- `RISK` documents accumulate in draft when promotion is forgotten, reducing the value of the documentation for the type most likely to matter in an incident.
- Two states must be reconciled by a human: approved, and current.
- The type behaves differently from every other, which is a special case to explain.

**What becomes harder.**

- Searching for current fraud rule documentation, since drafts may or may not surface depending on platform behaviour.
- Treating publication as the end of the request lifecycle for this type.

**What this closes off.**

- Uniform publication behaviour across types. Once one type differs, the design accepts per-type publication modes as a concept.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| An implementation-confirmed signal becomes observable rather than reported | Fraud system exposes rule state via API | Pipeline maintainer |
| Drafts routinely go unpromoted | Count of `RISK` pages in draft beyond a threshold age | Compliance Reviewer |
| Another type turns out to have the same property | A second request for draft-only mode | Pipeline maintainer |

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Draft status is visually and functionally distinct in the documentation platform | is wrong; the mechanism provides nothing | high |
| A false assurance costs more than a hard-to-find document | still holds; this is the load-bearing judgement | medium |
| Promotion is someone's identifiable responsibility | needs revisiting; unowned steps do not happen | low |

The last assumption is the weakest in this record. Nothing in the pipeline assigns the promotion step to anyone.

---

## Not decided here

- Who promotes a draft, and on what signal. Explicitly outside the pipeline and currently unassigned.
- Whether other types should adopt draft-only mode.
- What happens to a draft whose underlying change is abandoned.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Compliance Reviewer |
