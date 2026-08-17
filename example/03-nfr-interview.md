# 03 - NFR interview

**Produced by:** `skills/nfr-interrogator`
**Input:** `02-brd-draft.md`. The interviewer had the draft; the interviewee is
the requester.
**Status:** unedited, apart from the language note below

The interview was conducted in English and answered in Russian. The answers here
are translated, not verbatim. Nothing was added, removed or reordered in
translation, but this is a rendering rather than a transcript in the strict
sense, and that is a limitation of this record.

The skill instructs that the interview be conducted in the language the person is
writing in. It was not, because this file is published. The deviation is recorded
in `NOTES.md`.

---

## Phase 1 - scope

> Four questions before anything else.
>
> 1. **Scope.** From the draft I read this as one capability, not a system:
>    receiving a broker submission, extracting the data needed to route it,
>    assigning it to an underwriter, flagging it as incomplete when required
>    information is absent, and recording the timestamps needed to measure
>    elapsed time. Underwriting itself sits outside it. Is that the right
>    boundary?
> 2. **Users.** Internal on both sides - the operations person who reviews and
>    reroutes, and the underwriters who receive. Brokers are affected but do not
>    touch it. Correct?
> 3. **What exists.** A change to a process that runs today, where the step being
>    replaced is manual. Is there any existing software in this path?
> 4. **Anything already fixed.** Beyond authority limits and the absence of an
>    approved service level: is there a regulatory or contractual obligation that
>    lands on this scope?

**Answers.**

1. That is the right boundary. The capability covers receiving the submission,
   extracting routing data, assigning without depending on a particular person,
   allowing manual review and rerouting for uncertain or non-standard cases,
   detecting missing mandatory underwriting information and marking the
   submission incomplete, preparing a request for missing information for a
   person to send, and recording the timestamps needed to measure elapsed time.
   Underwriting decisioning is out of scope: appetite, authority rules, pricing,
   risk assessment and the decision to quote or decline are not changing. One
   nuance: the end point is not assignment to an underwriter. The business
   outcome is measured through to the first meaningful response to the broker.
   The capability does not necessarily produce that response, but it must capture
   enough timestamps to measure it.

2. Correct. Direct internal users are operations, for manual review and
   rerouting, and underwriters, who receive the routed submission. Brokers
   continue to submit through the existing channel and receive the normal
   broker-facing response. Sending a request for missing information
   automatically is out of scope, so no broker-facing application should be
   assumed.

3. Only what is in the draft can be confirmed: a shared submissions mailbox, and
   a manual routing step after it. Whether there is a mailbox rule or workflow
   automation, a policy administration system, a document management system, a
   broker portal, an underwriting workbench, or any case-management or queueing
   software is unknown. Record it as an open discovery point rather than assuming
   the mailbox is the only system involved. What matters is that this changes an
   existing operating process rather than creating underwriting from scratch.

4. Confirmed constraints: underwriter authority limits, manual review and
   rerouting remaining available, and the process continuing when any specific
   individual is unavailable. The service level is not fixed. For regulatory,
   contractual, security, privacy, retention, residency or audit requirements:
   unknown from the current request, and nothing in the draft establishes any of
   them. No values will be supplied for retention period, data residency, RTO or
   RPO, audit log retention, encryption standards, access-control model, or
   regulatory response times. Those belong to Legal and Compliance, Information
   Security, Data Protection, and the owners of the existing underwriting and
   document platforms. Given that submissions contain broker information and
   client property information, privacy, access control, retention and
   auditability would be expected to apply - but that is an inference, not
   something the BRD establishes.

---

## Category 1 - performance

> 1. Who actually waits - is there an interaction where a person is blocked on
>    the system, or is all the elapsed time queue time?
> 2. Is the cost of delay flat across the day?
> 3. Where does speed stop buying anything? What is the 15-30 minute figure
>    doing?

**Answers.**

1. The delay is queue time, not a person blocked by an interface. A submission
   waits in the shared mailbox until someone opens it, determines class and sum
   insured, chooses an underwriter and forwards it. Nothing in the BRD supports a
   synchronous scenario where a person cannot continue until the system responds.
   The critical performance scenario is therefore asynchronous processing from
   arrival to assignment or readiness for review, not UI response time. If an
   interface for manual review and rerouting is built, its interactive
   performance will need defining separately - the BRD simply gives no grounds to
   treat it as business-critical.

2. Not flat. The business outcome is a first meaningful response within an agreed
   time, with same-business-day as the reference point in discussion. A
   submission arriving at 09:00 and one at 16:55 are in different positions. But
   the BRD defines no cut-off time, no working hours, and no rule for a
   submission arriving late in the evening, and no approved service level. So:
   yes, later arrivals are potentially more time-sensitive; no, a cut-off cannot
   be named; and it must be decided separately whether a submission arriving
   after, say, 16:30 or 17:00 falls inside the same-day obligation or moves to
   the next business day. That is for the service level owner together with the
   underwriting leads.

3. The 15-30 minute figure is a working hypothesis that routing should occupy a
   small share of the same-day window, not a demonstrated point beyond which
   further speed buys nothing. If an underwriter takes around four hours after
   assignment, routing in 20 minutes rather than several hours clearly preserves
   working time in the day. But the difference between 2 and 20 minutes may have
   no noticeable business effect, and there is no data confirming it. So no
   target such as "95% of submissions assigned within 2 minutes" should be set
   yet. First establish which arrival-to-assignment threshold the business
   actually needs, how far it should depend on arrival time, and how it relates
   to the broker-facing service level. Otherwise there is a risk of optimising
   technical speed far below the level where it stops affecting the probability
   of a same-day quote.

---

## Category 2 - availability and reliability

> 1. What is actually lost in an outage?
> 2. Is the manual process still a fallback, and for how long is that acceptable?
> 3. Is partial service acceptable?

**Answers.**

1. Submissions should accumulate in the mailbox rather than being lost. The
   damage is additional elapsed time and the risk of breaching a future service
   level. The ingestion architecture still needs checking to rule out silent loss
   or skipped submissions.

2. The manual process must remain a fallback, since the BRD retains manual review
   and rerouting. The acceptable duration of that mode is not defined. A short
   fallback is acceptable; a multi-day one effectively returns us to the original
   problem.

3. Partial service is acceptable provided submissions are not lost - everything
   going to a manual queue, or completeness being checked by hand. Silent
   misrouting or loss of submissions should count as an outage, not a degraded
   mode.

---

## Category 3 - security

> 1. What is the worst thing that could happen with a submission, commercially?
> 2. Does routing carry a confidentiality decision?
> 3. Is there an information barrier between brokers?

**Answers.**

1. Commercially it is primarily the insurer's problem. A broker passed us
   sensitive information and expects us to control who sees it. An error could
   damage the client and the broker relationship, but the main risk to us is loss
   of broker trust and future business.

2. Yes - routing is sometimes more than efficiency. There can be conflicts of
   interest, restricted accounts, special broker or client arrangements, or other
   reasons a particular underwriter should not see a submission. Those
   restrictions must be part of the routing rules.

3. One underwriter seeing submissions on the same risk from different brokers is
   not usually unusual; competing broker submissions are ordinary. But one
   broker's information must not be used or disclosed to another. Where specific
   broker conflicts or exclusivity arrangements exist, they must be handled
   separately.

---

## Category 4 - data integrity and retention

> 1. When the extraction disagrees with the document, which is right?
> 2. What must survive?
> 3. Is there an existing answer you are inheriting?

**Answers.**

1. The original broker submission remains authoritative. Extracted data is a
   derived representation for routing and system use. The underwriter must have
   access to the original and be able to see what was extracted, particularly
   where a value affects routing or an authority limit.

2. Both are critical: the original submission and the history of its handling -
   who received it, when, whether there was rerouting or manual review. If a
   priority is needed, the original matters more as the primary business record,
   but losing routing and audit history is also serious, because it becomes
   impossible to reconstruct what happened to a submission.

3. Retention would be expected to be inherited from the existing underwriting and
   document management environment, if it already holds retention rules for
   broker submissions. This capability should not invent a new retention period.
   A separate decision is needed only if it creates new data that existing rules
   do not cover.

---

## Category 5 - scalability and capacity

> 1. Which dimension actually moves?
> 2. What happens to the rest of the business when it does?
> 3. What grows besides submissions?

**Answers.**

1. Renewal season is not only a deadline but a peak-volume event. Commercial
   renewals often arrive in waves, so what matters is load on peak days and weeks
   rather than an annual average. How pronounced the seasonality is for our top
   brokers needs checking separately.

2. Underwriter numbers do not usually grow in proportion to incoming volume, so
   at peak the bottleneck may simply move from routing into the underwriting
   queue. That is a significant risk: if routing accelerates and underwriting
   capacity stays the same, the capability meets its local goal and the same-day
   broker response may still not be achieved.

3. Growth is expected not only in submission count but potentially in broker
   count, territories, classes of business, and document complexity and size. New
   broker relationships, new regions or new lines in the next one to two years
   would need accounting for separately, because they change routing rules and
   extraction complexity rather than simply throughput.

---

## Category 6 - observability

> 1. How would you find out today?
> 2. What is worth waking someone for?
> 3. Who watches it?

**Answers.**

1. Today it is most likely found either by the person who sorts, seeing the
   accumulated mail, or by a broker chasing a submission that has disappeared
   from the process. For the target process that is unacceptable: the insurer
   must detect that a submission has not been processed within the expected time
   before the broker does.

2. Business hours and out-of-hours should be separated. During working hours,
   both events - submissions stopping arriving, and submissions arriving but
   routing stopping - should escalate quickly. At night it is not necessarily an
   on-call incident, provided the service level does not require 24/7 processing
   and mail is reliably retained until morning.

3. Business ownership stays with operations: they are the ones who must see queue
   state, backlog, unrouted and exception submissions, and understand whether
   this affects brokers. IT and support are responsible for technical alerts and
   service restoration, but business-flow monitoring cannot be handed over to IT
   entirely - otherwise the observation function that the person sorting performs
   informally alongside routing disappears.

---

## Category 7 - compatibility and portability

> 1. What must keep working exactly as it does now?
> 2. What has to be accepted, rather than supported?

**Answers.**

1. From the broker's side the existing channel is compatible by default: the same
   submission mailbox, the familiar way of sending, and ordinary human replies.
   Changing the channel later can be considered separately, but broker migration
   should not be a dependency of this initiative - otherwise we change the
   internal process and external behaviour at the same time.

2. The system must at minimum accept everything brokers send today: spreadsheets,
   PDFs, scans and fax-derived documents must either be processed automatically
   or land safely in a manual queue. A format can be declared unsupported only as
   a deliberate business decision. Telling a broker "we no longer accept this" is
   for the broker relationship owner, the Head of Broker Relations, together with
   underwriting and operations - not for a technical team.

---

## Category 8 - usability and accessibility

> 1. Who is the least experienced person who has to complete this unaided?
> 2. Is there an accessibility obligation?

**Answers.**

1. A trained colleague from operations, but not a specialist of the same depth as
   the person who does it today. The interface must not require knowledge of
   informal routing practices held in someone's head. It should not, however, be
   designed as though for someone with no understanding of the underwriting
   process at all.

2. No specific obligation is known for this internal tool. It needs checking with
   HR, Legal, Procurement or whoever owns corporate accessibility standards. If
   such a policy exists, the capability should inherit it rather than setting its
   own standard.
