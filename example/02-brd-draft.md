# 02 - BRD draft

**Produced by:** `skills/brd-drafter`
**Input:** `01-request.md`, plus one round of clarifying questions answered by the
requester
**Status:** unedited output

---

## Phase 1 - questions asked before drafting

> Before I draft this, seven things that change the shape of the document. I can
> draft without them - they become open questions instead.
>
> 1. Who has the problem? The insurer losing business, the broker waiting for a
>    quote, or the operations team absorbing the sorting work.
> 2. The two timings do not fit together. Turnaround is described as about four
>    hours once an underwriter has the submission, while brokers are described as
>    waiting long enough to place the risk elsewhere. What is measured today, and
>    from what moment does the clock start?
> 3. Which number is the outcome? Moving the 38% quote rate and being first back
>    are different targets that could pull in different directions.
> 4. What counts as fast enough, and who has the authority to set that figure?
> 5. Is the missing schedule data in scope?
> 6. Is there a written rule for which underwriter receives which submission, or
>    does that judgement live with the person doing the sorting?
> 7. Is the dependency on one person in scope?

All seven were answered. The answers are reflected in the draft below and are
not reproduced separately.

---

```yaml
---
id: BR-001
title: Commercial property submissions reach an underwriter fast enough to compete on response time
status: needs-info
owner: Head of Broker Relations - accountability not confirmed (OQ-007)
author: analyst
updated: day 0
traces:
  decided_in: []
  blocked_by: [OQ-001, OQ-002, OQ-003, OQ-005, OQ-006]
---
```

# BR-001 - Commercial property submissions reach an underwriter fast enough to compete on response time

---

## 1. Problem - required

Winnable commercial property business is being lost because submissions do not
reach an underwriter quickly enough. Submissions from brokers arrive in a shared
mailbox and wait there until a member of the operations team opens each one,
determines the class and sum insured, decides which underwriter should receive
it, and forwards it. Brokers place risks with whichever insurer returns a
workable quote first, so a submission that waits in the mailbox competes against
one that is already being underwritten elsewhere. The delay is described as
ranging from tens of minutes to several hours, and as extending to the next
working day when the person who normally performs the sorting is absent.

The insurer holds the problem. The broker experiences it as a service problem and
the operations team as workload, but both are described in the source as
consequences of the same delay rather than as separate problems.

**Evidence.** Partly measured, partly reported. The end-to-end duration that the
problem statement is about has never been measured, so the causal link between
sorting delay and lost business is a hypothesis rather than an established fact.

| Source | What it shows | Date |
|---|---|---|
| Figures reviewed with Marcus | 38% of commercial property submissions from the top five brokers are quoted. Period, denominator and method not stated - OQ-009 | Not stated |
| Reported broker statement | At least one broker stops approaching other insurers once a workable quote is in hand | Not stated |
| Figure attributed to Marcus | Average underwriting turnaround of about four hours, measured from the point at which a submission has been assigned to an underwriter. This excludes the waiting period the problem statement is about | Not stated |
| Reported observation | Time between arrival in the shared mailbox and assignment ranges from tens of minutes to several hours, and to the next working day when the person who sorts is absent | Not stated |

---

## 2. Outcome - required

**Target state.** A submission arriving from a broker reaches the correct
underwriter without waiting for a particular person to be available, and the
broker receives a first meaningful response within an agreed time. A first
meaningful response is a quote, a decline, or a request for missing information.

Increasing the proportion of submissions that are quoted is explicitly not the
target state. The requester states that quoting risks currently declined on
purpose would be a wrong outcome even if it moved the figure.

**How we will know.**

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| Elapsed time from submission arriving in the shared mailbox to first meaningful response to the broker | Not measured | Not set - OQ-002 | Timestamp availability not confirmed - OQ-001 | Not set |
| Share of submissions receiving a first meaningful response within the agreed service level | Not measured | Not set - OQ-002 | Depends on the same timestamps - OQ-001 | Not set |
| Elapsed time from arrival to assignment to an underwriter | Not measured. Reported as tens of minutes to several hours, and to the next working day when the person who sorts is absent | Not set - OQ-002 | Depends on the same timestamps - OQ-001 | Not set |
| Share of submissions where this insurer returned the first quote to the broker | Not measured | Not set | No internal source. Would require data the brokers hold - OQ-004 | Not set |

Conversion and win rate are named by the requester as a business effect worth
observing rather than as a measure this document is accountable for. They are
recorded here as such and given no target.

---

## 3. Scope

### In scope - required

- Reading a submission on arrival and extracting the data needed to route it
- Routing a submission to an underwriter without requiring a particular person to
  perform the sort
- Manual review and manual rerouting where the classification is uncertain or the
  case is non-standard
- Detecting that information required for underwriting is absent from a
  submission, and marking that submission incomplete
- Preparing a request to the broker for missing information, for a person to send
- Measuring elapsed time from arrival to first meaningful response

### Out of scope - required

| Not included | Why not | Revisit when |
|---|---|---|
| Sending requests for missing information to brokers without underwriter involvement | The rules for doing so are not established | Underwriting leads confirm the rules |
| Obtaining or reconstructing construction details that are absent from the submitted documents | The data is not present in the documents to be read | Not raised |
| Increasing the proportion of submissions that are quoted | Explicitly not the objective. Named by the requester as an outcome that would be wrong to pursue | Not raised |
| Replacing the operations role that performs sorting today | Explicitly not the objective. The requirement is that the process continues when a specific person is unavailable | Not raised |
| Changes to underwriting decisioning itself | Not raised in the source | Elapsed time proves to be dominated by something other than assignment delay - OQ-001 |

---

## 4. Stakeholders - required

| Role | Name | Interest in the outcome | Decides / consulted / informed |
|---|---|---|---|
| Head of Broker Relations | Not stated | Raised the request. Holds the commercial concern about lost business | Decides scope. Accountability for the outcome not confirmed - OQ-007 |
| Not stated | Marcus | Source of the figures. Named as one of the parties who must agree the service level | Consulted. Decision right unsettled - OQ-003 |
| Underwriting leads | Not stated | Must agree the service level and the rules for incomplete submissions | Consulted. Decision right unsettled - OQ-003 |
| Operations, submission sorting | Priya | Performs the routing today and holds routing knowledge that is not written down | Consulted - OQ-005 |
| Underwriters | Not stated | Receive routed submissions. Authority limits constrain what each may receive | Informed |
| Brokers | External | Receive the response. Place business with whoever responds first | Informed |

The service level is described as requiring agreement between Marcus and the
underwriting leads. Two parties are named for one decision and no single role
holds it, so this disagreement has been deferred rather than resolved - OQ-003.

---

## 5. Current process

**Trigger.** A broker sends a commercial property submission to the shared
submissions mailbox.

**Path.** A member of the operations team opens the submission, determines the
class of business and the sum insured, decides which underwriter should receive
it, and forwards it. In practice this is performed by one person. The underwriter
then works the submission; average turnaround from that point is reported as
about four hours.

**Where it breaks.**

| Step | What goes wrong | Frequency | Current workaround |
|---|---|---|---|
| Mailbox to assignment | Submission waits until a person is available to sort it | Not measured. Described as tens of minutes to several hours | None described |
| Mailbox to assignment | When the person who sorts is absent, submissions accumulate | Not measured. Described as extending to the next working day | The backlog is worked through on return |
| Reading the submission | Formats vary: spreadsheets, scanned PDFs, and at least one broker sending by fax | Not stated | Manual reading |
| Underwriting | Construction details are frequently absent from the schedule of values | Described as very often; not measured | Return to the broker to ask, described as costing a further day |

---

## 6. Business rules

No business rules can be recorded from the source.

The factors that appear to drive routing today - class of business, geography,
sum insured against underwriter authority limits, and some broker or account
ownership rules - are stated by the requester with explicit uncertainty about
whether they are complete or written down anywhere. They are recorded as
assumptions in section 7 rather than as rules, and OQ-005 exists to establish
which of the current routing decisions are business rules and which are working
practices.

The requester states directly that current routing decisions should not be
automated before that distinction is made.

---

## 7. Constraints and assumptions

**Constraints**

| Constraint | Type | Imposed by |
|---|---|---|
| Underwriter authority limits determine which underwriters may receive which submissions | commercial | Not stated |
| Manual review and manual rerouting must remain available | commercial | Requester |
| The process must continue to move submissions when any specific individual is unavailable | commercial | Requester |

**Assumptions**

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| The delay between arrival and assignment is the main contributor to slow broker-facing response | Removing the sorting delay would not change what brokers experience, and the initiative would not achieve its outcome | Measure elapsed time end to end before changing the process - OQ-001 |
| Routing today is driven at least partly by class of business, geography, sum insured against authority limits, and broker or account ownership | The routing capability would encode the wrong rules | Working session with the person who sorts today, plus underwriting leads - OQ-005 |
| Part of the routing judgement is held by the person performing it and is not written down | Nothing to reconstruct, and the work is smaller than assumed | Same session - OQ-005 |
| Submissions can be read reliably enough across the formats brokers use to extract routing data | The routing capability cannot be built as described | Not established |

---

## 8. Non-functional requirements

No NFR catalogue exists for this scope, so no `NFR-` identifiers can be
referenced. The categories below follow from the content of this document and
have no values established.

| NFR | Applies because | Value for this scope |
|---|---|---|
| Response time | The outcome is stated in elapsed time | Not established - OQ-002 |
| Availability | Submissions arrive continuously and the problem being solved is a queue that stops moving | Not established |
| Capacity | Sizing depends on submission volume and its variation, neither of which is stated | Not established - OQ-008 |
| Correctness of classification | A misrouted submission adds delay rather than removing it | Not established |

---

## 9. Open questions - required

| ID | Question | Blocks | Owner | Needed by | Default if unanswered |
|---|---|---|---|---|---|
| OQ-001 | Which timestamps exist today for submission arrival, assignment, and response to the broker? | Outcome measures, baseline, the central assumption in section 7 | Marcus | Before review | None. No baseline can be established and no improvement can be claimed |
| OQ-002 | What service level applies from arrival to first meaningful response, and should it differ by complexity or sum insured? | Outcome targets | Unsettled between Marcus and underwriting leads - see OQ-003 | Before review | None |
| OQ-003 | Which single role has the authority to set the service level? | Stakeholders, outcome targets | Unidentified | Before review | None. Two parties are currently named for one decision |
| OQ-004 | Is data on which insurer quoted first obtainable from the brokers? | One outcome measure | Head of Broker Relations | Before review | Drop the measure and state that being first back is not measurable |
| OQ-005 | Which of the routing decisions made today are business rules, and which are working practices? | Business rules, scope of the routing capability | Priya, Marcus, underwriting leads | Before review | None. The requester has stated that automation must not precede this |
| OQ-006 | Which fields are mandatory for underwriting, such that a submission without them is incomplete? | Scope of completeness detection | Underwriting leads | Before review | None |
| OQ-007 | Who is accountable for the business outcome? | Stakeholders, ownership of this document | Unidentified | Before review | The requester by implication, which is not the same as confirmed |
| OQ-008 | What volume of submissions arrives, and how does it vary across the year? | Capacity, sizing | Marcus | Before delivery planning | None |
| OQ-009 | What period, denominator and method produced the 38% figure? | Evidence | Marcus | Before review | Record the figure as reported and unverified |

---

## 10. Not chosen

| Option | Why not | ADR |
|---|---|---|
| A tool that reads each submission on arrival, extracts the key data, and routes it to the correct underwriter automatically | Not rejected. Recorded here because it is the approach proposed in the request rather than one selected after comparison. The rules it would encode are not yet established - OQ-005 | None |

No alternative approaches are recorded, because none were considered. The
request arrived naming this approach, and the problem statement in section 1 was
written from what the proposed approach implies is wrong today.

---

## 11. Downstream artifacts

| Artifact | Identifier | Status |
|---|---|---|
| User story | Not created | |
| NFR catalogue | Not created | |
| Decision record | Not created | |

---

## Change log

| Date | Change | By |
|---|---|---|
| day 0 | Created from the request in `01-request.md` and one round of answers | analyst |

---

**Drafted from:** a Slack request describing lost business on response speed, plus answers to seven clarifying questions.
**Status set to:** `needs-info` because no target value can be stated for any outcome measure (OQ-002), no single role holds the authority to set one (OQ-003), and the business rules section cannot be filled (OQ-005).
**Blocking gaps:** 5 - available timestamps; the service level itself; who sets it; which routing decisions are rules; which fields make a submission incomplete.
**Assumed:** that the delay between arrival and assignment is the main contributor to slow broker-facing response. The source treats this as established; nothing in it measures end-to-end elapsed time, so it is recorded as an assumption with the consequence of it being false. Also that the four-hour figure and the 38% figure describe the same book of business, which the source implies but does not state.
**Suggested next step:** OQ-001. Until the available timestamps are known, no target can be set, no baseline exists, and the assumption the whole initiative rests on is untested.
