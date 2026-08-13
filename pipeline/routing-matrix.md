# Routing Matrix

Which request type goes to which reviewer, on which template, under which publication mode.

## Authority

The routing table lives in two places: the `ROUTING` constant inside `ORC 04 Resolve routing rule` in `workflow/n8n-workflow.sanitized.json`, and this document. The workflow is what executes. This document is what gets reviewed, argued about and cited in a decision.

Duplication is deliberate and it is a liability. A matrix that exists only in code is not reviewable by the compliance officer who has an opinion about it; a matrix that exists only in a document is not the thing that runs. Both are kept, and any divergence between them is a defect in both, not a documentation lag. When the table changes, the two edits are one change and belong in one commit.

Context is synthetic throughout: a mid-size payment service provider, fictional. Reviewer roles, thresholds and SLA values are constructed to exercise the routing logic.

## The matrix

| Code | Request type | Template | Reviewer role | Secondary review | Publication mode | Review SLA |
|---|---|---|---|---|---|---|
| `FEE` | Fee schedule change | `brd-fee-change` | Finance Approver | none | standard | 48h |
| `RAIL` | Payout rail or corridor enablement | `brd-rail-enablement` | Compliance Reviewer | Backend Reviewer | standard | 120h |
| `RISK` | Fraud rule threshold adjustment | `brd-risk-threshold` | Compliance Reviewer | none | draft-only | 24h |
| `RPT` | Reconciliation or reporting request | `brd-reporting` | Finance Approver | none | standard | 72h |
| `MEX` | Merchant onboarding exception | `brd-merchant-exception` | Compliance Reviewer | none | standard | 48h |
| `INT` | Merchant integration or API change | `brd-integration` | Backend Reviewer | none | standard | 72h |

Destination channels and parent pages are placeholders in the export and are documented in `workflow/env.example` rather than repeated here, since they are deployment configuration rather than routing policy.

## Publication modes

**standard** - on approval, the page is created with status `current` and is immediately visible in the documentation space.

**draft-only** - on approval, the page is created with status `draft`. The record exists and is attributable, but it is not presented as settled documentation.

Only `RISK` uses draft-only, for a reason worth stating plainly: approving a fraud threshold change as a *document* is not the same act as changing the threshold. If publication produced a page reading like current policy, the page would start being cited as authority for a control that has not been implemented, tested or agreed with the risk owner. Draft status keeps the document honest about its own status. A separate, deliberately manual step promotes it once the change is actually live.

## Why each assignment

Routing decisions are defensible or they are arbitrary, so each one is stated.

**FEE to Finance Approver.** A fee change has direct revenue effect and interacts with existing merchant contracts. The failure mode is not incorrect prose, it is a documented fee that contradicts a signed agreement. Finance is the only role that holds both sides of that. Short SLA because fee questions block commercial conversations already in progress.

**RAIL to Compliance Reviewer, with Backend Reviewer secondary.** Enabling a payout rail or a new corridor is the heaviest request in the set: it carries licensing and sanctions-screening implications on one side and settlement, reconciliation and failure-handling implications on the other. Neither reviewer alone can approve it competently. The long SLA is honest rather than aspirational; treating this as a two-day decision would produce fast approvals of things nobody had time to check.

**RISK to Compliance Reviewer, draft-only.** A fraud threshold trades false positives against losses, and the person who feels the pain of one rarely feels the pain of the other. Compliance holds the mandate. The short SLA exists because these requests usually follow an incident, and the draft-only mode exists because urgency is exactly the condition under which a document gets mistaken for an implemented control.

**RPT to Finance Approver.** Reconciliation and reporting requests are the low-risk case and the reference for what the fast path looks like. They still go through review. A pipeline where some types skip review would immediately grow pressure to classify things into the skipping category, and the classification is made by the requester.

**MEX to Compliance Reviewer.** An onboarding exception is a documented decision to accept a merchant who did not clear standard KYC criteria. The document is the audit artifact and may be read years later by someone with subpoena power. It routes to compliance because the reasoning, not the outcome, is what has to survive.

**INT to Backend Reviewer.** Integration and API changes are judged on technical accuracy: endpoints, payload shapes, versioning, backward compatibility. This is the one type where the reviewer is checking whether the document describes a system that can exist.

## What routing deliberately does not consider

**Amount.** No route depends on a monetary threshold. Thresholds invite splitting: a request that would cross a limit gets submitted as two requests that do not. Type-based routing has no such gradient, and a fee change is a fee change whether it moves one basis point or fifty.

**Requester seniority.** A request from a director routes identically to one from a support agent. Seniority-sensitive routing turns the pipeline into an escalation ladder and the reviewer into a rubber stamp for anyone senior enough.

**Urgency as declared by the requester.** There is no priority field, because a self-declared priority field is a field that is always set to high. Urgency is expressed by SLA per type, which is set by the people who bear the consequences rather than by the person asking.

## Secondary review

Only `RAIL` has one. Secondary review means both reviewers must approve; there is no majority and no override. The order is not enforced.

The temptation is to apply secondary review widely, on the theory that two reviewers are better than one. The observed effect is the opposite: when two people are responsible, each assumes the other is reading carefully, and both skim. Secondary review is reserved for the case where the two reviewers are checking genuinely different things and neither could check the other's part.

## Escalation

**SLA breach.** The SLA is recorded on the record and is not enforced by the workflow. Nothing auto-approves, nothing auto-escalates, and nothing reassigns. A breached SLA is a signal about reviewer capacity, and the correct response to it is a capacity conversation rather than an automated shortcut. See `not-automated.md`.

**Reviewer unavailable.** Reassignment is manual and is recorded by changing the reviewer role on the registry record before the decision. Reassigning after a decision is not possible and not desirable; the record must show who actually decided.

**Requester is the reviewer.** A request must not be approved by the person who submitted it. The workflow does not detect this, because reviewer identity is a role and the resume links are posted to a role channel. This is a known limitation, stated in `workflow/env.example` and in `not-automated.md`, and it is currently a social control rather than a technical one. Any deployment needing non-repudiation must replace the resume-link mechanism with per-reviewer authentication.

**Cross-cutting requests.** A request that genuinely spans two types is not routed to both. It is returned as `Needs input` with an instruction to split it. Combined requests produce a document that neither reviewer owns and that gets approved on the strength of the half each reviewer understood.

**Misclassification.** If the reviewer determines the type is wrong, the decision is `Needs input`, the type is corrected on the registry record, and the request re-enters at Orchestration. The original identifier is retained; the identifier tracks the request, not its classification. This means a `BR-FEE-2026-014` may end up documented as a `RPT` request, which looks untidy and is preferable to renumbering something people have already linked to.

## Unmatched types

`ORC 04` has no default branch. A type code outside the six raises an error and halts the execution rather than routing to a general queue.

This is intentional. A default route is where unclassifiable requests accumulate, and a general queue has no owner, so it grows. Failing loudly puts the cost on the pipeline maintainer, who can act on it, rather than on a reviewer who will not notice a queue they were never told about.

## Changing the matrix

Adding a request type is not a configuration change; it is a decision about who is accountable for a class of documents. It requires, in order:

1. A named reviewer role that has accepted the work, not a role that seems appropriate.
2. A template in `framework/`, since routing to a template that does not exist produces drafts against a generic structure and quietly degrades every request of that type.
3. A knowledge pack the drafting agent can load, or an explicit decision that this type drafts from the request body alone.
4. An SLA agreed with the reviewer rather than assigned to them.
5. A publication mode, with draft-only as the default for anything describing a control that must be separately implemented.
6. A decision-log entry in `decision-log.md`.
7. The same change applied to `ORC 04` and to this table, in one commit.

Removing a type requires deciding what happens to open requests of that type. There is no migration path in the workflow, so removal is done by draining first and deleting after.
