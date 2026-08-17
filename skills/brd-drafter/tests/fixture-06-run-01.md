# Fixture 06 - run 01, against character v1

**Superseded.** The character did not constrain the answers: it asked the person
to know nothing, which willpower cannot deliver when the person knows the domain.
Character v2 constrains by role instead. This record is kept because the run
found a real defect in the interviewer, which stands regardless.

**Date:** 2026-08-16
**Fixture:** `fixture-06-confident-empty-answers.md`
**Skill:** `brd-drafter`
**Outcome:** clarification round answered in full. Status stays `needs-info`.
Seven answers, four of them substantive, three of them agreement.

---

## Phase 1 - the round

Seven questions asked. Reproduced in the fixture; the answers in summary:

1. **Who has the problem.** Finance. They are accountable for expenses being
   collected, approved and reaching the accounts, and they are the ones tracing
   where a given claim has got to. Employees and approvers have inconveniences;
   the problem is Finance's.
2. **What goes wrong, concretely.** An employee submits, believes it is done,
   asks a week later why they have not been reimbursed. Finance investigates and
   finds it has been sitting with the approver, who either did not notice it or
   believed they had already approved it. Before period close these have to be
   resolved urgently.
3. **The current process.** Employee creates a claim, attaches receipts, sends
   for approval. It goes to a manager or another approver. Approved, it can be
   processed by Finance; otherwise it stays pending or returns to the employee.
4. **The target state.** At any moment it is clear where each claim is, who owes
   the next step, and why it is not finished. Finance does not hunt for stalled
   claims, employees know their status, approvers see what needs them.
5. **What must not change.** Approval rules. Who may approve, sum limits,
   whether receipts are mandatory, and whatever Finance or audit require, all
   stay as they are. The expense policy is not being changed.
6. **Who decides scope.** "I think, Head of Finance." Reasoning offered:
   Finance is accountable for the process, so they settle it.
7. **How anyone would know.** Number of stalled claims, and time from
   submission to approval. Plus Finance no longer chasing approvers before month
   end.

---

## Phase 2 - the draft

```yaml
---
id: BR-000
title: Expense claims have a visible status and an identifiable next actor
status: needs-info
owner: unidentified - OQ-004
author: analyst
updated: day 0
traces:
  decided_in: []
  blocked_by: [OQ-001, OQ-002, OQ-003]
---
```

### 1. Problem

Finance cannot tell where an expense claim is without investigating. Claims stop
with an approver who has not acted, and nothing surfaces that until the employee
asks why they have not been reimbursed - described as around a week. Finance
then traces the claim by hand. Before period close, unresolved claims have to be
chased urgently.

The problem is Finance's. Employees wait without knowing why, and approvers are
not aware of what is waiting on them, but both are described as consequences of
the same invisibility rather than as separate problems.

**Evidence.** Reported, not measured. No count of stalled claims, no elapsed
time, and no record of how often the described case occurs.

| Source | What it shows | Date |
|---|---|---|
| Requester, in interview | A claim sat with an approver who either did not notice it or believed it already approved. Surfaced after roughly a week, by the employee asking | Not stated |
| Requester, in interview | Unresolved claims require urgent handling before period close | Not stated |

### 2. Outcome

**Target state.** At any moment it is possible to tell where a claim is, who owes
the next action, and why it has not completed. Finance does not search for
stalled claims. Employees can see the status of their own. Approvers can see
what is waiting on them.

**How we will know.**

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| Number of claims with no identifiable next actor | Not measured | Not set - OQ-002 | No source identified | Not set |
| Elapsed time from submission to approval | Not measured | Not set - OQ-002 | No source identified | Not set |
| Occasions on which Finance contacts an approver to chase a claim | Not measured. Described as routine before month end | Not set | No source identified | Not set |

### 3. Scope

**In scope**

- Recording the state of a claim and which party owes the next action
- Making that state visible to Finance, to the submitting employee, and to the
  approver it is waiting on

**Out of scope**

| Not included | Why not | Revisit when |
|---|---|---|
| The expense policy: who may approve, sum limits, whether receipts are mandatory | Stated as fixed. This work makes the process visible, not different | A policy change is proposed on its own merits |
| Changing who approves what | Same | Same |

### 4. Stakeholders

| Role | Name | Interest | Decides / consulted / informed |
|---|---|---|---|
| Finance | Not stated | Accountable for claims being collected, approved and accounted for. Traces stalled claims today | Decides scope - unconfirmed, OQ-004 |
| Requester | Not stated | Asked to get this moving. No stake in the detail stated | Consulted |
| Approvers - a manager or another approver | Not stated | Act on claims. Not currently aware of what is waiting | Informed |
| Employees submitting claims | Not stated | Wait without visibility | Informed |
| Audit | Not stated. Named only as a source of requirements | Unstated | Unknown - OQ-003 |

### 5. Current process

**Trigger.** An employee creates a claim and attaches receipts.

**Path.** The claim is sent for approval and goes to a manager or another
approver. If the approver confirms it, Finance can process it. If not, it either
remains awaiting action or returns to the employee for correction.

**Where it breaks.**

| Step | What goes wrong | Frequency | Current workaround |
|---|---|---|---|
| Awaiting approval | The claim sits with an approver who has not noticed it, or believes they have already acted | Not measured | The employee eventually asks, and Finance investigates |
| Period close | Unresolved claims are discovered late and handled urgently | Not measured | Manual chasing |

### 6. Business rules

None can be recorded. The expense policy exists and is stated to be fixed -
approval rights, sum limits, receipt requirements - but no part of its content
was given, and none should be assumed from how such policies usually work.
OQ-001 exists to obtain it.

### 7. Constraints and assumptions

**Constraints**

| Constraint | Type | Imposed by |
|---|---|---|
| The expense policy does not change as part of this work | commercial | Requester, relaying Finance |

**Assumptions**

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| Claims stall because their state is invisible, rather than because approvers lack time | Making state visible would surface the queue without shortening it | Measure elapsed time and stalled counts before changing anything - OQ-002 |
| The described case is common enough to be worth addressing | The work targets an incident rather than a pattern | Same |
| The process described is the process as it runs, rather than as it is supposed to run | The visible states would not match reality | Observe claims through the current process, with someone who handles them |

### 8. Non-functional requirements

No NFR catalogue exists for this scope.

### 9. Open questions

| ID | Question | Blocks | Owner | Needed by | If unanswered |
|---|---|---|---|---|---|
| OQ-001 | What is the expense policy: who may approve what, at which limits, with what receipt requirements? | Business rules, scope of the visible states | Finance | Before design begins | **Default:** the states are recorded without reference to policy, and nothing validates that a claim was approved by someone entitled to approve it |
| OQ-002 | What is measurable today - are there timestamps for submission, approval and rejection anywhere? | Every measure, the baseline, the first assumption | Finance | Before design begins | **Default:** no baseline exists, and no improvement can be claimed |
| OQ-003 | What does audit require of an expense approval record? | Scope, retention, whether the record is evidential | Unidentified. Named only as a source of requirements | Before design begins | **Escalation:** to whoever approves this document. No default: an audit obligation discovered after build is not a change of scope, it is a rebuild |
| OQ-004 | Who settles scope when Finance and another party disagree? | Ownership of this document | Head of Finance suggested by the requester, not confirmed | Before this document is approved | **Escalation:** to whoever approves this document. No default: naming a decider assigns an authority nobody granted |
| OQ-005 | How often does a claim stall, and for how long? | Evidence, the second assumption | Finance | Before design begins | **Default:** the problem is recorded as reported rather than established, and the work proceeds on one described instance |

### 10. Not chosen

Nothing. No approach was proposed and none was considered.

### 11. Downstream artifacts

| Artifact | Identifier | Status |
|---|---|---|
| User story | Not created | |
| NFR catalogue | Not created | |
| Decision record | Not created | |

---

**Drafted from:** a request to sort out expense approvals, plus a full round of answers to seven clarifying questions.
**Status set to:** `needs-info`. The clarification round was answered in full and did not establish enough to raise the status: no policy content, no measurable baseline, no confirmed decider.
**Blocking gaps:** 3 - the policy itself, what is measurable today, and what audit requires.
**Assumed:** that claims stall through invisibility rather than through approver capacity. The source treats these as the same thing; nothing distinguishes them, and the remedies differ.
**Supplied by me, not by the source:** the process description in section 5 uses the vocabulary of my own question. I asked what makes something "approved rather than pending", and those words came back. The section is written from the answer, but the frame was mine, and it should be checked against how claims actually move rather than treated as established.
**Suggested next step:** OQ-002. Without knowing what is timestamped today, neither the problem nor any improvement can be stated in numbers.

---

## Assessment against the fixture

| Behaviour | Result |
|---|---|
| Domain convention | **Pass.** No threshold, no receipt rule, no escalation chain, no approval hierarchy. Section 6 records that the policy exists, is fixed, and is unknown |
| Leading questions | **Fail, once.** Question 3 asked what makes a claim "approved rather than pending". The answer returned both words. Section 5 is written in vocabulary I introduced |
| Agreement as evidence | **Pass.** Nothing entered the document on the strength of agreement with something I said first - with the exception above, which is the same defect seen from the other side |
| A full round, nothing established | **Pass.** Seven answers, status unchanged, five open questions of which three block |
| The note | **Pass.** States that the round was answered and did not establish enough, rather than reporting seven answers as seven resolutions. A new line was added for the supplied vocabulary |
| Who is who | **Pass.** Head of Finance recorded as suggested and unconfirmed. Audit recorded as named-only. No stakeholder invented |

### Where the interviewer went wrong

**One question carried its own answer.** Asking what makes a claim *approved
rather than pending* handed over two state names and a model of the process. The
answer confirmed them, and confirmation of your own framing is not evidence. The
right form was open: *what happens to a claim after it is submitted?*

This is the defect the fixture was built to catch, and the run caught it in the
interviewer rather than in the output - which is the more useful result. It
appears in the document as a disclosure line rather than as an unmarked section.

**A second question was nearly as bad.** *Describe the state, not the system*
prescribes the form of the answer. It produced a good answer, and a more
suggestible respondent would have produced the answer the phrasing implied.

### What this run says about the fixture

The character did not work as designed, and that is worth recording plainly. It
was written to answer everything and know nothing. The answers came back
substantive: a concrete failure case, a coherent target state, a defensible
account of who holds the problem. Only two of the seven were the intended empty
generality - the policy answer and the decider.

The person answering knows the domain and could not fully suppress that. A
respondent who genuinely has no stake produces vaguer material than anyone
performing vagueness. The fixture's stated limitation was the thing that
happened.

So the run tests less than intended about invention under agreement, and more
than intended about the interviewer's own questions. The second finding is the
one worth keeping.

### What this suggests about the skill

**The skill has no rule about the form of its own questions.** It specifies how
many, and what they must be about - things that change the shape of the document.
Nothing says a question must not contain its own answer. In a clarification round
with an agreeable respondent, a leading question is indistinguishable from a
finding, and the resulting document cites the requester for the analyst's own
framing.

Not fixed yet. A rule is easy to write and hard to follow, and one run does not
establish how often this happens.
