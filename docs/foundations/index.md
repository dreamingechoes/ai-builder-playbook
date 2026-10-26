---
title: Foundations
description: How coding agents work and the primitives you build with — context, instructions, skills, tools, subagents, and hooks — explained once, properly.
---

# Foundations

You can use coding agents productively without understanding how they work. You cannot design a harness that way.

The gap shows up quickly. An engineer who has only *used* agents explains a bad run in human terms: it got confused, it forgot, it was lazy today. An engineer who understands the mechanism explains it in system terms: the instruction was 120,000 tokens back, the test output drowned it, nothing checked the claim that tests passed. The first explanation leads to retrying. The second leads to a change that stops the failure happening again — and that change is what a harness is made of.

This section is the minimum mental model I think every builder needs in late 2026: what an agent actually does in a loop, why context is the scarce resource, and what each of the primitives — instructions, skills, tools, subagents, hooks — is good for and bad at. It is deliberately tool-agnostic, with concrete examples from the tools whose behaviour is documented well enough to show precisely.

!!! tip "The one-liner"
    An agent is a model in a loop. Instructions, skills and tools shape what it sees; hooks and permissions decide what it may do; subagents run the loop again somewhere else. Everything in this playbook is built from those pieces.

---

## Why the primitives are worth learning once

A year ago, every agent had its own vocabulary and its own file formats, and learning one taught you little about the next. That has changed faster than most people noticed.

`AGENTS.md` and the Model Context Protocol now sit in a vendor-neutral home: the Agentic AI Foundation, launched under the Linux Foundation in December 2025 with MCP from Anthropic, `AGENTS.md` from OpenAI and goose from Block. Agent Skills became an open standard the same month. A July 2026 source-code study of eleven agent harnesses found `SKILL.md` skills in nine of them and MCP in eight, and noted that Codex had adopted Claude Code's hook vocabulary verbatim. The names still differ at the edges, and the details of loading and discovery differ more than the marketing suggests. But the ideas have converged.

That convergence is what makes a team harness possible. If the same `AGENTS.md`, the same skills and the same guards work across the agents your engineers prefer, the harness belongs to the team rather than to a tool. It is also why this section explains mechanisms rather than menus: the menu will change next month; the loop will not.

---

## Where each primitive acts

Every primitive in this section attaches to a specific point in the same loop. Holding that picture makes most design decisions easier.

```text
            ┌──────────────── context window ────────────────┐
            │ instructions (always)   skill descriptions     │
            │ tool definitions        conversation + results │
            └───────────────────────────┬────────────────────┘
                                        ▼
                                  model decides
                                        │ tool call
                                        ▼
                      hooks and permissions: allowed?
                         │ no: reason back       │ yes
                         ▼                       ▼
                    model adapts         tool runs (or a subagent
                                          runs its own loop)
                                                 │
                                       result appended to context
```

Instructions and skill descriptions are in the window from the start. Skills add their full text only when a task matches. Tools define what an action can be, and their results are what fills the window fastest. Hooks and permissions sit between the decision and the action. Subagents are a whole second loop whose only output, from the parent's point of view, is one tool result.

---

## What this section covers

| Page                                                            | What you will understand                                                                                         |
| --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [**How a Coding Agent Works**](how-a-coding-agent-works.md)     | The agent loop, what the model sees on every step, and why agents fail the way they do — traced through one run. |
| [**Context Engineering**](context-engineering.md)               | Why context degrades, why it is the lever you fully control, and how to manage it across long tasks.             |
| [**Instructions & AGENTS.md**](instructions-and-agents-md.md)   | How agents discover instruction files, what the evidence says about them, and why less is usually more.          |
| [**Skills**](skills.md)                                         | Packaged procedures that load only when relevant, the open standard, and how to design and test one.             |
| [**Tools & MCP**](tools-and-mcp.md)                             | Giving agents capabilities: when to use a CLI, a skill or an MCP server, and how to design tools well.           |
| [**Subagents & Orchestration**](subagents-and-orchestration.md) | When splitting work across agents helps, when it only adds handoffs, and what the evidence says.                 |
| [**Hooks & Deterministic Guards**](hooks-and-guards.md)         | Turning rules that must never be broken into code that cannot be ignored.                                        |

Read them in order if you are new to this. The first two pages are the mental model; the other five are applications of it. If you already work with agents daily, start wherever your harness hurts most — the pages stand alone.

---

## The primitives at a glance

| Primitive                      | Loaded                                                                                                       | Deterministic?                         | Best for                                                        | Worst for                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------ | -------------------------------------- | --------------------------------------------------------------- | ---------------------------------------- |
| **Instructions** (`AGENTS.md`) | Always                                                                                                       | No                                     | Short, universal facts: commands, boundaries, where things live | Long procedures, rarely needed knowledge |
| **Skills**                     | Description always, body on demand                                                                           | No (scripts inside can be)             | Repeatable procedures with a clear trigger                      | Rules that must always apply             |
| **Tools / MCP**                | Definitions up front in some agents, deferred behind a tool search in Claude Code and Codex; results on call | Yes (the tool), no (when it is called) | Live capabilities: APIs, data, browsers                         | Static knowledge                         |
| **Subagents**                  | On delegation                                                                                                | No                                     | Isolating noisy exploration; parallel independent work          | Tightly coupled tasks with many handoffs |
| **Hooks**                      | On events                                                                                                    | Yes                                    | Rules that must never be broken; formatting; logging            | Judgment calls                           |

A useful rule of thumb: **if it must always happen, make it a hook or a check. If it should happen when relevant, make it a skill. If the agent needs it on every task, make it an instruction — and keep it short.**

The rule of thumb has a corollary that teams learn the hard way: the cost of getting it wrong is asymmetric. A rule placed in a skill when it should have been a hook fails silently, on the one task where the skill did not load. A procedure placed in the instruction file when it should have been a skill fails slowly, by making every session a little heavier and a little less focused. Neither failure announces itself, which is why the pages that follow spend so much time on how to notice them.

---

## What good looks like

| Signal                               | What you observe                                                                |
| ------------------------------------ | ------------------------------------------------------------------------------- |
| **Small, curated instruction files** | A root `AGENTS.md` that fits on a screen, pointing to deeper docs               |
| **Skills with sharp descriptions**   | Agents load the right skill without being told to                               |
| **Hard rules enforced in code**      | Destructive commands blocked by hooks and permissions, not by polite paragraphs |
| **Exploration isolated**             | Research happens in subagents; the main context stays clean                     |
| **Primitives portable**              | The same instructions and skills work across the agents the team uses           |

---

## Failure modes and mitigations

Most harnesses go wrong in the same direction: towards *more*. More lines in the instruction file, more servers connected, more agents in the diagram. Each addition is locally reasonable and collectively expensive, and the cost is paid in attention on every step of every task.

| Failure mode                   | What it looks like                                      | Mitigation                                                                |
| ------------------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Instruction sprawl**         | A 1,500-line instruction file nobody has read in months | Move procedures to skills, rules to hooks, prune the rest                 |
| **Tool hoarding**              | Twenty MCP servers connected "in case"                  | Each server costs context and widens the attack surface; connect per task |
| **Agent org charts**           | Eight specialised agents with elaborate handoffs        | Start with one agent and subagents for isolation; add roles on evidence   |
| **Prose guardrails**           | "Never delete production data" in Markdown              | Enforce it in permissions, hooks or server-side controls                  |
| **Vendor lock-in by accident** | Everything written in one tool's proprietary format     | Prefer `AGENTS.md` and the open skills format                             |

---

## Further reading

- **[How coding agents work](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/)** by Simon Willison — The shortest good explanation of the loop.
- **[Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** by Anthropic — The attention-budget view of context that this section builds on. (Vendor engineering post.)
- **[Harness Engineering: anatomy, architecture, and evolution of coding agents](https://arxiv.org/abs/2609.00006)** by Paul Barbaste et al. — What eleven agent harnesses actually implement, from their source code. (Preprint.)
- **[Linux Foundation announces the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)** by the Linux Foundation — The neutral home for MCP and `AGENTS.md`.

---

## Related pages

- [Glossary](../principles/glossary.md) — Definitions for every term used here.
- [What a Harness Is](../harness/what-a-harness-is.md) — How the primitives combine into a system.
- [Portability](../tools/portability.md) — Keeping one source of truth across several agents.
