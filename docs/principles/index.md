---
title: Principles
description: The beliefs that shape every other chapter — what stays human, what becomes part of the system, and how honest we are about what the evidence shows.
---

# Principles

Most writing about AI in software engineering starts with a tool. This playbook starts with a position, because tools change every quarter and positions are what let you decide anything when the next one arrives.

Here is mine in one paragraph. Coding agents have made producing plausible software cheap. They have not made deciding what to build, checking that it is right, or owning it when it breaks any cheaper. The work of a builder in this period is to design a way of working where agents carry the volume and people keep — and get more room for — the judgment. Everything else in the playbook is an attempt to make that concrete.

!!! tip "The one-liner"
    The model is a component. The process is the craft.

---

## Why start with principles

A new model arrives every few weeks. Each one comes with a benchmark, a demo, and a wave of posts explaining that everything has changed. Some of the time, something has. Most of the time, what has changed is the ceiling of what one person can do in one session, while the things that decide whether a team ships good software — how intent is agreed, how work is checked, who owns the result — have not moved at all.

I learned this while building a harness for my team. The most useful outcome was not the automation itself. Describing the research phase precisely enough for an agent to follow it exposed a surprising number of unanswered questions: what counts as enough evidence, how far to investigate before asking someone, what the system should do when a fact is missing. Every one of those was a principle hiding inside a default.

That is why this section comes first. The principles are the decisions that sit underneath every other chapter, stated plainly enough that you can disagree with them. If you disagree, the rest of the playbook will still be useful, but you will know exactly where and why you would build it differently.

---

## What this section covers

The section has three parts, and they are meant to be read together. The principles say what I believe. The evidence says how much of that belief the research can carry. The glossary makes sure we are using the same words for the same things, which matters more than usual in a field where one vendor's "agent" is another's "workflow".

| Page                                                      | What it gives you                                                                                                                                                  |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **[Core Principles](core-principles.md)**                 | Eight operational principles — the lenses I use when the right answer is not obvious.                                                                              |
| **[The State of the Evidence](state-of-the-evidence.md)** | What research and field reports actually show about AI and engineering productivity, how that changed from 2025 to 2026, and how much weight each claim can carry. |
| **[Glossary](glossary.md)**                               | The vocabulary of agentic engineering, defined the way this playbook uses it, with origins where a term has one.                                                   |

---

## The principles at a glance

Each principle is short enough to remember and specific enough to change a decision. The details, the evidence behind each one, and the cases where I would bend it are in [Core Principles](core-principles.md).

| Principle                                                 | Core idea                                                                                                                 |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **The model is a component, the process is the craft**    | Competitive advantage lives in how the work around the model is organised, not in which model you picked this month.      |
| **Uncertainty becomes a question**                        | When information is missing, the system asks. A confident-looking assumption is worse than a visible gap.                 |
| **Delegate volume, protect judgment**                     | Hand agents the work whose cost is repetition and context-gathering. Keep — and expand — the work whose cost is thinking. |
| **The harness follows the conversation**                  | A harness is the team's working agreements made executable. It adapts to the team, not the other way round.               |
| **The check cannot come from the same place as the code** | Independent verification grows in importance as implementation moves away from human hands.                               |
| **Evidence over announcement**                            | Autonomy is earned workflow by workflow, with acceptance and rework data, not declared in a kickoff.                      |
| **Every control has a reason and an expiry**              | Rules exist because of observed failures, and they are removed when models or circumstances change.                       |
| **Authorship spreads, responsibility does not**           | Someone puts their name on work that nobody typed. Decide who, before it matters.                                         |

---

## Why the evidence sits next to the principles

It would be easy to write a set of principles and decorate them with whichever studies agree. I have tried not to do that. The evidence page exists because the honest answer to "does AI make engineering teams more productive?" is still "it depends, and here is on what" — and because the things it depends on turn out to be the same things the principles are about.

DORA's 2025 research describes AI as an amplifier of whatever system it lands in, and the seven capabilities it found to predict benefit include small batches, strong version control and a user-centric focus. Not one of them is a tool. METR's careful work shows how far perception and measurement can diverge, which is why "evidence over announcement" is a principle and not a slogan. The vendor reports and academic studies of 2026 agree on where the cost of faster code goes — review, QA, code health — which is why independent verification gets a principle of its own.

Where the evidence is thin, the evidence page says so. Some of these principles rest on experience more than on research, and you deserve to know which.

---

## How to use these principles

They are meant to be operational. If a principle does not change what you do on a Tuesday afternoon, it is a poster.

**In design reviews of the harness:** Every new gate, skill, or automated step should be able to name the principle it serves. "We added an independent browser check because the check cannot come from the same place as the code" is a reason. "Everyone has one" is not.

**In retrospectives:** Pick one principle and ask where the team upheld it and where it slipped. "Where did an agent fill a gap with something plausible this sprint?" is one of the most useful questions I know.

**When the tools change:** When a new model or tool arrives, the principles tell you what to re-test — usually your controls with an expiry — and what to leave alone.

**When someone shows you a number:** Run it through the four questions on [The State of the Evidence](state-of-the-evidence.md) before it reaches a plan.

---

## What good looks like

| Signal                                               | What you observe                                                      |
| ---------------------------------------------------- | --------------------------------------------------------------------- |
| **Gaps surface as questions**                        | Agents stop and ask; plans list open questions instead of hiding them |
| **Engineers spend more time with people**            | More time in product conversations, less assembling context by hand   |
| **Review is short because intent was settled early** | Arguments happen on the approach, not in the pull request thread      |
| **Autonomy expands on evidence**                     | Workflows earn auto-approval through a track record, one at a time    |
| **The harness shrinks as well as grows**             | Rules are removed when they stop earning their place                  |
| **Ownership is explicit**                            | Every merged change has a named human who stands behind it            |

---

## Failure modes and mitigations

Most of these start as reasonable decisions. A team adopts a new tool because the demo was impressive, tracks agent pull requests because it is the number that is easy to get, and trusts green CI because there is too much to read. None of it looks like a failure until a quarter later, when the review queue is long, the incidents are subtle, and nobody can say who owns the module that changed most.

| Failure mode              | What it looks like                                                    | Mitigation                                                    |
| ------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Model chasing**         | Every release triggers a tool migration; the process never matures    | Evaluate tools on your own tasks; invest in the process layer |
| **Plausible-gap filling** | Agents invent requirements; nobody notices until review or production | Make "stop and ask" an explicit, rewarded behaviour           |
| **Automation theatre**    | Dashboards show agent PR counts; nobody tracks rework                 | Measure acceptance, intervention, and rework, not adoption    |
| **Harness by decree**     | One person's habits encoded as team law                               | Treat the harness as a working agreement open to challenge    |
| **Rubber-stamp review**   | Green CI means approve                                                | Independent checks plus risk-tiered human attention           |
| **Orphaned code**         | Nobody feels responsible for agent-written modules                    | Named owners for every change and every area                  |

---

## Related pages

- [Start Here](start-here.md) — Why the playbook exists and which reading path fits you.
- [Core Principles](core-principles.md) — The eight principles in detail, with examples and when to bend them.
- [The State of the Evidence](state-of-the-evidence.md) — What we know, what we don't, and why it matters for every decision in this playbook.
- [What a Harness Is](../harness/what-a-harness-is.md) — Where the principles become a system.
