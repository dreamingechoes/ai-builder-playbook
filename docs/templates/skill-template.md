---
title: Skill Template
description: A starting point for turning a recurring procedure into a portable skill — a sharp description, focused steps, evidence-producing scripts, and explicit stopping points — with a fully worked example.
---

# Skill Template

A skill is a folder with a Markdown file in it. That simplicity is exactly why skills spread so quickly, and why so many of them do nothing. A team writes twenty skills in a burst of enthusiasm, half of them never load because their descriptions do not match the words a real task uses, and the rest are long prose procedures that the agent follows loosely and nobody has ever compared with doing the task without them.

This template exists to make the few skills you write earn their place. The decision it protects is **whether a procedure is consistent enough, frequent enough and checkable enough to be packaged** — and, once packaged, whether the agent will find it, follow it, and show its evidence. A good skill makes the right thing the easy thing for every engineer and every agent on the team. A bad one is a confident-looking document that quietly changes nothing.

!!! tip "The one-liner"
    One procedure per skill, a description written in task language, scripts that produce facts, and a clear place to stop.

---

## What problem this solves

Teams accumulate procedures: how we write a migration, how we prepare a release note, how we investigate a production error. Putting all of them in always-on instructions bloats every session. Leaving them out means the agent improvises, and each engineer's results depend on how well they happen to prompt.

Skills solve this with progressive disclosure. The agent sees only each skill's name and description until a task matches; then it loads the full instructions, and any bundled scripts or reference files only when the instructions call for them. The template gives that mechanism a shape that tends to work: a description that triggers reliably, short focused steps, scripts that turn opinions into evidence, an explicit point where the skill stops for a person, and a checkable definition of done.

It follows the open [Agent Skills](https://agentskills.io) format, so a skill written once can travel across the agents your team uses.

---

## When to use this

**Use it when:**

- A procedure recurs and has a recognisable trigger: "when adding a migration", "when investigating a production error".
- The procedure needs more than a few lines of instruction, or benefits from scripts.
- Results vary noticeably between engineers doing the same kind of task.
- A [Harness Brief](harness-brief.md) has agreed the procedure, and you are encoding it.

**Skip it when:**

- The rule must always apply. Put it in [`AGENTS.md`](agents-md-skeleton.md) or, if it must never be broken, a hook or check.
- The knowledge is a live capability — querying an API, reading a dashboard. That is a tool or an MCP server.
- It is a one-off. Prompt it directly.
- You cannot yet describe what "done" looks like. Do the task by hand a few more times first.

---

## Roles and ownership

| Role                                                 | Responsibility                                                                             |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Skill owner** (usually whoever owns the procedure) | Writes and maintains the skill; keeps scripts working; answers when it misfires            |
| **Harness owner**                                    | Curates the set; prevents overlapping triggers; checks discovery and effect before sharing |
| **Area owner**                                       | Confirms the procedure and stopping points are right for their domain                      |
| **Engineers**                                        | Propose skills from repeated work; report when a skill loads wrongly or not at all         |
| **Security reviewer**                                | Reviews bundled scripts and third-party skills like any other dependency                   |

---

## How to use it

### Step 1: Write the description first

The description is the only part the agent sees before deciding to load the skill. Write it in the words a task would actually use, say when it applies, and say what it does not cover. The specification allows up to 1,024 characters; a good description rarely needs a third of that.

**Tip:** collect five real task descriptions from your issue tracker where the skill should apply, and check that the words in them appear in your description.

### Step 2: Keep it to one procedure

If the description needs "and" three times, split the skill. Two small skills with sharp triggers beat one broad skill that competes with others.

### Step 3: Bundle scripts that produce facts

The most valuable skills include small scripts that measure something — lock behaviour for a migration, callers of a function, a bundle-size diff — and ask the agent to cite the output. Scripts measure; the agent judges. Keep scripts self-contained, give them clear error messages, and give them no network or secret access they do not need.

### Step 4: Encode where it stops

Write the conditions under which the agent must stop and ask, in terms it can recognise. Name who it should ask, if that is not obvious.

### Step 5: Define done as checks, not prose

The "Done when" list should be checkable by a person or a script. Always include a line requiring the summary to state what could not be verified.

### Step 6: Test discovery and effect before sharing

Run the testing checklist below. A skill that has not been tested is a hypothesis. Record the result, and the skill's introduction, with a [Harness Change Record](harness-change-record.md).

---

## The template

```text
<skill-name>/
├── SKILL.md
├── references/          # optional; loaded only when SKILL.md points to it
│   └── <topic>.md
└── scripts/
    └── <fact-finder>.sh # optional; produces evidence
```

The `name` must be lowercase letters, numbers and hyphens — no leading, trailing or doubled hyphens — at most 64 characters, and match the folder name.

```markdown
---
name: <skill-name>
description: Use when <trigger in task language>. Covers <scope>.
  Not for <what it does not cover>.
metadata:
  owner: <team or person>
  version: "<n>"
---

# <Skill title>

## When this applies
<One or two sentences. What situation triggers this skill.>

## Steps
1. Read `<doc>` for the conventions that apply.
2. Run `scripts/<fact-finder>.sh <args>` and keep its output.
3. <step>
4. <step>

## Evidence to include in the result
- Output of `scripts/<fact-finder>.sh`.
- <other evidence>

## Stop and ask if
- <condition> — ask <role>.
- <condition>

## Done when
- [ ] <checkable condition>
- [ ] The summary states what could not be verified.
```

### Testing checklist

```text
[ ] Loads on 5 tasks where it should, without being named
[ ] Does NOT load on 5 similar tasks where it should not
[ ] Outcome with skill vs without, 3+ runs each: better / same / worse
[ ] Scripts reviewed; no network or secrets access they do not need
[ ] Owner recorded; change record written
```

---

## Worked example

*Illustrative scenario: the team, incident and numbers below are hypothetical. They show what a finished skill and its test record can look like.*

This example comes from the same billing team as the others in this section. In August an agent-written migration added an index on a large `orders` table without building it concurrently, and writes to the table were blocked for about six minutes during business hours. The [postmortem](agent-incident-postmortem.md) found that the instruction file said "be careful with migrations" and nothing else. Lena, the billing area owner, wrote this skill with Kofi, who had written most of the team's migrations by hand.

```text
write-migration/
├── SKILL.md
├── references/
│   └── locking.md
└── scripts/
    └── check_locks.sh
```

```markdown
---
name: write-migration
description: Use when creating or changing a database migration, adding or
  dropping an index, adding a column, or backfilling data in Postgres. Covers
  locking, backfills and the review evidence for schema changes. Not for
  query tuning without schema changes or for seed data.
metadata:
  owner: billing-team
  version: "2"
---

# Writing a migration

## When this applies
Any change under `db/migrations/`, or any task that needs a schema change.

## Steps
1. Read `docs/database.md` for naming and rollback conventions.
2. Write the migration. Indexes on existing tables use
   `CREATE INDEX CONCURRENTLY` in a migration with transactions disabled.
3. Run `scripts/check_locks.sh <migration-file>` and keep its full output.
4. If a backfill is needed, write it as a separate batched job, not inside
   the migration. See `references/locking.md` for batch sizes.
5. Write the rollback and test it locally with `pnpm db:rollback`.

## Evidence to include in the result
- Full output of `scripts/check_locks.sh`.
- Estimated row count of each table touched (the script prints it).
- The rollback command and the result of running it locally.

## Stop and ask if
- The script reports a lock that blocks writes (SHARE or stronger) on a table
  over 1M rows — ask
  the billing area owner.
- The change drops or renames a column that any service still reads.
- You cannot run the script (missing database, connection error).

## Done when
- [ ] Migration and rollback both run cleanly locally.
- [ ] Lock check output is attached and shows no blocking lock on a large table,
      or the area owner has approved it.
- [ ] The summary states what could not be verified (e.g. production row
      counts are estimates).
```

The test record, written into the [Harness Change Record](harness-change-record.md) for the skill:

```text
Discovery:  loaded on 5/5 migration tasks without being named
            loaded on 0/5 near-misses (query tuning, seed data, ORM model changes)
            v1 description ("Database helper") loaded on 1/5 — rewritten
Effect:     8 migration tasks from the last two quarters, 3 runs each
            without skill: 11/24 runs used a blocking index build
            with skill:     0/24; 22/24 attached lock evidence
Cost:       ~2k more tokens per migration task; no change elsewhere
Scripts:    check_locks.sh reviewed by Daniel; read-only DB role, no network
```

Two details matter. The first version of the description, "Database helper", almost never loaded, because no real task uses those words; rewriting it in task language fixed discovery without touching anything else. And the effect test compared the skill with its absence on the team's own historical tasks, several runs each, rather than on one convincing demo. The numbers here are invented for the example; in your team, the comparison is what makes the skill worth its context.

---

## Signals it is working

- The right skill loads without anyone naming it, and the wrong ones do not.
- Skill runs produce script output and evidence, not just prose.
- The team has a dozen skills that are used rather than a hundred that are not.
- Each skill has an owner and a record of how it was tested.
- Failures the skill was written for stop recurring, and you can show it.
- The same skills work across the agents your team uses.

---

## Failure modes and mitigations

Skills tend to fail silently. Nobody notices a skill that never loads; the agent just improvises, as it did before, and the team assumes the skill is helping because it exists. The first sign is usually a repeat of the failure the skill was written to prevent.

| Failure mode                      | What it looks like                               | Mitigation                                             |
| --------------------------------- | ------------------------------------------------ | ------------------------------------------------------ |
| **Vague description**             | Loads only when named explicitly                 | Rewrite in task language; test discovery on real tasks |
| **Kitchen-sink skill**            | One skill covers five procedures                 | One procedure per skill; split on "and"                |
| **Skills as rules**               | "Always do X" inside a skill that may never load | Move always-on rules to instructions or hooks          |
| **Prose without evidence**        | The agent asserts the migration is safe          | Bundle a script; require its output in the result      |
| **Unreviewed third-party skills** | Downloaded skills with scripts nobody read       | Review, pin and own them like dependencies             |
| **Never evaluated**               | Nobody knows if it helps                         | With/without comparison, several runs, before sharing  |

---

## Further reading

- **[Agent Skills specification](https://agentskills.io/specification)** by Agent Skills (agentskills.io) — The open format: required fields, naming rules, optional `scripts/`, `references/` and `assets/` folders, and progressive disclosure.

---

## Related pages

- [Skills](../foundations/skills.md) — How skills load, when to write one, and why discovery is a design problem.
- [Guides & Sensors](../harness/guides-and-sensors.md) — Why scripts that measure beat prose that persuades.
- [Testing & Evolving the Harness](../harness/testing-and-evolving.md) — Running with/without comparisons and evals.
- [Agent Threat Model](../security/agent-threat-model.md) — Why skills with scripts deserve a security review.
