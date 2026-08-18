# US-078 - Refer a claim to the special investigations unit

*Status: in-review. Motor claims platform for a national insurer. Referral
replaces an informal process in which handlers emailed the investigations unit
directly and kept their own spreadsheets.*

---

## Story

**As a** claims handler
**I want** claims carrying fraud indicators to reach the investigations unit
through the platform
**So that** the unit's work is visible on the claim and payment is not released
while a concern is open

## Context

Fraud indicators are evaluated by the indicator service, delivered in US-061,
which returns a set of named indicators and a score for a claim version. This
story covers what happens to a claim once that output exists: how a referral is
created, how the investigations unit works it, and how the claim returns to the
normal process.

The investigations unit is a separate team of investigators with a unit
manager. Investigators do not decide claims. Claims are decided by the claims
manager.

## Definitions

**Referral.** The record created when a claim is sent to the investigations
unit. A claim version has at most one open referral.

**Material inconsistency.** A difference between two statements of fact in the
claim record that would change either the amount payable or the decision to
pay. A difference that changes neither is not material.

**Working day.** A day on the workspace business calendar, excluding weekends
and the published holiday list for the insurer's country of operation.

## Requirements

### Indicator evaluation and referral

**R1.** The system automatically reviews each claim for suspicious
characteristics and refers it to the investigations unit where appropriate.

**R2.** A claim scoring above 70 is referred to the investigations unit. A claim
scoring 70 or below continues without a referral.

**R3.** When a claim is referred, the investigations unit is notified and the
claim is put on hold.

**R4.** When a referral is created, the platform places it on the
investigations unit's queue.

**R5.** Since every claim has a completed indicator profile by the time it is
registered, the score is always available at first notification of loss.

**R6.** High-value claims are always reviewed by the unit.

**R7.** The handler adds a note explaining why the case was raised. The note is
stored with the investigation and is visible to the unit.

**R8.** Referrals are processed quickly.

**R9.** A daily CSV export of open referrals is placed on the shared drive so
the unit can see what is waiting.

### Handling by the investigations unit

**R10.** An investigator takes a referral from the unit queue. Taking a
referral sets it to `under-investigation`, records that investigator as its
owner, and writes the transition to the referral history with the actor and the
timestamp. A referral that already has an owner cannot be taken by a second
investigator; the attempt is refused with error `already-owned`.

**R11.** An investigator records exactly one outcome per referral, from
`no-concern`, `substantiated` or `inconclusive`. Every outcome requires a
written rationale; an outcome submitted without one is refused with error
`rationale-required`. A rationale of `substantiated` states the material
inconsistency the outcome rests on.

**R12.** A referral with no recorded activity for 10 working days is escalated
to the unit manager, who is notified and becomes accountable for its outcome
until it is reassigned. Inactivity is evaluated once per working day, and a
referral past the limit at the time of evaluation is escalated then; escalation
is determined at evaluation and not at the instant the limit passes.

**R13.** The unit manager may reassign a referral to a different investigator
while it is `under-investigation`. The previous owner and the new owner are
both notified, and the reassignment is written to the referral history.

**R14.** While a referral is `under-investigation`, payment on the claim is
held. A payment instruction submitted against a held claim is refused with
error `payment-held`, and no partial payment is made.

**R15.** An outcome of `substantiated` does not decline the claim. A decline is
decided by the claims manager on the investigator's recorded outcome and the
claim record, under the standard set out in the claims policy clause 4.2,
"reasonable grounds to suspect", which the clause defines as grounds a second
claims manager reading the same record would accept. The decline is stored as
its own decision with its author and date, separately from the referral.

### Closing a referral and returning the claim

**R16.** When an outcome is recorded, the referral moves to `closed` and the
payment hold is released, unless the claims manager has declined the claim
under R15.

**R17.** On closure, the claims handler named on the claim is notified with the
outcome and the rationale.

**R18.** The adjuster may reopen a closed referral within the reopening window
set by the claims policy clause 6.1. Reopening returns the referral to `under-investigation`, reinstates the payment
hold, and assigns the referral to the investigator who recorded the outcome.

**R19.** The case owner receives a monthly summary of the referrals raised on
their claims and the outcome of each.

**R20.** Referral history is append-only for the life of the referral record.
Every state transition, assignment, reassignment and escalation is written to
it with the actor, the timestamp and the preceding state. Transitions performed
by the platform rather than by a person record `system` as the actor.

## Acceptance criteria

**US-078/AC-1** - Payment refused while a referral is open

```gherkin
Given a claim with a referral in state "under-investigation"
When a payment instruction is submitted against that claim
Then the instruction is refused with error "payment-held"
  And no payment record is created
```

**US-078/AC-2** - Outcome requires a rationale

```gherkin
Given a referral in state "under-investigation" owned by the acting investigator
When the investigator submits the outcome "substantiated" with an empty rationale
Then the outcome is refused with error "rationale-required"
  And the referral remains in state "under-investigation"
```

**US-078/AC-3** - Escalation on inactivity

```gherkin
Given a referral in state "under-investigation" with no recorded activity for 10 working days
When the escalation check runs
Then the unit manager is notified
  And the escalation is written to the referral history with actor "system"
```

**US-078/AC-4** - A substantiated outcome does not decline the claim

```gherkin
Given a referral closed with outcome "substantiated"
  And no decline decision recorded by the claims manager
When the claim is examined
Then the claim is not in state "declined"
  And the payment hold is released
```

## Non-functional constraints

| NFR | Value | Condition | Verification | Source |
|---|---|---|---|---|
| NFR-031 | Referral created and visible on the unit queue within 60 s at p95 | Up to 400 claims registered per hour | Synthetic check in CI; alert at p95 above 45 s, chosen to leave headroom before the requirement is breached | Threshold agreed with the SIU manager, 2026-05-12 |
| NFR-034 | Referral history retained 7 years from closure, then deleted | All claim types | Retention test in the nightly suite | Records retention policy v4, clause 3.2 |

The append-only rule in R20 governs the referral record while it is retained.
Deletion at the end of the retention period is performed by the retention job
and is out of scope for this story.

## Out of scope

| Not included | Why not | Revisit when |
|---|---|---|
| Evaluating indicators and producing the score | Delivered in US-061 | - |
| Deciding or declining the claim | Owned by the claims decision process | - |
| Reporting fraud to external bodies | Handled outside the platform by the SIU manager | The regulator's reporting interface is specified |
| Deletion of history at end of retention | Owned by the retention job | - |

## Open questions

| ID | Question | Owner | Needed by | If unanswered |
|---|---|---|---|---|
| OQ-021 | May a claims handler request a referral on a claim the indicator service did not flag? | SIU manager | Sprint planning | Default: yes, and the request is recorded with the handler as its source |
| OQ-022 | Does reopening a referral restart the retention period in NFR-034? | Records manager | Before the retention test is written | Escalate to the records manager at the point the retention test is written. No default: the answer changes what is deleted |
