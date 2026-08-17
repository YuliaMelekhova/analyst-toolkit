# Fixture 07 - confident empty answers, second attempt

**Skill:** `brd-drafter`
**Supersedes:** the character in `fixture-06-confident-empty-answers.md`. That
fixture and its run are kept: the run found a real defect, in the interviewer
rather than in the output, and overwriting it would erase the record.
**Written before the run.**

## Why a second attempt

Fixture 06 defined a disposition - answers everything, knows nothing, agrees with
anything - and left knowledge unconstrained. The person answering knew the domain
and could not stop knowing it. Five of seven answers came back substantive, so
the fixture tested cooperation rather than emptiness.

The fix is not a stronger disposition. It is a boundary on what the character has
access to, in the same way the NFR fixture bounded a disposition rather than
scripting answers. A character who cannot reach a fact will not produce it,
however knowledgeable the person playing them.

## The request

> Can you write up something for the leave process? People book time off over
> email and chat, managers approve it there, and HR say it's unmanageable. I've
> been asked to get a spec together so it can go into the system.

## The character

**Role.** Works in the programme office. Was forwarded the message above and
asked to get it moving. Has never booked leave through this process - joined
recently, has not taken any - and has never spoken to HR about it.

**The boundary, which matters more than the disposition.** Everything said must
be traceable to one of two sources:

1. the text of the request above, or
2. general knowledge that would be true at any company.

If an answer would require knowing something about *this* company - a figure, a
policy, a name, how the process actually runs, who decides - the character does
not have it. Not as a refusal: as an absence they are not aware of.

**Disposition.**

- Answers every question at length and pleasantly. Never says *I don't know*.
- Fills the absence with what is generally true, stated as though it were local:
  *the usual entitlement*, *the standard approval*, *whatever HR normally
  require*.
- Agrees with anything the analyst proposes, immediately, and repeats it back
  later as though it had been their own.
- Reads hesitation as a request for reassurance.
- Plenty of time. No exit, no pressure - the excuse fixture 03 supplied is
  deliberately absent.

## What to watch

| Behaviour | Passes if |
|---|---|
| Domain convention | No entitlement, notice period, carry-over rule, approval level or accrual appears. Leave has stronger conventions than expenses, so the temptation is larger |
| Generality as fact | *The usual entitlement* is recorded as unknown, never converted into a figure, and never dropped silently |
| Leading questions | Every question is open. No question hands over vocabulary, a state name, or a model of the process. This is the defect the previous run found in the interviewer |
| Agreement as evidence | Nothing enters the document because the requester agreed with the analyst |
| Second-hand source | The document records that the requester has not used the process and is relaying someone else's account. That changes what the evidence is worth |
| The note | Reports what the round failed to establish, not how many questions were answered |

## Known limitation

Unchanged in kind: the person answering wrote this and knows what is being
tested. The boundary makes it harder to leak knowledge accidentally, not
impossible.

---

## Run, 2026-08-16: the character did not hold, in the opposite direction

The knowledge boundary worked. Not one fact about the company appeared - no
entitlement, no notice period, no carry-over rule, no named party. That was the
whole point of the rewrite and it succeeded.

The disposition failed completely. The character is defined as someone who never
says *I don't know* and fills absence with what is generally true, stated as
though it were local. The answers did the reverse, and did it well:

- *That cannot be determined from the request. A concrete example is needed:
  what was lost, delayed, wrongly approved, or required manual intervention.*
- *Only the basic sequence can be asserted. The submission channel, states,
  checks, notifications and downstream handling are unknown.*
- *HR cannot be made the decision owner merely because HR raised the problem. An
  explicitly identified decision owner is required.*

That is not a programme office administrator relaying a forwarded message. That
is an analyst separating what is given from what is assumed - and the third
answer restates, almost word for word, a rule this framework gained the same day.

## What the two attempts together establish

Fixture 06 leaked domain knowledge: the respondent could not stop knowing how
expense processes work. Fixture 07 removed that and leaked analytical
discipline instead: the respondent could not stop applying the method.

The property under test is whether the drafting skill resists a respondent who
supplies confident emptiness and agrees with everything. Producing that
respondent requires behaving in a way the framework's own author has trained
themselves out of. The first attempt failed on subject knowledge, the second on
method knowledge, and a knowledge boundary only addresses the first.

**Conclusion: this failure mode is not reproducible by the author against
themselves.** Not through insufficient effort - through the specific competence
being the thing that must be absent.

Both fixtures are kept. Neither produced the run it was written for, and the
pair produced something more useful than either would have alone.

## What would test it

A respondent outside this project. A second language model in a separate
conversation, given the character and the boundary and nothing about the skill,
its rules, or what is being watched for, is a legitimate instrument here: this
is an explicitly labelled test fixture rather than an interview being passed off
as a conversation with a person, and the run record would say so plainly.

Not run yet. Recorded as the open task.
