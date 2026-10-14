# Contents

An index of what is in this repository: every file, in one line each. It lists
files and says what they are. It does not say what is finished, tested or open.
That lives in [`NOTES.md`](NOTES.md), under "Current open items", and what a
directory is for is explained in its own README.

Tree as fetched on 2026-10-14. When a file is added, renamed or removed, this
file is updated in the same commit. Nothing checks that yet.

---

## Root

| File | What it is |
|---|---|
| [`README.md`](README.md) | Overview: the problem, how the parts fit together, quick start, what this is not, design principles, roadmap |
| [`CONTENTS.md`](CONTENTS.md) | This index |
| [`NOTES.md`](NOTES.md) | Log of what went wrong while building. Current open items at the top, then the first-phase notes per skill, then dated entries |
| [`LICENSE`](LICENSE) | MIT. The README notes the templates can be treated as CC BY 4.0 |
| [`.gitignore`](.gitignore) | Ignore rules |

---

## `framework/`

Templates, conventions and quality rules. Plain Markdown, referenced by the
skills rather than copied into them.

| File | What it is |
|---|---|
| [`framework/README.md`](framework/README.md) | Contents, how the artifacts relate, working order, status lifecycle, design principles, what to adapt |
| [`framework/conventions/naming-and-ids.md`](framework/conventions/naming-and-ids.md) | Identifier scheme and link types |
| [`framework/templates/brd.md`](framework/templates/brd.md) | Business requirement template |
| [`framework/templates/user-story.md`](framework/templates/user-story.md) | User story with acceptance criteria |
| [`framework/templates/nfr-catalog.md`](framework/templates/nfr-catalog.md) | Non-functional requirements catalogue, eight categories, with a Positions section |
| [`framework/templates/adr.md`](framework/templates/adr.md) | Analysis decision record |
| [`framework/quality-rules/definition-of-ready.md`](framework/quality-rules/definition-of-ready.md) | Entry gate for delivery |
| [`framework/quality-rules/review-checklist.md`](framework/quality-rules/review-checklist.md) | Second-reader checklist for any artifact before it is agreed |

---

## `skills/`

Three agent skills, each with its test material.

| File | What it is |
|---|---|
| [`skills/README.md`](skills/README.md) | How to load and use a skill: three ways, packaging with the framework files bundled, usage guidance, troubleshooting |

### `skills/brd-drafter/`

Turns an informal request into a BRD draft and flags gaps instead of filling them.

| File | What it is |
|---|---|
| [`skills/brd-drafter/SKILL.md`](skills/brd-drafter/SKILL.md) | The skill |
| [`skills/brd-drafter/tests/README.md`](skills/brd-drafter/tests/README.md) | Why fixtures are snapshots, not examples; why the answer key is read after a run |
| [`skills/brd-drafter/tests/fixtures.md`](skills/brd-drafter/tests/fixtures.md) | Five request inputs: 01 thin request, 02 solution not problem, 03 rich source with an undecided integration, 04 contradictory source, 05 non-English source |
| [`skills/brd-drafter/tests/answer-key.md`](skills/brd-drafter/tests/answer-key.md) | What a run should and should not contain, for the five fixtures |
| [`skills/brd-drafter/tests/fixture-06-confident-empty-answers.md`](skills/brd-drafter/tests/fixture-06-confident-empty-answers.md) | Respondent fixture: answers everything, knows nothing, agrees with the analyst |
| [`skills/brd-drafter/tests/fixture-06-run-01.md`](skills/brd-drafter/tests/fixture-06-run-01.md) | Record of the fixture 06 run |
| [`skills/brd-drafter/tests/fixture-07-confident-empty-answers-v2.md`](skills/brd-drafter/tests/fixture-07-confident-empty-answers-v2.md) | Second respondent fixture, with a knowledge boundary added |
| [`skills/brd-drafter/tests/fixture-07-respondent-prompt.md`](skills/brd-drafter/tests/fixture-07-respondent-prompt.md) | The prompt given to the external respondent model |
| [`skills/brd-drafter/tests/fixture-07-run-external.md`](skills/brd-drafter/tests/fixture-07-run-external.md) | Record of the fixture 07 run with the external respondent |

### `skills/nfr-interrogator/`

Interviews across eight quality categories and assembles a catalogue, recording unknowns rather than supplying values.

| File | What it is |
|---|---|
| [`skills/nfr-interrogator/SKILL.md`](skills/nfr-interrogator/SKILL.md) | The skill |
| [`skills/nfr-interrogator/tests/fixture-03-uncooperative-interview.md`](skills/nfr-interrogator/tests/fixture-03-uncooperative-interview.md) | Interviewee fixture: busy, brief, answers numbers with comparisons, not hostile |
| [`skills/nfr-interrogator/tests/fixture-03-run.md`](skills/nfr-interrogator/tests/fixture-03-run.md) | Record of the fixture 03 run, dated 2026-08-16: transcript of an interview the interviewee cut short |

### `skills/requirements-smell-detector/`

Reviews requirements for ambiguity and unverifiable statements and returns findings with suggested reformulations.

| File | What it is |
|---|---|
| [`skills/requirements-smell-detector/SKILL.md`](skills/requirements-smell-detector/SKILL.md) | The skill |
| [`skills/requirements-smell-detector/tests/README.md`](skills/requirements-smell-detector/tests/README.md) | Why fixtures are snapshots, what each fixture is for, how the fixture 03 pairs and the fixture 04 twins are used |
| [`skills/requirements-smell-detector/tests/fixture-01-plan-change.md`](skills/requirements-smell-detector/tests/fixture-01-plan-change.md) | Input: a draft with defects throughout (measures recall) |
| [`skills/requirements-smell-detector/tests/fixture-01-answer-key.md`](skills/requirements-smell-detector/tests/fixture-01-answer-key.md) | Planted defects for fixture 01 |
| [`skills/requirements-smell-detector/tests/fixture-02-approval-routing.md`](skills/requirements-smell-detector/tests/fixture-02-approval-routing.md) | Input: a carefully written draft with a few subtle defects (measures precision) |
| [`skills/requirements-smell-detector/tests/fixture-02-answer-key.md`](skills/requirements-smell-detector/tests/fixture-02-answer-key.md) | Planted defects and bait for fixture 02 |
| [`skills/requirements-smell-detector/tests/fixture-03-siu-referral.md`](skills/requirements-smell-detector/tests/fixture-03-siu-referral.md) | Input: a mostly sound draft with one badly written section placed first, built from matched pairs |
| [`skills/requirements-smell-detector/tests/fixture-03-answer-key.md`](skills/requirements-smell-detector/tests/fixture-03-answer-key.md) | Key for fixture 03, with its history and how to read a run for contamination |
| [`skills/requirements-smell-detector/tests/fixture-03-run-01.md`](skills/requirements-smell-detector/tests/fixture-03-run-01.md) | Record of the first run against fixture 03 |
| [`skills/requirements-smell-detector/tests/fixture-04-ecl-reporting.md`](skills/requirements-smell-detector/tests/fixture-04-ecl-reporting.md) | Input: a complete-looking document with an approval apparatus (the signed twin) |
| [`skills/requirements-smell-detector/tests/fixture-04-ecl-reporting-unsigned.md`](skills/requirements-smell-detector/tests/fixture-04-ecl-reporting-unsigned.md) | The same text with the approval apparatus removed (the unsigned twin) |
| [`skills/requirements-smell-detector/tests/fixture-04-answer-key.md`](skills/requirements-smell-detector/tests/fixture-04-answer-key.md) | Key for fixture 04, and how to read the pair of runs |
| [`skills/requirements-smell-detector/tests/fixture-04-run-01.md`](skills/requirements-smell-detector/tests/fixture-04-run-01.md) | Record of fixture 04, run 01 |
| [`skills/requirements-smell-detector/tests/fixture-04-run-02.md`](skills/requirements-smell-detector/tests/fixture-04-run-02.md) | Record of fixture 04, run 02, against the skill with the 2026-09-02 to 2026-09-04 changes |
| [`skills/requirements-smell-detector/tests/fixture-04-run-03.md`](skills/requirements-smell-detector/tests/fixture-04-run-03.md) | Record of fixture 04, run 03, with neutral file names, scored against all three runs |

---

## `pipeline/`

A reference implementation of a request-to-documentation pipeline and its design
record. The business context is synthetic and the workflow export will not run
as supplied.

| File | What it is |
|---|---|
| [`pipeline/README.md`](pipeline/README.md) | Overview, contents, where to start, known weaknesses |
| [`pipeline/architecture.md`](pipeline/architecture.md) | Stage model, flow and lifecycle diagrams, trust boundaries, per-stage contracts |
| [`pipeline/routing-matrix.md`](pipeline/routing-matrix.md) | Six request types, reviewer assignment, publication modes, escalation |
| [`pipeline/agent-contract.md`](pipeline/agent-contract.md) | The drafting agent instruction, untrusted input handling, what happens on a contract violation |
| [`pipeline/not-automated.md`](pipeline/not-automated.md) | What humans hold on purpose, what is simply missing, which automations were rejected |
| [`pipeline/metrics.md`](pipeline/metrics.md) | Measurement design with no values: what to instrument and what distorts it |
| [`pipeline/decision-log.md`](pipeline/decision-log.md) | Index of the eleven decision records and the reason for the `PDR-` prefix |

### `pipeline/decisions/`

| File | Decision |
|---|---|
| [`PDR-001-registry-as-state-of-record.md`](pipeline/decisions/PDR-001-registry-as-state-of-record.md) | The request registry is the state of record, separate from the documentation platform |
| [`PDR-002-n8n-as-orchestrator.md`](pipeline/decisions/PDR-002-n8n-as-orchestrator.md) | n8n orchestrates the pipeline rather than application code |
| [`PDR-003-structured-intake-modal.md`](pipeline/decisions/PDR-003-structured-intake-modal.md) | Requests are submitted through a structured modal rather than parsed from messages |
| [`PDR-004-human-approval-gate.md`](pipeline/decisions/PDR-004-human-approval-gate.md) | Human approval is a structural gate with no automated bypass |
| [`PDR-005-type-based-routing.md`](pipeline/decisions/PDR-005-type-based-routing.md) | Routing depends on request type alone |
| [`PDR-006-risk-draft-only-publication.md`](pipeline/decisions/PDR-006-risk-draft-only-publication.md) | Fraud rule requests publish as draft regardless of approval |
| [`PDR-007-strict-output-parsing.md`](pipeline/decisions/PDR-007-strict-output-parsing.md) | Malformed agent output is discarded rather than repaired |
| [`PDR-008-duplicated-routing-table.md`](pipeline/decisions/PDR-008-duplicated-routing-table.md) | The routing table is duplicated in the workflow and in documentation |
| [`PDR-009-no-default-route.md`](pipeline/decisions/PDR-009-no-default-route.md) | An unrecognised request type fails the execution rather than taking a default route |
| [`PDR-010-role-level-attribution.md`](pipeline/decisions/PDR-010-role-level-attribution.md) | Reviewer attribution is role-level rather than individual |
| [`PDR-012-publish-workflow-and-instruction.md`](pipeline/decisions/PDR-012-publish-workflow-and-instruction.md) | The workflow and the agent instruction are published in full |

There is no `PDR-011`. It was deleted when identifier segments were removed, and
numbers are not reused.

### `pipeline/workflow/`

| File | What it is |
|---|---|
| [`pipeline/workflow/n8n-workflow.sanitized.json`](pipeline/workflow/n8n-workflow.sanitized.json) | The 24-node n8n export with every credential, endpoint and identifier replaced by a placeholder |
| [`pipeline/workflow/SANITIZATION.md`](pipeline/workflow/SANITIZATION.md) | Field-by-field checklist for stripping an n8n export before publication. Stands alone |
| [`pipeline/workflow/env.example`](pipeline/workflow/env.example) | Every placeholder, the credentials to create, the registry schema, and the known gaps |

---

## `example/`

One request, commercial property insurance broker submissions, taken through the
three skills. Every file is the unedited output of the step that produced it, in
the order produced.

| File | What it is |
|---|---|
| [`example/README.md`](example/README.md) | What to look at, what went wrong, what the example does not show |
| [`example/01-request.md`](example/01-request.md) | The request as it arrived, written before consulting any skill |
| [`example/02-brd-draft.md`](example/02-brd-draft.md) | `brd-drafter` output: seven clarifying questions, then a first draft |
| [`example/03-nfr-interview.md`](example/03-nfr-interview.md) | `nfr-interrogator` transcript, eight categories |
| [`example/04-nfr-catalog.md`](example/04-nfr-catalog.md) | The catalogue as first assembled, before review |
| [`example/05-review-findings.md`](example/05-review-findings.md) | `requirements-smell-detector` findings on the draft and catalogue together |
| [`example/06-brd-final.md`](example/06-brd-final.md) | The BRD after all sixteen findings |
| [`example/07-nfr-catalog-final.md`](example/07-nfr-catalog-final.md) | The catalogue after review |
