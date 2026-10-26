---
title: Tools
description: How to choose AI engineering tools without chasing every release — the landscape by category, evaluating on your own work, portability across agents, and what I actually use.
---

# Tools

!!! note "Last reviewed"
    October 2026. This is the most perishable section of the playbook. Categories should hold; names and features will change.

A new model or tool arrives every few weeks. Every one of them is someone's favourite for a fortnight. If your process depends on which tool is winning this month, you will spend your year migrating.

This section is deliberately short on product recommendations and long on how to decide. The model is a component. So is the tool.

!!! tip "The one-liner"
    Choose tools by the workflow they must support and the evidence from your own work, and keep everything you author in formats that will outlive any one of them.

---

## Why a tools section ages faster than the rest

Most of this playbook is about things that change slowly: how a team frames work, where human judgment sits, what a harness is for. This section is about things that change quickly, and it is worth being honest about how quickly.

Between late 2024 and the end of 2025, the field acquired almost everything it now takes for granted. Anthropic open-sourced the Model Context Protocol in November 2024. Claude Code, OpenAI's Codex CLI and Google's Gemini CLI all appeared during 2025, and GitHub's Copilot coding agent began taking issues and returning pull requests; today most major vendors offer a cloud agent like it. In August 2025 a group of vendors settled on `AGENTS.md` as a shared instruction file; in December the Linux Foundation formed the Agentic AI Foundation to steward it and MCP, and Anthropic published Agent Skills as an open standard.

And then, in 2026, one of those tools was superseded. Google launched Antigravity 2.0 and the Antigravity CLI in May and encouraged Gemini CLI users to migrate; by June the Gemini CLI repository said it had been replaced for "unpaid tier and Google One users". A popular, open-source terminal agent had been Google's terminal agent for about a year.

That pattern is the main thing a tools section should teach. The products move. The categories, and the open formats underneath them, have held.

---

## What this section covers

| Page                                                          | What it covers                                                                                                                     |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **[Landscape by Category](landscape.md)**                     | The kinds of tools that exist, how they converged in 2025–2026, what each is for, and what to look for — without a vendor ranking. |
| **[Evaluating a Tool on Your Own Work](evaluating-tools.md)** | How to compare tools and models on the tasks your team actually does, with honest statistics and blind review.                     |
| **[Portability](portability.md)**                             | Keeping one source of truth for instructions, skills, configuration and memory across several agents.                              |
| **[My Stack](my-stack.md)**                                   | What I use, how it is wired together, and why.                                                                                     |

If you are new to this area, start with the landscape: it gives the product names somewhere to land. If you are choosing between two tools right now, go straight to evaluation. If you already use more than one agent, portability will probably save you the most time.

---

## Principles for choosing tools

1. **Process first.** Know what workflow the tool must support before evaluating it. "Overnight bug fixes from the tracker" and "faster refactoring in the editor" are different purchases.
2. **Evaluate on your tasks.** Public benchmarks track the field; they rarely look like your codebase. In a March 2026 note, METR reported that roughly half of SWE-bench-passing patches from 2024–2025 agents would not have been merged by the projects' maintainers.
3. **Prefer open formats.** `AGENTS.md`, Agent Skills, MCP, plain scripts. Your harness should survive a tool change, because sooner or later it will have to.
4. **Count total cost.** Tokens, compute, and human review time — per accepted change, not per seat.
5. **Check the security model.** Sandboxing, permission boundaries outside the model, data terms, audit. Cloud agents deserve the most scrutiny, because they run with your credentials somewhere you cannot see.
6. **Expect to use several.** A terminal agent, an editor agent and a cloud agent do different jobs, and a second vendor makes a better reviewer than the same one asked twice. Design for more than one.

---

## The tension this section tries to hold

There is a real tension here, and I do not want to pretend otherwise.

On one side, tool churn is expensive. Every migration costs setup time, relearning, and a dip in output while people find their feet again. Teams that switch every quarter rarely get good at any tool.

On the other side, capability jumps are real. METR's 2025 randomised trial found experienced open-source developers slower with early-2025 tools; by February 2026 METR said developers were probably faster, even though its newer study could not measure it cleanly. A team that refuses to look at anything new will eventually be using the wrong tool out of loyalty.

The way through is not a rule about how often to switch. It is a way of switching that is cheap: keep your harness in open formats so a change is an install rather than a rewrite, and keep a small set of your own tasks ready so a decision takes two days of evidence rather than two weeks of opinion. Then you can afford to look at new tools without being captured by them.

---

## Failure modes and mitigations

| Failure mode            | What it looks like                                       | Mitigation                                           |
| ----------------------- | -------------------------------------------------------- | ---------------------------------------------------- |
| **Tool churn**          | A migration every quarter                                | Evaluate on your own work; switch only on evidence   |
| **Lock-in by accident** | Harness written in one tool's proprietary format         | Open formats; one source of truth                    |
| **Benchmark shopping**  | Choosing by leaderboard                                  | Golden-task replay on your repository                |
| **Shadow tools**        | Engineers use unapproved tools because approved ones lag | Approve good options quickly; publish the evaluation |
| **Loyalty lock-in**     | Refusing to look at new tools at all                     | Keep evaluation cheap so looking is cheap            |

---

## Further reading

- **[Linux Foundation announces the formation of the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)** by the Linux Foundation — Why MCP and `AGENTS.md` moved to neutral governance.
- **[Many SWE-bench-passing PRs would not be merged into main](https://metr.org/notes/2026-03-10-many-swe-bench-passing-prs-would-not-be-merged-into-main/)** by METR — The clearest recent evidence that benchmark passes and mergeable code differ.
- **[We are changing our developer productivity experiment design](https://metr.org/blog/2026-02-24-uplift-update/)** by METR — Why measuring AI speed-up got harder in 2026, and what METR now believes.

---

## Related pages

- [Foundations](../foundations/index.md) — The primitives every tool in this section builds on.
- [Data & Policy](../security/data-and-policy.md) — The data questions to ask before adopting any tool.
- [Tool Scorecard](../templates/tool-scorecard.md) — A template for recording an evaluation.
