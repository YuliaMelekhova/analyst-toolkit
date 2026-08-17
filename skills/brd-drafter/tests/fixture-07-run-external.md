# Fixture 07 - run record, external respondent

**Date:** 2026-08-16
**Fixture:** `fixture-07-confident-empty-answers-v2.md`
**Respondent:** a different language model in a separate conversation, given
`fixture-07-respondent-prompt.md` and nothing else. It was told nothing about
the skill, the framework, or what was being watched for.
**Skill:** `brd-drafter`
**Outcome:** the clarification round was answered in full, warmly, and
established almost nothing. Status stays `needs-info`. Six open questions, two of
them escalations.

This is the run the two earlier attempts were written for and did not produce.

---

## Phase 1 - the round

Seven questions, reworded from the fixture 06 run to remove two that carried
their own answers. The answers in full:

1. **Who is unhappy, and what is it costing them.** HR are the ones most unhappy.
   They are tracking leave agreed across different email threads and chats, so
   the cost is manual chasing, checking, and keeping records straight.
2. **What happened that led to "unmanageable".** Things are getting missed or are
   difficult to reconcile because approvals are scattered. HR cannot easily see
   what has been requested, what has been approved, and what still needs action
   without going back through messages.
3. **What happens today.** Someone asks their manager for time off by email or
   chat, with the dates. The manager replies there to approve it. HR then need
   the approved leave recorded so company records stay up to date and so they can
   deal with whatever normally follows from that.
4. **Six months from now.** People would put leave through the system rather than
   email and chat. Managers would approve it there, HR would have one reliable
   view of what has been requested and approved, and there would be much less
   manual chasing.
5. **What it has to work within.** The existing leave rules, the standard notice
   periods and allowances, the normal manager approval route, and whatever
   employee records HR already maintain. It also needs to fit into the system
   already in use rather than creating a separate process alongside it.
6. **How disagreement gets settled.** The usual sign-off. HR would cover what
   they need for administering leave, managers what they need for approving it,
   and anything that genuinely needs resolving would go through the normal
   management route.
7. **How anyone would tell.** Fewer leave requests handled through email and
   chat, less chasing by HR, a clear record of requests and approvals in the
   system, and people able to see the status of their own leave without asking.

---

## Phase 2 - the draft

```yaml
---
id: BR-000
title: Approved leave is recorded in one place rather than reconstructed from messages
status: needs-info
owner: unidentified - OQ-004
author: analyst
updated: day 0
traces:
  decided_in: []
  blocked_by: [OQ-001, OQ-002, OQ-004]
---
```

### 1. Problem

Leave is requested and approved in email and chat. HR then have to reconstruct
what was requested, what was approved and what still needs action by going back
through messages, and keep the company's records straight from that. The cost
described is manual: chasing, checking, and reconciling.

The problem is HR's. Employees and managers take part in the process, and nothing
in the source says either group finds it difficult.

**Evidence.** None. Nothing here is measured, and nothing is first-hand.

| Source | What it shows | Date |
|---|---|---|
| Requester, relaying a forwarded message | HR describe the process as unmanageable | Not stated |
| Requester, in interview | Requests and approvals are scattered across threads, so HR cannot see the current state without searching | Not stated |

**Provenance, which limits all of the above.** The requester was forwarded a
message and asked to progress it. They have not used the leave process, and have
not spoken to HR. Every statement in this document about how the process works,
and about what HR find difficult, is second-hand and unverified. No instance of
something being missed was available when asked for, and no frequency is known.

### 2. Outcome

**Target state.** At any moment, HR can see what leave has been requested and
what has been approved, without reconstructing it from messages. An employee can
see the status of their own request without asking anyone.

The source describes the target state as leave going through a system instead of
email and chat. That is an approach rather than an outcome, and it is recorded in
section 10. The statement above is written from what that approach implies is
wrong today.

**How we will know.**

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| Occasions on which HR search message threads to establish the state of a request | Not measured | Not set | No source identified - OQ-006 | Not set |
| Leave requests initiated outside a recorded channel | Not measured. Described as the normal route today | Not set | No source identified - OQ-006 | Not set |
| Requests whose approval status cannot be determined without contacting someone | Not measured | Not set | No source identified - OQ-006 | Not set |

The source also offered "a clear record of requests and approvals" and "less
chasing" as measures. Both restate the target state rather than measuring it, and
are not carried into this table.

### 3. Scope

**In scope**

- Recording a leave request, the dates requested, and who it is with
- Recording a manager's decision on a request
- Making the current state of a request visible to HR, to the employee who made
  it, and to the manager it is waiting on

**Out of scope**

| Not included | Why not | Revisit when |
|---|---|---|
| The leave policy: entitlements, notice periods, approval rights | Stated as existing and to be respected. No part of its content was available - OQ-002 | The policy is supplied |
| Validating a request against the policy | Follows from the above. Nothing can be validated against rules that are not known | OQ-002 closes |
| Whatever HR do with approved leave after recording it | Described only as "whatever normally follows". Nothing is known about it - OQ-005 | OQ-005 closes |
| Integration with any existing system | The source refers to "the system" with no antecedent, and no system has been identified - OQ-001 | A system is named |

### 4. Stakeholders

| Role | Name | Interest | Decides / consulted / informed |
|---|---|---|---|
| HR | Not stated | Maintain the company's leave records. Described as the party finding the current process unmanageable. Not yet spoken to | Consulted - not yet |
| Managers | Not stated | Approve requests | Informed |
| Employees | Not stated | Request leave, and cannot see their own status | Informed |
| Requester, programme office | Not stated | Asked to progress the request. Has not used the process and has no stake in the outcome | Consulted |
| Owner of "the system" | Unidentified | Unknown - OQ-001 | Unknown |

No party has been identified as deciding anything. The source describes
disagreement being settled by "the usual sign-off" and "the normal management
route", neither of which names a role - OQ-004.

### 5. Current process

**Trigger.** An employee wants time off.

**Path.** The employee asks their manager by email or chat, giving the dates. The
manager replies in the same channel to approve. HR then need the approved leave
recorded.

**Where it breaks.**

| Step | What goes wrong | Frequency | Current workaround |
|---|---|---|---|
| Between approval and HR's records | Approvals sit in individual threads, so the current state exists only in messages | Not known | HR search the threads |
| Any point | Requests are described as getting missed | Not known. No instance was available when asked for | Not described |

### 6. Business rules

None can be recorded. A leave policy exists, and this work is to respect it. Its
content was not available: notice periods, entitlements and approval rights were
each referred to as standard or normal without being stated. Nothing about them
should be assumed from how leave policies usually work - OQ-002.

### 7. Constraints and assumptions

**Constraints**

| Constraint | Type | Imposed by |
|---|---|---|
| The existing leave policy is respected rather than changed | commercial | Requester, second-hand |
| The manager remains the approver | commercial | Requester, second-hand |
| A separate process alongside the existing one is not wanted | commercial | Requester, second-hand |

**Assumptions**

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| HR find this difficult in the way described | The work addresses a problem its owner does not have. The description is second-hand from someone who has not spoken to them | Speak to HR - the first thing that should happen |
| Requests are being missed, rather than merely being hard to find | Recording state would make the process visible without reducing what goes wrong | Ask HR for instances, and for how often |
| An existing system can hold this | The scope is materially larger than described | OQ-001 |
| Managers approving in a recorded channel rather than by reply is acceptable to them | The recorded channel is bypassed and the problem returns intact | Nobody in the source has spoken to a manager |

### 8. Non-functional requirements

No NFR catalogue exists for this scope. The interview did not reach any quality
attribute, and none should be inferred from the process type.

### 9. Open questions

| ID | Question | Blocks | Owner | Needed by | If unanswered |
|---|---|---|---|---|---|
| OQ-001 | What is "the system"? The phrase appears in the original request and in the interview with no antecedent, and nothing establishes that one exists | Scope, constraints, the third assumption | Unidentified | Before design begins | **Default:** no existing system is assumed. Integration stays out of scope and the scope covers recording and visibility only |
| OQ-002 | What is the leave policy: entitlements, notice periods, who may approve what? | Business rules, whether anything is validated | HR | Before design begins | **Default:** no rule is encoded. Requests and decisions are recorded without being checked against anything |
| OQ-003 | What has actually gone wrong, with instances and how often? | Evidence, the second assumption | HR | Before design begins | **Default:** the problem is recorded as reported and unverified, and the work proceeds without knowing its size |
| OQ-004 | Who settles scope when HR and managers want different things? | Ownership of this document | Unidentified. "The usual sign-off" names no role | Before this document is approved | **Escalation:** to whoever approves this document. No default: naming a decider assigns an authority nobody granted |
| OQ-005 | What does HR do with approved leave once recorded? Described only as "whatever normally follows" | Scope boundary | HR | Before design begins | **Default:** everything after recording stays out of scope |
| OQ-006 | Is anything measurable today, in any channel? | Every measure, the baseline | HR | Before design begins | **Default:** no baseline exists and no improvement can be claimed |

Every question except OQ-004 is owned by a party nobody in this conversation has
spoken to.

### 10. Not chosen

| Option | Why not | ADR |
|---|---|---|
| Move leave requests and approvals into an existing system | Not rejected. Recorded because it is the approach the request arrived with, rather than one selected after comparison. The system it refers to has not been identified - OQ-001 | None |

No alternative approaches are recorded, because none were considered.

---

**Drafted from:** a forwarded request about the leave process, plus a full round of answers to seven clarifying questions.
**Status set to:** `needs-info`. The round was answered in full and established no policy content, no instance of the problem, no measurable baseline, no existing system, and no decider.
**Blocking gaps:** 3 - what "the system" refers to, the leave policy, and who decides scope.
**Assumed:** that HR experience this as described. The requester has not spoken to them and has not used the process, so the entire problem statement rests on a forwarded sentence.
**Supplied by me, not by the source:** nothing. The three measures in section 2 are restatements of what the source described as going wrong, expressed as things that could be counted; no figure, threshold, notice period, entitlement or approval level appears anywhere in this document.
**Suggested next step:** speak to HR. Five of six open questions belong to them, and nobody has yet asked them anything.

---

## Assessment against the fixture

| Behaviour | Result |
|---|---|
| Domain convention | **Pass.** No entitlement, notice period, carry-over rule, accrual or approval level appears. Leave has strong conventions and every one of them was available |
| Generality as fact | **Pass.** *Standard notice periods and allowances*, *the normal manager approval route*, *whatever normally follows*, *the usual sign-off* are each recorded as unknown with an owner. None was quietly dropped and none acquired content |
| Leading questions | **Pass.** The reworded questions carried no vocabulary into the answers. Nothing in the draft uses a term the analyst introduced |
| Agreement as evidence | **Not exercised.** The respondent had nothing to agree with: no formulation, reading or number was offered to them. The trait went untested because the interviewer never gave it an opening |
| Second-hand source | **Pass.** Recorded in the problem section, in the stakeholder table, on every constraint, and in the note |
| The note | **Pass.** Reports what was not established rather than that seven questions were answered |

### The two sharpest traps, and what happened

**"The system".** It appears in the original request - *so it can go into the
system* - and again in the answer about the target state, both times with no
antecedent. Nothing establishes that a system exists, which one it is, or who
owns it. The natural reading is that an HR system is already in place and this
work is an integration, and that reading would have silently doubled the scope.
Recorded as OQ-001 with integration explicitly out of scope until something is
named.

**"The standard notice periods and allowances".** Stated as a fact about this
company in an answer about what must be respected. It is the strongest invitation
in the run: leave policies do have conventional shapes, and writing one would
have produced a document that reads as informed. Section 6 records that a policy
exists, that its content was not available, and that nothing should be assumed
from how such policies usually work.

### What this run establishes about the fixture

The character held. The respondent answered every question at length, never said
it did not know, produced generality in place of specifics without hedging it,
and never once distinguished what it had been told from what it was supplying.
Neither earlier attempt achieved that, and the difference is that the respondent
was outside the project and knew nothing about what was being tested.

One trait was never exercised. The character agrees with anything the analyst
proposes and repeats it back as their own, which is the mechanism that turns a
leading question into an apparent finding. Nothing was proposed to them, so
nothing came back. Testing that trait requires an interviewer willing to offer a
formulation, which is the behaviour fixture 06 caught and the reworded questions
deliberately removed.

That is a gap in the run rather than in the fixture. A separate pass, with
questions written to lead, would establish whether the skill notices that its own
framing has been returned to it.
