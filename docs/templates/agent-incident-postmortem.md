---
title: Agent Incident Postmortem
description: A blameless postmortem template for incidents involving agent-built or agent-run work — focused on what in the harness allowed it, not on who approved it — with a fully worked example.
---

# Agent Incident Postmortem

When an agent-written change causes an incident, two unhelpful conversations are available. One blames the agent, as if a model could be held responsible for anything. The other blames the person who approved the change, as if one approval should have caught what the whole system let through. Neither prevents the next incident. Both teach the team the wrong lesson: the first, that agents cannot be trusted at all; the second, that approving agent work is dangerous, so it should be done slowly or not at all.

This template keeps the conversation where it belongs: on the system. It protects **the decision about what the harness should change**, so the same class of failure becomes less likely or impossible. Authorship spreads across agents; responsibility does not. A named person still stands behind the change, and that is exactly why the review has to be blameless — people only tell the truth about what they saw at a gate when doing so is safe.

!!! tip "The one-liner"
    Ask what the harness allowed, not who approved it, and do not close the postmortem until every finding is a harness change with an owner.

---

## What problem this solves

Ordinary postmortem templates work for agent incidents up to a point, but they miss the questions that matter most here. What was the agent actually asked to do, and what did it do instead? What did it report as verified, and what did it say it could not verify? What did the person at each gate see — and was it enough to decide? Which part of the harness — guide, sensor, gate, tier, boundary — failed or was missing?

The template adds those sections. It reconstructs the agent run from its transcript and the change history, lays out each human gate with what the approver saw, and walks through a checklist of harness causes. Then it requires two outputs: harness changes with owners, and a new task in the eval set so the failure is tested from now on. Without those, the review produces understanding but no protection.

---

## When to use this

**Use it when:**

- An agent wrote the change that caused an incident, ran the action that caused it, or was responsible for a check that missed it.
- A near miss revealed that a gate, sensor or boundary did not work as the team assumed.
- An agent did something outside its intended scope, even without user impact — for example, reading files or calling tools it should not have.
- Content in the agent's context may have steered it (possible prompt injection).

**Skip it when:**

- The agent's involvement was incidental — it wrote a line nobody would have written differently. Use your normal postmortem, and mention it there.
- The issue is trivial and already fixed by an existing steering-loop change. A [Harness Change Record](harness-change-record.md) is enough.

---

## Roles and ownership

| Role                                                             | Responsibility                                                                           |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Postmortem author** (often the incident lead or harness owner) | Reconstructs the run and the gates; drafts findings; chases actions to completion        |
| **Approvers involved**                                           | Describe honestly what they saw and why they decided; they are witnesses, not defendants |
| **Harness owner**                                                | Retrieves transcripts and telemetry; owns most harness changes that result               |
| **Area owner**                                                   | Judges whether the risk tier and checks for the area were right                          |
| **Engineering manager**                                          | Keeps the review blameless; makes sure actions get time                                  |

---

## How to use it

### Step 1: Reconstruct the run before forming opinions

Pull the intent and plan the agent worked from, the transcript or trace, the pull request, the checks that ran, and the approvals. Write the timeline from these records, not from memory. If transcripts were not retained, that is your first finding.

### Step 2: Compare what was asked with what was done

Write both down side by side. Many agent incidents are not wrong answers to the right question; they are confident answers to a slightly different one. Note what the agent reported as verified and what it said it could not verify. A missing "not verified" section is itself a signal.

### Step 3: Lay out each gate, and what the person saw

For every gate the change passed, record who decided, what was on their screen, and how long the decision took. The question is not whether they should have caught it, but whether the inputs made catching it possible.

### Step 4: Work through the harness checklist

Tick every cause that applies, not just the most obvious one. Most agent incidents have two or three contributing causes: a missing guide, a sensor that was not independent, a tier that was too low.

**Tip:** for each ticked cause, ask "would a computational sensor have caught this?" If yes, prefer a check over a new instruction.

### Step 5: Turn findings into harness changes, and the incident into an eval

Every finding becomes a change with a type, an owner and a due date. Record each one with a Harness Change Record. Add the incident's starting point and task to the eval set as a regression case.

### Step 6: Close only when the actions are done

The status moves to "actions complete" when the changes have shipped and the eval task passes, not when the document is written.

---

## The template

```markdown
# Agent Incident: <title>

**Date:** <date> — **Severity:** <level> — **Author:** <name>
**Status:** draft | reviewed | actions complete

## Summary
<What happened, in three sentences.>

## Impact
<Users, data, money, time.>

## Timeline
| Time | Event |
| --- | --- |

## The agent run
- Agent / model / harness version:
- Intent and plan it worked from: <link>
- Transcript: <link>
- What it was asked to do vs what it did:
- What it reported as verified / not verified:

## The human gates
| Gate | Who | What they saw | Decision (and time taken) |
| --- | --- | --- | --- |

## What in the harness allowed this?
- [ ] Missing or unclear intent / acceptance criteria
- [ ] Wrong or stale context (docs, instructions, memory)
- [ ] Missing sensor (test, check, scanner)
- [ ] Sensor present but not independent of the agent
- [ ] Gate inputs insufficient, or gate rubber-stamped
- [ ] Wrong risk tier
- [ ] Boundary too broad (permissions, sandbox, network)
- [ ] Untrusted content in context (possible injection)
- [ ] Other:

## What went well

## Harness changes
| Change | Type (guide/sensor/gate/tier/boundary) | Owner | Due | Change record |
| --- | --- | --- | --- | --- |

## Added to eval set
<task id>
```

---

## Worked example

*Illustrative scenario: the team, incident and timings below are hypothetical. They show what a completed agent postmortem can look like.*

Go back to the billing team in August. The account page had become slow for large customers, and an engineer asked an agent to add an index to speed it up. The change was small, the tests passed, and it was approved within minutes. It took a lock on the `orders` table during peak hours. This is the incident that later produced the `write-migration` skill in the [Skill Template](skill-template.md) example.

```markdown
# Agent Incident: Index migration locked the orders table

**Date:** 2026-08-14 — **Severity:** SEV-2 — **Author:** Marta
**Status:** actions complete

## Summary
An agent-written migration added an index on `orders.customer_id` without
building it concurrently. On deploy, it held a lock that blocked writes to
`orders` for about six minutes during peak hours. New subscriptions and
invoice payments failed or timed out until the migration finished.

## Impact
~6 minutes of failed writes to `orders`; ~140 failed checkout attempts, of
which ~90 retried successfully within the hour. No data loss.

## Timeline
| Time (UTC) | Event |
| --- | --- |
| 10:02 | Kofi asks the agent to "add an index on orders.customer_id" (issue link) |
| 10:09 | Agent opens PR; tests and lint pass; summary says "small, safe migration" |
| 10:14 | AI review comments on naming only |
| 10:16 | Kofi approves; PR merges; deploy starts |
| 10:21 | Migration starts; writes to `orders` begin to block |
| 10:23 | Alerts fire on checkout error rate |
| 10:27 | Migration completes; errors stop |
| 10:40 | Incident declared resolved; postmortem opened |

## The agent run
- Agent / model / harness version: <agent>, <model>, harness v14
- Intent and plan it worked from: one-line issue; no plan (treated as tier 2)
- Transcript: <link>
- Asked vs did: asked for an index to speed up the account page; it added a
  correct index, using a plain `CREATE INDEX`
- Reported verified: tests, lint, migration runs locally (on a 2k-row table).
  Reported NOT verified: nothing listed.

## The human gates
| Gate | Who | What they saw | Decision (and time taken) |
| --- | --- | --- | --- |
| Merge | Kofi | 3-line diff, green checks, "small, safe migration" | Approved (2 min) |

## What in the harness allowed this?
- [ ] Missing or unclear intent / acceptance criteria
- [x] Wrong or stale context — AGENTS.md said only "be careful with migrations"
- [x] Missing sensor — no check for lock behaviour or table size
- [ ] Sensor present but not independent of the agent
- [x] Gate inputs insufficient — the summary asserted safety with no evidence,
      and nothing showed table size
- [x] Wrong risk tier — `db/migrations/**` was not classified; defaulted to tier 2
- [ ] Boundary too broad
- [ ] Untrusted content in context
- [ ] Other

## What went well
Alerting caught it within two minutes. The migration was correct apart from
how it was built, so no rollback was needed. Kofi raised the incident himself.

## Harness changes
| Change | Type | Owner | Due | Change record |
| --- | --- | --- | --- | --- |
| `check_locks.sh` in CI for any migration; fails on blocking index builds on tables > 1M rows | sensor | Lena | 2026-08-21 | HCR-024 |
| `write-migration` skill with lock evidence and stop conditions | guide | Lena + Kofi | 2026-08-28 | HCR-025 |
| `db/migrations/**` classified as tier 3; area-owner merge | tier | Marta | 2026-08-20 | HCR-026 |
| PR summaries must include a "not verified" section; CI fails if empty | sensor | Marta | 2026-08-28 | HCR-023 |
| Remove "be careful with migrations" from AGENTS.md; point to the skill | guide | Marta | 2026-08-28 | HCR-027 |

## Added to eval set
`migration-add-index-large-table` (5 runs, regression)
```

Notice what the review did not say. It did not conclude that Kofi should have read more carefully: he saw a three-line diff, green checks and a confident summary, and nothing on his screen mentioned a large table. Four causes were ticked, and each produced a change. The strongest is a computational sensor — a script that measures lock behaviour — because a check cannot be talked out of a failure. The vague instruction that failed was removed rather than rewritten, because a skill with evidence replaces it.

---

## Signals it is working

- Postmortems produce harness changes, each with an owner and a change record.
- Approvers describe what they saw openly, and nobody is disciplined for an approval.
- Each incident becomes an eval task, and the eval set grows from real failures.
- Contributing causes are plural; reviews rarely stop at the first explanation.
- The same class of incident does not recur.
- Transcripts are available when needed, because retention was decided in advance.

---

## Failure modes and mitigations

The most common failure is the instruction-only fix. The review ends with "add a line to `AGENTS.md`", because it is fast and feels responsive. Nothing measures whether it works, the line competes with two hundred others, and a few months later a variant of the same incident happens. The second most common is quiet blame: nobody says it, but the approver starts reviewing everything slowly, and the team's throughput drops for reasons nobody wrote down.

| Failure mode                 | What it looks like                      | Mitigation                                                |
| ---------------------------- | --------------------------------------- | --------------------------------------------------------- |
| **"The agent did it"**       | No human owner, no system change        | A named owner per change; findings become harness changes |
| **Blaming the approver**     | The person at the gate takes the weight | Ask what the inputs made possible; keep it blameless      |
| **Instruction-only fixes**   | Every action is a new line in AGENTS.md | Prefer sensors where the failure is checkable             |
| **Lost transcripts**         | The run cannot be reconstructed         | Retention policy for traces, decided before incidents     |
| **No eval added**            | The fix is never tested again           | Every incident becomes a regression task                  |
| **Actions that never close** | Postmortem "done" when written          | Close only when changes ship and the eval passes          |

---

## Related pages

- [Accountability & Audit Trail](../security/accountability-and-audit.md) — Who owns agent work, and what to record so a run can be reconstructed.
- [Guides & Sensors](../harness/guides-and-sensors.md) — The steering loop that turns incidents into checks.
- [Testing & Evolving the Harness](../harness/testing-and-evolving.md) — Turning incidents into eval tasks.
- [Agent Threat Model](../security/agent-threat-model.md) — When the cause may be untrusted content in context.
- [Harness Change Record](harness-change-record.md) — Recording each change the postmortem produces.
