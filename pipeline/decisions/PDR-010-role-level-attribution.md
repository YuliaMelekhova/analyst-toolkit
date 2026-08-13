---
id: PDR-010
title: Reviewer attribution is role-level rather than individual
status: accepted
decided_on:              # not recorded - see decision-log.md
decided_by: Pipeline maintainer
author: Systems analyst
traces:
  affects: []
  supersedes: null
---

# PDR-010 — Reviewer attribution is role-level rather than individual

---

## Context

`REV 01` posts approve, reject and needs-input links into a role-scoped Slack channel. The links are n8n resume URLs: anyone who can open one can decide, and the decision carries no identity beyond the role the channel represents.

Individual attribution would require an authenticated approval surface, which is deployment-specific and would make this reference implementation less portable. The question is whether to build one, approximate one, or state the limitation.

**Forces at play.**

| Force | Pulls towards |
|---|---|
| Audit and non-repudiation need individual identity | Authenticated per-reviewer approval |
| A reference implementation should be portable | Mechanisms that work anywhere |
| An approximation that looks like attribution is worse than none | Stating the limitation plainly |
| Approval happens where the reviewer already is | Slack |

---

## Decision

Accept role-level attribution for this reference implementation, and state the limitation in the record rather than implying an accountability the mechanism does not provide. The registry records which role approved, and does not record a person.

---

## Options considered

### Option A — Role-level attribution, limitation stated

**What it is.** Resume links in a role channel; the registry records `Approved by role`.

**Consequences.**

| Better | Worse |
|---|---|
| Works in any Slack workspace with no extra infrastructure | The registry cannot say who decided |
| Approval happens where reviewers already are | Requester-approves-own-request is undetectable |
| The record does not overstate what it knows | Resume links are bearer capabilities; anyone with one can decide |

**Why chosen.** For a published reference the honest limitation is more useful than a mechanism that would not transfer. The record says exactly what it can support.

### Option B — Per-reviewer authenticated approval

**What it is.** An approval surface outside Slack requiring authentication, with the decision attributed to the authenticated user.

**Consequences.**

| Better | Worse |
|---|---|
| Individual attribution, suitable for audit | Deployment-specific; not portable as a reference |
| Self-approval becomes detectable | Reviewers leave the tool they are already in, reducing responsiveness |
| Non-repudiation becomes possible | Significant additional build |

**Why not chosen here, and required for any real deployment.** This is the correct mechanism wherever audit obligations exist. It is omitted from the reference on portability grounds only, and that trade does not survive contact with a compliance function.

### Option C — Record the Slack user who clicked

**What it is.** Replace resume URLs with an interactive endpoint that captures the acting user ID.

**Consequences.**

| Better | Worse |
|---|---|
| A name appears in the record | Records who clicked, not who was authorised to decide |
| Modest additional work | Looks like individual accountability while providing less |
| Detects the requester-approves case | Slack identity is not an approval credential |

**Why not chosen.** It is the most dangerous of the three, because it produces a record an auditor would read as individual attribution. A weaker guarantee that looks stronger is worse than an acknowledged gap.

---

## Consequences

**Accepted costs.**

- The registry shows that a Compliance Reviewer approved, not which one.
- The pipeline cannot detect that the approver is also the requester; that constraint is social, not technical.
- Anyone in the reviewer channel, and anyone they forward a link to, can decide. Exposure is bounded by channel membership and nothing else.

**What becomes harder.**

- Any audit question of the form "who approved this".
- Measuring review behaviour per reviewer, which is partly a benefit (`metrics.md`) and partly a genuine loss.
- Distributing review load, since no mechanism knows who took what.

**What this closes off.**

- Non-repudiation. Nothing in this design supports a claim that a specific person decided anything.

---

## What would change this decision

| Trigger to revisit | How we would notice | Who watches for it |
|---|---|---|
| Any audit obligation attaching to approvals | Compliance requirement | Compliance Reviewer |
| A self-approval occurs | Only by someone noticing; nothing detects it | Reviewer roles |
| Reviewer channels grow beyond a small, known membership | Channel membership count | Pipeline maintainer |

This decision is expected to be reversed in any real deployment. It is stated as accepted because it is what the reference implements, not because it is adequate.

---

## Assumptions this rests on

| Assumption | If false, the decision | Confidence |
|---|---|---|
| Reviewer channels are private with small, known membership | is wrong; exposure becomes unbounded | medium |
| No audit obligation currently attaches to approvals | is wrong; Option B becomes mandatory | low |
| Reviewers will not approve their own requests | still holds socially; nothing enforces it | medium |

---

## Not decided here

- What the authenticated approval surface would be. Deployment-specific by design.
- Whether secondary review on `RAIL` requires two distinct individuals. Currently unenforceable, and the routing matrix states the intent only.

---

## Change log

| Date | Status change | Reason | By |
|---|---|---|---|
| | → accepted | Initial decision for the reference implementation | Pipeline maintainer |
