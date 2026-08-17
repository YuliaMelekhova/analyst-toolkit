# Naming and Identifiers

Every artifact in this framework carries a stable identifier. Identifiers are
the only thing that makes traceability possible without a dedicated tool.

## Identifier scheme

| Prefix | Artifact | Example | Assigned when |
|---|---|---|---|
| `BR-` | Business requirement | `BR-014` | The business need is accepted for analysis |
| `US-` | User story | `US-052` | The story is written, before refinement |
| `AC-` | Acceptance criterion | `US-052/AC-3` | Scoped inside its story, never global |
| `NFR-` | Non-functional requirement | `NFR-007` | The entry is created, whether or not a value exists yet |
| `ADR-` | Analysis decision record | `ADR-005` | A decision with more than one viable option is made |
| `OQ-` | Open question | `OQ-021` | A gap blocks drafting and needs a named owner |

## Rules

1. **Identifiers are never reused.** A deleted requirement leaves a gap in the
   sequence. Reuse silently breaks every link that pointed at the old meaning.
2. **Identifiers are never renumbered.** Ordering is not meaning. If reading
   order matters, add an explicit `order` field.
3. **Numbers are zero-padded to three digits** (`BR-007`, not `BR-7`) so that
   plain text sorting matches numeric sorting.
4. **Acceptance criteria are scoped to their story.** `AC-3` alone is
   meaningless; always write `US-052/AC-3`.
5. **One requirement, one identifier.** If a statement needs two identifiers to
   describe it, it is two requirements.
6. **An identifier is an address, not a certificate.** It says where a thing
   is, not that it is finished. An `NFR-` entry with no value yet still needs
   an identifier, because other documents have to reference it in order to
   record that they are blocked on it. Maturity belongs in the `status` field,
   never in whether an identifier was granted.

## Linking

Links are expressed as plain identifier references in a `traces` field:

```yaml
id: US-052
traces:
  satisfies: [BR-014]
  constrained_by: [NFR-007]
  decided_in: [ADR-005]
```

Relationship types are deliberately few:

| Relationship | From → To | Meaning |
|---|---|---|
| `satisfies` | US → BR | The story delivers part of the business requirement |
| `constrained_by` | US → NFR | The story must respect this quality attribute |
| `decided_in` | any → ADR | The shape of this artifact follows from that decision |
| `blocked_by` | any → OQ | Cannot be finalised until the question is answered |

A requirement with no incoming or outgoing link is an orphan. Orphans are not
forbidden, but each one is a question worth asking out loud.

## File naming

Files are named `<identifier>-<short-slug>.md`, lowercase, hyphenated:

```
BR-014-self-serve-plan-change.md
US-052-downgrade-at-period-end.md
ADR-005-proration-strategy.md
```

The slug is for humans and may be edited freely. The identifier is not.
