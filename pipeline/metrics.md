# Measurement Design

What this pipeline should measure, how each number is constructed, and what each one cannot tell you.

## No values are reported here

This directory documents a reference implementation against a synthetic context. There are no production figures behind it, and publishing illustrative numbers would be worse than publishing none: a reader retains "review latency around 30 hours" long after forgetting it was labelled illustrative. Numbers are sticky in a way that caveats are not.

So this is a measurement design. It defines what to instrument, where each figure comes from, and what distorts it. If a deployment later produces real observations, they belong in a separate document that cites this one, alongside the cohort and the period they cover.

A second reason for the separation. Any pipeline like this replaces a process that was never measured, so there is no baseline. Without a baseline, no measurement here supports a claim of improvement, only a claim about the current state. Comparisons against how it used to be are unavailable and should not be manufactured.

## Questions before metrics

Metrics chosen first and justified afterwards end up measuring what is easy. Four questions drive the design, in priority order.

1. **Is review real, or is it a click?** The pipeline's only substantive control is a human decision. If that decision is not being made, everything else is theatre.
2. **Where does work stop, and why?** Stalls are the expected failure mode, and the design deliberately makes them visible rather than resolving them.
3. **Are drafts worth reviewing?** A pipeline producing drafts that reviewers rewrite from scratch has moved work rather than reduced it.
4. **Is intake asking for the right things?** Validation failures and abandonment are feedback on the form, not on requesters.

## Metric families

### Flow

**Submissions by type, per period.** Count of records created at `REG 01`, grouped by `Type`. The base denominator for most other figures. Distortion: submissions cluster after incidents and after someone mentions the pipeline in a meeting, so short periods are not comparable.

**Validation failure rate.** Executions terminating at `ORC 03` on the false branch, over total submission attempts at `ITK 01`. Reported alongside a breakdown of which fields appear in `missingFields`, since the aggregate rate says nothing actionable and the field breakdown says everything. A rate concentrated on one field is a form defect.

Note the denominator: attempts, not registrations. Failed validation creates no registry record (see `architecture.md`), so this figure cannot be computed from the registry alone and requires execution-level instrumentation.

**Decision distribution.** Share of decided requests ending `Published`, `Rejected`, `Needs input`, by type. Reported by type or not at all; the aggregate mixes populations with different expected shapes, and `RISK` behaving like `RPT` would be the interesting finding that an aggregate hides.

**Reclassification rate.** Requests whose `Type` changed after `REG 01`, over all registered. Feedback on intake, not on reviewers. Not currently derivable, since the registry stores current type rather than type history.

### Latency

Three intervals, deliberately separated, because combining them hides which part is slow.

**System latency.** `Review requested at` minus `Submitted at`. Everything the pipeline does unaided: validation, routing, registration, knowledge pack load, generation, parsing. This is the only interval the pipeline controls and the only one that improves with better models or infrastructure.

**Review latency.** `Decided at` minus `Review requested at`. The one that matters and the one most distorted:

- Weekends and holidays inflate it. Report in business hours against the reviewer's working calendar, or report the median rather than the mean and say which.
- Timezone spread across reviewer roles makes the same responsiveness look different by role.
- A `Needs input` decision followed by resubmission produces several review intervals for one request. Report per review cycle, and separately report cycles per request, rather than summing them into a single number that describes neither.

**Total elapsed.** Submission to terminal state, including time spent with the requester after `Needs input`. Useful for the question "how long does this take" as a requester experiences it, and useless for diagnosing where the delay is, which is why it never appears alone.

**SLA breach rate.** Review cycles exceeding the type's `Review SLA hours`. Nothing acts on this and nothing should (see `not-automated.md`); it exists to make capacity shortages arguable with evidence.

### Review quality

This family is the point of the exercise and the weakest part of it. Every indicator below is a proxy, each is gameable, and none establishes that review happened. They are worth collecting because the alternative is having no signal at all, and worth labelling clearly because a dashboard makes weak proxies look authoritative.

**Correlation between gap count and decision.** The strongest available indicator. `Open gaps` is on every record at review time and is shown to the reviewer before the approve link. If approval rate is statistically independent of gap count, the gap list is not being read. This does not prove review is happening when the correlation is present, but its absence is hard to explain innocently.

**Review dwell time distribution.** Interval between `REV 01` posting and the decision. A cluster of decisions under a minute is informative; anything above that is not, since the interval measures elapsed time and not attention. Never report a mean. The distribution's left tail is the whole signal.

**Approval rate by reviewer role and type.** A role approving everything, over a meaningful count, is worth a conversation. It is not evidence of rubber-stamping on its own, since a role may simply receive well-formed requests. It is a prompt to look, not a finding.

**Post-publication edit rate.** Share of published pages edited within thirty days of publication, and the size of those edits. The best available lagging proxy for whether the reviewed draft was actually correct. Distortions run both ways: a page nobody edits may be a page nobody reads, and heavy editing may indicate a document that became useful enough to maintain.

**Zero-gap draft frequency.** How often the agent reports no gaps, and how those drafts fare on the post-publication edit rate. A rising zero-gap rate alongside a rising edit rate is the clearest available signature of an agent that has learned to look complete.

### Agent behaviour

**Contract violation rate.** Executions failing at `DFT 04`, split by cause: unparseable response, missing `gapList`, empty `body`. Segmented by instruction version, because an instruction change that raises this rate needs to be attributable to the change.

**Gap count distribution by type.** Not an average. A type whose drafts routinely carry many gaps indicates either a knowledge pack that does not cover it or an intake form that does not collect what the template needs, and the two are distinguishable by looking at which fields recur.

**Knowledge pack staleness at time of use.** Age of the loaded pack when `DFT 01` ran. There is no drift detection in this design (see `not-automated.md`), so age is the crude stand-in, and it says nothing about whether the pack is still correct.

### Requester experience

**Abandonment.** Records sitting in `Needs input` beyond a threshold, by type and by the reason recorded. Diagnostic, not a queue to be cleaned; auto-closing these destroys the signal.

**Cycles per request.** Number of times a request re-entered Orchestration. High cycle counts on one type point at the intake form or the template, not at the requester.

## Deliberate anti-metrics

Not measured, on purpose.

**Drafts generated.** Counts output, not outcome. It rises when the pipeline produces more work for reviewers, which is not obviously good, and it is the number most likely to be quoted in a summary because it is the largest.

**Hours saved.** Requires a counterfactual that does not exist. There is no baseline, and constructing one from estimated manual drafting time produces a figure whose magnitude is set by whoever chose the estimate.

**Automation rate, or share of requests completed without human intervention.** This metric is directly adversarial to PDR-004. The target value is zero by design, so tracking it creates pressure toward a number the architecture exists to prevent. A metric that rewards removing the control is worse than no metric.

**Any agent accuracy or quality score.** Would require either a second model grading the first, rejected in `not-automated.md`, or human grading at a volume nobody will sustain past the first month. A quality score that decays into being computed from whatever is cheap is worse than an acknowledged absence.

**Per-individual reviewer statistics.** Unavailable anyway, since attribution is role-level (PDR-010). Worth stating as a choice rather than a limitation: measurement aimed at individuals becomes performance management, and reviewers who are measured on throughput approve faster. The metric would change the behaviour it claims to observe, in the wrong direction.

## Instrumentation gaps

What the current design cannot measure without changes. Listed so that nobody reports a figure the data does not support.

| Needed | Currently | Change required |
|---|---|---|
| Validation failure rate | Failed validation creates no record | Execution-level logging, or a lightweight rejected-submissions store |
| Reclassification rate | Registry stores current type only | Type change history, or a reclassification counter field |
| Cycles per request | Not counted | Increment a counter on each re-entry at Orchestration |
| Instruction version per draft | Computed in `DFT 02`, never persisted | Write `instructionVersion` to the registry record |
| Zero-gap frequency | `gapListEmpty` computed in `DFT 04`, never persisted | Write the flag to the record |
| Post-publication edit rate | Not collected | Periodic query against page version history |
| Knowledge pack age at use | Not captured | Record the pack's last-modified date at `DFT 01` |

The instruction version gap is the one to close first. Without it, no measurement can be attributed to a version of the agent contract, which makes every trend uninterpretable across a change to the instruction.

## Reporting

Monthly, in one document, read by the pipeline maintainer and by the reviewer roles.

Two conventions worth fixing in advance. Report medians with the distribution shape, not means, since every latency family here is skewed and a mean describes a request that does not exist. And report by type, since aggregates across six types with different SLAs and different reviewer populations mostly measure the mix.

A dashboard is not proposed. A dashboard is looked at when someone remembers to, and the questions in this document benefit from being answered deliberately, in prose, by someone who then has to defend the interpretation.
