---
title: Team
description: How AI changes a team — measuring impact honestly, adopting without mandates, reshaping roles, growing judgment, sustaining intensity, and paying for it.
---

# Team

A team does not move at the speed of its tooling.

Most of what has been written about AI in engineering is about the tooling, because the tooling changes every week and makes for good demos. The harder part, and I think the more interesting one, is what happens to the people: how they learn, how they trust each other, how their work changes shape, how tired they get, and how the organisation decides what any of this is worth.

This section is about that part. It is also the section where my years leading teams matter most. Before I went back to building, I spent a long stretch on the other side of hiring panels, one-on-ones, and quarterly reviews, and most of what I learned there turns out to apply, sometimes uncomfortably directly, to teams that now work alongside agents. Where a topic needs the general leadership treatment rather than the AI-specific one, I point to the [Engineering Leadership Playbook](https://dreamingecho.es/playbooks/engineering-leadership/) instead of repeating it.

!!! tip "The one-liner"
    The tools change how fast work can be produced. The team decides whether that speed turns into understanding, or into debt nobody can see.

---

## What problem this solves

When a team adopts coding agents, the visible change is in output: more pull requests, shorter cycle times, bigger diffs. The invisible changes are in everything around the output. Who reviews, and how carefully. Who learns the codebase, and how. What a productive day feels like, and whether anyone still has the energy for the expensive decisions at four in the afternoon. What the finance team sees on the invoice.

Organisations tend to notice those changes late, and in the wrong order. They notice the bill before they notice the rubber-stamped reviews. They notice the rubber-stamped reviews before they notice that nobody junior has been inside a real decision for six months. By the time the third one shows up, it shows up as a hiring problem, years away from its cause.

The six pages in this section are an attempt to make those changes visible early, while they are still cheap to correct.

---

## What this section covers

The pages follow roughly the order in which a team runs into each question. Measurement comes first, because without it every later argument is about feelings. Adoption follows, because how a team changes decides whether the measurements mean anything. Then come three pages about people (roles, judgment, intensity), and finally the money.

| Page                                                            | The question it answers                                                                   |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **[Measuring Impact Honestly](measuring-impact.md)**            | How do we know whether AI is helping, and avoid fooling ourselves?                        |
| **[Adoption Without Mandates](adoption-without-mandates.md)**   | How do teams actually change how they work, and what makes it stick?                      |
| **[Roles & the Shape of Work](roles-and-the-shape-of-work.md)** | What changes in the engineer's job, and in what we value and promote?                     |
| **[Growing Judgment](growing-judgment.md)**                     | Where do future senior engineers learn when agents take the work that used to teach them? |
| **[Human Latency & Sustainable Intensity](human-latency.md)**   | Why can faster delivery make teams more tired, and what do we do about it?                |
| **[Cost & Token Economics](cost-and-token-economics.md)**       | What does this cost, how do we budget it, and how do we know it is worth it?              |

The pages lean on each other. The fatigue described in human latency is one of the reasons self-reported productivity is unreliable in measuring impact. The human gates described in growing judgment are the same ones the [harness](../harness/human-gates.md) uses for control. Cost per accepted change only means something if the acceptance it counts comes from reviewers who still had the attention to say no.

---

## Three kinds of resistance

Every change to how a team works meets resistance, and resistance to agent-heavy ways of working tends to take three shapes. Only one of them is loud:

1. **The engineer who argues in planning.** Visible, and often asking the right questions.
2. **The one who agrees and keeps doing the work by hand.** Because they do not trust what comes out the other end.
3. **The one who goes along completely,** approving whatever is green. From the outside it looks like adoption, and it is probably the riskiest of the three.

The easy move is to treat all of them as a mindset problem. That misreads what most of them are reacting to. For many engineers, writing code is not only the job. It is how they come to understand a system well enough to be responsible for it. Some of the scepticism is simply correct: the person asking who will understand this code when it breaks at three in the morning is asking the right question.

I count myself in this picture rather than above it. I was one of the people who built my team's harness, and I found the whole thing exciting, and from the other side of the table that enthusiasm can look like a decision already taken. Some of the doubt in a team is about agents. Some of it is about not having been asked. That second part, at least, a team can fix.

---

## The thread running through all six pages

If there is one idea that connects the section, it is that the slow parts of the work were doing more than anyone noticed.

The time it took to write code used to double as time spent thinking about trade-offs. The time a review took was also time a reviewer spent building a mental model. The small, safe tasks a junior engineer was given were also how they learned the codebase. The question you asked a colleague was also how the team found out what was confusing. None of these side effects were ever designed. They arrived bundled with the slow parts of the work, and when the slow parts got fast, they quietly left.

So most of what follows is about deliberately rebuilding things that used to happen by accident: measurement that can tell output from value, adoption that earns trust instead of extracting compliance, career paths that reward decisions rather than volume, learning positions inside real decisions, limits on intensity, and budgets tied to outcomes. None of it requires slowing the tools down. It requires noticing what the old pace was doing for you.

---

## What good looks like

| Signal                           | What you observe                                                |
| -------------------------------- | --------------------------------------------------------------- |
| **Impact measured, not assumed** | Baselines, acceptance, rework, and outcomes, not just adoption  |
| **Change through evidence**      | Teams adopt workflows they watched succeed, fail, and get fixed |
| **Judgment visibly valued**      | Promotion and recognition reward decisions, not output volume   |
| **Learning designed in**         | Juniors sit inside decisions, not downstream of them            |
| **Sustainable pace**             | Intensity is budgeted; recovery is protected                    |
| **Spend understood**             | Cost per outcome is known and discussed                         |

---

## Failure modes and mitigations

These are the section-level versions. Each page has its own, more specific list.

| Failure mode          | What it looks like                                 | Mitigation                                                |
| --------------------- | -------------------------------------------------- | --------------------------------------------------------- |
| **Adoption mandates** | "Everyone must use agents for 50% of PRs"          | Clear stance, champions, opt-in workflows                 |
| **Output worship**    | More PRs celebrated; rework ignored                | Measure acceptance and outcomes                           |
| **Senior-only teams** | Juniors not hired "because AI"                     | Design learning positions deliberately                    |
| **Invisible fatigue** | Rubber-stamping, irritability, quiet disengagement | Intensity budgets, no-LLM blocks, recovery after shipping |
| **Budget shock**      | Annual AI spend gone in a quarter                  | Per-workflow budgets and cost per outcome                 |

---

## Further reading

- **[From coding agents to software factories](https://dreamingecho.es/blog/coding-agents-to-software-factories)** by Iván González Sáiz — Where the three kinds of resistance and "one workflow before a transformation" come from.
- **[How AI is transforming work at Anthropic](https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic)** by Anthropic — An unusually candid internal study that covers productivity, skill atrophy, and mentorship in one place. (Vendor study of its own staff.)
- **[State of AI-assisted Software Development 2025](https://dora.dev/research/2025/dora-report/)** by DORA — The research behind the idea that AI amplifies an organisation's existing strengths and weaknesses.

## Related pages

- [Engineering Leadership Playbook](https://dreamingecho.es/playbooks/engineering-leadership/) — The people side of engineering in depth: one-on-ones, growth, performance, sustainable pace.
- [Principles](../principles/index.md) — The eight principles this section applies to teams.
- [Ownership & Change](../harness/ownership-and-change.md) — Who owns the shared harness, and how changes to it are made.
