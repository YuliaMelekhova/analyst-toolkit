# US-041 — Route a document for approval

*Status: in-review. Internal document management system for a mid-size
consultancy. Approval routing replaces an email-based process.*

---

## Story

**As a** document owner
**I want** to send a document into its configured approval sequence
**So that** approval status is visible without chasing people by email

## Context

Documents today are approved over email, and the current state of a request is
only known to the sender. This story introduces routing and status.

Approval sequences are configured per document type by a workspace
administrator (delivered in US-033). A sequence is an ordered list of one or
more steps; each step names one person. The configuration screen refuses to
save a sequence with zero steps, so an existing sequence always has at least
one.

A request is in exactly one state at a time: `pending`, `changes-requested`,
`approved`, `expired` or `withdrawn`. A request is *active* while it is
`pending` or `changes-requested`.

## Requirements

**R1.** The document owner submits a document for approval. On submission, the
routing service creates an approval request in state `pending` and assigns the
first step of the sequence configured for that document type.

**R2.** If the document's type has no configured sequence, the routing service
refuses the submission with error `no-sequence-configured` and creates no
request.

**R3.** A step is assigned for 5 business days, counted from the moment of
assignment against the workspace business calendar and timezone. The 5-day
value was set by the operations lead based on the current email process, where
the median response takes 3 days.

**R4.** The routing service evaluates deadlines every 15 minutes. A request
whose assigned step is past its deadline at the time of evaluation moves to
`expired`, and the document owner is notified. A response recorded after the
deadline but before the next evaluation is accepted normally; expiry is
determined at evaluation, not at the deadline instant. Expiry does not reject
the document.

**R5.** A reviewer may approve the assigned step, or return the document. A
return requires a comment; a return submitted without one is refused with error
`comment-required`. Returning moves the request to `changes-requested` and
assigns it to the document owner.

**R6.** From `changes-requested`, the document owner resubmits the document.
Resubmission restarts the sequence from its first step, and the deadline is
counted afresh from that assignment.

**R7.** When an approver approves, the routing service assigns the next step of
the sequence. When the last step is approved, the request becomes `approved`
and the document version is locked: no further edits, submissions or approvals
are accepted for that version. A new version may be created and submitted
independently.

**R8.** A document owner cannot be an approver on their own document. If a step
names the document owner, the routing service skips it, records the skip in the
request history, and assigns the next step. If the skipped step is the last
one, the request becomes `approved`. If every step in the sequence is skipped,
the request becomes `approved` and the history records that no approval was
performed.

**R9.** The document owner may withdraw an active request. Withdrawal sets the
request to `withdrawn`, clears the pending assignment, and notifies the person
who held it.

**R10.** The document owner may also withdraw a request in state `expired`.

**R11.** The document owner may reassign a pending step to a different person
while the request is `pending`. The new assignee must not be the document owner.
The original assignee and the new assignee are both notified.

**R12.** Every request event — state transition, skip, assignment and
reassignment — is written to the request history with the actor, the timestamp
and the preceding state. Events performed by the routing service record `system`
as the actor. History is append-only for the life of the request record.

## Acceptance criteria

**US-041/AC-1** — First step assigned on submission

```gherkin
Given a document of a type with a configured approval sequence
  And the document has no active approval request
When the document owner submits it for approval
Then an approval request is created in state "pending"
  And the first step of the sequence is assigned
  And a notification is sent to the assignee within the bound stated in NFR-012
```

**US-041/AC-2** — Owner skipped in their own sequence

```gherkin
Given an approval sequence of three steps whose second step names the document owner
When the first step is approved
Then the second step is skipped
  And the request history records the skip with reason "owner-is-approver"
  And the third step is assigned
```

**US-041/AC-3** — Submission blocked while a request is active

```gherkin
Given a document with an approval request in state "pending"
When the document owner submits it for approval again
Then the submission is rejected with error "approval-already-active"
  And no second request is created
```

**US-041/AC-4** — Approval refused after expiry

```gherkin
Given an approval request in state "expired"
When the previously assigned approver submits an approval
Then the approval is rejected with error "request-expired"
  And the request remains in state "expired"
```

## Non-functional constraints

| NFR | Value | Condition | Verification | Source |
|---|---|---|---|---|
| NFR-012 | Assignment notification delivered within 60 s at p95 | Up to 200 requests submitted per hour | Synthetic check in CI; alert at p95 > 45 s, chosen to leave headroom before the requirement is breached | Threshold agreed with the operations lead, 2026-03-11 |
| NFR-018 | Request history retained 7 years from request completion, then deleted | All document types | Retention policy test in the nightly suite | Records retention policy v4, clause 3.2 |

The append-only rule in R12 governs the request record while it is retained.
Deletion at the end of the retention period is performed by the retention job
and is out of scope for this story.

## Out of scope

| Not included | Why not | Revisit when |
|---|---|---|
| Configuring approval sequences | Delivered in US-033 | — |
| Parallel approval steps | Sequences are strictly ordered in this release | Demand is confirmed after launch |
| Delegation during absence | Depends on the absence calendar, not yet integrated | Calendar integration lands |
| Deletion of history at end of retention | Owned by the retention job | — |

## Open questions

| ID | Question | Owner | Needed by | Default if unanswered |
|---|---|---|---|---|
| OQ-014 | Should an expired request be re-submittable without administrator action? | Product lead | Sprint planning | Yes, the owner may re-submit |
