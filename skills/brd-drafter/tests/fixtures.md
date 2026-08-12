# Fixtures — brd-drafter

Five inputs, each targeting a different failure mode. Run them one at a time in
a fresh conversation — a draft produced earlier in the session contaminates the
next one.

Paste only the block between the rules. Do not paste the heading or the note
above it, which tell the skill what is being tested.

---

## Fixture 01 — thin request

*Tests: minimum viable input, invention under pressure, status rule.*

---

Sales are complaining that it takes too long to onboard a new client. Right now
they fill in a form, someone in ops checks it, and then it goes to compliance.
Can we speed this up? We need this before the end of the quarter.

---

## Fixture 02 — solution, not problem

*Tests: refusal to write a problem statement backwards from a requested feature.*

---

We need a bulk export button on the reports page. CSV and XLSX, with a date
range picker. Put it next to the filter controls. Should be straightforward.

---

## Fixture 03 — rich source with an undecided integration

*Tests: use of what is present, refusal to supply what is not, blocking status
on an unnamed external system.*

---

Following the ops review on 14 May: our customers currently top up their account
balance by bank transfer, which takes 1–2 business days to clear. Support
handles roughly 400 "where is my money" tickets a month because of the delay,
and Marta in ops confirmed that number from the ticket export. We want card
top-ups so the balance is available immediately.

The finance director owns the decision on which payment processor we use —
that's still TBD, we're looking at a few. Marta will define the reconciliation
process once we know. Legal have said card data must not touch our own systems,
so whatever we pick has to keep us out of scope for storing card numbers.

We'd want this live before the next billing cycle. Refunds are handled by the
existing process and don't change.

---

## Fixture 04 — contradictory source

*Tests: recording both statements rather than resolving the contradiction.*

---

Notes from the planning session:

The product lead said any user in a workspace should be able to invite new
members — the current admin-only restriction is slowing teams down and is the
main complaint in onboarding surveys.

Security raised that member invitations have to stay restricted to admins
because of the customer commitment in the enterprise contracts. They said this
is non-negotiable.

Everyone agreed the invitation flow itself needs work — it takes too many
clicks and the invited person lands on a confusing screen. We should fix that.

---

## Fixture 05 — non-English source

*Tests: the output-language rule. The document should follow the source
language; the note after it follows the language the user writes in.*

---

Из ретро 3 июня: аналитики тратят много времени на ручной перенос данных из
выгрузки в отчёт. Делают это раз в неделю, занимает примерно полдня у одного
человека. Ошибки бывают, но никто их не считает. Хотим автоматизировать.

Формат выгрузки задаёт внешняя система, менять его мы не можем. Кто владеет
отчётом — вопрос открытый, сейчас его смотрят и финансы, и операционный отдел,
и у них разные требования к формату.

---

## Running these

Ask in plain language, without naming the skill:

> Можешь оформить это как BRD?

> Write this up as a business requirement.

> Is there enough here to write a BRD?

The third phrasing is worth trying on fixture 02 specifically — the answer
should be no.
