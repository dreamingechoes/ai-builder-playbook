---
title: Tool Scorecard
description: A template for evaluating a coding agent, model, or AI tool on your own work — quality, cost, workflow fit, security, and what engineers think — with a fully worked example.
---

# Tool Scorecard

Teams choose AI tools in ways they would never accept for a database. A leaderboard position, a demo that went well, the strongest opinion in the channel. Months later they discover the tool struggles with their stack, costs more than expected at their volume, or does not fit how they actually work — and switching is now expensive, so they live with it or churn again. Meanwhile a new model arrives every few weeks, and each one reopens the question.

The Tool Scorecard records an evaluation run on your own work: the same tasks, the same harness, several runs each, graded on outcomes. It protects **the adoption decision** — adopt, trial further, or reject — and makes it comparable with the next one. The most useful benchmark a team has is its own recent work; the scorecard is how you turn that work into a decision someone can defend six months later.

!!! tip "The one-liner"
    Replay your own tasks, with your own harness, several times each; decide on cost per accepted change, security and what engineers say — not on a demo.

---

## What problem this solves

Public benchmarks track the field well, but they use public repositories, well-specified tasks and clean environments. None of that describes your codebase on a Tuesday afternoon. Demos are worse: one run, chosen by the vendor, on a task picked to impress.

The scorecard replaces both with a small, repeatable evaluation. It records the task set and harness version, so results can be compared over time. It measures quality the way the rest of this playbook does — acceptance without rework and interventions, not just "checks pass". It puts cost per accepted task next to quality, because a tool that is slightly better and three times more expensive is a different decision. And it gives security, workflow fit and engineers' experience their own sections, because a tool that wins on paper and that nobody wants to use will not be used well.

---

## When to use this

**Use it when:**

- Choosing a primary coding agent, IDE or model for the team.
- A new model or major version of your current tool arrives.
- Deciding whether a significant harness change helps — the same scorecard works with the harness as the variable.
- Before renewing a significant contract.
- Quarterly, as a regression check on your current setup.

**Skip it when:**

- An individual is trying a tool for personal use. Let them; a scorecard is for team decisions.
- You have no task set yet. Build one first from 20–50 recent tasks; a scorecard on five tasks is an anecdote.
- The tool fails your data or security requirements outright. Record the rejection and stop; there is no point grading quality.

---

## Roles and ownership

| Role                          | Responsibility                                                                                             |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Harness or platform owner** | Maintains the task set; runs evaluations with the same harness for every candidate; fills in the scorecard |
| **Engineers**                 | Contribute tasks; take part in blind reviews; use candidates for real work and report how it felt          |
| **Security**                  | Reviews sandboxing, permissions, data retention and training terms                                         |
| **Engineering leadership**    | Decides, with cost and security alongside quality                                                          |

---

## How to use it

### Step 1: Build or refresh the task set

Pick 20–50 recently completed tasks: bug fixes, small features, refactors and tests, across the areas you care about. Include some that went badly for humans. Snapshot the starting commit and the original issue or intent for each. Rotate in new tasks every quarter so you are not optimising for old problems.

### Step 2: Hold the harness constant

Run every candidate with the same `AGENTS.md`, skills and checks. A candidate run without your harness is evaluated on a different problem. If a tool cannot use part of the harness, record that under workflow fit — it is a real cost.

### Step 3: Run each task several times

Agents are non-deterministic. Three runs per task is a reasonable minimum; report aggregates and note variance where it is large.

### Step 4: Grade outcomes, blind where you can

Checks passing is the floor. Above it: does the result meet the acceptance criteria according to an independent check, and would a reviewer accept it without rework? Strip tool names from the outputs before human review.

**Tip:** break results down by task type. Tools often differ more between fixes and refactors than overall.

### Step 5: Record cost, time and review effort

Tokens and money per task, wall-clock time, and human review time. Then compute cost per accepted task: total cost divided by results accepted without rework. This is the number that usually changes the decision.

### Step 6: Ask the people who used it

Have a few engineers use the candidate on real work for a week or two and write down what they thought. Then recommend: adopt, trial further, or reject, with reasons.

---

## The template

```markdown
# Tool Scorecard: <tool / model + version>

**Evaluated by:** <names> — **Date:** <date>
**Task set:** <N tasks, link> — **Runs per task:** <N>
**Harness used:** <AGENTS.md / skills / checks version>
**Compared with:** <current tool / model + version>

## Quality
| Measure | Result | Current |
| --- | --- | --- |
| Checks pass | % | % |
| Meets acceptance criteria (independent check) | % | % |
| Accepted without rework (blind review) | % | % |
| Interventions needed per task | avg | avg |
| By task type: fixes / features / refactors / tests | % each | % each |

## Cost and time
| Measure | Result | Current |
| --- | --- | --- |
| Tokens / cost per task | | |
| Cost per accepted task | | |
| Wall-clock time per task | | |
| Human review time per task | | |

## Workflow fit
- [ ] Reads AGENTS.md
- [ ] Supports the skills standard
- [ ] Supports our hooks / guards
- [ ] Supports our MCP servers
- [ ] Background / cloud execution
- [ ] Triggers from our issue tracker / chat
- [ ] Telemetry / transcript export

## Security and data
- Sandbox model:
- Permission boundaries outside the model: yes / no
- Data retention / training terms:
- Approved under our data policy: yes / no / pending

## Engineers' view
<What did people who used it say? Would they choose it?>

## Recommendation
Adopt / trial further / reject — and why.
**Re-evaluate on:** <date or trigger>
```

---

## Worked example

*Illustrative scenario: the team, tools and numbers below are hypothetical, and the model names are placeholders. They show what a completed scorecard can look like.*

Consider the billing team when a new version of the model behind their coding agent becomes available. Several engineers are enthusiastic after trying it; one has seen it rewrite more than it was asked to. Marta runs the team's golden-task set — 32 tasks from the last two quarters, including the migration incident task — against both versions, three runs each, with harness v17.

```markdown
# Tool Scorecard: <agent> with Model B (vs Model A)

**Evaluated by:** Marta, Daniel, Kofi — **Date:** 2026-10-20
**Task set:** 32 tasks (golden-set-2026Q3) — **Runs per task:** 3
**Harness used:** v17 (AGENTS.md rev 22, 9 skills, CI checks)
**Compared with:** same agent with Model A (current)

## Quality
| Measure | Model B | Model A |
| --- | --- | --- |
| Checks pass | 94% | 90% |
| Meets acceptance criteria (independent check) | 85% | 81% |
| Accepted without rework (blind review) | 66% | 63% |
| Interventions needed per task | 0.4 | 0.6 |
| Fixes / features / refactors / tests | 78 / 55 / 48 / 82% | 74 / 52 / 61 / 77% |

## Cost and time
| Measure | Model B | Model A |
| --- | --- | --- |
| Cost per task | $1.90 | $1.20 |
| Cost per accepted task | $2.88 | $1.90 |
| Wall-clock time per task | 7.5 min | 9 min |
| Human review time per task | 11 min | 9 min |

## Workflow fit
- [x] Reads AGENTS.md
- [x] Supports the skills standard
- [x] Supports our hooks / guards
- [x] Supports our MCP servers
- [x] Background / cloud execution
- [x] Triggers from our issue tracker / chat
- [x] Telemetry / transcript export
(Same agent; only the model changes.)

## Security and data
- Sandbox model: unchanged
- Permission boundaries outside the model: yes (unchanged)
- Data retention / training terms: same contract; confirmed with vendor
- Approved under our data policy: yes

## Engineers' view
Kofi and two other engineers liked it for fixes and tests: "fewer nudges".
Daniel found refactors touched more files than asked in 9 of 30 refactor runs,
which is why review time went up. Two engineers said they would choose it for
bug fixes but not refactors.

## Recommendation
Trial further. On fixes, features and tests Model B is not clearly better —
the differences are within noise on 32 tasks — and it is clearly worse on
refactors, at ~50% more per accepted task. What justifies a trial is the drop
in interventions and what engineers said about bug fixes. Trial it for 4 weeks on the
flaky-test and bug-fix workflows only; keep Model A for refactors. Add a
sensor that flags PRs touching files outside the plan (HCR-033) and re-run.
**Re-evaluate on:** 2026-11-20, or at the next model release.
```

The headline numbers seem to favour Model B: more checks pass, fewer interventions, faster runs. But differences of a few points on 32 tasks are noise, and the breakdown by task type and the cost per accepted task tell a more careful story. Refactors got worse, review time went up because the model touched more than it was asked to, and each accepted change costs more. Daniel's observation — something no aggregate captured — pointed to a missing sensor that would help with either model. The recommendation is narrow, reversible and dated, which is what a good one usually looks like.

That is why the recommendation does not rest on the headline percentages. The trial is justified by the signals that were large and consistent — fewer interventions, and engineers' experience on bug fixes — and limited by the one that was clearly negative. If your differences are small, run more tasks or more runs before you decide.

---

## Signals it is working

- Tool and model decisions cite your own results: "on our 32 tasks…", not a leaderboard.
- Cost per accepted task appears next to quality in every recommendation.
- Reviews are blind, and reviewers cannot tell which tool produced which result.
- The same task set runs on every model upgrade and quarterly, so results are comparable.
- Recommendations are often "trial further" with a scope and a date, not just adopt or reject.
- Engineers' views are recorded, and sometimes change the outcome.

---

## Failure modes and mitigations

The usual failure is an evaluation designed after the decision. Someone already wants the new tool; the task set is chosen to show it well, it runs once, without the team's harness, and the scorecard becomes a justification. The second is evaluation fatigue: the first scorecard is careful, the next three are skipped because a new model "is obviously better", and the team loses the ability to say whether anything improved.

| Failure mode             | What it looks like                       | Mitigation                                |
| ------------------------ | ---------------------------------------- | ----------------------------------------- |
| **Leaderboard choice**   | Picked from a public benchmark           | Your own tasks, your own harness          |
| **Single-run verdicts**  | One impressive demo decides              | Several runs per task, aggregated         |
| **Unfair comparison**    | Candidates run without your harness      | Same harness version for every candidate  |
| **Quality without cost** | The better tool wins regardless of price | Cost per accepted task in every scorecard |
| **Stale task set**       | The same tasks for a year                | Rotate in recent work quarterly           |
| **Ignoring humans**      | Wins on paper; engineers avoid it        | Engineers' view is a required section     |

---

## Further reading

- **[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)** by Anthropic — Practical guidance on building task sets from real failures, combining graders, and running several trials. (Vendor engineering blog.)

---

## Related pages

- [Evaluating a Tool on Your Own Work](../tools/evaluating-tools.md) — Golden-task replay step by step.
- [Testing & Evolving the Harness](../harness/testing-and-evolving.md) — Evals for harness changes, using the same task set.
- [Cost & Token Economics](../team/cost-and-token-economics.md) — Making sense of cost per accepted change.
- [Data & Policy](../security/data-and-policy.md) — The data and retention questions every scorecard has to answer.
