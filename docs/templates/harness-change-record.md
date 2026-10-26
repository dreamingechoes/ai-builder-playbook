---
title: Harness Change Record
description: A short record for every change to the harness — what changed, which failure or decision prompted it, how it was evaluated, what it costs, and when it should be revisited or removed — with fully worked examples.
---

# Harness Change Record

Harnesses grow by accretion. A rule is added after an incident, a skill because someone wrote it, a reviewer agent because removing it feels risky. Each addition is reasonable on the day. A year later, nobody can say which controls still earn their place, which were workarounds for a model two generations old, and which quietly make outcomes worse. Removing anything feels dangerous, because nobody remembers why it is there — so nothing is removed, and the harness only ever gets heavier.

The Harness Change Record is a short note written with every non-trivial harness change. It protects **the future decision to keep, change or remove that control**. It does that by recording the two things the future decision will need: the reason the control exists, and the condition under which it should be revisited. Every control should have both. Over time, the collection of these records becomes the most useful document about how the team works — and the safest way to know what can be deleted.

!!! tip "The one-liner"
    Every control has a reason and an expiry; write both down when you add it, so someone can safely remove it later.

---

## What problem this solves

Without records, a harness can only be changed by people who remember its history, and memory does not survive team changes or busy quarters. Model upgrades make this worse. A newer model may no longer need a workaround, or may behave differently enough that an old rule starts to hurt. Without knowing why a rule exists, nobody can test whether it is still needed.

The record is deliberately small: what changed, why, what else was considered, how it was evaluated, what it costs, and when to review it. It distinguishes changes to a team agreement from implementation changes, so agreements are not quietly rewritten as technical details. And it treats removals as first-class changes, because the reverse steering loop — taking controls out when they stop earning their place — is as important as the forward one.

---

## When to use this

**Use it when:**

- You add, change or remove an instruction, skill, hook, check, gate, risk-tier rule, tool or memory source.
- A postmortem or impact review produces a harness change.
- A model upgrade leads you to remove or relax a control.
- A gate's inputs or owner change.

**Skip it when:**

- The change is a typo, formatting or a broken link in a doc.
- You are experimenting locally and nothing reaches the shared harness. Write the record when it does.

---

## Roles and ownership

| Role                     | Responsibility                                                          |
| ------------------------ | ----------------------------------------------------------------------- |
| **Author of the change** | Writes the record in the same pull request as the change                |
| **Harness owner**        | Reviews records; runs the expiry review on model upgrades and quarterly |
| **The team**             | Decides changes marked as agreement changes                             |
| **Reviewers**            | Ask for the record when it is missing; challenge weak "why" sections    |

---

## How to use it

### Step 1: Write it in the same pull request as the change

The record lives next to the harness — for example in `docs/harness/changes/` — and the change links it. A record written later is a reconstruction.

### Step 2: Make the "why" concrete

Link the failure, incident or decision that prompted it. "It happened twice" is a good reason; "it might happen" usually is not. If the reason is a team decision, mark it as an agreement change and link the decision.

### Step 3: Record what you rejected

Say what else could have addressed the problem and why this won. In particular, if you chose a guide (an instruction or skill) for a failure a script could check, say why. Prefer sensors when the failure is checkable.

### Step 4: Say how you know it helps

A with/without comparison on a fixed task set, eval results, or a plain statement of why no evaluation was needed. Several runs, not one.

### Step 5: Write down the cost

Added context and tokens, latency, and — easiest to forget — human attention. A new gate costs someone's time on every run.

### Step 6: Set an expiry or review condition

Make it something you can check: "remove when the model reliably does X without being told", "review after the billing migration", "review at the next model upgrade". On each upgrade and each quarter, the harness owner filters records by expiry and re-runs the relevant evals with those controls removed.

**Tip:** number records sequentially and never reuse a number. Removal records should link the record that added the control.

---

## The template

```markdown
# HCR-<number>: <short title>

**Date:** <date> — **Author:** <name>
**Type:** add | change | remove
**Component:** instruction | skill | hook | check | gate | tier | tool | memory
**Agreement change?** yes (team decision: <link>) | no (implementation only)
**Related records:** <HCR links, postmortem, impact review>

## What changed

## Why
<The failure, incident, or decision that prompted it. Link evidence.
"It happened twice" is a good reason; "it might happen" usually is not.>

## Alternatives considered
<What else could have addressed it, and why this won.
Prefer a sensor over a guide when the failure is checkable.>

## Evaluation
<With/without comparison, eval results, or why none was needed.>

## Cost
<Added context, latency, tokens, human attention.>

## Expiry / review condition
<e.g. "Remove when the model reliably does X without being told",
"Review after the billing migration", "Review at next model upgrade".>
```

---

## Worked example

*Illustrative scenario: the team, records and numbers below are hypothetical. They show two completed records — an addition and a later removal — so you can see the full life of a control.*

Start with the billing team after the August migration incident described in the [Agent Incident Postmortem](agent-incident-postmortem.md) example. The postmortem produced five harness changes. This is the record for the main sensor:

```markdown
# HCR-024: Lock check for migrations in CI

**Date:** 2026-08-19 — **Author:** Lena
**Type:** add
**Component:** check
**Agreement change?** no (implementation only)
**Related records:** postmortem 2026-08-14; HCR-025 (skill); HCR-026 (tier)

## What changed
CI runs `scripts/check_locks.sh` on every file under `db/migrations/`. It fails
when a migration would take a blocking lock on a table with more than 1M
estimated rows, and prints the table, the lock type and a link to
`docs/database.md#concurrent-indexes`.

## Why
The 2026-08-14 incident: a non-concurrent index build blocked writes to
`orders` for ~6 minutes. Two earlier near misses in 2025 (linked) had the same
cause and were caught in review by chance.

## Alternatives considered
- A line in AGENTS.md: that was the pre-incident state ("be careful with
  migrations") and it failed. Rejected as the main control.
- AI review prompt to look for locks: inferential, can be argued with, and it
  missed the original PR. Rejected.
- Check in CI (chosen): computational, fast, fails with an agent-readable
  message, and applies to human-written migrations too.

## Evaluation
Replayed 14 migrations from the last year: the check flagged the incident
migration and both near misses; no false positives. Eval task
`migration-add-index-large-table` now fails without the check and passes with
it (5/5 runs).

## Cost
~4s per CI run that touches migrations; nothing elsewhere. No added context.

## Expiry / review condition
Keep while we run Postgres and agents write migrations. Review if we move to a
migration tool that enforces concurrent builds itself.
```

Two months later, during the quarterly review and while preparing the evaluation of a new model version, the harness owner ran the reverse loop on the current model, removing groups of instruction lines and re-running the evals. One rule from early in the year turned out to be doing nothing:

```markdown
# HCR-039: Remove "always run typecheck before finishing" from AGENTS.md

**Date:** 2026-10-23 — **Author:** Marta
**Type:** remove
**Component:** instruction
**Agreement change?** no (implementation only)
**Related records:** HCR-007 (added the line, 2026-02)

## What changed
Removed the line "Always run `pnpm typecheck` before saying you are done" from
the root AGENTS.md.

## Why
HCR-007 added it because the agent often finished without typechecking. Since
HCR-019, the definition of done runs typecheck in a pre-finish hook, so the
line duplicates a computational control. HCR-007's expiry said "remove once
typecheck is enforced by a hook".

## Alternatives considered
Keeping it as a reminder. Rejected: the hook already fails with a clear
message; the line costs context on every task for no benefit.

## Evaluation
Golden set (32 tasks, 3 runs) with and without the line on the current model:
accepted without rework 63% vs 63%; typecheck failures at finish 0 vs 0.

## Cost
Saves one line of always-on context. No other effect.

## Expiry / review condition
n/a (removal). Reinstate only if the hook is removed.
```

Read the pair together. The first record names the incident, explains why a check beat an instruction, shows the evaluation and gives a realistic review condition. The second shows the value of the expiry written eight months earlier: Marta did not have to guess whether the line was safe to delete, because HCR-007 had already said when it would be. The harness got slightly smaller, and nobody had to be brave.

---

## Signals it is working

- Every harness change in the history has a record linked from its pull request.
- Records include removals, and removals link the records that added the control.
- "Why" sections link to incidents, postmortems or decisions, not general caution.
- Model upgrades trigger a review of records by expiry, and some controls come out.
- Agreement changes are visibly different from implementation changes, and link a team decision.
- New engineers read the records to understand how the harness came to be.

---

## Failure modes and mitigations

Records usually fail by becoming bureaucracy. They get long, so people skip them for "small" changes, so the history gets gaps, so nobody trusts it. Or they get written but the expiry field says "n/a" every time, which turns a removal tool into an archive. The test is simple: in the last quarter, did a record make a removal easier? If not, the expiry conditions are not doing their job.

| Failure mode                    | What it looks like                                 | Mitigation                                          |
| ------------------------------- | -------------------------------------------------- | --------------------------------------------------- |
| **Missing records**             | Controls with no history                           | Reviewers ask for the record in the PR              |
| **Vague "why"**                 | "Best practice", "to be safe"                      | Link a failure, incident or decision                |
| **No expiry**                   | Every record says "n/a"                            | Require a checkable review condition for additions  |
| **Only additions**              | The harness grows every month                      | Run the reverse loop on upgrades; record removals   |
| **No evaluation**               | "It feels better"                                  | With/without comparison, or say why none was needed |
| **Agreements changed silently** | Team decisions rewritten as implementation details | Mark agreement changes and link the decision        |

---

## Related pages

- [Testing & Evolving the Harness](../harness/testing-and-evolving.md) — Evals, the reverse loop, and why controls need expiry conditions.
- [Guides & Sensors](../harness/guides-and-sensors.md) — The steering loop that produces most records, and why sensors usually beat guides.
- [Ownership & Change](../harness/ownership-and-change.md) — Who may change agreements, and how.
- [Agent Incident Postmortem](agent-incident-postmortem.md) — Where many records start.
