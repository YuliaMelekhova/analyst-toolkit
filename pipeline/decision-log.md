# Decision Log

Eleven decision records for the pipeline, one file each in [`decisions/`](decisions).

Each follows the structure of `framework/`'s ADR template: context with the forces at play, the decision, every option considered with its own better and worse columns, consequences including what becomes harder and what is closed off, the triggers that would justify revisiting, the assumptions the decision rests on, and what is explicitly not settled.

## The records

| ID | Decision | Status |
|---|---|---|
| [PDR-001](decisions/PDR-001-registry-as-state-of-record.md) | The request registry is the state of record, separate from the documentation platform | accepted |
| [PDR-002](decisions/PDR-002-n8n-as-orchestrator.md) | n8n orchestrates the pipeline rather than application code | accepted |
| [PDR-003](decisions/PDR-003-structured-intake-modal.md) | Requests are submitted through a structured modal rather than parsed from messages | accepted |
| [PDR-004](decisions/PDR-004-human-approval-gate.md) | Human approval is a structural gate with no automated bypass | accepted |
| [PDR-005](decisions/PDR-005-type-based-routing.md) | Routing depends on request type alone, not on amount, seniority or declared urgency | accepted |
| [PDR-006](decisions/PDR-006-risk-draft-only-publication.md) | Fraud rule requests publish as draft regardless of approval | accepted |
| [PDR-007](decisions/PDR-007-strict-output-parsing.md) | Malformed agent output is discarded rather than repaired | accepted |
| [PDR-008](decisions/PDR-008-duplicated-routing-table.md) | The routing table is duplicated in the workflow and in documentation | accepted |
| [PDR-009](decisions/PDR-009-no-default-route.md) | An unrecognised request type fails the execution rather than taking a default route | accepted |
| [PDR-010](decisions/PDR-010-role-level-attribution.md) | Reviewer attribution is role-level rather than individual | accepted |
| [PDR-012](decisions/PDR-012-publish-workflow-and-instruction.md) | The workflow and the agent instruction are published in full | accepted |

If you read one, read PDR-004. Everything else in this directory follows from it or works around it.

## Why the prefix is `PDR-` and not `ADR-`

`framework/conventions/naming-and-ids.md` reserves `ADR-` for Analysis Decision Records: choices made during analysis about how a process is bounded, which system owns a concept, what a term means. These records are a different genre. They are decisions about the pipeline itself - which tool orchestrates it, where state lives, what the agent may not do - and would sit in the same global sequence as a project's analysis decisions while describing something else entirely.

`PDR-` is therefore an extension to the framework scheme rather than a use of it. If the prefix is adopted, `framework/conventions/naming-and-ids.md` should gain a row for it. Until then it is defined here and nowhere else, which is a known inconsistency rather than a silent one.

Everything else follows the framework rules: zero-padded to three digits, never reused, never renumbered.

## Why PDR-011 does not exist

There was a record numbered 011, about request identifiers being retained through reclassification. It existed because an earlier identifier format embedded the request type, so a reclassified request carried an identifier that no longer described it, and the decision was to accept that rather than renumber.

Aligning with `framework/conventions/naming-and-ids.md` removed the premise. Identifiers are now `BR-<NNN>` with no type segment, so reclassification changes one registry field and touches nothing else. The record had nothing left to decide and was deleted.

The number stays retired. Identifiers are never reused, and renumbering the records above to close the gap would break every reference to them. A gap in a sequence is the framework working as intended.

## Why `decided_on` is empty

Every record carries an empty `decided_on` field. This is a reference implementation with no real timeline, and filling those fields would present a constructed sequence as history. The frontmatter key is retained so the records validate against the template and so a real deployment adopting them has the field ready.

Numbering reflects dependency, not chronology. PDR-001 does not precede PDR-012 in time.

## Related

- [`architecture.md`](architecture.md) - the design these decisions produced
- [`not-automated.md`](not-automated.md) - what was left out, and the automations rejected outright
- [`routing-matrix.md`](routing-matrix.md) - the table PDR-005, PDR-008 and PDR-009 govern
