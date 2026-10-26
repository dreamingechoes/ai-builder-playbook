---
title: Harness Brief
description: A template for describing one part of your team's workflow precisely enough for an agent to execute — and for your team to challenge — with a fully worked example.
---

# Harness Brief

Most of a team's process has never been written down. It lives in habits, in the way the most experienced engineer happens to do things, and in an informal sense of what "it depends" means in each situation. That works surprisingly well until you try to hand part of the process to an agent. An agent cannot borrow the team's intuition. It needs inputs, sources of truth, boundaries, and a rule for what to do when information is missing. The Harness Brief is the document where you supply those, one workflow step at a time.

I think of it as the most useful exercise in this playbook, and not mainly because of the harness it produces. Describing a step precisely enough for an agent to execute it exposes the decisions nobody ever made explicitly. The brief protects one decision above all others: **what the team actually agrees on, as opposed to what each person assumes**. The automation is the second outcome. The first is a conversation the team should have had years ago.

!!! tip "The one-liner"
    Write one step down precisely enough for an agent to execute, then let the people who do the work tear it apart.

---

## What problem this solves

Teams that adopt agents without a shared description of their work end up with a row of separate workshops. Each engineer has their own prompts and their own idea of when to trust the output. Some of those workshops are excellent, but they do not add up. Quality depends on who picked up the ticket, and nothing anyone learns reaches anyone else.

The brief fixes the order of operations. It forces you to describe a step — its purpose, inputs, boundaries, gates and outputs — before anyone builds a skill, a hook or a reviewer prompt. When the team challenges the description, disagreements surface while they are cheap: in a document, rather than in a rollout, a rejected pull request, or an incident.

It also makes proportionality explicit. A step that applies to a major launch probably does not apply in full to a one-line internal fix. The brief asks you to say how the step scales down and how "not applicable" is recorded, so that steps are never silently skipped.

---

## When to use this

**Use it when:**

- You are about to encode a workflow step into skills, checks or gates for more than one person.
- Several engineers already use agents for the same step, and their results vary noticeably.
- A recurring step is done differently every time, or only when somebody remembers.
- You are preparing a step that will later run in the background or as part of a factory workflow.

**Skip it when:**

- The step is still changing week to week. Write the brief after the process has settled, or you will be encoding a moving target.
- The team has not yet used agents enough to know where they fail. Use them interactively for a few weeks first; the brief will be much better for it.
- You are describing a personal workflow nobody else uses. A short note in your own instructions is enough.
- You want to automate the whole lifecycle at once. Write one brief per step. A brief that covers everything decides nothing.

---

## Roles and ownership

| Role                                                                     | Responsibility                                                                                                                 |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| **Brief owner** (often the harness owner, a tech lead or staff engineer) | Writes the first draft quickly, runs the review, records what changed, and builds the smallest harness that encodes the result |
| **The people who do the work**                                           | Challenge every section; say where the description matches reality and where it encodes one person's habit                     |
| **Engineering manager**                                                  | Protects time for the review; makes sure sceptics are invited and heard                                                        |
| **Product partner**                                                      | Owns any product-side judgment the step contains, and the definition of what the step's output is for                          |
| **Gate owners**                                                          | Confirm they can actually make the decision the brief assigns to them, with the inputs it promises                             |

---

## How to use it

### Step 1: Pick one step, and name the decision it protects

Choose one part of the workflow where the cost is mostly volume and the process is reasonably stable: research before planning, approach review, rollout preparation, error investigation, impact review. Then write the purpose in one or two sentences. If you cannot name the decision the step protects, or what goes wrong when it is skipped, stop there. That is your first finding.

### Step 2: Draft it alone, quickly, with gaps visible

Fill in the template in under an hour. Do not polish it. Where you do not know the answer, write the question instead. A draft with visible gaps invites challenge; a polished draft invites approval.

**Tip:** the sections that will cause the most debate are usually "What the agent must never assume" and "When information is missing". Spend your thinking there.

### Step 3: Take it to the people who do the work

Share it with everyone who performs the step and ask them to challenge every section. The five questions at the bottom of the template are a good agenda. Expect to discover that the team agrees on less than it thought. That discovery is the point of the exercise, not a failure of it.

**Tip:** before the group review, ask one person who does this step differently from you to read it. The differences between your two versions are usually the most important open questions.

### Step 4: Record what changed, and why

Every disagreement resolved in review goes into the "Open questions from review" table with its resolution. Those rows become the reasoning behind the harness: the explanation the next person will need when they want to change it.

### Step 5: Build the smallest harness that encodes it

Only now build: usually a skill for the procedure, one or two sensors that check its output, a human gate with defined inputs, and an artifact the next step consumes. Keep it small enough that the team can read all of it. Record each piece with a [Harness Change Record](harness-change-record.md).

### Step 6: Run it in shadow, then promote

Let the harness produce its output while people still do the step the old way, and compare. Promote it when its output is usually accepted without rework. Then move the brief's status to "implemented" and link the components it produced.

---

## The template

```markdown
# Harness Brief: <workflow step>

**Owner:** <name>
**Status:** draft | in review | agreed | implemented
**Last updated:** <date>
**Applies to risk tiers:** <0 / 1 / 2 / 3>

## Purpose
What decision does this step protect? What goes wrong when it is skipped?

## Trigger
When does this step start? (event, label, schedule, previous step's output)

## Inputs
| Input | Source of truth | Trusted? |
| --- | --- | --- |
| <e.g. usage metrics> | <system> | yes / with caveats / no |

## What the agent may decide on its own
- <decision>

## What the agent must never assume
- <e.g. pricing, user-facing copy, data retention, anything legal>

## When information is missing
For each kind of missing information: infer (and say so) / skip with reason / ask whom.

## Volume vs judgment
| Activity | Volume or judgment? | Who does it |
| --- | --- | --- |
| <activity> | volume | agent |
| <activity> | judgment | person |

## Human gate(s)
Where does this step stop for a person? What do they need to see?
What understanding should they add that is not in the data?

## Output
What artifact does this step produce, and who or what consumes it next?

## Definition of done
- [ ] <checkable condition>

## Proportionality
How does this step scale down for small work? When is it "not applicable",
and how is that recorded?

## What currently happens only when somebody remembers?

## Harness components (fill in when implemented)
| Component | Type (skill / sensor / gate / hook) | Change record |
| --- | --- | --- |

## Open questions from review
| Question | Raised by | Resolution |
| --- | --- | --- |
```

### Questions to ask in review

- Which parts of this are volume, and which require judgment?
- Where should the harness stop for an engineer, and what understanding should they add?
- When information is missing, should the harness infer or ask?
- Which steps must stay consistent, and where should the outcome adapt?
- What important work happens only when somebody remembers?

---

## Worked example

*Illustrative scenario: the team, people and details below are hypothetical. They show what a completed brief can look like; they do not describe a real organisation.*

Consider a product team of seven engineers building a subscription-billing product. Marta, a staff engineer, owns the team harness. The team has a habit everybody praises and nobody does consistently: reviewing the implementation approach before a pull request exists, so that the expensive disagreements happen before code. Marta drafted this brief in forty minutes and took it to the team's weekly engineering sync. The review took an hour and changed four things, recorded at the bottom.

```markdown
# Harness Brief: Approach review before code

**Owner:** Marta (staff engineer, harness owner)
**Status:** implemented
**Last updated:** 2026-10-02
**Applies to risk tiers:** 2 and 3 (tier 0–1: not applicable, recorded on the issue)

## Purpose
Protects the decision "is this the right way to build it?" before code exists.
When skipped, we discover design disagreements in PR review, after the agent
has produced 600 lines in the wrong shape, and rework costs a day instead of
ten minutes.

## Trigger
An issue labelled `ready-for-approach` with a linked, approved problem framing.

## Inputs
| Input | Source of truth | Trusted? |
| --- | --- | --- |
| Problem framing and acceptance criteria | Linked framing doc | yes |
| Architecture and ADRs | `docs/architecture.md`, `docs/adr/` | yes |
| Code paths touched | Repository at `main` | yes |
| Prior similar changes | Merged PRs, last 12 months | with caveats (patterns may be outdated) |
| Performance data | Dashboards | with caveats (7-day window only) |

## What the agent may decide on its own
- Which files and modules are in scope, with evidence.
- Which existing pattern the change should follow, citing an example.
- Proposed test strategy, mapped to the acceptance criteria.

## What the agent must never assume
- Changes to pricing, proration or tax logic are acceptable.
- A new public API field or event schema is acceptable.
- A database migration is "small" (lock behaviour must be checked by script).
- Anything about customer-visible copy.

## When information is missing
- Missing acceptance criterion: ask the issue owner. Do not infer.
- Two plausible patterns in the codebase: present both, recommend one, say why.
- Performance impact unknown: say "unknown", propose how to measure it.

## Volume vs judgment
| Activity | Volume or judgment? | Who does it |
| --- | --- | --- |
| Reading the code paths and ADRs | volume | agent |
| Listing affected modules and callers | volume | agent (script) |
| Drafting two candidate approaches | volume | agent |
| Choosing between approaches | judgment | engineer + area owner |
| Deciding a migration is acceptable | judgment | billing area owner |

## Human gate(s)
"Approach review" — a judgment and control gate. The engineer sees a one-screen
summary: recommendation, alternatives, files touched, risks, and what the agent
could not determine. Before deciding, they check unknowns with the area owner
if the change is tier 3.

## Output
`approach.md` attached to the issue. The build step reads it as its plan; the
PR description links it.

## Definition of done
- [ ] Every acceptance criterion maps to a planned test.
- [ ] Every file the plan touches is listed, with a reason.
- [ ] Risks and unknowns are listed explicitly, even if "none found".
- [ ] A named person approved or rejected it.

## Proportionality
Tier 0–1: not applicable; the issue records "approach review: n/a (tier 1)".
Tier 2: summary only, for changes over ~200 lines (smaller: n/a, recorded on
the issue). Tier 3: summary plus area-owner sign-off.

## What currently happens only when somebody remembers?
Checking whether a similar change was tried and reverted before.

## Harness components (fill in when implemented)
| Component | Type | Change record |
| --- | --- | --- |
| `review-approach` skill | skill | HCR-028 |
| `list-callers.sh` | sensor (computational) | HCR-028 |
| Approach review gate | gate | HCR-029 |

## Open questions from review
| Question | Raised by | Resolution |
| --- | --- | --- |
| Should tier 1 changes get a lightweight review? | Kofi | No. Cost exceeds benefit; revisit in Q1 with data. |
| Can the agent pick the approach when alternatives are equivalent? | Daniel | No. It recommends; a person chooses. Cheap, and keeps people in the design. |
| Who decides when a migration is acceptable? | Lena | Billing area owner, always. Added to "never assume". |
| What if the framing doc is missing? | Aisha | The step does not start. The label is rejected with a comment. |
```

A few things about why it reads the way it does. The purpose names a real cost — a day of rework instead of ten minutes — rather than a virtue. The "never assume" list is short and specific to this product; it is the list of things that, in a billing system, have expensive consequences. The "When information is missing" section gives a different answer for each kind of gap, which is the part teams most often leave vague. And the review table shows that the most useful changes came from other people: Daniel, the team's most vocal sceptic, is the reason the agent recommends rather than chooses.

Notice what it does not contain: tool names, prompts, or model settings. Those belong to the implementation, and they will change. The brief describes an agreement, and it should survive a change of tools.

---

## Signals it is working

- Engineers read the brief and say "yes, that's how we do it — written down", or argue with it. Both are good; silence is not.
- Disagreements happen about the description, before anything is built, rather than after rollout.
- The "Open questions from review" table has entries from several people, not just the owner.
- Harness components link back to a brief, and briefs link forward to components and change records.
- Small work produces small artifacts, and "not applicable" is recorded with a reason rather than silently skipped.
- When someone proposes changing the harness, they start by editing the brief.

---

## Failure modes and mitigations

The typical failure unfolds quietly. The owner writes a careful, complete brief on their own, shares it with a request for "any feedback", gets three thumbs-up reactions, and builds it. Six weeks later, half the team works around the harness because it encodes one person's style, and nobody remembers agreeing to it. The brief existed; the conversation did not.

| Failure mode                          | What it looks like                                                          | Mitigation                                                                  |
| ------------------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Polished first draft**              | The brief arrives looking finished, so people approve rather than challenge | Share it rough, with open questions written in                              |
| **Brief as specification**            | Prompts, tools and model settings fill the page                             | Describe the agreement; leave implementation to the components              |
| **Vague missing-information rules**   | "Use judgment when data is missing"                                         | One answer per kind of gap: infer and say so, skip with reason, or ask whom |
| **No named gate owner**               | "The team reviews it"                                                       | A role, with a backup, who can actually judge the inputs                    |
| **One brief for the whole lifecycle** | Twelve sections per step, nobody reads it                                   | One brief per step; extend one step at a time                               |
| **Never revisited**                   | Status stuck at "implemented" for a year                                    | Revisit when the process, the model, or the team changes                    |

---

## Related pages

- [Designing a Team Harness](../harness/designing-a-team-harness.md) — The six-step process this brief supports.
- [Human Gates](../harness/human-gates.md) — Designing the stopping points the brief names.
- [Guides & Sensors](../harness/guides-and-sensors.md) — Choosing the components that encode the brief.
- [Human Gate Definition](human-gate-definition.md) — The template for each gate the brief names.
- [Harness Change Record](harness-change-record.md) — The record for each component you build from the brief.
