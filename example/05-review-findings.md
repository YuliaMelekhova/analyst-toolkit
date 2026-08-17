# 05 - Review findings

**Produced by:** `skills/requirements-smell-detector`
**Input:** `02-brd-draft.md` and `04-nfr-catalog.md`, reviewed together
**Status:** unedited output

**Assumptions made without asking.** The artifact is a business requirements
document at first-draft stage with an accompanying NFR catalogue, both marked
`needs-info`. A BRD tolerates open questions that a ready-for-sprint story does
not, so a gap already recorded as an open question with an owner is not reported
as a finding. No glossary exists for this domain.

---

## Findings

| # | Quote | Smell | Class | Suggested reformulation |
|---|-------|-------|-------|------------------------|
| 1 | "Measuring elapsed time from arrival to first meaningful response" | Unverifiable outcome | Blocking | The scope requires measuring to an event the scope excludes. The first meaningful response is sent by a person, and the boundary places underwriting outside the capability. Nothing states where the end timestamp comes from. As written, the primary measure cannot be produced by anything this document authorises. Either bring the recording of that event inside the boundary, name the system that holds it, or state that the measure depends on an external source not yet identified. |
| 2 | "Manual review and manual rerouting where the classification is uncertain or the case is non-standard" | Unverifiable outcome | Should fix | No criterion distinguishes uncertain from certain, or standard from non-standard. This is the trigger that decides whether a submission is handled automatically or by a person, so two implementations would divide the work differently and both would satisfy the text. State what makes a classification uncertain, or record that the threshold is undecided and name who sets it. |
| 3 | "reaches the correct underwriter" | Unverifiable outcome | Should fix | Correctness has no definition here. OQ-005 asks which routing decisions are business rules, but the outcome statement asserts a correct answer exists before the rules that determine it are known. Restate the outcome in terms the document can support - reaching an underwriter with the authority and appetite for the submission - or make it explicit that "correct" is defined by the rules OQ-005 will establish. |
| 4 | "Winnable commercial property business is being lost" | Terminology drift | Should fix | "Winnable" carries the problem statement and is never defined. It is not the same as "quoted", and the document elsewhere states that raising the quoted proportion is explicitly not the objective. Define it, or state the problem without it. |
| 5 | "Reading a submission on arrival and extracting the data needed to route it" | Unbounded quantifier | Should fix | The set is defined by its purpose rather than enumerated, and the purpose is itself undetermined until OQ-005 resolves. Nothing here bounds what must be extracted. Record the data set as dependent on OQ-005 rather than leaving it implied. |
| 6 | "Submissions can be read reliably enough across the formats brokers use to extract routing data" | Subjective quality | Should fix | "Reliably enough" has no threshold, and the row's own "how we would find out" column reads "Not established". An assumption with no test is not being tracked. State the accuracy the process would need, or record that it is unknown with an owner. |
| 7 | "Unsettled between Marcus and underwriting leads - see OQ-003" | Missing actor | Should fix | OQ-002 has no owner. The template's own rule is that a question with no owner is a note rather than a question, and this one is blocked by another open question rather than by a person. Either assign it provisionally or record explicitly that OQ-002 cannot be worked until OQ-003 closes. |
| 8 | "Before review" (9 occurrences in the open questions table) | Unverifiable outcome | Should fix | The template asks for a date by which an unanswered question becomes a decision by default. "Before review" names no date and no review, so no question can ever be overdue. Reported once for all occurrences. |
| 9 | "n/a - scoping statement" (6 entries: NFR-002, 004, 008, 014, 015, 016) | Unverifiable outcome | Should fix | Six catalogue entries carry no value because they are policy, allocation or classification statements rather than requirements. The catalogue's own rule is that an entry missing value, condition, verification or source is not a requirement yet. They are worth keeping, but as constraints in the BRD or as notes, not as numbered NFR entries - the count of 17 currently overstates what was established. |
| 10 | "No business rules can be recorded from the source." | Missing failure path | Should fix | Contradicted by NFR-006, which states that routing must honour conflicts of interest, restricted accounts and specific broker or client arrangements. Those are business rules, established in interview after the BRD was written. Neither document reconciles this: section 6 says none exist, the catalogue says some do, and OQ-005 does not ask about them. Both statements are reported together; the weaker is the BRD's. |
| 11 | "average turnaround from that point is reported as about four hours" | Untraceable number | Consider | An average with no percentile, period or method, attributed to a person rather than a measurement. The document already flags that it excludes the waiting period, which is the important part. Recording the origin matters when the figure is challenged, not before. |
| 12 | "The process must continue to move submissions when any specific individual is unavailable" | Unverifiable outcome | Should fix | "Continue to move" states no rate. NFR-005 records that the acceptable duration of manual fallback is undefined, so the constraint and the catalogue both stop at the same place. As written, a process that moves one submission a day satisfies it. |
| 13 | - | Missing failure path | Should fix | Cross-cutting, the action is repeated: brokers chase submissions, and the document says so. Nothing states what happens when the same submission arrives twice, or when a broker resends after no reply. Duplicate handling is neither specified nor placed out of scope, and it is close to certain in this process. |
| 14 | - | Missing failure path | Should fix | Cross-cutting, state changes between read and write: manual rerouting is in scope, but nothing states what happens when a submission is rerouted after an underwriter has begun work on it, or when two operations people act on the same submission. Neither case is excluded. |
| 15 | - | Missing failure path | Consider | Cross-cutting, configuration changes mid-flight: authority limits and routing rules will change while submissions are in progress. Nothing states whether a submission binds to the rules in force at arrival or re-reads them at assignment. Lower urgency than 13 and 14 because the window is short. |
| 16 | - | Missing failure path | Should fix | Cross-document traceability: BRD section 8 lists four NFR categories, and the catalogue records entries in eight. Compatibility, usability and the security entries have no counterpart in the BRD, and no catalogue entry carries an `applies_to` reference back to BR-001. The two documents cannot currently be checked against each other. |

**Cross-cutting scenarios that do not produce findings.** The assigned party
becoming unavailable is covered by a constraint in section 7 and by NFR-005. A
dependency not answering is covered by NFR-003 and NFR-004, which state that
submissions accumulate rather than being lost and that degraded operation is
acceptable. The empty or boundary case is covered: a submission whose
classification cannot be determined falls into the manual review path, which is
in scope.

---

## Summary

- Statements reviewed: 64 - 47 in the BRD, 17 catalogue entries
- Findings: 1 blocking, 13 should-fix, 2 consider
- Most frequent smell: Missing failure path, 4 occurrences, all from the
  cross-cutting scan
- Cleanest statements: the definition of a first meaningful response as a quote,
  a decline, or a request for missing information; the out-of-scope table, which
  states reasons and revisit conditions rather than asserting exclusions; the
  evidence table in section 1, which labels the causal claim as a hypothesis
  rather than a finding; NFR-013, which states its own condition of validity
- Not reviewed: whether the scope is right, whether the approach is sound, and
  whether the open questions are the right ones. Different review. Gaps already
  recorded as open questions with owners were not reported again as findings -
  eleven such gaps exist and were passed over deliberately
