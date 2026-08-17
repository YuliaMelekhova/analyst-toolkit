# 04 - NFR catalogue

**Produced by:** `skills/nfr-interrogator`, phase 3
**Input:** `03-nfr-interview.md`
**Status:** unedited output

```yaml
---
id: NFR-CATALOGUE
scope: Broker submission intake and routing capability (BR-001)
status: needs-info
owner: not established - OQ-007
updated: day 0
---
```

**No value in this catalogue is a number.** Not one of the eight categories
produced a figure. Every entry below carries a statement and a status, and the
value field is empty throughout. This is the state of knowledge, not a gap in
the interview: the interviewee declined to set values ahead of a service level
that nobody has authority to set yet (OQ-002, OQ-003).

---

## Established

Entries that exist as catalogue entries. None reached `agreed`, because an entry
missing a value, a condition or a verification method is not a requirement yet.

| ID | Category | Statement | Value | Condition | Verification | Source | Status |
|---|---|---|---|---|---|---|---|
| NFR-001 | Performance | Elapsed time from a submission arriving in the shared mailbox to assignment to an underwriter | Not established - deliberately not set ahead of the broker-facing service level | Not established. Working hours, cut-off time and the treatment of late arrivals are undefined | Not established | Requester, in interview | deferred |
| NFR-002 | Performance | The critical performance path is asynchronous processing from arrival to readiness for review, not interactive response time. Interactive performance of any manual review interface is a separate question | n/a - scoping statement | n/a | n/a | Requester, in interview | deferred |
| NFR-003 | Availability | No submission is lost when the capability is unavailable. Submissions accumulate in the mailbox and are processed on recovery | Zero lost submissions | Any period of unavailability | Not established. Ingestion path not yet examined for silent loss or skipped submissions | Requester, in interview | deferred |
| NFR-004 | Availability | Degraded operation is acceptable where submissions are not lost - routing to a manual queue, or completeness checked by hand. Silent misrouting or loss counts as an outage, not a degraded mode | n/a - classification statement | n/a | Not established | Requester, in interview | deferred |
| NFR-005 | Availability | Manual sorting remains available as a fallback. A short fallback period is acceptable; a multi-day one reinstates the problem this work exists to solve | Not established | Not established | Not established | Requester, in interview | deferred |
| NFR-006 | Security | Routing honours confidentiality restrictions: conflicts of interest, restricted accounts, and specific broker or client arrangements determine which underwriters may not receive a submission | Not established - the restrictions themselves are not documented | Not established | Not established | Requester, in interview | deferred |
| NFR-007 | Security | One broker's information is not used for, or disclosed to, another broker. Competing submissions on the same risk reaching one underwriter is ordinary; specific conflicts or exclusivity arrangements are handled separately | Not established | Not established | Not established | Requester, in interview | deferred |
| NFR-008 | Data integrity | The original broker submission is authoritative. Extracted data is a derived representation. The underwriter has access to the original and can see what was extracted, where a value affects routing or an authority limit | n/a - authority statement | Applies where an extracted value affects routing or an authority limit | Not established | Requester, in interview | deferred |
| NFR-009 | Data integrity | The original submission and the record of its handling - who received it, when, and whether it was rerouted or manually reviewed - must both survive. The original takes priority as the primary business record | Not established | Not established | Not established | Requester, in interview | deferred |
| NFR-010 | Data integrity | Retention is inherited from the existing underwriting and document management environment. A separate retention decision is required only for data this capability creates that existing rules do not cover | Inherited - source environment not yet identified | Conditional on discovery of the existing platforms | Not established | Requester, in interview | deferred |
| NFR-011 | Capacity | Load is driven by peak days and weeks around renewal season rather than by an annual average | Not established - volume and seasonality unmeasured (OQ-008) | Not established | Not established | Requester, in interview | deferred |
| NFR-012 | Observability | The insurer detects that a submission has not been processed within the expected time before the broker does | Not established - depends on the service level | Not established | Not established | Requester, in interview | deferred |
| NFR-013 | Observability | During working hours, both submissions ceasing to arrive and routing ceasing to progress escalate quickly. Outside working hours neither is necessarily an on-call incident | Not established | Conditional: holds only while the service level does not require 24/7 processing and mail is reliably retained until morning | Not established | Requester, in interview | deferred |
| NFR-014 | Observability | Operations holds business-flow monitoring - queue state, backlog, unrouted and exception submissions. IT holds technical alerting and service restoration | n/a - allocation statement | n/a | Not established | Requester, in interview | deferred |
| NFR-015 | Compatibility | The existing broker submission channel is unchanged. Broker migration is not a dependency of this work | n/a - constraint statement | n/a | Not established | Requester, in interview | deferred |
| NFR-016 | Compatibility | Every format brokers send today is accepted: spreadsheets, PDFs, scans and fax-derived documents are either processed automatically or land safely in a manual queue. Declaring a format unsupported is a business decision of the Head of Broker Relations with underwriting and operations, not a technical one | n/a - acceptance statement. Automatic processing of any given format is not required | Not established | Not established | Requester, in interview | deferred |
| NFR-017 | Usability | Manual review and rerouting are completable unaided by a trained operations colleague who is not the specialist who performs the work today, without knowledge of undocumented routing practices | Not established - no completion rate or time given | Not established | Not established | Requester, in interview | deferred |

---

## Not established

| Category | What is missing | Who would know | Status |
|---|---|---|---|
| Performance | The arrival-to-assignment threshold, and whether it varies by arrival time | Service level owner - authority unsettled (OQ-003) | unknown |
| Performance | Working hours, cut-off time, and whether a late-afternoon arrival falls inside the same-day obligation | Service level owner with underwriting leads | unknown |
| Availability | Maximum acceptable duration of manual fallback | Requester with operations | unknown |
| Availability | RTO and RPO | Owners of the existing underwriting and document platforms | unknown |
| Security | Encryption standards, access-control model, audit log retention, data residency | Information Security, Data Protection, Legal and Compliance | unknown |
| Security | Which specific confidentiality restrictions exist and how they are recorded today | Underwriting leads with the broker relationship owner | unknown |
| Data integrity | The retention period itself, and whether routing and handling history falls outside existing rules | Owners of the existing document management environment | unknown |
| Capacity | Submission volume, its variation across the year, and the seasonality of the top brokers | Marcus (OQ-008) | unknown |
| Capacity | Whether broker count, territories or classes of business are expected to grow within one to two years | Requester with the business | unknown |
| Observability | Detection time, alert thresholds, and what "expected time" means without a service level | Operations with the service level owner | unknown |
| Compatibility | The systems already in this path - mailbox automation, policy administration, document management, underwriting workbench, queueing | Open discovery point; owner not identified | unknown |
| Accessibility | Whether a corporate accessibility obligation applies to internal tooling | HR, Legal, Procurement, or the owner of corporate accessibility standards | unknown |

---

## Not applicable

No category was marked not applicable. All eight were addressed and all eight
produced something, even if only an owner.

---

**Interviewed:** the requester, Head of Broker Relations.
**Scope:** broker submission intake and routing capability, as bounded in BR-001. Underwriting decisioning excluded. The capability does not produce the broker-facing response but must capture the timestamps needed to measure it.
**Established:** 0 of 8 categories established with a value. 8 of 8 addressed. 17 entries recorded, all `deferred`; 12 gaps recorded with owners.
**Suggested by me and accepted:** none. No value was proposed by the interviewer, and none was accepted.
**Next:** the service level from arrival to first meaningful response, and which single role has authority to set it. Six of the seventeen entries have a value that cannot be set until it exists, and the two performance gaps are entirely downstream of it. This is OQ-002 and OQ-003 in BR-001, and it is the same blocker the drafting step identified.

---

## Notes on this catalogue that are not part of it

Two things surfaced that are worth recording separately.

**A functional gap was found by asking about a quality attribute.** NFR-006
records that routing must honour confidentiality restrictions. Those restrictions
are routing rules, and they appear nowhere in BR-001 - not in section 6, and not
in OQ-005, which asks only which routing decisions are business rules and which
are working practices. Class of business, geography and authority limits do not
imply conflicts of interest or restricted accounts. This changes the consequence
of a misrouting from lost time to a confidentiality incident, and it should go
back into the BRD rather than staying in the catalogue.

**One risk was accepted rather than requirement-ised.** The interviewee stated
that at peak the bottleneck may move from routing into the underwriting queue, so
the capability could meet its local goal while the same-day broker response still
fails. That is the assumption already recorded in section 7 of BR-001, now named
and accepted by the business but still unmeasured. It is not an NFR and no entry
was created for it.
