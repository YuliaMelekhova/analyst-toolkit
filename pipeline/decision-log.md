# Decision Log

Architecture decision records for the pipeline. Each records what was decided, what it costs, what was rejected, and what would make it worth revisiting.

> Pending check: `framework/adr-template.md` is the authority for ADR structure in this repository. The records below use Context / Decision / Consequences / Rejected alternatives / Revisit when. If the template defines different or additional fields, that definition wins and these records are amended to match.

Dates are omitted. This is a reference implementation with no real timeline, and stamping invented dates on ADRs would make the sequence look like history. Numbering reflects dependency, not chronology.

---

## ADR-001 Registry is the state of record, separate from the documentation platform

**Status:** Accepted

**Context.** A request has a state: submitted, drafted, awaiting review, published, rejected. That state could live in the documentation platform as a page property, in Slack as thread status, or in a separate registry. The documentation platform is the obvious candidate, since the output ends up there anyway.

**Decision.** State lives in a Notion registry. Confluence holds published output only. Slack holds conversation only. Neither is authoritative about where a request stands.

**Consequences.** An extra system to operate, an extra integration to maintain, and an extra place for state to drift out of sync with reality. Every stage transition costs an API call. In exchange, a request that is rejected or abandoned still has a record, which it would not have if state lived on a page that is only created on approval. Reporting on throughput, rejection rate and review latency becomes possible against one table rather than by scraping page histories.

**Rejected alternatives.** Page properties in the documentation platform: fails because unpublished requests have no page, so the most interesting states are invisible. Slack thread status: fails because Slack is not queryable as a record and messages are edited and deleted.

**Revisit when.** The documentation platform gains a first-class request object with lifecycle states, or registry maintenance cost exceeds the reporting value it provides.

---

## ADR-002 n8n as orchestrator rather than application code

**Status:** Accepted

**Context.** The pipeline is six integrations and a branch. It could be a small service in any language, deployed and monitored like other services.

**Decision.** n8n, with the workflow as the deployable artifact.

**Consequences.** The flow is legible to non-engineers, which matters because the people who argue about routing are compliance and finance, not backend. Changing a reviewer channel does not require a deploy. The costs are real: workflow JSON diffs poorly in review, testing is largely manual, there is no type checking across nodes, and business logic drifts into Code nodes where it is neither typed nor unit tested. The `ORC 04` routing table is exactly this drift, accepted knowingly and mitigated by ADR-008.

Positioning matters more than the tool. n8n is the right answer while the pipeline is integration glue with one decision point. It becomes the wrong answer once the logic between nodes outgrows what a reader can follow on a canvas, and the failure mode is gradual: nobody notices the day it stops being legible.

**Rejected alternatives.** A custom service: better testing and versioning, worse legibility to the stakeholders who need to challenge routing. A managed iPaaS: comparable, with less control over where request data is processed, which matters in a payments context.

**Revisit when.** Code node logic exceeds roughly a screen per node, or a routing change requires an engineer to explain the diff.

---

## ADR-003 Structured intake modal rather than free-form messages

**Status:** Accepted

**Context.** Requests already arrive in Slack as prose. The pipeline could parse those messages directly, which requires nothing from requesters.

**Decision.** Submission goes through a Slack modal with typed fields. Free prose is confined to one `body` field.

**Consequences.** Requesters must do more work up front, and some will not, which shows up as requests that never get submitted rather than as bad requests. Against that, classification becomes a lookup rather than an inference, validation can name a missing field precisely, and the drafting agent receives labelled input rather than a paragraph it has to interpret. Classifying by model would have introduced a second place where the pipeline can be wrong without telling anyone.

**Rejected alternatives.** Parse free-form messages with the model: silent misclassification, and the requester has no way to see what it decided. A web form outside Slack: better fields, worse adoption, since the request originates in a conversation.

**Revisit when.** Submission volume drops in a way that suggests the form is the barrier, or field abandonment concentrates on specific fields that could be removed.

---

## ADR-004 Human approval is a structural gate with no automated bypass

**Status:** Accepted

**Context.** The pipeline drafts documents that will be read as internal reference material in a payments business. Generation is cheap and getting cheaper; reviewing is not. The pressure to add an automated path to publication is permanent and takes reasonable-sounding forms: auto-approve low-risk types, auto-approve when the agent reports no gaps, auto-approve after the SLA expires so requests do not stall.

**Decision.** No automated actor can write the `Published` status. The transition is reachable only from `In review` and only via a human decision. There is no timeout that approves, no request type that skips review, and no confidence signal from the agent that shortens the path.

The enforcement is structural rather than procedural. The agent holds no credentials and cannot reach the registry or the documentation platform; every write is performed by n8n on its behalf. `REV 02` halts execution indefinitely waiting for a decision, and `REV 03` has no fallback output, so an unrecognised decision value fails the execution rather than defaulting.

**Consequences.** Throughput is bounded by reviewer attention and does not improve as generation improves. Requests stall when reviewers are unavailable, visibly and without a workaround, which is the intended behaviour: a stalled queue is a capacity problem stated honestly, and an auto-approving queue is the same problem hidden. Executions can remain open for a long time, which has operational cost in n8n.

The gate compels a click, not a reading. It cannot prevent rubber-stamping, and claiming otherwise would be the more dangerous error. `metrics.md` proposes instrumentation intended to make rubber-stamping visible; visibility is the only available control.

**Rejected alternatives.** SLA timeout with auto-approval: the requests that breach SLA are disproportionately the ones reviewers found hard, so a timeout approves exactly the wrong subset. Auto-approve on empty gap list: makes the gap list an incentive target for the agent, and the agent that reports fewest gaps is not the one drafting best. Auto-approve `RPT` as low-risk: pressure would immediately move to classifying things as `RPT`, and the requester chooses the classification.

**Revisit when.** Never on throughput grounds alone. Legitimately revisitable only if a class of requests is identified whose documents carry no downstream authority at all, and that class turns out to be smaller than it looks.

---

## ADR-005 Routing on request type only, without monetary thresholds

**Status:** Accepted

**Context.** Routing by amount is the conventional design: larger financial impact gets more senior review. The type set already has financial impact attached, so thresholds would be straightforward to add.

**Decision.** Routing depends on type alone. No route consults an amount, a requester's seniority, or a self-declared priority.

**Consequences.** A trivial fee change gets the same reviewer as a significant one, which will occasionally feel wasteful and will occasionally be wasteful. The reviewer can triage inside their own queue, which is where the judgement belongs.

Thresholds create a gradient, and a gradient creates an incentive to sit just below it, so a request that would cross a limit gets submitted as two that do not. This is not hypothetical behaviour; it is what approval thresholds reliably produce. Seniority-based routing turns the pipeline into an escalation ladder. A self-declared priority field ends up permanently set to high.

**Rejected alternatives.** Amount bands per type: splitting incentive, plus the amount is self-reported and unverified at intake. Model-assessed risk scoring: introduces a second judgement the pipeline makes silently, which is the thing ADR-004 exists to prevent.

**Revisit when.** Reviewer load data shows a specific type dominated by requests all reviewers agree are trivial, and there is a verifiable, non-self-reported field that separates them.

---

## ADR-006 RISK requests publish as draft regardless of approval

**Status:** Accepted

**Context.** A fraud threshold adjustment is documented before, or in parallel with, the threshold actually changing. If the approved document is published as current documentation, it reads as settled policy for a control that may not be implemented, tested or agreed with the risk owner.

**Decision.** `RISK` uses `draft-only` publication mode. The page is created with draft status. Promotion to current status is a separate, deliberately manual step outside this pipeline.

**Consequences.** An extra manual step that will sometimes be forgotten, leaving accurate documentation in draft where it is harder to find. This is the better failure: an implemented control that is under-documented is a gap, while a documented control that was never implemented is a false assurance that someone will cite.

Approval and implementation are separated in what they authorise. Approving a `RISK` request authorises the record, not the activation of a threshold.

**Rejected alternatives.** Publishing as current with an "implementation pending" banner: banners are read once and then not read. A separate implementation-confirmed status in the pipeline: would require the pipeline to know about system state it has no access to.

**Revisit when.** An implementation confirmation signal becomes available that the pipeline can observe rather than be told about.

---

## ADR-007 Malformed agent output is discarded, not repaired

**Status:** Accepted

**Context.** Models occasionally return JSON wrapped in prose, truncated, or missing a field. A tolerant parser recovers most of these cases.

**Decision.** `DFT 04` parses strictly. Invalid JSON, a missing or non-array `gapList`, or an empty `body` fails the execution and discards the draft.

**Consequences.** Some executions fail that could have been salvaged, and the requester sees a failure rather than a draft. Failures are visible and traceable to a contract violation.

A salvage path is the least-tested code in the pipeline and produces a document nobody specified. Repairing a response that omitted `gapList` is worst of all, since it manufactures the appearance of a complete gap analysis that never happened.

**Rejected alternatives.** Tolerant parsing with a repair pass: hides contract violations, so instruction regressions go unnoticed. Automatic retry on parse failure: reasonable and deliberately omitted here, because a retry loop against a paid API with no cap is a cost incident waiting to happen. A capped retry is a sensible addition for a real deployment.

**Revisit when.** Parse failure rate is high enough to be an operational problem, in which case the instruction is the thing to fix.

---

## ADR-008 Routing table duplicated in code and in documentation

**Status:** Accepted, with known cost

**Context.** The routing table exists in `ORC 04` and in `routing-matrix.md`. Duplication of a decision table is normally a defect.

**Decision.** Keep both. Any divergence is a defect in both, not a documentation lag. Table changes are one commit touching both files.

**Consequences.** They will drift, because manual synchronisation always does. The alternatives were worse. A table only in the workflow is not reviewable by the compliance officer who has an opinion about which requests reach them, and reviewability by that person is the point of having a matrix. A table only in a document is not what executes.

Generating one from the other was considered and rejected as disproportionate: it adds a build step and a generator to maintain for six rows. This is a defensible call at this size and stops being defensible somewhere around twenty rows or two tables.

**Rejected alternatives.** Generate the document from the workflow JSON: build tooling for six rows. Load the table from external configuration at runtime: moves the reviewable artifact somewhere with no review process attached.

**Revisit when.** The table exceeds roughly twenty rows, a second such table appears, or a drift incident occurs.

---

## ADR-009 No default route for unrecognised request types

**Status:** Accepted

**Context.** `ORC 04` looks up a routing rule by type code. An unknown code needs a behaviour: a general queue, a fallback reviewer, or a failure.

**Decision.** No default branch. An unknown type throws and halts the execution.

**Consequences.** A validation gap or a new type code added in one place and not the other produces a hard failure rather than a degraded path. Someone must notice and fix it, which is the intent.

A general queue has no owner, and unowned queues grow. Failing loudly puts the cost on the pipeline maintainer, who can act on it, rather than on a reviewer who was never told the queue existed.

**Rejected alternatives.** Route unknown types to a general channel: accumulates. Route to the strictest reviewer: penalises compliance for a classification defect and trains everyone that unknown types are compliance's problem.

**Revisit when.** Types are added frequently enough that a staging route for a type awaiting a routing decision becomes worthwhile.

---

## ADR-010 Reviewer identity is a role, not an individual

**Status:** Accepted, with known limitation

**Context.** `REV 01` posts approve, reject and needs-input links into a role-scoped channel. Anyone in that channel can act on them, and the decision is recorded against the role.

**Decision.** Accept role-level attribution for this reference implementation, and state the limitation rather than implying individual accountability the mechanism does not provide.

**Consequences.** The registry shows that a Compliance Reviewer approved, not which one. The pipeline cannot detect that the approver is also the requester; that constraint is social, not technical. This is adequate for a role-scoped private channel with a small membership and inadequate for anything requiring non-repudiation or individual accountability under audit.

Resume links are bearer capabilities. Anyone who obtains one, including by a channel member forwarding it, can decide. The exposure is bounded by channel membership and nothing else.

**Rejected alternatives.** Per-reviewer authenticated approval: correct, and requires an authenticated approval surface outside Slack, which is deployment-specific and would make this reference less portable. Recording the Slack user who clicked: only available with an interactive endpoint rather than resume URLs, and would record who clicked rather than who was authorised, which is a weaker guarantee that looks stronger.

**Revisit when.** Any deployment carrying audit obligations. This decision does not survive contact with a real compliance function and is not intended to.

---

## ADR-011 Request identifiers are retained through reclassification

**Status:** Accepted

**Context.** A reviewer may determine the type is wrong. The identifier embeds the type code, so a corrected `FEE` request that is really `RPT` carries an identifier that no longer describes it.

**Decision.** The identifier is assigned once and never changes. `BR-FEE-2026-014` may end up documented as a reporting request.

**Consequences.** Identifiers become unreliable as type indicators, and anyone filtering by the type segment of the identifier gets wrong results. The type is a registry field and that field is authoritative; the identifier segment is a convenience that must not be treated as data.

Reissuing would break every existing reference: Slack threads, the registry record, and anything already linked. An identifier whose purpose is to be referenced must not change, which is more important than its being descriptive.

**Rejected alternatives.** Reissue with a pointer from the old identifier: two identifiers for one request, and someone will cite the wrong one. Type-free identifiers such as `BR-2026-014`: cleaner, and loses the at-a-glance readability that makes the scheme useful in a Slack message. Reasonable to choose differently at the outset; not worth changing afterwards.

**Revisit when.** Reclassification is frequent enough that identifiers routinely mislead, which would also indicate the intake form needs work.

---

## ADR-012 The workflow and the agent instruction are published

**Status:** Accepted

**Context.** This directory publishes the flow structure, the routing logic and the full agent instruction including its negative constraints. A reader interested in producing a draft that evades those constraints now knows what they are.

**Decision.** Publish, after sanitisation for credentials, identifiers and context per `workflow/SANITIZATION.md`.

**Consequences.** The design is inspectable and reusable, and it is also inspectable by someone with reason to work around it. This is acceptable because no control in the pipeline depends on the instruction being secret. The control is that the agent cannot write, and that a human decides. Both hold with the prompt fully disclosed.

Stating it as a decision matters more than the decision. A design whose safety depends on prompt confidentiality would not survive publication, and the fact that this one does is evidence about the design rather than about the sanitisation.

**Rejected alternatives.** Publish the architecture and withhold the instruction: implies the instruction is a control, which would be misleading about where the safety actually sits.

**Revisit when.** A control is added that does depend on non-disclosure, at which point that control is the thing to reconsider first.
