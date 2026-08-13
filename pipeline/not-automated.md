# What Is Not Automated, and Why

The decision log records what was built. This records what was not, which is the more informative half.

Three categories, and the distinction between them matters more than any individual entry:

1. **Held by humans on purpose.** Automating these would remove the thing that makes the pipeline trustworthy. No amount of model improvement changes the answer.
2. **Missing from this reference, needed in a real deployment.** Not principled. These are gaps, listed so nobody has to discover them in production.
3. **Rejected automations.** Each one is a reasonable-sounding proposal. Each argument is stated at its strongest before it is answered, because an argument summarised weakly has not been addressed.

The test that separates category 1 from category 2: **would automating this make a wrong outcome less visible?** If yes, it stays human regardless of how well it could be done. If it would only make a right outcome faster, it belongs in category 2 and is simply unbuilt.

---

## 1. Held by humans on purpose

### The approval decision

Covered in full by ADR-004. Summarised here because every other entry in this section depends on it: no automated actor can write the `Published` status, there is no timeout that approves, and no request type skips review.

### Any compliance, legal or risk position

The drafting agent is forbidden from stating or implying that a change is compliant, permitted, or that a review passed. It documents what the request says and marks what is absent.

A model can produce a well-calibrated compliance assessment and still must not, because the sentence survives the assessment. Once "this change is consistent with existing KYC policy" is inside an approved document, it becomes citable evidence that someone checked. The person citing it years later will not know it was generated, and the document does not carry its own provenance into the conversation where it is used.

*Revisit when:* never in this form. A reviewed and attributed compliance sign-off is a different artifact with a different author, and could reasonably be added as a separate document that references the request.

### Whether a gap matters

The agent reports what it could not fill. It does not rank gaps, mark any as blocking, or estimate whether a draft is good enough. Triage is the reviewer's.

Ranking gaps sounds harmless and is not. A severity label produces a reading order, gaps marked low get skimmed, and the agent's assessment of which absences matter becomes the de facto review. The gap list is deliberately flat and deliberately shown before the approve link rather than after it.

*Revisit when:* gap lists grow long enough that flatness is itself an obstacle, which would be a signal to improve intake rather than to rank.

### Reclassification

If a reviewer determines the type is wrong, a human corrects it and the request re-enters at Orchestration. Nothing detects misclassification automatically.

The pipeline has exactly one place where it decides something about a request, and that place is a lookup against a requester-supplied code. Adding a model-based check would create a second, silent judgement, which is the pattern ADR-004 exists to prevent.

### Promoting a RISK draft to current status

`RISK` requests publish as draft (ADR-006). Promotion is manual and lives outside this pipeline.

The pipeline cannot observe whether a fraud threshold was actually implemented. Automating promotion would mean automating a claim about system state the pipeline has no access to. The predictable failure is a forgotten promotion leaving accurate documentation in draft, which is the better failure than a false assurance in the current space.

*Revisit when:* an implementation-confirmed signal exists that the pipeline can observe rather than be told about.

### Splitting cross-cutting requests

A request spanning two types is returned as `Needs input` with an instruction to split. Nothing splits it automatically.

An automatic split produces two documents, each containing half the reasoning, and each reviewer approves the half they understand. Requiring the requester to split forces the decision about where the boundary lies onto the person who knows why the request exists.

### The response to an SLA breach

Review SLA hours are recorded on every record and enforced by nothing. Nothing auto-escalates, reassigns, or approves.

This is the entry most likely to be read as an oversight, so plainly: a breached SLA means a reviewer did not have time. Every automated response to that fact either moves the work to someone else who also does not have time, or removes the review. Neither addresses the cause, and both make the shortage less visible by resolving its symptom. A queue that visibly stalls is a capacity problem stated honestly.

*Revisit when:* never for enforcement. Measurement is a different matter and is covered in `metrics.md`.

---

## 2. Missing from this reference, needed in a real deployment

Not principled positions. Gaps, stated so they are not discovered later. Also listed in `workflow/env.example`.

| Gap | Consequence if deployed as-is | What it needs |
|---|---|---|
| No Slack request signature verification in `ITK 01` | The intake webhook accepts anything that reaches it. Path obscurity is not a control | A verification node before `ORC 01`, rejecting on signature mismatch |
| Sequence generation via workflow static data in `REG 02` | Identifier collisions under concurrent submissions | A registry count query, or a dedicated counter with atomic increment |
| Resume links are bearer capabilities in a shared channel | Anyone in the channel can decide; no individual attribution (ADR-010) | An authenticated approval surface, deployment specific |
| No requester-is-reviewer detection | Self-approval is possible and undetected | Follows from individual attribution; unenforceable without it |
| No error workflow | Failures surface only in the n8n execution log. A stalled request is silent | An error workflow notifying the requester and the maintainer with the request identifier |
| No retry on `DFT 03` failure | A transient API error discards a request entirely | A capped retry with backoff. Capped, because an uncapped retry against a paid API is a cost incident (ADR-007) |
| No delimiter escaping in `ORC 02` | A request body containing the closing delimiter breaks the request block | Reject or escape the delimiter sequence during validation |
| No knowledge pack drift detection | Drafting quality decays silently as packs diverge from reality | Change monitoring on pack pages, with staleness surfaced to the reviewer at review time |

The last one deserves emphasis rather than a table row. Nothing in this pipeline notices when a knowledge pack stops describing reality, and drafting quality degrades gradually rather than failing. It is also the highest-value unaddressed risk in the design for a second reason given in `agent-contract.md`: knowledge pack content enters the agent's instruction half, so write access to the documentation space is effectively write access to the agent.

---

## 3. Rejected automations

### A reminder bot for reviewers

**The case for.** Requests stall because reviewers forget, not because they refuse. A nudge after half the SLA costs nothing and would clear most of the queue.

**Why not.** The first nudge works. By the fourth, the channel has trained everyone to ignore that message class, and the signal is spent on exactly the requests where it was needed. Worse, a reminder makes a stalled queue feel handled: someone is on it, the bot is doing something. The queue depth stops prompting the capacity conversation.

A digest of open requests older than their SLA, posted to a channel humans read for other reasons, is a different proposition and is compatible with this design. That surfaces state without simulating action.

### Load-balanced assignment to individual reviewers

**The case for.** Role-level assignment means everyone assumes someone else will pick it up. Assigning to a named person creates ownership and would fix both the diffusion of responsibility and the attribution gap in ADR-010.

**Why not.** These are two different problems and the second one is the reason to be careful. Assignment by round-robin creates attribution without accountability: the record shows a name, and that name was allocated by an algorithm rather than chosen by someone weighing who should look at this. It reads like individual accountability under audit while being load balancing.

Individual authenticated approval is the right fix for attribution, and it belongs in category 2. Assignment should follow from someone deciding, not from a queue.

### Automatic quality scoring of drafts by a second model

**The case for.** A reviewer facing a long queue benefits from knowing which drafts are weak. A second model scoring completeness and internal consistency is cheap and would direct attention.

**Why not.** A score becomes a reading order, and a high score becomes permission to skim. This is the gap-ranking objection at larger scale: the pipeline's one human judgement gets shaped by a machine assessment that nobody reviews and that has no accountability attached.

There is a second problem specific to using a model to check a model. The two share failure modes. A draft that confidently states an invented figure is precisely the draft most likely to score well, because it is complete, coherent and internally consistent. The scoring layer would be strongest exactly where the reviewer most needs to be unaided.

### Auto-closing stale requests

**The case for.** Records sit in `Needs input` forever after requesters lose interest. Closing them after ninety days keeps the registry meaningful.

**Why not.** Abandoned requests are data about the pipeline. A cluster of them on one type indicates the intake form asks for something requesters cannot supply, and auto-closing removes the evidence at the point it becomes interpretable. A `Stale` view is the answer, not a `Closed` status.

### Model-based classification at intake

Covered by ADR-003. Restated because it is the most frequently proposed change: classification by model is a second place where the pipeline is silently wrong, and the requester never sees what it decided.

### Publishing directly on approval, skipping the registry write-back

**The case for.** `REG 05` is an extra API call between approval and publication, and a failure there leaves a published page with a registry record still reading `In review`.

**Why not.** The failure is real and the ordering is deliberate: publication happens first, then the write-back. A failed write-back leaves a published page and a stale record, which is recoverable and detectable. The reverse ordering would leave records claiming publication for pages that do not exist, which is a lie the registry tells confidently. The registry is the state of record (ADR-001), and a state of record that overstates is worse than one that lags.

---

## What this costs

In one place, so it is not spread thin across the document.

This pipeline is slower than a fully automated one and requires more human attention per request. Its throughput does not improve as models improve, because the bound is reviewer capacity and always was. Several failure modes have been chosen rather than removed: requests stall visibly, drafts are discarded rather than repaired, `RISK` documents sit in draft until someone remembers them.

Every one of those is a decision to keep a problem visible rather than to resolve its symptom. That is the design, not a limitation of it. A pipeline that made all of them go away would not be a better version of this one; it would be the thing this one is built to avoid.
