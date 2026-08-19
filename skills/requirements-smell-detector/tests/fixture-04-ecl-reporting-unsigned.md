# BR-112 - Expected credit loss reporting for the quarterly close

---

## 1. Problem

Expected credit loss figures for the quarterly close are assembled by hand. Data
is extracted from three systems into a spreadsheet held by the financial
reporting team, model outputs are pasted in, adjustments are applied, and the
result is copied into the reporting pack. The spreadsheet has no version
control, and the person who maintains it is the only person who understands it.

The close is compressed. Preparation cannot begin until the credit risk data is
final, and the reporting deadline does not move.

| Source | What it shows | Date |
|---|---|---|
| Close retrospective | The close is under pressure | 2026-04-18 |
| Group Finance feedback | Concerns raised about the manual process | 2026-05-02 |
| Internal audit observation | Controls over the spreadsheet are informal | 2026-03-11 |

## 2. Outcome

A controlled and repeatable process for producing the expected credit loss
figures, with the steps recorded and the inputs traceable.

| Measure | Current | Target | Measured how | By when |
|---|---|---|---|---|
| Time to produce the pack | Not currently measured | Improved | Via the new process | At go-live |
| Manual steps in the process | Not currently measured | Reduced | Process review | At go-live |
| Audit observations on the process | 1 open | None open | Audit follow-up | Next audit cycle |

## 3. Scope

**In scope.** Calculation of expected credit loss for the in-scope portfolios at
each period end, application of post-model adjustments, assembly of the
reporting pack, and submission to Group Finance.

**Out of scope.**

| Not included | Why not | Revisit when |
|---|---|---|
| Changes to the credit risk models themselves | Owned by Credit Risk | Model governance cycle |
| Retail portfolio | Phase 2 | Phase 2 is scheduled |
| Reconciliation to the general ledger | See known limitations | - |
| Regulatory reporting | Separate programme | - |

## 4. Definitions

**Period end.** The last business day of the quarter, as fixed by the Close
Calendar 2026.

**Reporting pack.** The set of expected credit loss figures, stage distribution
and movement analysis submitted to Group Finance for a period.

**Significant increase in credit risk.** An increase in credit risk since
initial recognition that is significant.

**Stage.** The classification of an exposure as stage 1, stage 2 or stage 3 in
accordance with the staging rules.

**Material.** As set out in the Group Materiality Standard, clause 3.1, which
requires an item to be treated as material where its omission would be material
to the reader.

## 5. Stakeholders

| Role | Name | Interest in the outcome | Decides / consulted / informed |
|---|---|---|---|
| Head of Financial Reporting | I. Vasnetsova | Owns the close | Decides |
| Group Financial Controller | M. Ortega | Receives the pack | Decides |
| Head of Credit Risk | P. Lindqvist | Owns the models and the input data | Consulted |
| Internal Audit | S. Abara | Raised the observation | Informed |

## 6. Business rules

| Rule | Statement | Source of authority |
|---|---|---|
| BR-112/R1 | The expected credit loss calculation is performed for all in-scope portfolios at each period end. | Group Accounting Policy 12.4 |
| BR-112/R2 | Each exposure is assigned to a stage in accordance with the staging rules. | Group Accounting Policy 12.4 |
| BR-112/R3 | A significant increase in credit risk since initial recognition results in migration to stage 2. | IFRS 9 |
| BR-112/R4 | The expected credit loss figures are reviewed and approved in line with the governance framework before the pack is released. | Governance Framework |
| BR-112/R5 | Model inputs are sourced from the group data warehouse. | Data Strategy 2025 |
| BR-112/R6 | Post-model adjustments are documented and approved before they are applied. | Group Accounting Policy 12.4 |
| BR-112/R7 | The reporting pack is submitted to Group Finance by the reporting deadline, which the Close Calendar 2026 fixes as the fifth business day after period end. | Close Calendar 2026 |
| BR-112/R8 | Where a required input is missing at the point of calculation, the calculation is not run for the affected portfolio, an entry is written to the exceptions log recording the portfolio, the missing input and the timestamp, and Group Finance is notified. | Group Accounting Policy 12.4 |
| BR-112/R9 | Results are reconciled to the general ledger. | Group Accounting Policy 12.4 |
| BR-112/R10 | Disclosures are prepared in accordance with the applicable standards. | IFRS 7, IFRS 9 |

## 7. Constraints and assumptions

| Constraint | Type | Imposed by |
|---|---|---|
| The reporting deadline cannot move | temporal | Group Finance |
| Model changes follow the model governance cycle | technical | Credit Risk |
| The pack must be produced within the approved cost envelope | commercial | Finance Transformation |

| Assumption | If it turns out false | How we would find out |
|---|---|---|
| The group data warehouse holds all inputs required by the models | Scope would need to be revisited | During implementation |
| Credit risk data is final before preparation begins | The timetable would need to be revisited | During testing |
| Current adjustment practice is compliant | Would be addressed as it arises | Through review |

## 8. Non-functional constraints

| NFR | Value | Condition | Verification | Source |
|---|---|---|---|---|
| NFR-041 | Reporting pack produced within 5 working days of period end | Under normal load | Monitored in production | Agreed at the Q2 planning workshop |
| NFR-047 | Recalculations complete in good time for review | All in-scope portfolios | Reviewed during UAT | Group Finance |
| NFR-052 | Access to the pack restricted to authorised users | All environments | Access review | Information Security |

## 9. Known limitations

Recorded here rather than left implicit.

- The staging rules have not been agreed in their final form. The process will
  follow the rules current at the time of each close.
- Reconciliation to the general ledger is not part of this scope. The reconciling
  items are understood by the team.
- The retail portfolio is not covered. Phase 2 will address it.
- No formal threshold has been set for post-model adjustments requiring senior
  approval. Existing practice will continue.

## 10. Acceptance criteria

**BR-112/AC-1** - The pack is produced

```gherkin
Given the credit risk data for the period is available
When the close process is run
Then the reporting pack is produced completely and accurately
```

**BR-112/AC-2** - Adjustments are controlled

```gherkin
Given a post-model adjustment has been prepared
When the adjustment is applied
Then it has been documented and approved appropriately
```

**BR-112/AC-3** - Missing input handled

```gherkin
Given a required input is missing for one portfolio
When the calculation is run
Then no figures are produced for that portfolio
  And an entry appears in the exceptions log naming the portfolio, the missing input and the timestamp
```

## 11. Open questions

| ID | Question | Blocks | Owner | Needed by | If unanswered |
|---|---|---|---|---|---|
| OQ-031 | Which portfolios are in scope for phase 1? | R1 | Credit Risk | Design | As per current practice |
| OQ-032 | What threshold requires senior approval of a post-model adjustment? | R6 | Financial Reporting | Build | To be discussed |
| OQ-033 | Who signs off the pack when the Group Financial Controller is unavailable? | R4 | Group Finance | Go-live | Team to decide |
| OQ-034 | Is the current adjustment practice compliant? | R6 | Financial Reporting | Before go-live | Existing practice continues |

## 12. Not chosen

| Option | Why not | ADR |
|---|---|---|
| Extend the existing spreadsheet with version control | Does not address the control observation | - |
| Buy a vendor ECL platform | Cost envelope | - |
