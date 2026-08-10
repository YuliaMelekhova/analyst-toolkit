---
id: NFR-CATALOGUE
scope: <system or capability this catalogue applies to>
status: draft            # draft | needs-info | in-review | approved
owner: <role accountable for quality attributes>
updated: YYYY-MM-DD
---

# Non-Functional Requirements — Catalogue

Functional requirements describe what the system does. These describe how well
it has to do it, and they are where most delivery surprises come from.

## How an entry is written

Every entry in this catalogue has four parts. An entry missing any of them is
not a requirement yet.

| Part | Question it answers | Without it |
|---|---|---|
| **Value** | How much, expressed as a number and a unit | The requirement cannot fail, so it cannot pass |
| **Condition** | Under what load, on what path, for which users | The number is unverifiable in practice |
| **Verification** | How this will be checked, by whom, when | Nobody ever checks it |
| **Source** | Who asked for this value and on what basis | It cannot be renegotiated when it turns out expensive |

### Entry format

```yaml
id: NFR-007
category: performance
statement: >
  The plan comparison page renders interactively within 2.0 s at the 95th
  percentile for authenticated users on the primary region.
value: 2.0 s
percentile: p95
condition: 500 concurrent sessions, warm cache, primary region
verification: synthetic check in CI, alert at p95 > 1.6 s
source: support tickets Q2, threshold agreed with <role> on YYYY-MM-DD
applies_to: [BR-014]
status: agreed          # proposed | agreed | deferred | unknown
```

> `status: unknown` is a legitimate value and is preferable to an invented
> number. An unknown NFR is a visible risk; a guessed one is an invisible
> commitment.

---

## The eight categories

For each category: what to ask, what unit the answer comes in, and the
difference between a usable and an unusable formulation.

---

### 1. Performance

**Ask.** Which interaction feels slow today, and what is the user doing while
they wait? What happens downstream if a response is late — retry, timeout,
duplicate? Is this a perceived-speed problem or a throughput problem?

**Units.** Latency (ms/s) at a stated percentile, throughput (requests or
records per second), processing time for a batch.

| Usable | Unusable |
|---|---|
| Search results return within 800 ms at p95 for queries of up to 3 terms, at 200 req/s | The system must be fast |
| A nightly reconciliation of 2M records completes within a 4-hour window | Reports should generate quickly |

> Averages hide the cases people complain about. Always ask for a percentile.
> If the stakeholder cannot name one, p95 is a reasonable default to propose —
> and record that it was proposed, not requested.

---

### 2. Availability and reliability

**Ask.** What is the cost of one hour of unavailability, and to whom? Is
degraded service acceptable, or is it all-or-nothing? What is the acceptable
data loss window if the worst happens?

**Units.** Uptime percentage over a stated period, RTO (time to restore), RPO
(acceptable data loss), MTBF, error budget.

| Usable | Unusable |
|---|---|
| 99.9% monthly uptime measured on the public API, excluding announced maintenance windows | The system must be highly available |
| RTO 4 h, RPO 15 min for the subscription store | The system should recover quickly from failures |

> Ask what is excluded from the measurement. Announced maintenance, third-party
> outages and partial degradation are where uptime numbers are quietly won.

---

### 3. Scalability and capacity

**Ask.** What does the system look like at 10× today's volume — and is 10×
realistic within the horizon we are designing for? Which dimension grows:
users, data, transactions, or integrations? Is growth gradual or spiky?

**Units.** Peak concurrent users, records stored, growth rate per period,
headroom before intervention.

| Usable | Unusable |
|---|---|
| Sustains 5,000 concurrent sessions with a 3× spike for 30 min without manual intervention | The system must be scalable |
| Stores 18 months of event history at a projected 40M events/month | Must handle future growth |

> "Scalable" without a number is a wish. The useful question is not "how big"
> but "at what point does someone have to do something about it".

---

### 4. Security

**Ask.** What is the worst thing someone could do with this data or this
function? Who is allowed to see what, and who decides? What has to be provable
after the fact?

**Units.** Access model, encryption standard, session and credential lifetimes,
audit retention period, time to revoke access.

| Usable | Unusable |
|---|---|
| Personal data encrypted at rest (AES-256) and in transit (TLS 1.2+); keys rotated annually | The system must be secure |
| All changes to subscription state produce an immutable audit entry retained 7 years | Actions should be logged |
| Access revocation takes effect within 5 minutes across all sessions | Access must be controlled |

> Security requirements that name only a standard ("must be PCI compliant") are
> pointers, not requirements. Ask which specific control the standard imposes
> on *this* scope, and write that.

---

### 5. Data integrity and retention

**Ask.** Which record is authoritative when two systems disagree? What must
never be lost, and what may be lost? How long must this be kept, and who says
so? What has to happen when someone asks for their data to be deleted?

**Units.** Retention period, consistency model, reconciliation frequency and
tolerance, deletion SLA.

| Usable | Unusable |
|---|---|
| The billing ledger is authoritative; the CRM reconciles nightly with zero tolerance for amount mismatch | Data must be consistent |
| Deletion requests are fulfilled across primary and analytical stores within 30 days | We must support data deletion |

> The two questions that surface real complexity: *which copy wins*, and *what
> does deletion mean in the warehouse and in backups*.

---

### 6. Observability

**Ask.** How will we know this is broken before a user tells us? What does the
on-call person need in the first five minutes? What question will someone ask
in six months that the logs need to answer?

**Units.** Metrics exposed, alert thresholds, log retention, trace coverage,
time to detect.

| Usable | Unusable |
|---|---|
| Failed plan changes emit a counter by failure reason; alert at >5 in 10 min | The system should be monitored |
| Every request carries a correlation ID propagated across all internal calls | Logs should be useful for debugging |

> This is the category most often skipped and most often regretted. It is also
> the one where an analyst adds the most value, because it requires knowing
> what questions the business will ask later.

---

### 7. Compatibility and portability

**Ask.** What do we have to keep working that already exists? Which clients,
browsers, versions, or integration partners are in the supported set — and who
decides when something leaves it? What happens to consumers when the contract
changes?

**Units.** Supported versions and platforms, deprecation notice period,
backward-compatibility window.

| Usable | Unusable |
|---|---|
| API v2 remains available for 12 months after v3 GA, with a Sunset header from day one | We should not break existing clients |
| Supports the two most recent major versions of Chrome, Safari, Firefox and Edge | Must work in all modern browsers |

---

### 8. Usability and accessibility

**Ask.** Who is the least experienced person who has to complete this
unaided? What proportion of users are on mobile, on a small screen, or using
assistive technology? Is there a legal accessibility obligation, and in which
jurisdictions?

**Units.** Conformance level, task completion rate and time, error rate,
supported locales.

| Usable | Unusable |
|---|---|
| Conforms to WCAG 2.2 AA; all flows completable by keyboard alone | The interface must be accessible |
| A first-time user completes plan change unaided in under 90 s in usability testing (n≥8) | The interface should be intuitive |

> "Intuitive" is the single least verifiable word in requirements work. It
> almost always means "completable without help", which is measurable.

---

## Working rules

1. **Not every category applies to every scope.** Mark a category
   `not applicable` with one line of reasoning rather than leaving it blank.
   The blank is indistinguishable from an oversight.
2. **Do not invent values.** If the number is unknown, record `status: unknown`
   with an owner and a date. A guessed threshold becomes a contractual one the
   moment somebody reads it.
3. **Every value has a cost.** Before recording a number, ask what would change
   if it were half as strict. If nothing, the number is arbitrary.
4. **Defaults are inherited, overrides are explicit.** Scope-level documents
   reference catalogue identifiers and state only what differs.
5. **An NFR nobody verifies is documentation, not a requirement.** The
   verification field is not optional.

---

## Catalogue

| ID | Category | Statement | Value | Status | Applies to |
|---|---|---|---|---|---|
| NFR-001 | | | | | |
