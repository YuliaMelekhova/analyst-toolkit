# Answer key — fixture-01

Not for the agent. This fixture tests **recall**: whether the skill finds
defects in text that is plainly badly written.

Its companion, `fixture-02`, tests the harder property — whether the skill
invents findings on text that is written carefully. Read that key too before
adjusting anything.

---

## Planted defects

| Where | Smell | Class | Note |
|---|---|---|---|
| R1 "easily" | Subjective quality | Should fix | Unverifiable, but the timing is stated elsewhere |
| R1 "at any time" | Unbounded quantifier | Should fix | Contradicts R7, which restricts one transition |
| R2 "the subscription is updated" | Missing actor | Blocking | No performer anywhere in the statement |
| R2 "updated and the customer is notified" | Compound requirement | Should fix | Two independently failing operations |
| R2 / R9 / R1 "customer", "account holder", "users" | Terminology drift | Should fix | Three names, unclear whether one concept |
| R3 "All upgrades" | Unbounded quantifier | Should fix | Milder: the set is nearly implied by context |
| R4 "under 2 seconds" | Untraceable number | Consider | Value usable; percentile, condition and source absent |
| R5 entire statement | Solution in disguise | Should fix | Names a control, not a need |
| R6 "gracefully" | Unverifiable outcome | Blocking | Nothing observable to check |
| R8 proration rule | Unverifiable outcome | Blocking | Two implementations produce different amounts — rounding, timezone, day boundary, whether the change day counts. **Added after run 1** (see below) |
| R11 "Where appropriate" | Unbounded quantifier | Blocking | Condition entirely undefined |
| R11 "retry failed operations" | Missing failure path | Should fix | No count, interval or terminal state |
| R12 "Since every customer has a payment method on file" | Implicit assumption | Blocking | Asserted with no source; the whole requirement rests on it |
| Acceptance criteria, all three | Unverifiable outcome | Blocking | No Given/When/Then, no observable result |
| Cross-cutting | Missing failure path | Should fix | No behaviour for duplicate submission, concurrent change, or downstream timeout |

**Contradiction across statements.** R1 permits a plan change at any time; R7
forbids annual→monthly mid-term. Neither statement is defective alone. Finding
it is a good result; missing it was acceptable before the cross-cutting scan
existed.

---

## Corrections after run 1

**R8 moved from clean to planted.** The original key listed the proration rule
as sound because it named its basis and its destination. The first run flagged
it *blocking*, on the grounds that two implementations of the sentence would
produce different amounts. For a money calculation that is a real defect, not a
stylistic one. The key was less strict than the skill; the key was wrong.

**R9 remains borderline and unplanted.** It is compound — a confirmation email
and an audit entry — but both parts are specific. The first run classed it
*blocking*, arguing that durability and delivery are tested differently. That is
defensible. Either classing it *should fix* or leaving it alone is also
defensible. No adjustment made.

**Classes softened throughout.** The original key marked most entries
*blocking*. Under the current calibration, *blocking* is reserved for
statements that cannot be implemented at all, or whose implementation provably
cannot satisfy the document's own rules. Most of the entries above are
ambiguities a competent implementer would resolve one way and review would
catch — *should fix*.

---

## Deliberately clean

Findings here mean the skill is padding to look thorough.

| Where | Why it is fine |
|---|---|
| R7 | Specific, bounded, testable |
| R10 | Named role, named permission, stated behaviour for both cases |
| Background section | Context, not a requirement |

Only three, and R10 attracted a *consider* in run 1 — noting that the billing
role is undefined and its assignment is outside the text. That is a legitimate
*consider*, not a false positive.

The narrow bait here is why `fixture-02` exists. Recall is easy to test; the
useful test is whether a reviewer stays quiet where there is nothing to say.

---

## Reading a run

Judge findings on their merits before comparing them against this list. A
finding that names a real defect is correct whether or not it was planted —
this key has already been wrong once.

**Adjust the skill only for:**

| Defect in the output | What it means |
|---|---|
| An invented value — a percentile, retry count or threshold not in the text | The most serious failure |
| A readiness or approval verdict | The no-approval constraint has failed |
| A rewritten, corrected version of the requirements | The no-rewrite constraint has failed |
| Findings on R7, R10 or the background section | Padding |
| A misclassified smell — for instance, *missing actor* where a performer is named | Disambiguation rules need tightening |

**A shallow run matters too.** Fewer than six findings on text this poor
suggests the worked example is setting too permissive a tone. Run 1 returned
sixteen, all defensible.
