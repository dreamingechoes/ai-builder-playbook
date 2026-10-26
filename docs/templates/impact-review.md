---
title: Impact Review
description: A template for reviewing what shipped work achieved at the end of a pilot, at 7 days and at 30 days — expected versus observed, what users say, and an explicit keep, iterate, or reconsider decision — with a fully worked example.
---

# Impact Review

Most teams agree that checking what a feature actually achieved is important. Few do it consistently. As I wrote in [The AI harness should follow the conversation](https://dreamingecho.es/blog/the-ai-harness-should-follow-the-conversation), the reason is usually not a lack of care. Once something ships, the natural urgency disappears: no deployment waiting, no pull request blocking anyone, no incident demanding attention. By the time the thirty-day mark arrives, the next piece of work is already louder, and the review that would have told you the feature was not used — or was used in a way nobody expected — never takes place.

The Impact Review exists to protect a valuable step that has no natural urgency at all. It returns to the hypothesis the work started from, compares what was expected with what was observed, and ends with an explicit decision: **keep, iterate, or reconsider**. A harness is well suited to the preparation, because it can remember what the work was meant to achieve and assemble the evidence without anyone having to remember to ask. The decision stays with people, ideally after they have spoken to someone the change affected.

!!! tip "The one-liner"
    Write the hypothesis before release, let the harness prepare the evidence, and make a person decide out loud.

---

## What problem this solves

Agents increase how much a team ships. They do not, on their own, increase how much a team learns. Without a deliberate review, throughput goes up while product understanding stays flat: more features, no clearer idea which ones mattered. The cost shows up later as roadmap debates based on opinion, features nobody dares remove, and the same mistaken assumption made twice.

The template keeps the review short and comparable across initiatives. It records the original hypothesis, puts expected and observed signals side by side with their sources, makes room for surprises and for what users say, states what the evidence cannot tell you, and forces a decision with reasoning. Its last section asks what the harness got wrong on this piece of work, which turns every review into input for the [steering loop](../harness/guides-and-sensors.md).

---

## When to use this

**Use it when:**

- Shipped work had a hypothesis: it was meant to change a behaviour, a metric or a cost.
- At the end of a pilot or staged rollout, at 7 days, and at 30 days.
- A factory workflow has run long enough to judge its effect on the team, not just its acceptance rate.

**Skip it when:**

- The change is internal, invisible, and had no hypothesis — a dependency bump or a refactor. Mark it "not applicable" on the issue with that reason.
- The day-7 checkpoint for a small change would only repeat the rollout status. Keep day 30.
- Nobody will make a decision from it. Fix the ownership first; a report nobody discusses is worse than none, because it looks like learning.

---

## Roles and ownership

| Role                                 | Responsibility                                                                                               |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **Product partner**                  | Owns the hypothesis, the success signals and the keep/iterate/reconsider decision                            |
| **Engineer who led the work**        | Co-owns the review; explains what the signals mean technically; owns the harness section                     |
| **Agent**                            | Retrieves the hypothesis, assembles evidence, fills the observed column, drafts surprises and open questions |
| **Engineering manager**              | Makes sure reviews happen on schedule and that their decisions reach planning                                |
| **Support or customer-facing teams** | Contribute what users are saying that never reaches a dashboard                                              |

---

## How to use it

### Step 1: Record the hypothesis before release

Fill in the hypothesis and the expected column before the change reaches users. Include at least one guardrail signal — something that should not get worse. Success criteria written after launch always describe what happened.

### Step 2: Let the harness schedule and prepare the checkpoints

The review should appear without anyone remembering it. At each checkpoint, an agent retrieves the hypothesis, queries the sources, fills in the observed column with links, and lists anything unexpected. It should say plainly where data is missing rather than estimating.

**Tip:** ask the agent to separate "observed" from "interpreted". The observed column is numbers and links; interpretation belongs to people.

### Step 3: Talk to at least one person the change affected

Before deciding, the owner speaks to a user, a customer-facing colleague or a stakeholder. The metric shows that something happened; a conversation shows why it matters. Record who was spoken to.

### Step 4: Write down what the evidence cannot tell you

Small samples, confounding launches, seasonality, metrics that only proxy for the outcome. Writing them down keeps the decision honest and makes the next review better.

### Step 5: Decide, with reasoning

Tick one box and write the reasoning in a few sentences. "Iterate" needs a specific next change. "Reconsider" needs a reason and, if it means rollback, an owner.

### Step 6: Feed the result forward

Link the decision to the next piece of work, and turn the harness section into changes where something recurred. A review that does not change anything downstream did not finish.

---

## The template

```markdown
# Impact Review: <initiative> — <checkpoint: pilot end | day 7 | day 30>

**Owner:** <product + engineering>
**Shipped on:** <date> — **Exposure at review:** <%>
**Original framing:** <link>

## Hypothesis
We believed that <change> would <effect> for <whom>, which we would see as <signal>.

## Expected vs observed
| Signal | Expected | Observed | Source |
| --- | --- | --- | --- |
| <primary metric> | | | |
| <guardrail metric> | | | |
| <support / feedback volume> | | | |

## What changed that we did not expect

## What users and stakeholders say
<quotes or summary; who was spoken to>

## What the evidence cannot tell us

## Decision
- [ ] Keep as is
- [ ] Iterate: <what>
- [ ] Reconsider / roll back: <why>

**Reasoning:**

## What this means for what we do next
<links to issues, framing docs, roadmap changes>

## What the harness got wrong on this initiative
<where agents guessed, which checks missed something, which gate was a formality>
```

---

## Worked example

*Illustrative scenario: the team, feature and numbers below are hypothetical. They show what a completed day-30 review can look like.*

This continues the invoice-reminder feature from the [Human Gate Definition](human-gate-definition.md) example. At the 25% rollout stage, customer calls showed that reminders were reaching finance inboxes nobody read, and the team changed the recipient logic before going to 100%. Thirty days after full release, the harness opened the review, filled in the observed column, and assigned it to Aisha and Kofi.

```markdown
# Impact Review: Automated invoice reminders — day 30

**Owner:** Aisha (product), Kofi (engineering)
**Shipped on:** 2026-09-22 (100%) — **Exposure at review:** 100%
**Original framing:** <link to framing doc>

## Hypothesis
We believed that sending a reminder 3 days after an invoice's due date would
reduce late payment for small-business customers, which we would see as a
higher share of invoices paid within 7 days of the due date.

## Expected vs observed
| Signal | Expected | Observed | Source |
| --- | --- | --- | --- |
| Invoices paid within 7 days of due date | 62% → 68% | 62% → 69% | Billing warehouse, `invoice_payments` |
| Same, for accounts with an assigned billing contact | — | 63% → 74% | Same, split by contact field |
| Unsubscribe from reminders (guardrail) | < 3% | 1.8% | Email provider |
| Support tickets tagged `billing-reminder` (guardrail) | < 20 / week | 11 / week, falling | Support tool |
| Disputed charges (guardrail) | no change | no change | Payments dashboard |

## What changed that we did not expect
Accounts with a named billing contact improved far more than others. Accounts
without one improved only slightly (61% → 64%). 14 customers asked for a second
reminder at 10 days.

## What users and stakeholders say
Spoke to 3 customers and the support lead. Customers with a billing contact
called the reminder "the email we needed"; one owner said reminders still go
to her personal inbox and she forwards them. Support says most tickets are
"can this go to someone else?".

## What the evidence cannot tell us
Whether part of the gain is seasonal (September has historically better
payment rates; last year's +2pt is a rough comparison only). Revenue impact
beyond payment timing. Long-term reminder fatigue.

## Decision
- [ ] Keep as is
- [x] Iterate: prompt account owners without a billing contact to add one;
      test a second reminder at 10 days behind a flag
- [ ] Reconsider / roll back

**Reasoning:** The primary signal met the target and no guardrail moved. The
effect is concentrated where the reminder reaches the right person, which
matches what we heard at the 25% stage. The cheapest next gain is getting the
email to the right recipient, not sending more email.

## What this means for what we do next
- New framing: "Billing contact setup" (linked).
- Flagged experiment: second reminder at day 10, 10% of accounts.
- Review again at day 90 to check for fatigue.

## What the harness got wrong on this initiative
- The rollout agent recommended advancing at 25% on metrics alone; the hold
  came from customer calls. Expected, but worth keeping visible.
- The impact agent's first draft compared against last month, not the
  pre-launch baseline in the framing doc. Fixed: the skill now reads the
  baseline from the framing doc (HCR-034).
```

The interesting part is the second row of the table. It was not in the original hypothesis; the agent added it because the team had asked it to split results by the recipient change made during rollout. That split turned a reasonable "keep" into a more useful "iterate" — and it was a person's conversation at the rollout gate, not a metric, that made the split worth looking for.

The harness section is short and specific. One line records an expected limitation, the other a real mistake with a fix and a change record. That is the right size.

---

## Signals it is working

- Reviews happen at 7 and 30 days without anyone scheduling them by hand.
- Hypotheses and expected signals are recorded before release, and nobody rewrites them later.
- Decisions are explicit, and "iterate" and "reconsider" appear as often as the evidence warrants — not just "keep".
- Reviews name who was spoken to.
- New framing documents cite earlier impact reviews.
- The harness section regularly produces change records.

---

## Failure modes and mitigations

The usual decay is from decision to report. The first reviews are discussed in a meeting and change the roadmap. Then they become documents the agent produces on schedule, linked in a channel, read by nobody. The template still gets filled in — by the agent — and the decision box is ticked "keep" by default, because nobody wants to argue with a feature that already shipped.

| Failure mode                       | What it looks like                           | Mitigation                                              |
| ---------------------------------- | -------------------------------------------- | ------------------------------------------------------- |
| **Rewritten success criteria**     | Success criteria written after launch        | Hypothesis and expected column filled before release    |
| **Report without decision**        | Generated, linked, never discussed           | The decision is a human gate with a named owner         |
| **Default "keep"**                 | Every review concludes nothing should change | Require reasoning; track the mix of decisions over time |
| **Dashboard-only evidence**        | No user or stakeholder voice                 | Record at least one conversation per review             |
| **False precision**                | The agent estimates missing data             | Observed means sourced; gaps are written as gaps        |
| **Learning that stops at the doc** | Next work never cites the review             | Link the decision to the next issue or framing          |

---

## Further reading

- **[Shipping Debrief: The Work Is Not Done When It Ships](https://dreamingecho.es/blog/shipping-debrief-work-not-done)** by Iván González Sáiz — Why post-launch learning is lost unless a team protects it deliberately.
- **[The AI harness should follow the conversation](https://dreamingecho.es/blog/the-ai-harness-should-follow-the-conversation)** by Iván González Sáiz — Using a harness to preserve steps that have no natural urgency, such as the 7- and 30-day review.

---

## Related pages

- [Rollout & Learning](../workflow/rollout-and-learning.md) — Staged exposure, and returning to the hypothesis at 7 and 30 days.
- [Measuring Impact Honestly](../team/measuring-impact.md) — Baselines, guardrails and the metrics worth distrusting.
- [Memory & Continuity](../harness/memory-and-continuity.md) — How the harness remembers what work was meant to achieve.
- [Human Gates](../harness/human-gates.md) — Why the impact decision is a judgment gate.
