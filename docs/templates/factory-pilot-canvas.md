---
title: Factory Pilot Canvas
description: A one-page plan for a first (or next) factory workflow — trigger, scope, checks, gates, metrics, promotion thresholds, and what the team will learn — with a fully worked example.
---

# Factory Pilot Canvas

The first factory workflow a team builds tends to decide what the team believes about factories for the next year. If it is ambitious, opaque and hard to reverse — "agents build features from tickets" — the first visible failure confirms every sceptic and the experiment quietly stops. If it is small, boring, measured and reversible, the team gets to watch it succeed, fail, and get fixed in the open, and forms an opinion from evidence rather than from an announcement.

The canvas is a one-page plan for that kind of pilot. It protects **the promotion decision**: the moment, weeks later, when someone proposes giving the workflow more autonomy. By writing down the baseline, the metrics, the threshold and the demotion triggers before anything runs, the canvas makes sure that decision is taken against criteria nobody could bend after seeing the results.

!!! tip "The one-liner"
    Pick something boring, write the threshold down before you start, and let the record — not the demo — decide what happens next.

---

## What problem this solves

Teams that try to build a whole factory at once stall: too many decisions, too many unknowns, too much change for people to absorb. Teams that start small often fail differently. They skip shadow mode, do not count rework, decide promotion criteria after the fact, and leave the workflow without an owner. The result is a pilot that cannot be judged, because nobody knows what it was supposed to achieve.

The canvas puts every one of those decisions on one page before the work starts: why this workflow, where work arrives, what is in and out of scope, the steps and checks, the human gate, the blast radius, what stays human, who designs the checks, what is measured against which baseline, and the thresholds for promotion and demotion. After the pilot window, the same page holds the results and the decision.

---

## When to use this

**Use it when:**

- You are planning your first factory workflow, or the next one.
- An existing interactive use of agents is about to move to a trigger, a schedule or a background runner.
- A workflow is being proposed for promotion and nobody wrote down what it was meant to prove.

**Skip it when:**

- The work needs product judgment, novel architecture or exploratory debugging. Those stay interactive; a canvas will only show that the definition of done cannot be checked.
- You cannot name a workflow owner. Fix that before anything else.
- The team has no baseline at all for the work. Measure the manual version for a couple of weeks first; otherwise every result is a story.

---

## Roles and ownership

| Role                                   | Responsibility                                                                          |
| -------------------------------------- | --------------------------------------------------------------------------------------- |
| **Workflow owner** (one named person)  | Fills in the canvas; accountable for outcomes and metrics; makes the promotion proposal |
| **Check designer** (ideally a sceptic) | Designs the verification and the definition of done                                     |
| **Approvers**                          | Review every output during shadow mode; record rework honestly                          |
| **Harness owner**                      | Provides the environment, checks and telemetry                                          |
| **The team**                           | Reviews the canvas before the pilot and decides on promotion after it                   |

---

## How to use it

### Step 1: Choose a workflow that is boring on purpose

Look for a clear trigger, a checkable definition of done, a small blast radius and a real recurring cost today — something people spend time on or quietly skip. Dependency upgrades, flaky-test quarantine, documentation drift and lint-rule migrations are good candidates.

### Step 2: Capture the baseline before anything changes

How long does the work take now? How often is it skipped? How often does the manual version need rework? Without these numbers, the metrics table has nothing to compare with.

### Step 3: Fill in every section

If a section cannot be filled in, the workflow is probably not ready. "Stays human" and "Blast radius & rollback" are the ones teams most often leave vague. Write the stays-human list somewhere the agent can read it, too.

### Step 4: Hand the checks to the most sceptical person

Ask the person most worried about the pilot to design the checks and the definition of done. Someone who built the verification has much less reason to distrust the result — and often finds the gaps everyone else missed.

### Step 5: Agree the thresholds in advance, in writing

Promotion and demotion criteria go in before the first run, and the team reviews the canvas before the pilot starts. After that, they do not change during the window.

**Tip:** count rework explicitly, every time. A pilot that only counts merged pull requests will always look like a success.

### Step 6: Fill in the results and decide

At the end of the window, the workflow owner fills in the results section, including what failed and what was changed. The team decides: promote, extend the pilot, change the workflow, or stop. All four are legitimate outcomes.

---

## The template

```markdown
# Factory Pilot: <workflow name>

**Workflow owner:** <name>
**Repository / area:** <where> — **Risk tier:** <0 / 1 / 2>
**Pilot window:** <start> → <end>

## Why this workflow
What does it cost today? What gets skipped because nobody has time?

## Trigger (intake)
Where does the work arrive? (dependency bot, label, alert, schedule, chat)

## Scope
In: <what the agent handles>
Out: <what it must not touch>

## Steps
1. <agent step>
2. <agent step>
3. <deterministic step: lint, tests, build>
4. <independent check>
5. Human gate: <who, deciding what>

## Definition of done (checkable)
- [ ] <condition>

## Blast radius & rollback
What happens if it goes wrong? How is it reverted, and by whom?

## Stays human
Decisions this workflow will NOT take, written where the agent can read them.

## Who designs the checks
<name — ideally someone sceptical>

## Metrics
| Metric | Baseline | Target |
| --- | --- | --- |
| Acceptance without rework | | |
| Intervention rate | | |
| Escaped defects | | |
| Cost per accepted change | | |
| Trigger-to-merge time | | |

## Promotion threshold (agreed in advance)
After <N> runs with <X>% accepted without rework and no escaped defects,
we will consider: <next autonomy step>.

## Demotion triggers
- <condition>

## Results (fill in after the window)
| Metric | Result |
| --- | --- |

What worked, what failed, what we changed, what we decided.
```

---

## Worked example

*Illustrative scenario: the team, workflow and numbers below are hypothetical. They show what a completed canvas, including its results, can look like.*

Look at the billing team's CI. About one in twelve main-branch builds fails on a test that passes on retry. Nobody owns flaky tests; engineers re-run the build and move on, and the backlog of known-flaky tests grows. Marta proposed a quarantine workflow as the team's first factory pilot. Daniel, who thought the idea would hide real failures, agreed to design the checks.

```markdown
# Factory Pilot: Flaky-test quarantine

**Workflow owner:** Marta
**Repository / area:** billing monorepo, all test suites — **Risk tier:** 1
**Pilot window:** 2026-09-15 → 2026-10-13 (4 weeks)

## Why this workflow
~1 in 12 main builds fails on a flaky test; engineers re-run and lose
~20 minutes each time. 41 tests are known-flaky; nobody has time to fix them,
so they are never quarantined or tracked.

## Trigger (intake)
CI detects a test that failed and then passed on retry in the same commit,
twice within 7 days. The detector opens an issue labelled `flaky`.

## Scope
In: tests under `**/*.test.ts` outside `services/billing/`.
Out: billing tests (tier 3 area — a flaky billing test may be a real race);
any change to application code; deleting tests.

## Steps
1. Agent reads the test, its failure logs and recent changes to the code under test.
2. Agent classifies the cause (timing, shared state, order dependence, external
   call, unknown) and cites the log lines.
3. Agent opens a PR that adds the test to `quarantine.json` with the issue link
   and a 30-day expiry. No other files change.
4. Independent check (CI): the PR touches only `quarantine.json`; the quarantined
   test still runs in the nightly flaky suite; the expiry is set.
5. Human gate: the owning area's on-duty engineer approves the merge, reading
   the classification.

## Definition of done (checkable)
- [ ] PR changes only `quarantine.json`.
- [ ] Entry has an issue link, a cause classification and an expiry date.
- [ ] The test still runs in the nightly suite.
- [ ] The issue has the cited log lines and a suggested fix, or "unknown".

## Blast radius & rollback
Worst case: a real failure is hidden for up to 30 days. Mitigated by the nightly
suite and the expiry. Rollback: revert the PR; the workflow is disabled by
removing the `flaky` label rule.

## Stays human
Deciding a test is obsolete; fixing application code; quarantining billing tests;
extending an expiry.

## Who designs the checks
Daniel

## Metrics
| Metric | Baseline | Target |
| --- | --- | --- |
| Acceptance without rework | n/a (not done today) | ≥ 85% |
| Intervention rate | n/a | ≤ 10% of runs |
| Escaped defects (real bug quarantined) | n/a | 0 |
| Cost per accepted change | ~30 min engineer time when done by hand | < $2 + < 5 min review |
| Trigger-to-merge time | never (backlog of 41) | < 1 working day |
| Main builds failing on known flaky tests | ~8% | < 2% |

## Promotion threshold (agreed in advance)
After 30 runs with ≥ 90% accepted without rework and no escaped defects,
we will consider sampled approval (1 in 5), then tier 0 auto-merge.
(The ≥ 85% target says the pilot is worth continuing; promotion asks for more.)

## Demotion triggers
- Any quarantined test later found to hide a real bug → back to approve-every-output.
- Acceptance without rework below 80% over two weeks → pause and review.
- Anyone may pause the workflow with `factory-pause`.

## Results (fill in after the window)
| Metric | Result |
| --- | --- |
| Runs | 37 |
| Accepted without rework | 31 / 37 (84%) |
| Intervention rate | 3 / 37 (8%) |
| Escaped defects | 1 (see below) |
| Cost per accepted change | ~$0.90 tokens + ~3 min review (median) |
| Trigger-to-merge time | median 5h |
| Main builds failing on known flaky tests | 8% → 1.5% |

What worked: classification was right on 31/37; the backlog of 41 dropped to 9.
What failed: the other 6 runs were misclassified causes. 5 were caught
at the gate and reworked (agent said "timing" for
shared-state leaks). 1 escape: a quarantined test in `workers/` was hiding a
real race in the retry queue (the agent had called it "timing"); the
nightly suite caught it on day 9.
What we changed: added a check that diffs the test's recent failure rate
against code changes in the module under test (HCR-030); added "a module
changed in the last 7 days" to the stays-human list.
Decision: not promoted. Extend 4 weeks at approve-every-output; threshold
unchanged; count restarts from the escape.
```

The decision at the end is the most valuable line on the page. The pilot missed its threshold, and the team said so, kept the threshold, and extended rather than abandoning a workflow that had cut flaky failures on main from 8% to 1.5%. The escape was caught by the safety net Daniel insisted on, and became a new check. That is what a healthy pilot looks like: a visible failure, a fix in the open, and no goalposts moved.

---

## Signals it is working

- The first workflow is deliberately unglamorous, and the team can explain why it was chosen.
- Every output is approved by a person until the threshold is met.
- Rework is counted every time, including the embarrassing cases.
- The promotion decision cites the results table, not a demo.
- Failures produce harness changes with records, not quiet workarounds.
- A second canvas follows, and it is visibly shaped by what the first one taught.

---

## Failure modes and mitigations

The classic failure is a pilot that cannot fail. The thresholds are vague, rework is not counted, the window keeps being extended, and after three months the workflow is "basically working" without anyone being able to say what that means. Meanwhile it has quietly started auto-merging because approving every output got tedious.

| Failure mode                       | What it looks like                         | Mitigation                                         |
| ---------------------------------- | ------------------------------------------ | -------------------------------------------------- |
| **Starting with the exciting one** | First pilot is "agents build features"     | Bounded, checkable, reversible work first          |
| **No baseline**                    | Results cannot be compared with anything   | Measure the manual version before the pilot        |
| **Skipping shadow mode**           | Auto-merge from day one                    | A person approves every output until the threshold |
| **Moving goalposts**               | Threshold redefined after results arrive   | Agree and record it before the first run           |
| **Uncounted rework**               | Fixes happen in follow-up PRs nobody links | Track rework explicitly on every run               |
| **Owner-less workflow**            | "The bot did it"                           | One named workflow owner                           |

---

## Related pages

- [Your First Factory Workflow](../factories/first-factory-workflow.md) — Choosing the workflow and the four stages from mirror to measure.
- [Risk Tier Matrix](risk-tier-matrix.md) — Where the pilot sits and how promotion is recorded.
- [Measuring Impact Honestly](../team/measuring-impact.md) — Baselines, and the metrics worth trusting.
- [Adoption Without Mandates](../team/adoption-without-mandates.md) — Why a visible, boring first workflow moves people.
