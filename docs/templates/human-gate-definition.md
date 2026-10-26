---
title: Human Gate Definition
description: A template for defining a point where the harness stops for a person — trigger, inputs, decision, owner, output, timeout, and what understanding the person adds — with a fully worked example.
---

# Human Gate Definition

Every harness has places where it stops and waits for a person. Most teams define them in one line — "a human approves before merge" — and then discover, a few months later, that the line has become one of two things. Either a rubber stamp, approved in seconds because the inputs were a wall of agent output nobody could evaluate. Or a bottleneck, where work queues for days because nobody knew they owned the decision.

A written gate definition is how you avoid both. It protects **the decision the harness cannot make for itself**, and makes sure the person making it has what they need: a short recommendation, the evidence behind it, an explicit list of what the agent could not verify, and a clear idea of what understanding they are expected to bring from outside the tool. That last part is the one most often missing. Some gates exist to stop an irreversible action. Others exist because the answer is not in the data at all, and someone has to go and talk to people.

!!! tip "The one-liner"
    Define what the person sees, what they decide, and what they bring that the data cannot; a gate without inputs becomes a reflex.

---

## What problem this solves

Without explicit gates, agents run straight through decisions that needed a person, and you get a polished answer to an incomplete question. With gates that are named but not designed, people approve out of habit, and the gate is theatre.

The template forces the design questions that matter: what triggers the stop, which question the person is answering, exactly what they see, what they should do before deciding, which options they have, what gets recorded, and what happens if nobody decides. It also asks you to name health metrics, because a gate is the one part of the harness whose failure is invisible from the inside. A gate approved 100% of the time looks exactly like a gate that works.

It covers both kinds of gate described in [Human Gates](../harness/human-gates.md): **control gates**, which prevent irreversible or high-risk actions, and **judgment gates**, which bring in understanding that does not exist in the data. Many gates are both.

---

## When to use this

**Use it when:**

- You add any point where the harness stops for a person: problem commitment, scope, approach review, merge, rollout advance, impact decision, external communication.
- An existing gate is approved almost every time, or work waits at it for days.
- You change the risk tiers, and gate requirements change with them.
- A [Harness Brief](harness-brief.md) names a gate and you are about to implement it.

**Skip it when:**

- The check is fully mechanical. If a script can decide, make it a sensor, not a gate.
- The action is trivially reversible and tier 0. A gate there spends attention for nothing.
- You cannot name an owner who can actually judge the inputs. Fix that first; a gate owned by "the team" is owned by nobody.

---

## Roles and ownership

| Role                                         | Responsibility                                                                                    |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **Gate owner** (a named role, with a backup) | Makes the decision; challenges weak inputs; does the work outside the tool that the gate requires |
| **Harness owner**                            | Implements the gate; builds the input summary; reports approval rate, wait time and overrides     |
| **Engineering manager**                      | Makes sure gate owners have time for the work behind the gate, not just the click                 |
| **Product partner**                          | Co-owns problem, scope, rollout and impact gates                                                  |
| **Area owner**                               | Owns gates on tier 3 changes in their area                                                        |

---

## How to use it

### Step 1: Write the question before the mechanism

Start with one sentence: the question the person answers at this gate. "Is the evidence enough to trust this change?" is a control question. "What do these signals mean for real users, and should exposure grow?" is a judgment question. If you cannot write the sentence, you do not yet know why the gate exists.

### Step 2: Design the inputs carefully

The inputs decide whether the gate gets a real decision or a reflex. Aim for one screen: a recommendation of a few lines, the evidence with links, the things the agent could not verify, and open questions. Put the unknowns near the top. They are what the person is there for.

**Tip:** ask the gate owner to approve the input format before you build it. They know what they need to decide; the harness owner usually guesses.

### Step 3: Say what understanding the person adds

For judgment gates, write down what the person should do outside the tool: talk to an affected customer, check with support, ask the platform team. This turns the gate from approval into engineering work, and makes that work visible and planned.

### Step 4: Make every option cheap, including rejection

List the options: approve, request changes, reject, not applicable — each with a reason. If rejecting means hours of rework, people approve marginal work. Earlier, smaller gates make rejection cheaper.

### Step 5: Define the timeout

Decide what happens if nobody decides. The safe default is that nothing proceeds and the owner, then the backup, is reminded. Never let a timeout approve.

### Step 6: Review the health metrics after a month

Look at approval rate, wait time, and how often the decision differed from the agent's recommendation. A gate that is always approved needs better inputs, a smaller scope, or conversion into an automated check. A gate that never changes a decision may not need to exist.

---

## The template

```markdown
# Gate: <name>

**Type:** judgment | control | both
**Owner (role):** <role> — **Backup:** <role>
**Applies to risk tiers:** <0 / 1 / 2 / 3>

## Trigger
When does the harness stop here?

## The question the person answers
<One sentence. e.g. "Is this problem worth solving, and for whom?">

## Inputs (what the person sees, on one screen)
- Recommendation (max 5 lines)
- Evidence: <links, metrics, test output>
- What the agent could NOT verify or determine
- Open questions
- <other>

## Understanding the person adds
What should they do outside the tool before deciding?
<e.g. talk to two affected customers; check with the platform team>

## Decision options
- Approve
- Request changes (with what)
- Reject (with reason)
- Not applicable (with reason)

## Output
What is recorded, where, and what the next step consumes.

## Timeout
What happens if no decision is made within <time>?
(Default: nothing proceeds; owner, then backup, is reminded.)

## Health metrics
- Approval rate: <target range>
- Median wait time: <target>
- Decisions that differed from the agent's recommendation: <tracked>
- Reviewed on: <date>
```

---

## Worked example

*Illustrative scenario: the team, feature and numbers below are hypothetical. They show what a completed gate definition, and its first health review, can look like.*

Imagine the billing team shipping automated invoice-reminder emails: customers with an overdue invoice receive a reminder three days after the due date. The rollout runs in stages — internal accounts, 5% of customers, 25%, everyone — and at each stage an agent watches the signals and prepares a status. Aisha, the product manager, and Tomás, the engineering manager, agreed that advancing exposure would be a gate co-owned by the engineer running the rollout and Aisha herself.

```markdown
# Gate: Rollout advance — invoice reminders

**Type:** both (control: exposure is hard to undo for emails already sent;
judgment: what the signals mean for customers)
**Owner (role):** Engineer running the rollout, with product manager
**Backup:** Engineering manager
**Applies to risk tiers:** 2 and 3

## Trigger
The current exposure stage has run for its minimum window (48h internal,
72h at 5%, 72h at 25%) and the agent's stage status is ready.

## The question the person answers
Do the signals at this stage show that reminders help customers pay on time
without creating complaints or support load we did not expect?

## Inputs (what the person sees, on one screen)
- Recommendation: advance / hold / roll back, with three lines of reasoning
- Evidence: payment-within-7-days rate vs control cohort; unsubscribe rate;
  support tickets tagged `billing-reminder`; email bounce rate; error logs
  from the reminder worker
- Could NOT verify: whether customers who paid would have paid anyway
  (control cohort is small at 5%); sentiment beyond ticket tags
- Open questions: <listed by the agent>

## Understanding the person adds
Read five of the tagged tickets in full. Ask support whether anything about
reminders came up that was not tagged. At the 25% stage, call two customers
who received a reminder.

## Decision options
- Advance to next stage
- Hold for another window (with what we are waiting to learn)
- Roll back (with reason; reminders stop, worker disabled by flag)
- Not applicable — not used for this feature

## Output
Decision and reasoning posted on the rollout issue; the agent records it in
the rollout log and either changes the flag or schedules the next status.

## Timeout
If no decision within 24h of the status, nothing advances; owner is reminded,
then the backup at 48h. Exposure never grows by default.

## Health metrics
- Approval rate: expected 60–90% (holds are normal)
- Median wait time: under 8 working hours
- Decisions that differed from the agent's recommendation: tracked per stage
- Reviewed on: 2026-10-08 (two weeks after reaching 100%, one month after first use)
```

After the rollout, the harness owner reviewed the gate. Four decisions were taken: two advances that matched the agent's recommendation, one advance where the agent had recommended holding (a spike in bounces turned out to be a single customer's misconfigured mail server, which the engineer confirmed with support), and one hold where the agent had recommended advancing — because Aisha's two customer calls revealed that reminders were reaching finance inboxes nobody read, which no metric showed. Median wait was five hours.

That last decision is the reason the gate exists. The data said advance. A conversation said the feature was reaching the wrong person. The team changed the recipient logic before going to 100%, and the impact review thirty days later credits that change for most of the effect.

---

## Signals it is working

- A meaningful share of decisions differ from the agent's recommendation, and the reasons are recorded.
- Judgment gates trigger conversations with users, support and stakeholders, and those conversations show up in the reasoning.
- Inputs fit on one screen, and unknowns are near the top.
- Wait times are visible and mostly within target.
- Small changes pass through fewer gates than large ones.
- Gates get removed or converted to checks when they stop changing outcomes.

---

## Failure modes and mitigations

Gates decay in a predictable order. First the inputs grow, because adding evidence feels safer than removing it. Then the owner, faced with three screens of output, starts skimming. Approval time drops to seconds. Nobody notices, because the approval rate looks like quality. The first sign is usually an incident where everyone involved "approved" something nobody read.

| Failure mode                       | What it looks like                          | Mitigation                                                  |
| ---------------------------------- | ------------------------------------------- | ----------------------------------------------------------- |
| **Rubber-stamping**                | Approval within seconds, every time         | One-screen inputs; approval-time and override tracking      |
| **Approval without understanding** | The owner cannot evaluate what they approve | Owners must be able to judge the evidence; reassign if not  |
| **Unowned gate**                   | "The team" approves; nobody actually does   | A named role and a named backup                             |
| **Expensive rejection**            | Marginal work approved to avoid rework      | Earlier, smaller gates                                      |
| **Timeout that approves**          | Work proceeds when nobody answers           | Nothing proceeds by default; escalate to the backup         |
| **Gate sprawl**                    | Every step needs approval                   | Tie gates to risk tiers; convert checkable gates to sensors |

---

## Related pages

- [Human Gates](../harness/human-gates.md) — Control and judgment gates, and where they usually belong.
- [Risk Tier Matrix](risk-tier-matrix.md) — Which gates each tier requires.
- [Rollout & Learning](../workflow/rollout-and-learning.md) — Staged exposure and the rollout-advance gate in context.
- [Review at Agent Speed](../workflow/review-at-agent-speed.md) — Designing the merge gate when volume rises.
