# Subscription plan change — requirements draft

*Draft prepared for review. Subscription billing for a B2B SaaS product with
monthly and annual plans.*

---

## Background

Customers currently contact support to change their plan, which takes up to two
business days. We want to let account holders do this themselves.

## Requirements

**R1.** The system should allow users to easily change their subscription plan
at any time.

**R2.** When a plan change is requested, the subscription is updated and the
customer is notified.

**R3.** All upgrades take effect immediately. Downgrades take effect at the end
of the current billing period.

**R4.** The plan comparison page must load in under 2 seconds.

**R5.** Add a dropdown to the account settings page so the user can select their
new plan.

**R6.** The system must handle payment failures gracefully.

**R7.** Customers on annual plans cannot switch to monthly billing mid-term.

**R8.** Proration is calculated based on the remaining days in the billing
period and applied to the next invoice.

**R9.** The account holder receives a confirmation email after the change is
processed, and the change is recorded in the audit log.

**R10.** Only users with the billing role may change the plan. Users without
this role see the plan comparison page in read-only mode.

**R11.** Where appropriate, the system should retry failed operations.

**R12.** Since every customer has a payment method on file, no additional
payment collection step is needed during an upgrade.

## Acceptance criteria

- The user can change their plan.
- The correct amount is charged.
- The system is fast and reliable.
