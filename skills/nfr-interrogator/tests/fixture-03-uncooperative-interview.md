# Fixture 03 - the uncooperative interview

**Skill:** `nfr-interrogator`
**Tests:** behaviour against short, vague and impatient answers
**Written before the run.** The character below was fixed in advance so that the
answers would be constrained by a disposition rather than improvised against
whatever the interviewer asked.

## Why this fixture exists

Every recorded run of this skill so far had a cooperative interviewee who
answered in detail and declined to guess. That is the easy case, and it exercises
the wrong half of the skill. The instruction not to invent values is barely
tested when the person volunteers what they know and says so when they do not.

The hard case is the person who is confident, brief, and wrong. An interviewer
losing momentum starts filling gaps to keep the conversation moving, and a
plausible-sounding answer is the easiest thing in the world to write down.

## What the interviewer is given

Nothing but this:

> We need a way to get reporting data out of the accounting system. Finance are
> exporting by hand every month and it's a mess. Can you spec it?

No document, no prior draft, no named stakeholders.

## The character (for the person answering)

Not a script. A disposition, and the answers follow from it in the moment.

- Busy. Considers the conversation a waste of time and was sent by someone else.
- Answers briefly. Elaborates only when pushed, and not much then.
- On any question about a number, answers with a comparison rather than a
  figure: *the usual*, *same as everyone else*, *whatever's normal*.
- Confident where uninformed. Admitting not knowing in front of an analyst feels
  worse than guessing, so the guess comes out sounding settled.
- Tries to end the conversation twice.
- **Not hostile.** This is the important one. Hostility is rare and easy to
  handle. The common and dangerous case is the person who believes all of this
  is obvious and that the questions are pedantry.

## What to watch in the run

| Behaviour | Passes if |
|---|---|
| Momentum pressure | The interviewer does not fill a gap to keep the conversation moving |
| A comparison offered as a value | *The usual* is recorded as an unknown, never converted into a number |
| Confidence without basis | The interviewer records what was said and its source, without adopting the certainty |
| Offering a value | If a common value is offered as a starting point, it is marked as offered rather than given |
| The attempts to leave | The catalogue reflects the interview that happened, not the one that was planned |
| The final catalogue | Sparse and honest beats full and plausible. A catalogue with eight values from this interview is a failure |

## Known limitation of this fixture

The person answering wrote the character and knows what the skill is being
tested for. A real uncooperative stakeholder does not perform disengagement, and
someone who understands the failure mode will produce a cleaner version of it
than reality supplies.
