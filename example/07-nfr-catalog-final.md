# 07 - NFR catalogue after review

**Input:** `04-nfr-catalog.md`, revised against `05-review-findings.md`
**Status:** revised

Two changes. Six entries that were not requirements moved to *Positions*, so the
catalogue holds eleven entries rather than seventeen. Every entry now carries
`applies_to`, which it did not before - finding 16.

```yaml
---
id: NFR-CATALOGUE
scope: Broker submission intake and routing capability (BR-001)
status: needs-info
owner: not established - OQ-007
updated: day 0
---
```

**No value in this catalogue is a number.** Every quantity depends on the service
level in OQ-002, which nobody currently has the authority to set (OQ-003).

---

## Catalogue

| ID | Category | Statement | Value | Verification | Status | Owner | Needed by | Applies to |
|---|---|---|---|---|---|---|---|---|
| NFR-001 | Performance | Elapsed time from arrival in the shared mailbox to assignment to an underwriter | Not established - deliberately not set ahead of the broker-facing service level. Working hours, cut-off time and the treatment of late arrivals are undefined | Not established | deferred | Service level owner - unidentified (OQ-003) | Before design begins | BR-001 |
| NFR-003 | Availability | No submission is lost when the capability is unavailable. Submissions accumulate in the mailbox and are processed on recovery | Zero lost submissions, over any period of unavailability | Not established. The ingestion path has not been examined for silent loss or skipped submissions | deferred | Operations, submission handling | Before design begins | BR-001 |
| NFR-004 | Availability | Manual sorting remains available as a fallback. A short fallback period is acceptable; a multi-day one reinstates the problem this work exists to solve | Not established | Not established | deferred | Operations, submission handling | Before design begins | BR-001 |
| NFR-005 | Security | Routing honours confidentiality restrictions: conflicts of interest, restricted accounts, and specific broker or client arrangements determine which underwriters may not receive a submission | Not established - the restrictions themselves are not documented (OQ-011) | Not established | deferred | Underwriting leads | Before design begins | BR-001, BR-001/R1 |
| NFR-006 | Security | One broker's information is not used for, or disclosed to, another broker. Competing submissions on the same risk reaching one underwriter is ordinary; specific conflicts or exclusivity arrangements are handled separately | Not established | Not established | deferred | Underwriting leads | Before design begins | BR-001 |
| NFR-007 | Data integrity | The original submission and the record of its handling - who received it, when, and whether it was rerouted or transferred - must both survive. The original takes priority as the primary business record | Not established | Not established | deferred | Owners of the document management environment | Before design begins | BR-001 |
| NFR-008 | Data integrity | Retention is inherited from the existing underwriting and document management environment. A separate retention decision is required only for data this capability creates that existing rules do not cover | Inherited - source environment not yet identified | Not established | deferred | Owners of the document management environment | Before design begins | BR-001 |
| NFR-009 | Capacity | Load is driven by peak days and weeks around renewal season rather than by an annual average | Not established - volume and seasonality unmeasured (OQ-008) | Not established | deferred | Underwriting operations | Before delivery planning | BR-001 |
| NFR-010 | Observability | The insurer detects that a submission has not been processed within the expected time before the broker does | Not established - depends on the service level | Not established | deferred | Operations, submission handling | Before design begins | BR-001 |
| NFR-011 | Observability | During working hours, both submissions ceasing to arrive and routing ceasing to progress escalate quickly. Outside working hours neither is necessarily an on-call incident. Conditional: holds only while the service level does not require 24/7 processing and mail is reliably retained until morning | Not established | Not established | deferred | Operations with the service level owner | Before design begins | BR-001 |
| NFR-012 | Usability | Manual review, rerouting and resolution of uncertain duplicate matches are completable unaided by a trained operations colleague who is not the specialist who performs the work today, without knowledge of undocumented routing practices | Not established - no completion rate or time given | Not established | deferred | Operations, submission handling | Before design begins | BR-001 |

Identifiers are not renumbered from the previous version, and the gaps left by
the six entries that moved to Positions are not reused. `NFR-002` does not exist
here because it became position 6.

---

## Positions

> Qualities asserted without a value. Checkable, not measurable, and not yet
> deliberated: none was chosen from alternatives, so none is a decision record
> yet. Positions are numbered within this document and carry no framework
> identifier.

| # | Statement | Stated by | Becomes a decision when |
|---|---|---|---|
| 1 | Silent misrouting or loss of a submission is an outage, not a degraded mode. Degraded operation is acceptable where submissions are not lost - routing to a manual queue, or completeness checked by hand | Requester, in interview | Any alerting or fallback behaviour is designed against it |
| 2 | The original broker submission is authoritative; extracted data is a derived representation. The underwriter has access to the original and can see what was extracted, where a value affects routing or an authority limit | Requester, in interview | Extraction is designed, or an interface shows an extracted value |
| 3 | Operations holds business-flow monitoring - queue state, backlog, unrouted and exception submissions. IT holds technical alerting and service restoration | Requester, in interview | Monitoring is built, or on-call responsibility is assigned |
| 4 | The existing broker submission channel is unchanged. Broker migration is not a dependency of this work | Requester, in interview | Any change to the broker-facing channel is proposed |
| 5 | Every format brokers send today is accepted, automatically or into a manual queue. Declaring a format unsupported is a business decision of the Head of Broker Relations with underwriting and operations | Requester, in interview | A format is proposed as unsupported |
| 6 | The critical performance path is asynchronous processing from arrival to readiness for review, not interactive response time. Interactive performance of any manual review interface is a separate question | Requester, in interview | A manual review interface is specified |

---

## Not established

| Category | What is missing | Who would know | Status |
|---|---|---|---|
| Performance | The arrival-to-assignment threshold, and whether it varies by arrival time | Service level owner - authority unsettled (OQ-003) | unknown |
| Performance | Working hours, cut-off time, and whether a late-afternoon arrival falls inside the same-day obligation | Service level owner with underwriting leads | unknown |
| Availability | Maximum acceptable duration of manual fallback | Requester with operations | unknown |
| Availability | RTO and RPO | Owners of the existing underwriting and document platforms | unknown |
| Security | Encryption standards, access-control model, audit log retention, data residency | Information Security, Data Protection, Legal and Compliance | unknown |
| Security | Which confidentiality restrictions exist and how they are recorded today | Underwriting leads with the broker relationship owner (OQ-011) | unknown |
| Data integrity | The retention period, and whether routing and handling history falls outside existing rules | Owners of the existing document management environment | unknown |
| Capacity | Submission volume, its variation across the year, and the seasonality of the top brokers | Marcus (OQ-008) | unknown |
| Capacity | Whether broker count, territories or classes of business are expected to grow within one to two years | Requester with the business | unknown |
| Observability | Detection time, alert thresholds, and what "expected time" means without a service level | Operations with the service level owner | unknown |
| Compatibility | The systems already in this path - mailbox automation, policy administration, document management, underwriting workbench, queueing | Open discovery point; owner not identified | unknown |
| Accessibility | Whether a corporate accessibility obligation applies to internal tooling | HR, Legal, Procurement, or the owner of corporate accessibility standards | unknown |

---

## Not applicable

No category was marked not applicable. All eight were addressed, and all eight
produced something, even if only an owner.

---

**Interviewed:** the requester, Head of Broker Relations.
**Scope:** broker submission intake and routing capability, as bounded in BR-001.
**Established:** 0 of 8 categories established with a value. 8 of 8 addressed. 11 entries, all `deferred`, each with a role and a moment; 6 positions; 12 gaps recorded with owners.
**Suggested by me and accepted:** none. No value was proposed by the interviewer, and none was accepted.
**Next:** the service level from arrival to first meaningful response, and which single role has authority to set it - OQ-002 and OQ-003.
