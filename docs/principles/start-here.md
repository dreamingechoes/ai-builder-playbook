---
title: Start Here
description: Why this playbook exists, how to read it depending on who you are, what each section covers, and how every page is built.
---

# Start Here

## Why this playbook exists

Producing a first version of code has become cheap. So has gathering context, drafting a plan, writing a test, or summarising what happened in production last week. Most of us noticed that a while ago, and nobody finds it remarkable any more.

What has not become cheap is everything that decides whether that output is worth anything: knowing what is worth building, agreeing what "done" means, checking the work from somewhere other than where it was produced, and learning from what happens after it ships. Those parts are made of judgment, and judgment is still a human, slow, and deeply social activity.

This playbook is about the space between those two facts. It is how I think about building software with AI in late 2026 — as an engineer, as a product builder, and as someone who has spent recent months designing a harness and workflows for a team rather than for myself alone.

The industry's conversation still happens mostly at the model layer: which tool, which benchmark, which provider is winning this month. The interesting problem has already moved somewhere else. Software is not built one prompt at a time. It is built across stages — exploring a problem, defining it, implementing, verifying, shipping, learning from what happened — and its quality depends on how well those stages connect. A brilliant generation built on a requirement nobody examined is still worthless.

So this is not a guide to prompts or a comparison of tools, although both appear. It is a guide to designing the way of working around the model, so that the cheap parts get cheaper and the parts made of judgment get more of everyone's time. The goal was never simply to produce more code. It is engineers spending less time producing the inputs to product decisions and more time taking part in them.

It is opinionated, and the opinion fits in one line:

!!! tip "The one-liner"
    The model is a component. The process is the craft.

---

## Reading paths

Pick the path that matches where you are. Each one takes an afternoon, and each one starts where that reader's most pressing question is — not at the beginning of the book.

| If you are…                                                            | Read in this order                                                                                                                                                                                                                                                                                                        |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **New to agentic engineering** and want a solid mental model           | [Core Principles](core-principles.md) → [How a Coding Agent Works](../foundations/how-a-coding-agent-works.md) → [Context Engineering](../foundations/context-engineering.md) → [What a Harness Is](../harness/what-a-harness-is.md) → [Building with Agents](../workflow/building-with-agents.md)                        |
| **An engineer** who already uses agents daily and wants the next level | [Guides & Sensors](../harness/guides-and-sensors.md) → [Intent Before Code](../workflow/intent-before-code.md) → [Verification](../workflow/verification.md) → [Review at Agent Speed](../workflow/review-at-agent-speed.md) → [Testing & Evolving the Harness](../harness/testing-and-evolving.md)                       |
| **A tech lead** designing how your team works with AI                  | [Designing a Team Harness](../harness/designing-a-team-harness.md) → [Human Gates](../harness/human-gates.md) → [Ownership & Change](../harness/ownership-and-change.md) → [Your First Factory Workflow](../factories/first-factory-workflow.md) → [Harness Brief](../templates/harness-brief.md)                         |
| **An engineering manager or leader** deciding where to invest          | [The State of the Evidence](state-of-the-evidence.md) → [What a Factory Is (and Isn't)](../factories/what-a-factory-is.md) → [Measuring Impact Honestly](../team/measuring-impact.md) → [Adoption Without Mandates](../team/adoption-without-mandates.md) → [Cost & Token Economics](../team/cost-and-token-economics.md) |
| **Curious about where this is going**                                  | [Experiments in the Field](../frontier/experiments-in-the-field.md) → [Field Reports](../factories/field-reports.md) → [Beyond Code](../frontier/beyond-code.md) → [What Comes Next](../frontier/what-comes-next.md)                                                                                                      |

---

## What is inside

| Section                                         | The question it answers                                                                                          |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [**Principles**](index.md)                      | What do I believe about building with AI, and what does the evidence actually say?                               |
| [**Foundations**](../foundations/index.md)      | How do coding agents work, and what are the primitives — context, instructions, skills, tools, subagents, hooks? |
| [**AI Harness**](../harness/index.md)           | How do we design the system around the model so it works for a whole team, not just for whoever is driving?      |
| [**Workflow**](../workflow/index.md)            | How does a piece of work move from a vague idea to a measured result when agents do most of the typing?          |
| [**Software Factories**](../factories/index.md) | What happens when the way of working stops belonging to individuals and starts belonging to the team?            |
| [**Team**](../team/index.md)                    | How do we measure impact, adopt without mandates, grow judgment, and keep the pace sustainable?                  |
| [**Security**](../security/index.md)            | What changes when software that reads untrusted input can also act on your systems?                              |
| [**Frontier**](../frontier/index.md)            | Where are the pioneers, what are they finding, and where is this heading?                                        |
| [**Tools**](../tools/index.md)                  | How do I choose tools without chasing every release, and what do I actually use?                                 |
| [**Templates**](../templates/index.md)          | What can I copy into my team today?                                                                              |

---

## How every page is built

Every chapter answers the same questions, so you can skim for what you need. The structure mirrors its sibling, the Engineering Leadership Playbook, because the questions a leader asks about a practice do not change when agents arrive:

1. **What problem does this solve?**
2. **When should I use it — and when not?**
3. **Who owns it?**
4. **What does good look like?**
5. **What usually goes wrong?**
6. **What can I copy and use today?**

Two additions are specific to this playbook, because the field moves faster than most:

!!! info "What we know / what we don't"
    Pages that lean on research say plainly how strong the evidence is. A lot of what is written about AI in software engineering comes from vendors, or from teams describing young systems of their own. That does not make it wrong, but it changes how much weight it can carry.

!!! note "Last reviewed"
    Pages with perishable content — tools, numbers, vendor features — carry the date they were last checked. The principles should age well. The tool names will not.

---

## Who I am, and why I wrote this

I'm Iván González Sáiz, an engineering leader and senior software engineer with over 15 years of experience building product software, shaping technical direction, managing teams, and helping organisations turn ambiguity into clear, sustainable execution. In 2026, much of my building has gone into one question: how should software be built when agents can do a large share of the implementation?

I have been answering it from two sides at once, and each keeps the other honest.

As co-founder and product engineer of [Avenida](https://www.avenida.so/), a product that helps people organise their shopping with smart wishlists, I own the engineering side end to end in close collaboration with Product: from shaping ideas, defining scope and weighing trade-offs to architecture, delivery planning and hands-on implementation, across web, a browser extension and mobile. Avenida has also become an important space for exploring AI-native product development in practice — using AI not only to write code, but to improve ideation, product definition, technical proposals, scope slicing, development workflows, testing and delivery. It keeps me close to product thinking, fast iteration, and the reality of building from scratch with strong ownership and limited resources.

In the company where I work, the question changes shape: not how a founding team works with agents, but how an established product team does. That meant building an AI harness for my team. It covers most of the path from an early product idea to what happens after that idea reaches real users: gathering context and metrics, defining the problem, planning, implementing, running quality checks, reviewing, preparing the rollout, measuring the impact, and capturing what we learned seven or thirty days later. Agents, instructions, checks, human gates and evaluations all sit around that flow.

The most valuable outcome was not the automation. It was being forced to describe, with far more precision than usual, how we believe work should move through the team. Describing the research phase precisely enough for an agent to follow exposed questions we had always answered with "it depends": what counts as enough evidence, how far to investigate before asking someone, which conclusions may be inferred and which need an explicit product decision. The answer we kept returning to — that uncertainty should become a question, not a confident-looking assumption — became the second of the [Core Principles](core-principles.md).

I built the first version largely on my own, and that taught me the other half of the lesson. A working prototype is persuasive, which makes the assumptions inside it harder to see. The most useful changes came when people who had to work inside it asked why a gate was optional or why a step happened where it did. I count myself in this picture rather than above it: I am one of the people who found the whole thing exciting, and from the other side of the table that enthusiasm can look like a decision already taken. A lot of what this playbook says about adoption and ownership comes from noticing that.

This playbook is what I learned from both: defining a harness for a team, building Avenida from scratch with AI in every part of the product cycle, working across several coding agents, and reading everything I could find from the people pushing this further than anyone. I write more about the human side of it on [my blog](https://dreamingecho.es/blog), and the leadership side lives in its sibling, the [Engineering Leadership Playbook](https://dreamingecho.es/playbooks/engineering-leadership/).

If something here is wrong or out of date, [open an issue](https://github.com/dreamingechoes/ai-builder-playbook/issues). The playbook is meant to keep redrawing its own lines, the same way a good harness does.

---

## Related pages

- [Principles overview](index.md) — The position this playbook takes, in a page.
- [Core Principles](core-principles.md) — The eight principles every other chapter builds on.
- [The State of the Evidence](state-of-the-evidence.md) — What the research supports, before you plan on it.
