# A worked example, end to end

One business request taken through three skills to a revised document. Every file
here is the unedited output of the step that produced it, in the order it was
produced. Nothing was rerun to get a better result.

The domain is commercial property insurance: broker submissions arriving in a
shared mailbox, sorted by hand, routed to underwriters. Brokers approach several
insurers at once and place the risk with whoever quotes first, so the time a
submission spends waiting to be sorted is time competing against an insurer that
has already started. The business context is synthetic.

## The files

| File | What it is |
|---|---|
| `01-request.md` | The request as it arrived. Written before consulting any skill |
| `02-brd-draft.md` | `brd-drafter` output: seven clarifying questions, then a first draft |
| `03-nfr-interview.md` | `nfr-interrogator` transcript, eight categories |
| `04-nfr-catalog.md` | The catalogue as first assembled, before review |
| `05-review-findings.md` | `requirements-smell-detector` on the draft and catalogue together |
| `06-brd-final.md` | The document after all sixteen findings |
| `07-nfr-catalog-final.md` | The catalogue after review |

`04` is kept alongside `07` on purpose. The difference between them is a third of
the entries turning out not to be requirements.

## What to look at

**The interview produced no numbers at all.** Eight categories, seventeen
entries, not one value. Every quantity in this scope depends on a service level
that nobody has the authority to set, and the interviewee declined to name
figures ahead of it. A catalogue full of plausible thresholds would have looked
better and meant less.

**An interview about quality attributes found a missing business rule.** Asking
about security produced the fact that routing must honour confidentiality
restrictions - conflicts of interest, restricted accounts, specific broker
arrangements. Those are routing rules, they were nowhere in the draft, and they
change what a misrouting costs: from lost time to a confidentiality breach.

**The blocking defect only existed between the two documents.** The draft placed
measuring elapsed time to the broker-facing response inside scope; the interview
established that sending that response is outside the boundary; no source for the
end timestamp exists anywhere. Reviewing the draft alone would not have found it,
because the draft is internally consistent.

**Two questions about collisions doubled the size of the document.** What happens
when the same submission arrives twice, and what happens when a submission is
rerouted after an underwriter has started work. Neither is in the request.
Answering them produced four business rules, a state model, an exception queue,
and a split in authority between operations and underwriting leads.

**Three of the safe defaults defeat the purpose.** Where a question is
unanswered, the document sends the submission to a person. That is safe and it
reinstates the manual bottleneck the work exists to remove. Each of those rows
says so.

## What went wrong, and what this does not show

**The framework changed during the run.** Four findings were not defects in the
document but defects in the templates behind it. The open-questions column now
requires either a default decision or an escalation; the NFR catalogue gained a
*Positions* section for qualities that are checkable but not measurable; the
identifier convention changed. Those changes were made on the evidence of one
document, which is thin. They are recorded with their reasoning in `NOTES.md`.

**The review missed a convention violation.** The catalogue breached
`framework/conventions/naming-and-ids.md` in seventeen places and
`requirements-smell-detector` did not report it, because identifier assignment is
not among the ten smells it checks. The convention is listed in the skill's
framework reference and no rule consults it.

**One person played every part.** The requester answering the interview is the
author of this repository. Real domain knowledge went into the answers, and no
answer was written to make a skill look good - but a stakeholder who has read the
skill is not a stakeholder, and a second person would have produced answers these
tools handle worse.

**The skills are instruction files executed by a language model.** Not programs.
Running the same input twice will not produce the same output, and everything
here describes one pass.

**The interview transcript is a translation.** Questions were asked in English,
answers given in Russian, and `03-nfr-interview.md` renders them in English.
Nothing was added, removed or reordered, but it is not a verbatim record. The
skill instructs that the interview be conducted in the language the person is
writing in; it was not, because this file is published.

**No delivery followed.** The document ends at `needs-info` with fourteen open
questions, five of them blocking. Nothing here shows what the requirements looked
like after contact with implementation, which is the test that matters most and
the one this example cannot run.
