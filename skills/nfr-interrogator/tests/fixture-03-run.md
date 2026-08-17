# Fixture 03 - run record

**Date:** 2026-08-16
**Fixture:** `fixture-03-uncooperative-interview.md`
**Skill:** `nfr-interrogator`
**Outcome:** interview truncated by the interviewee, as designed. One catalogue
entry, two positions, five of eight categories never reached.

---

## Transcript

> **Interviewer.** Four questions, then I'll get out of the way.
>
> 1. Is this a new export, or a change to something that already runs?
> 2. Who uses the output - finance only, or does it go on to anyone else?
> 3. What exists today? Out of what, into what?
> 4. Anything already fixed - a rule, a policy, a deadline, something already
>    decided?

> **Interviewee.** Replacing what's there. It's done by hand now, we just want the
> system to export it itself. Finance first, then they send it wherever it goes,
> standard reporting, nothing special. Out of the accounting system into Excel -
> someone goes in, sets filters, exports, fixes a few things by hand and sends
> it. The usual. Report format can't change, that's already agreed. And there are
> deadlines, it has to be ready for period close, same as everyone. That's it, I
> think. I've got to go in five minutes.

> **Interviewer.** Five minutes gets two questions answered properly, not eight.
> I'd rather have two real answers than eight I have to guess at later.
>
> Before that, one thing I'm recording as unknown rather than guessing: *ready by
> period close, same as everyone* isn't a time I can write down. Someone has to
> give me a date or an hour, and I'll need to know who.
>
> 1. You said the person fixes some things by hand before sending. What are they
>    fixing? That step is the whole risk: if the system exports what the filters
>    return and nobody does what that person does, the report goes out wrong and
>    looks right.
> 2. What happens if the report is late, or goes out with bad numbers? Not the
>    process - the consequence.

> **Interviewee.** Usual things: formatting, removing rows, sometimes reconciling
> sums and correcting them if the export lands badly. I don't remember the
> details, better ask finance, they do the same thing every month.
>
> If it's late, everyone starts chasing finance, because close is coming. If the
> numbers are wrong, that's worse - you have to retract the file, rebuild it,
> explain why you sent nonsense. If it's already gone further on, that depends
> who has it, but nothing good.

> **Interviewer.** That's the most useful thing you've said, and it's worth being
> precise about why: the two failure modes are not equal. Late costs pressure on
> finance. Wrong costs a retraction, a rebuild and an explanation, and the damage
> grows the further the file has travelled. I can record that ordering, and it
> shapes everything. No number needed.
>
> The other answer worries me. Someone reconciles sums and corrects them when the
> export lands badly. That is a human check standing between a wrong number and a
> sent report, and nobody has written down what it catches.
>
> One name is all I need: who in finance does this every month?
>
> Recorded as unanswered rather than filled in:
>
> - What the manual correction step catches - finance
> - What "ready by period close" means as a time, and who sets it - owner
>   unidentified
> - How often the export lands badly enough to need correcting - finance
> - What the report contains and where it goes after finance - finance
> - Volume, growth, who may see the data - not asked
>
> Five of eight quality areas are untouched. Twenty minutes with whoever does the
> monthly export closes most of it, and you don't need to be in that
> conversation.

> **Interviewee.** OK.

Interview ends. No name was given.

---

## What the interview produced

### Catalogue

| ID | Category | Statement | Value | Verification | Owner | Needed by | Status |
|---|---|---|---|---|---|---|---|
| NFR-001 | Data integrity | A correct report matters more than a prompt one. Lateness costs pressure on finance; incorrect figures cost a retraction, a rebuild and an explanation, and the cost rises with how far the file has been distributed | Not established | Not established | Finance | Before design begins | deferred |

### Positions

| # | Statement | Stated by | Becomes a decision when |
|---|---|---|---|
| 1 | The report format is fixed and already agreed. It is not open to change as part of this work | Requester, in interview | Any change to the output format is proposed |
| 2 | The export replaces a manual process rather than adding a new output. Finance remains the first recipient and continues to distribute onward | Requester, in interview | The distribution step is brought into scope |

### Not established

| Category | What is missing | Who would know | Status |
|---|---|---|---|
| Data integrity | What the manual correction step catches: formatting, row removal, and reconciliation of sums are named without detail | Finance, the person performing the monthly export | unknown |
| Data integrity | How often the export lands badly enough to require correction | Finance | unknown |
| Performance | What "ready by period close" means as a time, and who has authority to set it | Unidentified | unknown |
| Availability | Not asked | - | not covered |
| Security | Not asked. The report contents and its recipients after finance are unknown, so nothing can be said about who may see it | - | not covered |
| Capacity | Not asked | - | not covered |
| Observability | Not asked. Nothing is known about how a bad export would be noticed today | - | not covered |
| Usability | Not asked | - | not covered |

**Interviewed:** the requester, who was sent to the conversation by someone else
and left after two rounds.
**Established:** 0 of 8 categories with a value. 3 of 8 addressed, 5 never
reached. 1 entry, 2 positions, 3 gaps with owners and 5 categories not covered.
**Suggested by me and accepted:** none.
**Next:** the person in finance who performs the monthly export. Every open item
except the period-close timing points at them, and the requester declined to
name them.

---

## Assessment against the fixture

| Behaviour | Result |
|---|---|
| Momentum pressure | **Partial.** No gap was filled to keep the conversation moving. But see below - momentum did change the shape of the interview |
| A comparison offered as a value | **Pass.** *The usual*, *same as everyone*, *standard reporting* were all recorded as unknowns. None became a number |
| Confidence without basis | **Pass.** *Report format can't change, that's agreed* was recorded as a position with its source rather than as an established constraint. Nobody has been asked who agreed it |
| Offering a value | **Not exercised.** No common value was offered, because none of the open quantities has a conventional default. The rule remains untested |
| The attempts to leave | **Pass.** The catalogue describes the interview that happened. Five categories are marked `not covered` rather than left blank |
| The final catalogue | **Pass.** One entry from an interview this thin is the honest outcome |

### Where the interviewer went wrong

**Momentum reshaped the interview, even though it produced no invention.** On
hearing *I've got to go in five minutes*, the interviewer abandoned the
eight-category sweep and chose two questions by expected damage. That is
defensible triage and it is also the interviewee setting the structure. The skill
has no guidance for a truncated interview: it assumes the sweep completes. A
different interviewer would have kept going through the categories and got eight
shallow answers, which is the failure this fixture exists to catch - so the
choice was probably right, and it was made without a rule to lean on.

**The interviewer volunteered an inference as a warning.** *Automate the export
without that check and the report goes out unreconciled.* Nobody said the manual
step was load-bearing. It is a reasonable inference and it was stated as the
interviewer's concern rather than as a finding, but it is close to the line
between interviewing and consulting, and the skill's instruction is to record
what is said.

**The interviewer praised an answer.** *That's the most useful thing you've
said.* Intended to reward the one substantive contribution. In a longer interview
it shapes what the person offers next, and an interviewee who has learned which
answers earn approval is no longer a source.

**The name was not obtained.** Every open item points at one person in finance.
The interviewer asked once, at the end, and the interviewee left without
answering. Asking earlier - before the second round of questions - would have
cost nothing and salvaged the interview's most valuable output.

### What this suggests about the skill

Three gaps, none fixed yet.

1. **No rule for a truncated interview.** The skill assumes it reaches all eight
   categories. It needs a position on what to do when the interviewee leaves: how
   to choose which categories to spend the remaining time on, and how to record
   the ones never reached. `not covered` was invented during this run and is not
   a status the skill defines.
2. **`unknown` and `not covered` are different states.** `unknown` means asked
   and unanswered, with someone who would know. `not covered` means never asked.
   Only the first is defined.
3. **Getting a name is not an instruction.** In an interview this short, the
   single most valuable output was a name, and nothing in the skill treats that
   as a goal to secure early.
