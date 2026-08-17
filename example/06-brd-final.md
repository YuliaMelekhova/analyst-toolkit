# 06 - BRD after review

**Input:** `02-brd-draft.md`, revised against `05-review-findings.md`
**Decisions taken by:** the requester, in session
**Status:** revised, still `needs-info`

All sixteen findings are addressed. Six were decided by the requester; the rest
were editorial. What changed and why is listed at the end of this file.

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
  blocked_by: [OQ-001, OQ-002, OQ-003, OQ-005, OQ-006, OQ-010, OQ-011, OQ-012, OQ-013]
---
```

# BR-001 - Commercial property submissions reach an underwriter fast enough to compete on response time

---

## 1. Problem - required

Commercial property business the insurer would have quoted on its existing
appetite is being lost because submissions do not reach an underwriter quickly
enough. Submissions arrive in a shared mailbox and wait until a member of the
operations team opens each one, determines the class and sum insured, decides
which underwriter should receive it, and forwards it. Brokers place risks with
whichever insurer returns a workable quote first, so a submission waiting in the
mailbox competes against one already being underwritten elsewhere. The delay is
described as ranging from tens of minutes to several hours, and as extending to
the next working day when the person who normally performs the sorting is absent.

The insurer holds the problem. The broker experiences it as a service problem and
the operations team as workload, but both are described in the source as
consequences of the same delay rather than as separate problems.

**Evidence.** Partly measured, partly reported. The end-to-end duration that the
problem statement is about has never been measured, so the link between sorting
delay and lost business is a hypothesis rather than an established fact.

| Source | What it shows | Date |
|---|---|---|
| Figures reviewed with Marcus | 38% of commercial property submissions from the top five brokers are quoted. Period, denominator and method not stated - OQ-009 | Not stated |
| Reported broker statement | At least one broker stops approaching other insurers once a workable quote is in hand | Not stated |
| Figure attributed to Marcus | Average underwriting turnaround of about four hours, measured from the point at which a submission has been assigned. No percentile, period or method was given, and it excludes the waiting period this document is about. Treated as reported rather than measured | Not stated |
| Reported observation | Time between arrival in the shared mailbox and assignment ranges from tens of minutes to several hours, and to the next working day when the person who sorts is absent | Not stated |

---

## 2. Outcome - required

**Target state.** A submission arriving from a broker reaches an underwriter who
holds the authority for its sum insured and against whom no confidentiality
restriction applies, without waiting for a particular person to be available. The
broker receives a first meaningful response within an agreed time. A first
meaningful response is a quote, a decline, or a request for missing information.

Increasing the proportion of submissions that are quoted is explicitly not the
target state. Quoting risks currently declined on purpose would be a wrong
outcome even if it moved the figure.

**How we will know.**

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| Elapsed time from a submission arriving in the shared mailbox to first meaningful response to the broker | Not measured | Not set - OQ-002 | Start timestamp observable within this capability. End timestamp requires the external source in OQ-010 | Not set |
| Share of submissions receiving a first meaningful response within the agreed service level | Not measured | Not set - OQ-002 | Depends on the same two timestamps | Not set |
| Elapsed time from arrival to assignment to an underwriter | Not measured. Reported as tens of minutes to several hours, and to the next working day when the person who sorts is absent | Not set - OQ-002 | Both timestamps observable within this capability, subject to OQ-001 | Not set |
| Share of submissions where this insurer returned the first quote to the broker | Not measured | Not set | No internal source. Would require data the brokers hold - OQ-004 | Not set |

Conversion and win rate are a business effect worth observing rather than a
measure this document is accountable for. They are given no target.

---

## 3. Scope

### In scope - required

- Reading a submission on arrival and extracting the data needed to evaluate the
  conditions for automatic routing below
- Matching an arriving submission against submissions already received, and
  linking it to an existing one where the match is confident
- Holding a submission whose match is uncertain in an exception queue, unrouted,
  until a person confirms whether it is new or a continuation
- Routing a submission automatically only where every condition for automatic
  routing is met. A submission failing any condition, or whose classification
  cannot be established, goes to a person
- Recording the state of a submission and which underwriter currently owns it. A
  submission has at most one current owner at any moment
- Manual review and manual rerouting before an underwriter has accepted a
  submission, within the ordinary routing rules
- Transfer of ownership after acceptance, as an explicit act that notifies both
  the current and the new underwriter and records a reason
- Detecting that information required for underwriting is absent, and marking the
  submission incomplete
- Preparing a request to the broker for missing information, for a person to send
- Recording the timestamps this capability can observe: arrival of a submission,
  and assignment to an underwriter

**Conditions for automatic routing.** A submission routes automatically only
where all of the following hold. Any submission failing one of them, or where one
cannot be evaluated, is routed to a person.

- The fields required to determine the route are present and readable
- Exactly one routing rule applies
- The sum insured falls within the authority of an available underwriter
- No confidentiality restriction applies to the submission, or the applicable
  restriction can be evaluated against the available underwriters
- Either no existing submission matches this one, or exactly one does and the
  match is confident

The list is not complete. These conditions are structural and hold independently
of how routing is performed today. Conditions arising from judgement currently
exercised by the person who routes - workload, familiarity with a broker, the
risk being unusual - cannot be stated until OQ-005 establishes which of those
judgements are business rules. Until then they are covered by the default: not
evaluated means not routed automatically.

**Repeat submissions.** Repeats are ordinary in this process, not an edge case.
The request records that brokers chase, and chasing is the current mechanism by
which a lost submission is noticed. A repeat may be an identical resend, a
response supplying previously missing information, an updated version of the same
risk, or the same risk arriving from a different broker - the last of which is
competing business and must not be suppressed.

No reliable identifier exists for matching. A broker reference may be present and
is not guaranteed to survive forwarding, so matching rests on a combination of
signals: broker, insured, property or address, dates, sums, and reference number
where present. This is a stated rule with a confidence threshold, not a hidden
heuristic - OQ-012.

**Measuring the broker-facing outcome.** The timestamp of the first meaningful
response is produced outside this capability, by whoever sends it. Measuring the
broker-facing outcome therefore depends on retrieving that event from the system
that records outbound responses to brokers, which is not yet identified - OQ-010.
Until it is, the broker-facing measure cannot be produced.

### Out of scope - required

| Not included | Why not | Revisit when |
|---|---|---|
| Sending requests for missing information to brokers without underwriter involvement | The rules for doing so are not established | Underwriting leads confirm the rules |
| Obtaining or reconstructing construction details absent from the submitted documents | The data is not present in the documents to be read | Not raised |
| Increasing the proportion of submissions that are quoted | Explicitly not the objective. Named as an outcome that would be wrong to pursue | Not raised |
| Replacing the operations role that performs sorting today | Explicitly not the objective. The requirement is that the process continues when a specific person is unavailable | Not raised |
| Changes to underwriting decisioning itself - appetite, authority rules, pricing, risk assessment, the decision to quote or decline | Not raised in the source, and excluded explicitly in interview | Elapsed time proves to be dominated by something other than assignment delay - OQ-001 |
| Any change to the broker-facing submission channel | Changing the internal process and external behaviour at once was rejected. Broker migration is not a dependency of this work | A change to the channel is proposed on its own merits |

---

## 4. Stakeholders - required

| Role | Name | Interest in the outcome | Decides / consulted / informed |
|---|---|---|---|
| Head of Broker Relations | Not stated | Raised the request. Holds the commercial concern about lost business. Authorises declaring a broker format unsupported | Decides scope. Accountability for the outcome not confirmed - OQ-007 |
| Not stated | Marcus | Source of the figures. Named as one of the parties who must agree the service level | Consulted. Decision right unsettled - OQ-003 |
| Underwriting leads | Not stated | Must agree the service level, the rules for incomplete submissions, and the confidentiality restrictions. Authorise transfer of ownership after acceptance | Consulted. Decision right unsettled - OQ-003 |
| Operations, submission handling | Priya named as the person who performs routing today | Performs routing and holds routing knowledge that is not written down. Owns business-flow monitoring in the target process. Resolves uncertain duplicate matches | Consulted - OQ-005 |
| Underwriters | Not stated | Receive routed submissions and accept ownership. Authority limits constrain what each may receive | Informed |
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
| Detection of a stalled submission | A submission that stops moving is noticed either by the person who sorts seeing accumulated mail, or by the broker chasing | Not measured | The broker chases |

---

## 6. Business rules

Four rules. All four were established in the NFR interview rather than from the
original request, and none has been confirmed by the underwriting leads.

| Rule | Statement | Source of authority |
|---|---|---|
| BR-001/R1 | A submission is not assigned to an underwriter for whom a confidentiality restriction applies to it - a conflict of interest, a restricted account, or a specific broker or client arrangement | Head of Broker Relations, in interview. Not confirmed - OQ-011 |
| BR-001/R2 | A submission that repeats or continues one already received is linked to it and does not create a second independent case. Where the match is uncertain, the submission is not routed until a person confirms whether it is new or a continuation | Head of Broker Relations, in interview |
| BR-001/R3 | A submission has at most one current assigned underwriter. Others may view it or take part in review; ownership is not shared | Head of Broker Relations, in interview |
| BR-001/R4 | Once an underwriter has accepted a submission, rerouting is no longer an operations action. It requires the decision of an underwriting lead or another authorised senior role, and is recorded with its reason | Head of Broker Relations, in interview. Not confirmed - OQ-013 |

BR-001/R1 is recorded; its content is not. Which restrictions exist, how they are
recorded today, and how one would be evaluated against a submission are
unestablished - OQ-011.

The factors that appear to drive routing otherwise - class of business,
geography, sum insured against underwriter authority limits, and some broker or
account ownership rules - remain assumptions in section 7 rather than rules,
pending OQ-005.

---

## 7. Constraints and assumptions

**Constraints**

| Constraint | Type | Imposed by |
|---|---|---|
| Underwriter authority limits determine which underwriters may receive which submissions | commercial | Not stated |
| Manual review and manual rerouting must remain available | commercial | Requester |
| No step in the routing path may require a specific named individual to be present | commercial | Requester |
| Ownership, state and any parallel activity on a submission are visible to everyone who can see it | commercial | Requester |
| Every format brokers send today is accepted. A format may be declared unsupported only as a business decision of the Head of Broker Relations with underwriting and operations | commercial | Requester |

**Assumptions**

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| The delay between arrival and assignment is the main contributor to slow broker-facing response | Removing the sorting delay would not change what brokers experience, and the initiative would not achieve its outcome | Measure elapsed time end to end before changing the process - OQ-001, OQ-010 |
| Enough submissions satisfy all conditions for automatic routing to reduce elapsed time materially | The capability routes a minority automatically and the delay it exists to remove largely remains | Establish the proportion once OQ-005 defines the conditions and OQ-008 gives volume |
| Authority for the sum insured and the absence of a confidentiality restriction are sufficient to define a correct routing outcome | Submissions route to underwriters who are formally eligible but poorly matched - by experience with the property type, or by familiarity with the broker - and the quality of the response suffers without the measure showing it | OQ-005 establishes which of the judgements currently exercised are business rules. Until then, no measure distinguishes a well-matched routing from a merely permitted one |
| Routing a submission to a person is sufficient to prevent a confidentiality breach where the restriction cannot be evaluated automatically | The manual path appears to be a safeguard and is not. Restrictions recorded nowhere are unknown to the person receiving the submission as well as to the system, so a breach passes through both | OQ-011. Until the restrictions are recorded somewhere a person can consult, neither path enforces BR-001/R1 |
| Routing today is driven at least partly by class of business, geography, sum insured against authority limits, and broker or account ownership | The routing capability would encode the wrong rules | Working session with the person who routes today, plus underwriting leads - OQ-005 |
| Part of the routing judgement is held by the person performing it and is not written down | Nothing to reconstruct, and the work is smaller than assumed | Same session - OQ-005 |
| Submissions can be read accurately enough across the formats brokers use to evaluate the conditions for automatic routing | The conditions cannot be evaluated automatically, every submission falls to the manual path, and the capability delivers nothing beyond measurement | Sample submissions across each format in use and compare extraction against manual reading. The accuracy required follows from which conditions depend on which fields - OQ-006, OQ-012 |
| At peak the bottleneck may move from routing into the underwriting queue | The capability meets its local goal and the broker-facing outcome still fails. Accepted by the business as a known risk rather than mitigated here | Measure both queues separately once OQ-001 establishes the timestamps |

---

## 8. Non-functional requirements

The catalogue for this scope is `04-nfr-catalog.md`. No entry carries a value:
all quantities depend on the service level in OQ-002, which nobody currently has
the authority to set (OQ-003).

| Category | Entries | Value established |
|---|---|---|
| Performance | NFR-001 | No - OQ-002 |
| Availability | NFR-003, NFR-004, NFR-005 | No |
| Security | NFR-006, NFR-007 | No - OQ-011 |
| Data integrity | NFR-008, NFR-009, NFR-010 | No |
| Capacity | NFR-011 | No - OQ-008 |
| Observability | NFR-012, NFR-013 | No |
| Compatibility | see Positions in the catalogue | n/a |
| Usability | NFR-014 | No |

---

## 9. Open questions - required

No calendar dates appear in this column. No delivery schedule has been agreed,
and the only date in the source - renewal season, four months out - is a delivery
deadline rather than a business requirement and is not recorded here. The moments
below are process gates that exist in this document.

| ID | Question | Blocks | Owner | Needed by | If unanswered |
|---|---|---|---|---|---|
| OQ-001 | Which timestamps exist today for submission arrival, assignment, and response to the broker? | Outcome measures, baseline, the central assumption in section 7 | Marcus | Before design begins | **Default:** the baseline is limited to arrival and assignment, and no claim about broker-facing elapsed time is made |
| OQ-002 | What service level applies from arrival to first meaningful response, and should it differ by complexity or sum insured? | Outcome targets, every value in the NFR catalogue | Unowned - blocked by OQ-003 | Before design begins | **Escalation:** at the point a target value is first required, with OQ-003. No default: setting one would either abandon the broker-facing promise or invent a figure |
| OQ-003 | Which single role has the authority to set the service level? | Stakeholders, outcome targets, OQ-002 | Unidentified | Before design begins | **Escalation:** at the point a target value is first required. No default: naming an owner by default assigns an authority nobody granted |
| OQ-004 | Is data on which insurer quoted first obtainable from the brokers? | One outcome measure | Head of Broker Relations | Before design begins | **Default:** the measure is dropped and being first back is recorded as not measurable |
| OQ-005 | Which of the routing decisions made today are business rules, and which are working practices? | Business rules, the conditions for automatic routing | Priya, Marcus, underwriting leads | Before design begins | **Default:** only the structural conditions apply. The proportion routed automatically may be small enough that the capability does not achieve its outcome |
| OQ-006 | Which fields are mandatory for underwriting, such that a submission without them is incomplete? | Completeness detection, the first condition for automatic routing | Underwriting leads | Before design begins | **Default:** no field set is treated as sufficient, completeness detection does not run, and every submission goes to a person |
| OQ-007 | Who is accountable for the business outcome? | Stakeholders, ownership of this document | Unidentified | Before this document is approved | **Escalation:** to whoever approves this document. No default: the requester by implication is not the same as confirmed |
| OQ-008 | What volume of submissions arrives, and how does it vary across the year? | Capacity, sizing | Marcus | Before delivery planning | **Default:** capacity is sized against observed volume once the capability runs, and no seasonal peak is designed for in advance |
| OQ-009 | What period, denominator and method produced the 38% figure? | Evidence | Marcus | Before this document is approved | **Default:** the figure is recorded as reported and unverified |
| OQ-010 | Which system records the response sent to the broker, and can its timestamp be retrieved per submission? | The primary outcome measure | Unidentified - part of the systems discovery | Before design begins | **Default:** the outcome measure is reduced to arrival-to-assignment and the broker-facing figure is recorded as not measurable. Same reduction as OQ-001 |
| OQ-011 | Which confidentiality restrictions exist, where are they recorded today, and how can one be evaluated against a submission? | BR-001/R1, the conditions for automatic routing, the definition of a correct routing outcome | Underwriting leads with the Head of Broker Relations | Before design begins | **Escalation:** to the decision of whether to proceed without an applicable rule. No default: BR-001/R1 cannot be applied by any actor, automated or human, until the restrictions are recorded somewhere both can consult |
| OQ-012 | Which signals constitute a confident duplicate match, and at what threshold? | The conditions for automatic routing, BR-001/R2 | Operations with underwriting leads | Before design begins | **Default:** every match is treated as uncertain, so every repeat goes to the exception queue. Safe, and it reinstates the manual bottleneck for a share of submissions that is unknown |
| OQ-013 | Which states does a submission pass through, what marks acceptance by an underwriter, and which role authorises transfer after it? | BR-001/R4, the boundary between operations rerouting and ownership transfer | Underwriting leads with operations | Before design begins | **Default:** the stricter rule applies throughout - all rerouting requires senior authorisation. Safe, and it removes operations' ability to correct a route |
| OQ-014 | Does a submission bind to the routing rules and authority limits in force when it arrived, or are these re-evaluated at assignment and at transfer? | Behaviour when rules or limits change while submissions are in progress | Underwriting leads | Before design begins | **Default:** rules are evaluated at assignment and re-evaluated at transfer of ownership, not at arrival |

---

## 10. Not chosen

| Option | Why not | ADR |
|---|---|---|
| A tool that reads each submission on arrival, extracts the key data, and routes it to the correct underwriter automatically | Not rejected. Recorded because it is the approach proposed in the request rather than one selected after comparison. The rules it would encode are not yet established - OQ-005 | None |
| Defining a correct routing outcome to include suitability - experience with the property type, familiarity with the broker | Rejected for now in favour of a narrower definition that can be checked today: authority for the sum insured, and no applicable confidentiality restriction. The cost is recorded as an assumption in section 7 | None |
| Treating an unmatched repeat as a new submission | Rejected. The error creates two underwriters on one risk, which BR-001/R3 exists to prevent. Uncertain matches go to an exception queue instead | None |

---

## 11. Downstream artifacts

| Artifact | Identifier | Status |
|---|---|---|
| NFR catalogue | `04-nfr-catalog.md` | Drafted, no values established |
| User story | Not created | |
| Decision record | Not created | Four candidate decisions are recorded as Positions in the catalogue |

---

## Change log

| Date | Change | By |
|---|---|---|
| day 0 | Created from the request in `01-request.md` and one round of answers | analyst |
| day 0 | Revised against `05-review-findings.md`. Six decisions taken by the requester; five open questions added; four business rules recorded | analyst |

---

## What changed, and which finding caused it

| Finding | Class | Change |
|---|---|---|
| 1 | Blocking | The scope no longer claims to measure to the broker-facing response. It records the timestamps this capability can observe and names the dependency on an external source - OQ-010 |
| 2 | Should fix | The manual path is no longer defined by exceptions. Automatic routing now has stated conditions, and anything failing or unevaluable goes to a person |
| 3 | Should fix | A correct routing outcome is defined as authority for the sum insured with no applicable confidentiality restriction. The cost of the narrow definition is recorded as an assumption |
| 4 | Should fix | "Winnable" replaced with business the insurer would have quoted on its existing appetite |
| 5 | Should fix | The extracted data set is now bounded by the conditions for automatic routing rather than by its purpose |
| 6 | Should fix | "Reliably enough" replaced with accuracy sufficient to evaluate the conditions, and the assumption now carries a method for testing it |
| 7 | Should fix | OQ-002 is marked unowned and blocked by OQ-003, with an escalation rather than a false owner |
| 8 | Should fix | "Before review" replaced throughout with process gates that exist in this document, and a note explains why no calendar dates appear |
| 9 | Should fix | Six catalogue entries that were not requirements moved to Positions in `04-nfr-catalog.md`. Section 8 references eleven entries, not seventeen |
| 10 | Should fix | Section 6 records BR-001/R1 with its source and without its content, and OQ-011 carries the only escalation with no defensible default |
| 11 | Consider | The four-hour figure is marked as reported rather than measured, with its missing percentile, period and method stated |
| 12 | Should fix | "Continue to move submissions" replaced with a structural constraint: no step may require a specific named individual |
| 13 | Should fix | Repeat submissions addressed as ordinary. BR-001/R2, a fifth routing condition, an exception queue, and OQ-012 |
| 14 | Should fix | BR-001/R3 and BR-001/R4, a single current owner, a visibility constraint, and OQ-013 for the state model |
| 15 | Consider | OQ-014 records the question of when rules bind, with a default rather than a decision |
| 16 | Should fix | Section 8 now lists all eight categories against catalogue entries, and the catalogue carries `applies_to` back to BR-001 |
