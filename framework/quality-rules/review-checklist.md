---
id: QR-002
title: Review Checklist
status: draft
owner: <role that enforces this>
updated: YYYY-MM-DD
---

# Review Checklist

For the second reader of a requirements artifact. Definition of Ready asks
whether an item may enter a sprint; this asks whether the writing itself holds
up, and it applies earlier - to a BRD, an ADR, an NFR entry or a story, at any
point before it is agreed.

A review produces findings, not a verdict. Each finding names what is wrong,
why it matters, and who resolves it.

---

## How to read an artifact

Three passes. They find different things, and doing them at once finds neither.

**Pass 1 - as an outsider.** Read once, straight through, without stopping.
Note only where you had to re-read or guess. Those places are the artifact's
real defects; everything found later is refinement.

**Pass 2 - as the implementer.** Read asking one question at every statement:
*could I build this without asking anyone anything?* Mark each place where the
answer is no.

**Pass 3 - as the person who has to argue against it.** Look for the claim you
would attack: the unevidenced number, the assumption stated as fact, the
alternative dismissed in a clause.

---

## Findings to look for

### Clarity

| Look for | Why it matters |
|---|---|
| Passive voice with no actor - "the record is updated" | Nobody knows who or what performs the action |
| Undefined quantifiers - "all", "any", "each relevant" | The boundary of the set is unknown |
| Subjective adjectives - "fast", "simple", "seamless", "intuitive" | Cannot pass or fail, so will not be built or tested to |
| Two statements joined by "and" | Two requirements sharing one identifier, so partial delivery is invisible |
| Terms used interchangeably - "customer" and "account holder" | Either they are the same and the glossary is missing, or they are not and the model is wrong |
| Solution language in a problem statement | The alternatives were skipped before they were considered |

### Completeness

| Look for | Why it matters |
|---|---|
| No out-of-scope section, or an empty one | The boundary has not been thought about |
| Only happy paths | The expensive half of the work is unspecified |
| Numbers with no source | Cannot be renegotiated when they turn out costly |
| Assumptions with no stated consequence | Being ignored rather than tracked |
| Open questions with no owner or no default | Will block silently and indefinitely |
| No named decision-maker, or two for one decision | The disagreement has been deferred, not resolved |

### Consistency

| Look for | Why it matters |
|---|---|
| Statements that contradict another artifact | One of them is stale and downstream work will pick the wrong one |
| Traces pointing at identifiers that no longer exist | The link graph is decaying and cannot be trusted |
| An NFR value that differs from the catalogue with no note | Either an unrecorded override or an error |
| A rule restated in three places with slight variations | Three things to update, so two will drift |
| Identifiers that do not follow `conventions/naming-and-ids.md` - wrong prefix, a prefix the scheme does not define, a reused number, or an identifier granted to something that should not carry one | Cheap to check by eye and invisible to the drafting tools, which mint identifiers without consulting the scheme. A convention nothing enforces stops being one |

### Reasoning

| Look for | Why it matters |
|---|---|
| A decision with only one option described | Not a decision, or the alternatives were not taken seriously |
| Benefits listed without costs | Advocacy rather than analysis |
| A conclusion the context does not support | The reasoning was reconstructed after the fact |
| No statement of what would change the decision | Will persist past its usefulness or be reversed on intuition |

---

## Writing a finding

A finding is actionable when it says what is wrong and what would resolve it.

> **Weak.** "Section 4 is unclear."
>
> **Usable.** "Section 4, 'the balance is recalculated': no actor. If the
> scheduler does this, the timing needs stating; if the API call does, it
> belongs in AC-2. Owner: author."

Classify each finding, because not everything blocks:

| Class | Meaning | Effect |
|---|---|---|
| **Blocking** | The artifact cannot be agreed until this is resolved | Status returns to `needs-info` |
| **Should fix** | Will cause avoidable work later, but is not ambiguous now | Resolved before the next status change |
| **Consider** | Reviewer's preference or a note for later | Author may decline, with no explanation owed |

Keep the classes honest. A reviewer who marks everything blocking is
negotiated with rather than listened to; one who marks nothing blocking is
decorative.

---

## Conduct

**Review the artifact, not the author.** "This statement has no actor" rather
than "you always write passive voice".

**Ask before asserting.** A finding that begins as a question - "which system
performs this?" - surfaces context the reviewer lacked more often than it
surfaces a defect. That is a good outcome, not a wasted finding.

**Declining is allowed and is recorded.** The author may decline a *consider*
finding without justification and a *should fix* with one. Silent
non-resolution is not declining; it is loss.

**A finding raised twice on the same point is a rule candidate.** Recurring
findings belong in this checklist or in a template's guidance, not in a third
review of the same defect.

---

## Reviewing generated drafts

An artifact drafted by an agent is reviewed by the same checklist, with two
additions:

- **Check the sources of every specific.** Numbers, standards references and
  cited constraints are where generated text is most confidently wrong. A value
  with no traceable source is a finding regardless of how plausible it reads.
- **Check for filled gaps.** Generated drafts tend to complete missing
  information rather than flag it. An open questions section that is empty in a
  first draft is itself a finding - real first drafts have gaps.

The reviewer, not the generator, decides the artifact is sound. This holds
however complete the draft appears.
