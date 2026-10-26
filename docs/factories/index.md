---
title: Software Factories
description: What happens when the way of working stops belonging to individual engineers and starts belonging to the team — and how to get there one workflow at a time.
---

# Software Factories

!!! note "Last reviewed"
    October 2026. This section describes a field that changes month to month; the examples and figures will date.

We are starting to build software faster than our teams are learning to work at that speed. Coding agents have changed how most of us approach implementation. Work that used to take a focused afternoon can be delegated, reviewed and iterated on before lunch, and nobody finds that remarkable any more.

Most of what surrounds that work has barely moved. Many teams still plan, coordinate, review and measure delivery the way they did two years ago, with rituals designed around a constraint that is quietly disappearing: the time it took an engineer to write the code.

That gap is why the idea of a software factory caught on so quickly in 2026, and also why it is so easy to misunderstand.

!!! tip "The one-liner"
    Using agents to write code does not turn a team into a factory. The factory is what grows around the agents once the way of working stops belonging to individual engineers and starts belonging to the team.

---

## The idea

Depending on who says it, a software factory is anything from a team with good agent habits to a building with the lights off. In January, Dan Shapiro described five levels of AI-assisted programming ending in a "dark factory" that turns specifications into software with no human in the loop, and observed that almost everyone tops out two levels earlier, where a person still reviews what the agents write. A few weeks later StrongDM described a factory run on two rules: code must not be written by humans, and code must not be reviewed by humans. Most teams using the word sit somewhere far more ordinary. Stripe merges over a thousand agent-written pull requests a week and a person reviews every one.

What I find more interesting than the level of autonomy is a different shift: from individual engineers running their own agents, with their own prompts and habits, to a delivery system the whole team can share, inspect and improve. If every engineer on your team has an excellent personal agent workflow, what you have is a row of separate workshops. They do not add up to a factory on their own. The factory is the shared part: where work comes in, the harness the agents run inside, the checks and gates everyone trusts, and the way lessons from one run reach the next.

Getting there has less to do with finding more things for agents to do than with deciding which parts of your process can become part of the system, and which need to stay with people.

---

## What this section argues

Four ideas run through the pages that follow.

**The bottleneck moves.** When writing code stops being the slow part, the pressure goes to the places where people decide: what is worth building, and what is safe to trust. The middle gets fast and the ends get buried. Teams that conclude "agents don't work for us" after a rough month have often just met their new constraint. Spotify, which has gone further than most, puts it in one line in its write-up of a 2026 talk: the bottleneck moves from coding to decision-making.

**Redesign the workflow before automating it.** A two-line ticket worked because whoever picked it up would wander over and ask what you meant. An agent does not wander over. Much of the work of building a factory is making explicit what a team used to leave in conversations, shared assumptions and one person's head — intent, product context, verification, and what happens after the merge — because automation only pays off once those things have somewhere to live.

**The transition happens through people.** Resistance tends to take three shapes, and only one of them is loud: the engineer who argues in planning, the one who agrees and keeps doing the work by hand, and the one who approves whatever is green — which looks like adoption and is probably the riskiest of the three. For many engineers, writing code is how they come to understand a system well enough to be responsible for it. Some of their scepticism is correct.

**One workflow before a transformation programme.** The transitions that seem to hold up start small, stay reversible for a while, and earn autonomy with evidence rather than announce it. Something deliberately unglamorous, like dependency upgrades, with a person approving every output until the record says otherwise.

---

## How to read this section

| Page                                                         | What it covers                                                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| **[What a Factory Is (and Isn't)](what-a-factory-is.md)**    | The autonomy spectrum, the workshops-versus-factory axis, and where real teams sit.        |
| **[The Bottleneck Moves](the-bottleneck-moves.md)**          | Where the pressure goes, the theory of constraints, and how to find your own constraint.   |
| **[Risk Tiers & Autonomy](risk-tiers-and-autonomy.md)**      | Deciding how much autonomy each kind of change gets, and how it is earned and taken back.  |
| **[Your First Factory Workflow](first-factory-workflow.md)** | One bounded, reversible workflow, step by step, with a six-week worked example.            |
| **[Factory Infrastructure](factory-infrastructure.md)**      | Intake, environments, feedback loops, observability, cost and security boundaries.         |
| **[Field Reports](field-reports.md)**                        | What OpenAI, Stripe, Spotify, StrongDM, PostHog, Warp and others have published, compared. |

If you are new to the idea, read the first two pages in order. If you are about to start, go straight to [Your First Factory Workflow](first-factory-workflow.md) and come back for the rest when you need it. If someone has just sent you a conference talk and asked why your team is not doing that, start with [Field Reports](field-reports.md).

!!! info "What we know / what we don't"
    **We know** that a handful of organisations with deep platform investment run agent workflows at very high volume, and that their own accounts credit infrastructure, verification and existing developer tooling more than any model. **We don't know** what happens to a codebase that few people read closely after a year or two; the most candid report so far, HumanLayer's, describes a lightly supervised codebase archived after three or four months. Cost is unsettled. Most of what has been written about software factories this year comes from companies selling them, or from teams describing young systems of their own. I do not know where most teams will settle. My guess, and it is only a guess, is closer to the light end than the keynotes suggest, because the parts of the process that matter most are made of people, and people change how they work at a different pace from their tools.

---

## What good looks like

| Signal                                  | What you observe                                             |
| --------------------------------------- | ------------------------------------------------------------ |
| **Shared, inspectable system**          | Anyone on the team can see how work flows and change it      |
| **Autonomy earned per workflow**        | Each automated workflow has a track record that justified it |
| **Ends get investment**                 | Intent and verification capacity grow with throughput        |
| **Learning reaches the next iteration** | Production outcomes and rework patterns feed the harness     |
| **People still own decisions**          | Named humans stand behind what ships                         |

---

## Failure modes and mitigations

| Failure mode                 | What it looks like                                  | Mitigation                                               |
| ---------------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| **Factory as tool rollout**  | Licences bought; nothing compounds                  | Treat it as an infrastructure and process project        |
| **Autonomy by announcement** | "From next quarter, agents merge low-risk changes"  | Earn autonomy workflow by workflow on evidence           |
| **Fast middle, buried ends** | More pull requests; planning and review overwhelmed | Invest in intent and verification first                  |
| **Counting pull requests**   | Success measured by agent PR volume                 | Acceptance, rework, intervention, cost per merged change |
| **Transformation programme** | Big-bang reorganisation around agents               | One workflow, one repository, reversible                 |

---

## Further reading

- **[From coding agents to software factories: rethinking how teams work](https://dreamingecho.es/blog/coding-agents-to-software-factories)** by Iván González Sáiz — The essay this section grows from.
- **[The Five Levels: from Spicy Autocomplete to the Dark Factory](https://www.danshapiro.com/blog/2026/01/the-five-levels-from-spicy-autocomplete-to-the-software-factory/)** by Dan Shapiro — The autonomy vocabulary most of the conversation borrows.
- **[Minions: Stripe's one-shot, end-to-end coding agents](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents)** by Alistair Gray (Stripe) — The most transferable large-scale report.
- **[Coding Is No Longer the Constraint](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)** by Spotify Engineering — The moved bottleneck, from the inside.
- **[How do you move from coding agents to a software factory?](https://www.warp.dev/articles/coding-agents-to-software-factory)** by Warp — A practical four-step transition. (Vendor guide.)
- **[Can software factories actually work?](https://posthog.com/newsletter/software-factories)** by Jina Yoon (PostHog) — The middle of the spectrum and the case for product context.

---

## Related pages

- [AI Harness](../harness/index.md) — The shared system a factory is built on.
- [Team](../team/index.md) — Measurement, adoption, roles and the human side of the transition.
- [Review at Agent Speed](../workflow/review-at-agent-speed.md) — Where the moved bottleneck usually lands first.
