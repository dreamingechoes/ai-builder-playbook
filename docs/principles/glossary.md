---
title: Glossary
description: The vocabulary of agentic engineering, defined the way this playbook uses it, with origins where a term has one.
---

# Glossary

The vocabulary of this field is young and inconsistent. The same word means different things in different vendors' docs, and new terms appear monthly. These are the definitions this playbook uses. Where a term has a clear origin, it is noted, with a link in [Further reading](#further-reading). Where it does not, the definition is mine and should be read as such.

!!! note "Last reviewed"
    October 2026. Terms in this field are coined faster than they settle. Some of these will be renamed within a year; the ideas underneath them should last longer.

---

## Core concepts

**Agent**
: A model running in a loop with tools: it reads its context, decides on an action, calls a tool, observes the result, and repeats until it decides it is done or something stops it. A coding agent is an agent whose tools act on a codebase and its environment.

**Agentic coding**
: Using a coding agent that can read, edit, run and test code across many steps, rather than an assistant that suggests completions or answers questions one turn at a time. This playbook uses it for the activity and *agentic engineering* for the discipline around it.

**Agentic engineering**
: Building software with coding agents doing a significant share of the implementation, while engineers design, direct, and verify the work. Simon Willison's preferred term, which he uses for "the practice of developing software with the assistance of coding agents". In this playbook it is the opposite of *vibe coding*.

**Vibe coding**
: Building software by prompting and accepting output without reading or understanding it. Coined by Andrej Karpathy in February 2025. Fine for throwaway prototypes; a liability for anything someone will have to maintain.

**Harness**
: Everything around the model that turns it into a reliable agent: instructions, tools, permissions, context management, memory, checks, gates, and the workflow itself. Commonly summarised as **Agent = Model + Harness**. See [What a Harness Is](../harness/what-a-harness-is.md).

**Inner harness**
: The harness a coding agent ships with — Claude Code, Codex, Cursor, and the rest each have one. Birgitta Böckeler's article describes the coding agent's own harness as the *builder harness*; "inner harness" is this playbook's shorthand.

**Outer harness**
: The harness a team builds on top of the inner one, for its own codebase and way of working. Böckeler also calls it the *user harness*. Most of this playbook is about the outer harness.

**Harness engineering**
: The practice of improving the harness every time an agent makes a mistake, so the mistake becomes less likely or impossible. The term was popularised in early 2026 by Mitchell Hashimoto and by OpenAI's write-up of building a product with no manually written code.

**Background agent / cloud agent**
: An agent that runs asynchronously in a remote environment, usually triggered from an issue, chat message, or schedule, and returns a pull request or report. The person who started it is not watching while it works, which moves all of the checking to the end.

**Software factory**
: A delivery system where the way of working belongs to the team rather than to individual engineers — shared intake, harness, checks, gates, and learning loops through which agents do most of the implementation. See [What a Factory Is (and Isn't)](../factories/what-a-factory-is.md).

**Dark factory / lights-off factory**
: The far end of the factory spectrum: specifications go in, software comes out, and no human writes or reviews the code. Dan Shapiro's top level of AI-assisted programming. An experiment, not a destination.

---

## Context and instructions

**Context window**
: Everything the model can see in a single call: system prompt, instructions, conversation, tool results, files read. Finite, and performance degrades well before it is full.

**Context rot**
: The decline in a model's reliability as its input grows longer, even on simple tasks. Popularised by a July 2025 technical report from Chroma, which tested 18 models and found performance "grows increasingly unreliable as input length grows". The practical reason to keep context small even when the window is large.

**Context engineering**
: Deciding what goes into the context window, in what form, and when — including what to leave out. The successor to prompt engineering once agents run for many steps. See [Context Engineering](../foundations/context-engineering.md).

**Compaction**
: Summarising the work so far into a smaller form so an agent can continue with a fresh or reduced context. Most agents do it automatically when the window fills.

**Intentional compaction**
: Compacting deliberately, at phase boundaries, rather than waiting for the tool to do it — typically by writing research or a plan to a file and starting the next phase in a fresh session. Dex Horthy of HumanLayer described *frequent intentional compaction* in August 2025: designing the whole research, plan and implement workflow around context management, and keeping utilisation in roughly the 40–60% range.

**AGENTS.md**
: A plain Markdown file at the root of a repository (and optionally in subdirectories) that gives coding agents project context. An open convention that emerged from several agent vendors and was contributed by OpenAI in December 2025 to the Agentic AI Foundation under the Linux Foundation, which at the time reported it had been adopted by more than 60,000 open-source projects and agent frameworks. `CLAUDE.md` and similar files are tool-specific equivalents. See [Instructions & AGENTS.md](../foundations/instructions-and-agents-md.md).

**REVIEW.md**
: A short file in which code owners tell an AI code reviewer what matters in their part of the code: what counts as a failure, what not to nitpick, which paths to ignore. Described by the Claude Code team in October 2026. See [Review at Agent Speed](../workflow/review-at-agent-speed.md).

**Instruction**
: Persistent guidance loaded into an agent's context automatically: conventions, commands, boundaries. Always-on, so every line has a cost.

**Skill**
: A folder containing a `SKILL.md` file with a name, a description, and instructions, plus optional scripts and references. Only the name and description are loaded up front; the rest loads when the task matches. Originally developed by Anthropic and released as an open standard (Agent Skills) in December 2025. See [Skills](../foundations/skills.md).

**Plugin**
: A package that bundles several extensions — skills, subagents, hooks, MCP servers — so they install, update and switch on as one unit. In Claude Code, a plugin is "a directory of skills, agents, hooks, MCP servers, or other components" loaded together, usually from a marketplace. Useful for sharing a team's harness; costly if nobody checks what each enabled plugin adds to every session's context.

**Progressive disclosure**
: Loading information into context only when it becomes relevant. The design principle behind skills.

**Spec**
: A written description of intended behaviour — the problem, the expected outcome, acceptance criteria and constraints — agreed before implementation. In agentic work it replaces the conversation by the desk that used to fill in what a two-line ticket left out. See [Intent Before Code](../workflow/intent-before-code.md).

---

## Tools and execution

**Tool**
: A function an agent can call — read a file, run a command, query an API, open a browser.

**MCP (Model Context Protocol)**
: An open protocol for connecting agents to external tools and data sources through servers that expose tools, resources, and prompts. Open-sourced by Anthropic in November 2024 and contributed to the Agentic AI Foundation in December 2025. See [Tools & MCP](../foundations/tools-and-mcp.md).

**Subagent**
: A separate agent instance, with its own context window, launched by a main agent to perform a bounded task and return a condensed result.

**Hook**
: A script the agent's runtime executes at a defined point — before a tool call, after an edit, at session start — that can observe, modify, or block the action. Deterministic, unlike instructions. See [Hooks & Deterministic Guards](../foundations/hooks-and-guards.md).

**Sandbox**
: An isolated environment that limits what an agent can read, write, execute, or reach on the network. See [Sandboxing & Permissions](../security/sandboxing-and-permissions.md).

**Worktree**
: A Git feature for managing "multiple working trees attached to the same repository", so several branches can be checked out side by side. Commonly used to run agents in parallel without them overwriting each other's changes.

---

## Quality and control

**Guide (feedforward control)**
: Anything that steers an agent before it acts — instructions, skills, specs, examples, templates. The term as Böckeler uses it.

**Sensor (feedback control)**
: Anything that observes after the agent acts and lets it self-correct — tests, linters, type checkers, review agents, runtime signals.

**Computational control**
: A deterministic check run by ordinary code: tests, linters, type checkers, structural analysis. Fast, cheap, and cannot be argued with.

**Inferential control**
: A check performed by a model: AI code review, LLM-as-judge. Slower, costlier, non-deterministic, and able to reason about meaning.

**Back pressure**
: Automated feedback built into an agent's environment — build output, type errors, failing tests, rendered UI checks — that resists progress until the work passes, so the agent corrects itself instead of a human doing it. Geoffrey Huntley used the term in his July 2025 Ralph post; Moss's January 2026 post "Don't waste your back pressure", which Huntley endorsed, gives the fullest description.

**LLM-as-judge**
: Using a model to grade the output of another model (or agent) against a rubric. The term was popularised by Zheng et al.'s 2023 paper, which found strong models agreed with human preferences more than 80% of the time — about as often as humans agree with each other. Useful for judging meaning; dangerous as the only check, because a judge can share the author's blind spots. Anthropic's engineers found agents "reliably skew positive when grading their own work" — most of all on subjective tasks such as design, where there is no test to fail.

**Human gate**
: A point in a workflow where an agent must stop and a named person must decide before work continues. See [Human Gates](../harness/human-gates.md).

**Risk tier**
: A classification of changes by blast radius and reversibility that determines how much autonomy and review they get. See [Risk Tiers & Autonomy](../factories/risk-tiers-and-autonomy.md).

**Eval**
: A repeatable task with a grader, used to measure whether an agent, skill, or harness change performs better or worse. *Capability evals* start with low pass rates and give you something to improve; *regression evals* should stay near 100%.

**pass@k / pass^k**
: Two ways of scoring an agent that is run *k* times on the same task. **pass@k** counts the task as solved if *any* of the k attempts succeeds — the right measure when a human will pick the best attempt. **pass^k** counts it as solved only if *all* k attempts succeed — the right measure when the agent runs unattended and has to be reliable every time. pass^k was introduced in the τ-bench paper (Yao et al., 2024) to measure the reliability of agent behaviour over multiple trials. The gap between the two numbers is a good measure of how much you can trust a workflow without watching it.

**Harnessability**
: How amenable a codebase is to being harnessed — typed languages, clear module boundaries, fast tests, and opinionated frameworks all help. The term as Böckeler uses it. See [Harnessability](../harness/harnessability.md).

---

## Security

**Prompt injection**
: Instructions hidden in content an agent reads — a web page, an issue, a document, a tool result — that the model follows as if they came from its user. The term was first used in May 2022 and popularised by Simon Willison in September 2022. *Indirect* prompt injection, where the instructions arrive through retrieved content rather than the user's own input, was described by Greshake and colleagues in 2023. There is no reliable general fix yet; the defences are architectural. See [Agent Threat Model](../security/agent-threat-model.md).

**Lethal trifecta**
: Simon Willison's name, from June 2025, for the combination that makes prompt injection a data-theft risk: an agent with access to private data, exposure to untrusted content, and the ability to communicate externally. My reading is that any two can be managed; all three together let an attacker plant instructions that read your data and send it out. The practical rule is to never give one agent all three at once.

---

## Measurement

**Acceptance rate**
: The share of agent output merged without rework.

**Intervention rate**
: How often a human has to step in during an agent run.

**Rework**
: Changes made to agent output after it was first proposed, or shortly after it merged, to correct it.

**False velocity**
: Rising activity metrics (pull requests, commits, lines) without a corresponding rise in delivered value. A phrase used by DX in its 2026 research, under the warning that "more code does not necessarily mean more business value".

**Verification tax**
: The extra effort spent checking that AI-generated work is correct and safe. Used by InfoQ in summarising DORA's 2026 ROI report, as one of the causes of the temporary dip — a *J-curve* — that the report expects most organisations to go through before gains arrive. See [The State of the Evidence](state-of-the-evidence.md).

**Human latency**
: The time it takes people to understand, trust, and integrate change. AI compresses cycle time; it does not compress this. My own term, from [AI shrinks cycle time. Humans don't.](https://dreamingecho.es/blog/ai-shrinks-cycle-time-humans-dont) See [Human Latency & Sustainable Intensity](../team/human-latency.md).

**Cognitive debt**
: The erosion of a team's shared understanding of its own system, accumulated when changes land faster than people absorb them. Margaret-Anne Storey's 2026 paper describes it as "the erosion of shared understanding across a team", a team-level and project-level property, and pairs it with *intent debt*: the absence or erosion of the explicit rationale, goals and constraints that people and agents need to change code safely. The phrase also appears in an MIT Media Lab preprint on essay writing with ChatGPT, where it describes an individual's reduced engagement. Both senses matter: the team can lose the map, and so can the person.

---

## Further reading

- **[What is agentic engineering?](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/)** by Simon Willison — The definition behind *agentic engineering*, from his Agentic Engineering Patterns guide.
- **[Vibe Coding](https://martinfowler.com/bliki/VibeCoding.html)** by Martin Fowler — The origin and meaning of *vibe coding*.
- **[Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)** by Birgitta Böckeler (martinfowler.com) — Guides, sensors, computational and inferential controls, harnessability.
- **[My AI adoption journey](https://mitchellh.com/writing/my-ai-adoption-journey)** by Mitchell Hashimoto — Harness engineering as a reactive practice.
- **[The five levels: from spicy autocomplete to the software factory](https://www.danshapiro.com/blog/2026/01/the-five-levels-from-spicy-autocomplete-to-the-software-factory/)** by Dan Shapiro — Where *dark factory* comes from.
- **[Context Rot](https://www.trychroma.com/research/context-rot)** by Kelly Hong, Anton Troynikov and Jeff Huber (Chroma) — How performance degrades with input length. (Vendor technical report.)
- **[Advanced context engineering for coding agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)** by Dex Horthy (HumanLayer) — Frequent intentional compaction.
- **[Linux Foundation announces the formation of the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)** by the Linux Foundation — AGENTS.md, MCP and goose under open governance.
- **[Harness engineering](https://openai.com/index/harness-engineering/)** by Ryan Lopopolo (OpenAI) — The write-up that popularised the term alongside Hashimoto. (Vendor post.)
- **[Agent Skills](https://agentskills.io/)** — The open specification for skills and progressive disclosure.
- **[Plugins overview](https://code.claude.com/docs/en/plugins)** by Anthropic — What a plugin bundles and what it costs in context. (Vendor docs.)
- **[git-worktree](https://git-scm.com/docs/git-worktree)** — The Git reference.
- **[Ralph Wiggum as a "software engineer"](https://ghuntley.com/ralph/)** by Geoffrey Huntley — The looping-agent technique, with its section on backpressure.
- **[Don't waste your back pressure](https://banay.me/dont-waste-your-backpressure/)** by Moss — The fullest write-up of back pressure as the agent's self-correction loop.
- **[Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685)** by Zheng et al. — The paper that named and measured LLM-as-judge. (Preprint.)
- **[τ-bench](https://arxiv.org/abs/2406.12045)** by Yao et al. — Where pass^k comes from. (Preprint.)
- **[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)** by Anthropic — Capability versus regression evals, and pass@k versus pass^k in practice. (Vendor engineering post.)
- **[The lethal trifecta for AI agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)** by Simon Willison — Private data, untrusted content, external communication.
- **[Prompt injection](https://en.wikipedia.org/wiki/Prompt_injection)** on Wikipedia — History of the term, with links to the original sources.
- **[From technical debt to cognitive and intent debt](https://www.alphaxiv.org/abs/2603.22106)** by Margaret-Anne Storey — Cognitive and intent debt for software teams. (Preprint.)
- **[Your brain on ChatGPT](https://arxiv.org/abs/2506.08872)** by Kosmyna et al. (MIT Media Lab) — The individual sense of cognitive debt. (Preprint; 54 participants.)
- **[The current impact of AI on engineering velocity](https://newsletter.getdx.com/p/the-current-impact-of-ai-on-engineering)** by DX — The warning about false velocity. (Vendor research.)

---

## Related pages

- [Core Principles](core-principles.md) — Where most of these terms are put to work.
- [The State of the Evidence](state-of-the-evidence.md) — The research behind the measurement terms.
- [Foundations](../foundations/index.md) — The primitives — context, instructions, skills, tools, subagents, hooks — in depth.
