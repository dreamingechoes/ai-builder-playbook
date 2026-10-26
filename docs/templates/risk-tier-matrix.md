---
title: Risk Tier Matrix
description: A template for deciding how much autonomy and review each kind of change gets, how tiers are assigned, and how autonomy is promoted and demoted — with a fully worked example.
---

# Risk Tier Matrix

Every team already has risk tiers. They are just not written down. One reviewer approves a lockfile bump without opening it; another reads every line of a billing change twice; a third does the opposite on a busy Friday. Autonomy is decided implicitly, change by change, by whoever is reviewing and however much time they have. With agents multiplying the number of changes, that improvisation stops scaling. Trivial work queues behind careful reviewers, and dangerous work slips past tired ones.

The Risk Tier Matrix makes autonomy a decision the team takes in advance, on purpose, and can change on evidence. It protects **how much a change is trusted before a person looks at it** — and, just as important, who stands behind it when nobody did. Deciding where those lines sit is a decision about people as much as about code, which is why it belongs in a shared document and not inside a reviewer's head.

!!! tip "The one-liner"
    Classify changes mechanically, earn autonomy per workflow on evidence, and design the way back before you need it.

---

## What problem this solves

Without explicit tiers, three problems compound. Review effort is spread evenly, so it is wasted on trivial changes and too thin on dangerous ones. Agents, or the people directing them, end up deciding how risky their own work is. And autonomy only ever increases: once a workflow merges without review, nobody remembers to ask whether it still should.

The matrix addresses all three. It defines a small number of tiers by blast radius and reversibility, maps paths and change types to tiers mechanically so classification happens outside the agent, sets promotion thresholds before anyone has a stake in the result, defines demotion triggers, and names who is accountable at each tier — including tier 0, where nobody reviewed the change at all.

---

## When to use this

**Use it when:**

- Agents open pull requests in a shared repository, interactively or in the background.
- Reviewers disagree about how closely to look at agent output.
- You are planning a factory workflow and need to decide what it may do without a person.
- After an incident, to check whether the change was in the right tier.
- Quarterly, and after any major model or harness change.

**Skip it when:**

- One or two people work on a small codebase and review everything anyway. A short note in `AGENTS.md` is enough until that changes.
- You have no checks worth trusting yet. Tiers depend on [harnessability](../harness/harnessability.md); fix the tests before you grant autonomy based on them.

---

## Roles and ownership

| Role                          | Responsibility                                                            |
| ----------------------------- | ------------------------------------------------------------------------- |
| **Tech lead and area owners** | Propose tiers for their areas and change types; own tier 3 reviews        |
| **The team**                  | Agrees the tier model, promotion thresholds and demotion triggers         |
| **Harness owner**             | Implements classification outside the agent; reports per-workflow metrics |
| **Workflow owners**           | Accountable for anything their workflow auto-merges                       |
| **Engineering manager**       | Makes sure accountability per tier is understood and fair                 |

---

## How to use it

### Step 1: Start from the default four tiers

Begin with the tiers in the template — mechanical, low, standard, high — and adjust definitions to your context. Resist adding more. Four tiers that everyone can name are better than seven that need a lookup table.

### Step 2: Map paths and change types to tiers

Write assignment rules as patterns: paths, file types, labels, change kinds such as migrations. The highest matching tier wins. Classification runs in CI or a bot, not in the agent's own judgment. Reserve human judgment for edge cases, and record those decisions so they become rules.

**Tip:** list your ten most sensitive paths first. Getting tier 3 right matters far more than getting tier 1 perfect.

### Step 3: Combine blast radius with how well the area is checked

Two areas with the same blast radius can deserve different tiers if one has strong tests and types and the other has none. Be honest about which areas your sensors actually cover.

### Step 4: Write promotion and demotion rules before you need them

Agree the evidence that moves a workflow up one notch — for example, from "a person approves every output" to "sampled approval" — and the events that move it back. Thresholds set after the fact are always met.

### Step 5: Name accountability per tier

Every tier has a person who stands behind the change in production. For tier 0, that is the workflow owner.

### Step 6: Review it on a schedule and after incidents

Put a review date on the matrix. After any incident, ask whether the change was in the right tier and whether the tier's checks were enough. Record changes with a [Harness Change Record](harness-change-record.md).

---

## The template

```markdown
# Risk Tiers: <team / codebase>

**Owner:** <name> — **Agreed on:** <date> — **Review by:** <date>

## Tiers
| Tier | Definition | Agent may | Verification | Human review | Merge |
| --- | --- | --- | --- | --- | --- |
| 0 Mechanical | No behaviour change; machine-checkable | Implement, open PR | Static + tests | None once proven | Auto (after track record) |
| 1 Low | Small blast radius, easy revert | Implement, open PR | Static + tests + AI review | Light skim | Human |
| 2 Standard | Feature work in harnessed areas | Implement after approved plan | + independent acceptance checks | Evidence + key logic | Human |
| 3 High | Money, auth, PII, migrations, public APIs, infra | Research + plan only until approach approved | + walkthrough + staged rollout | Full, by area owner | Human (area owner) |

## Assignment rules
| Pattern | Tier |
| --- | --- |
| `<path or change type>` | <tier> |
| Anything else | 2 |

Highest matching tier wins. Classification runs outside the agent.

## Gates per tier
| Tier | Gates required |
| --- | --- |
| 0 | <e.g. none after promotion; merge before> |
| 3 | <e.g. approach review, merge by area owner, rollout advance> |

## Promotion (per workflow)
- From "human approves every output" to "sampled approval" after: <N> consecutive runs accepted without rework, no escaped defects.
- From "human merge" to "auto-merge" (tier 0 only) after: <criteria>.

## Demotion triggers
- Any escaped defect or revert → back one notch.
- Acceptance without rework below <X>% over <period> → back one notch.
- New model or major harness change → re-evaluate.
- Anyone may pause a workflow; record why.

## Accountability
| Tier | Accountable person |
| --- | --- |
| 0 | Workflow owner |
| 1–2 | Approver |
| 3 | Area owner |

## Change log
| Date | Change | Evidence |
| --- | --- | --- |
```

---

## Worked example

*Illustrative scenario: the team, paths and numbers below are hypothetical. They show what a completed matrix and its first promotion can look like.*

Return to the billing team. Before writing the matrix, they had a one-line tier note in `AGENTS.md` ("billing paths are high risk"), one rule added after the August migration incident (`db/migrations/**` is tier 3, HCR-026), and otherwise a single habit: "a human reviews every agent PR". In practice, Daniel read everything closely and was always behind, while two others approved dependency bumps in seconds. In September Marta formalised all of it into the matrix, folding in the migration rule; Lena set the billing rules; the team argued for an hour about whether `apps/web/` copy changes were tier 1 or tier 2, and settled on tier 1 behind a flag.

```markdown
# Risk Tiers: billing monorepo

**Owner:** Marta (harness owner) — **Agreed on:** 2026-09-10 — **Review by:** 2026-12-10

## Tiers
(Default four-tier table adopted unchanged, except: tier 2 requires an
approved approach.md for changes over ~200 lines.)

## Assignment rules
| Pattern | Tier |
| --- | --- |
| `services/billing/**`, `**/tax/**`, `**/refunds/**` | 3 |
| `db/migrations/**` | 3 |
| `apps/api/routes/v2/**` changing a response schema | 3 |
| `infra/**`, `.github/workflows/**` | 3 |
| `**/auth/**`, `**/permissions/**` | 3 |
| `apps/web/**` behind a feature flag, copy or styling only | 1 |
| `docs/**`, `**/*.test.ts` only | 1 |
| Lockfile-only patch upgrades in `workers/` and `apps/web/` | 0 |
| Flaky-test quarantine PRs from the quarantine workflow | 0 (after promotion) |
| Anything else | 2 |

Highest matching tier wins. Classification runs in CI and labels the PR.

## Gates per tier
| Tier | Gates required |
| --- | --- |
| 0 | Merge (until promoted); none after |
| 1 | Merge |
| 2 | Approach review (>200 lines), merge |
| 3 | Approach review, merge by area owner, rollout advance |

## Promotion (per workflow)
- Approve-every-output → sampled (1 in 5) after 30 consecutive runs accepted
  without rework and no escaped defects.
- Human merge → auto-merge (tier 0 only) after a further 30 sampled runs with
  no rework and no reverts.

## Demotion triggers
- Any escaped defect or revert → back one notch, immediately.
- Acceptance without rework below 85% over two weeks → back one notch.
- Model change in the workflow → back to sampled until 10 runs are clean.
- Anyone may pause a workflow with the `factory-pause` label; record why.

## Accountability
| Tier | Accountable person |
| --- | --- |
| 0 | Workflow owner (patch upgrades: Kofi; quarantine: Marta) |
| 1–2 | Approver |
| 3 | Area owner (billing: Lena; infra: platform team) |

## Change log
| Date | Change | Evidence |
| --- | --- | --- |
| 2026-08-20 | `db/migrations/**` → tier 3 (HCR-026) | Postmortem 2026-08-14 |
| 2026-09-10 | Matrix agreed, formalising the earlier note and HCR-026 | Team sync notes |
| 2026-10-14 | Patch upgrades promoted to sampled approval | 34 runs, 33 accepted without rework, 1 rework (changelog misread), 0 escapes. Threshold counted from the rework. |
| 2026-10-21 | API schema changes moved from 2 to 3 | Partner integration broke on a renamed field (postmortem link) |
```

The change log is the part worth studying. The first promotion is backed by its record, and the team counted the thirty consecutive runs from the last rework rather than from the start, as the rule says. The second entry is a move in the other direction: an incident showed that response-schema changes were riskier than their tier, so they went up. A matrix that has only ever granted autonomy is not yet being used honestly.

Notice too that tier 0 has named owners. Once patch upgrades reach auto-merge, and one breaks something at night, Kofi is the first person called — not because he reviewed it, but because he owns the workflow that decided it did not need review.

---

## Signals it is working

- Anyone on the team can say which tier a change is in, and why, without debate.
- Most classification is mechanical; edge-case decisions turn into new rules.
- Review effort is visibly uneven: tier 3 gets deep review, tier 0 gets none.
- Every promotion links to a track record, and demotions actually happen.
- Auto-merged changes have a named owner who knows they are accountable.
- Incident reviews ask "was the tier right?" as a matter of routine.

---

## Failure modes and mitigations

The common slide is towards everything being tier 2. Nobody wants to argue a change down to tier 1, and nobody wants to be the person who put the incident in tier 0, so the matrix collapses into "normal review for everything" — which is where the team started. The opposite failure is quieter: autonomy granted on enthusiasm, never taken back, until an incident takes it back for you.

| Failure mode             | What it looks like                          | Mitigation                                                        |
| ------------------------ | ------------------------------------------- | ----------------------------------------------------------------- |
| **Everything is tier 2** | No real differentiation                     | Push clearly mechanical work to tier 0; protect tier 3 explicitly |
| **Self-classification**  | The agent decides its own tier              | Classify by path and type in CI, outside the agent                |
| **One-way ratchet**      | Autonomy only increases                     | Demotion triggers that fire automatically                         |
| **Tier splitting**       | High-risk work split into "low-risk" pieces | Classify by what each piece touches, not its size                 |
| **Orphaned auto-merges** | Tier 0 changes nobody owns                  | Workflow owner accountable by name                                |
| **Moving thresholds**    | Promotion criteria set after results are in | Agree thresholds before the pilot starts                          |

---

## Related pages

- [Risk Tiers & Autonomy](../factories/risk-tiers-and-autonomy.md) — The two dimensions of risk, and how autonomy is earned and taken back.
- [Harnessability](../harness/harnessability.md) — Why well-checked areas can safely get more autonomy.
- [Review at Agent Speed](../workflow/review-at-agent-speed.md) — Matching review depth to tier.
- [Factory Pilot Canvas](factory-pilot-canvas.md) — Setting promotion thresholds for a specific workflow.
