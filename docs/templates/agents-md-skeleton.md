---
title: AGENTS.md Skeleton
description: A short, human-written starting point for a repository's AGENTS.md — commands, layout, conventions, boundaries, and definition of done — with a fully worked example.
---

# AGENTS.md Skeleton

The instruction file is the first piece of harness almost every team writes, and the one most likely to rot. It usually starts as generated output or a quick list of commands. Then every surprise adds a paragraph, nothing is ever removed, and six months later it is a document nobody has read end to end — including, in any meaningful sense, the agent. This skeleton is a deliberate counterweight: a short, human-written file that holds only what an agent would get wrong without being told.

The decision it protects is easy to miss: **what deserves to be in front of the agent on every single task**. Every line in an always-on file is paid for on every run, in tokens and in attention. A line that applies to one task in fifty should live somewhere else. A rule that must never be broken should not be prose at all.

!!! tip "The one-liner"
    Write it yourself, keep it to one screen, give every rule a reason, and grow it only from failures you have actually seen.

---

## What problem this solves

An agent starts every session knowing nothing about your repository. Without instructions, it rediscovers the basics each time: how to run a single test, where the API conventions are written down, why the obvious approach is wrong in this codebase. With too many instructions, the important lines are lost among the unimportant ones, and the cost of every session goes up.

The [Instructions & AGENTS.md](../foundations/instructions-and-agents-md.md) chapter covers the evidence. In short: context files, generated or human-written, do not generally improve task success and add over 20% to inference cost; human-written ones do clearly better than generated ones, and are most useful for non-standard practices the agent cannot infer. Agents also follow what they are told, even when it does not help. The skeleton turns that into a shape: orientation, exact commands, pointers to deeper docs, the few conventions that are not obvious, boundaries, when to stop and ask, and a definition of done.

It also gives a team one source of truth across tools. `AGENTS.md` is an open format, now stewarded by the Agentic AI Foundation under the Linux Foundation, and its site lists more than twenty agents that read it — Codex, Cursor, GitHub Copilot's coding agent, Gemini CLI, Junie and Zed among them. The ones that use their own file name can be pointed at it. One file, reviewed like code, beats four that drift apart.

---

## When to use this

**Use it when:**

- A repository has no instruction file yet, or only generated output that nobody has edited.
- An existing file has grown past a screen or two and nobody can say why half of its lines exist.
- You are adding a nested `AGENTS.md` for a directory with its own domain rules.
- A new model or a major codebase change makes you suspect the file is out of date.

**Skip it when:**

- The knowledge is a procedure for one kind of task. That is a [skill](skill-template.md).
- The rule must never be violated. Enforce it with a hook, linter, permission rule or CI check; at most, mention it in the file so the agent understands the failure it will see.
- The material is long reference documentation. Point to it; do not paste it in.
- You are tempted to write general programming advice the model already follows. Leave it out.

---

## Roles and ownership

| Role                           | Responsibility                                                                                  |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| **Harness owner or tech lead** | Owns the root file; reviews every change; prunes it at least quarterly and after model upgrades |
| **Area owners**                | Own nested files for their directories; make sure they refine, never contradict, the root       |
| **Every engineer**             | Proposes additions when they see a recurring failure, with the failure as evidence              |
| **Reviewers**                  | Treat changes to the file like code changes: ask what failure prompted each new line            |

---

## How to use it

### Step 1: Start from commands, not from philosophy

Fill in the commands section first, exactly as someone would type them, including how to run one test file. Commands are the single most useful thing in the file and the part most often wrong. Run each one before you commit the file.

### Step 2: Add only what an agent would get wrong

For each candidate line, ask: would a capable engineer who has never seen this repository get this wrong? If not, leave it out. Linter rules, formatting preferences and general good practice already have better enforcement.

### Step 3: Write the reason next to every rule

"Use `Money`, never floats, for amounts — floats caused rounding errors in invoices" is followed more reliably, and pruned more safely, than "Use `Money`". The reason is also what lets someone delete the line later when it no longer applies.

### Step 4: Point, do not copy

Architecture, ADRs and API conventions already exist somewhere. Link them. The file works best as a table of contents with a few rules attached.

### Step 5: Make stopping explicit

"Stop and ask before adding a migration" is actionable. "Be careful with migrations" is not. List the situations where the agent must stop, in terms it can recognise from the task.

### Step 6: Grow it from failures, prune it on a schedule

When the same mistake happens twice, consider a line — or better, a check. When a model upgrade arrives, re-run a few representative tasks with suspect lines removed. Record additions and removals with a [Harness Change Record](harness-change-record.md).

**Tip:** if the file passes about 150 lines, something in it belongs in a skill, a nested file, or a check.

---

## The template

```markdown
# AGENTS.md

<One paragraph: what this system is, who uses it, and the shape of the repo.>

## Commands
- Install: `<command>`
- Run locally: `<command>`
- Test (all): `<command>`
- Test (single file): `<command> path/to/file`
- Lint / typecheck: `<command>`
- Format: `<command>`

## Where things live
- `<path>/` — <what>
- `<path>/` — <what>
- Architecture overview: `docs/architecture.md`
- Decisions: `docs/adr/`
- API conventions: `docs/api.md`

## Conventions that are not obvious
- <Convention> — because <reason>.
- <Convention> — because <reason>.

## Boundaries
- Never <action> — because <reason>. (Enforced by: <hook / CI / permission>.)
- Do not modify `<path>` — <owner / reason>.

## Stop and ask when
- A database migration is needed.
- A change affects pricing, billing, permissions, or personal data.
- Requirements are ambiguous or contradictory.
- You would need to change a public API or event schema.

## Definition of done
- [ ] Tests, lint, and typecheck pass.
- [ ] New behaviour has tests derived from the acceptance criteria.
- [ ] The summary lists what changed, how it was verified, and what was NOT verified.
```

### For tools that use a different file name

Many agents read `AGENTS.md` directly. Claude Code, for example, reads it as project instructions from v2.1.277 onwards, but only when there is no `CLAUDE.md` — and no personal `CLAUDE.local.md` — in the working directory or above it; otherwise it reads the `CLAUDE.md` files, which can import `AGENTS.md` with an `@AGENTS.md` line. For tools that need configuring, keep `AGENTS.md` as the source and point the tool at it. The agents.md site gives two examples:

```text
# Aider: .aider.conf.yml
read: AGENTS.md

# Gemini CLI: .gemini/settings.json
{ "context": { "fileName": "AGENTS.md" } }
```

For older tool versions that only read their own file name, a symlink such as `CLAUDE.md -> AGENTS.md` does the same job.

See [Portability](../tools/portability.md) for the trade-offs.

---

## Worked example

*Illustrative scenario: the team, repository and rules below are hypothetical. They show what a finished file can look like; they do not describe a real codebase.*

Take a seven-engineer team maintaining a subscription-billing product in a TypeScript monorepo. Their first instruction file was 640 lines of generated text plus a year of incident paragraphs. In late August, after an agent-written migration locked writes to a large table (the [postmortem example](agent-incident-postmortem.md)), Marta, the harness owner, rewrote it from this skeleton in an afternoon. She kept only lines she could trace to a real failure or a real command, replaced the old "be careful with migrations" line with a pointer to the new `write-migration` skill, moved the "never commit secrets" paragraph into a pre-commit hook, and replaced the copied API guide with a link. Lena, who owns billing, wrote a nested file for `services/billing/`. The version below is the file as it stood in late October, after one further removal recorded in the [Harness Change Record](harness-change-record.md) example.

```markdown
# AGENTS.md

Subscription-billing product for B2B customers: a Next.js web app, a Node API,
and background workers, in one pnpm monorepo. Money moves through
`services/billing/`; everything else is product UI and integrations.

## Commands
- Install: `pnpm install`
- Run locally: `pnpm dev` (needs `docker compose up db` first)
- Test (all): `pnpm test`
- Test (single file): `pnpm test -- path/to/file.test.ts`
- Lint / typecheck: `pnpm lint && pnpm typecheck`
- Format: `pnpm format`

## Where things live
- `apps/web/` — customer-facing web app
- `apps/api/` — REST API (v2 only; v1 is frozen)
- `services/billing/` — invoicing, proration, tax. Read its AGENTS.md first.
- `workers/` — queues and scheduled jobs
- Architecture: `docs/architecture.md` · Decisions: `docs/adr/` · API: `docs/api.md`

## Conventions that are not obvious
- Amounts use `Money` from `@acme/money`, never numbers — floats caused
  rounding errors on invoices (ADR-014).
- New endpoints go in `apps/api/routes/v2/` — v1 is frozen for a partner integration.
- Feature flags are read via `flags.isOn()` only — direct env checks bypass
  the kill switch.
- Tests use the factories in `test/factories/`, not inline fixtures — fixtures
  drifted from the schema twice in 2026.

## Boundaries
- Never edit files under `generated/` — they are rebuilt from the OpenAPI spec.
  (Enforced by a pre-commit hook.)
- Never read or print `.env*` files. (Enforced by permission rules.)
- Do not change `infra/` — owned by the platform team; open an issue instead.

## Stop and ask when
- A database migration is needed (use the `write-migration` skill, then stop).
- A change touches pricing, proration, tax, refunds, or permissions.
- Acceptance criteria are missing or contradict the code.
- You would change a public API field or an event schema.

## Definition of done
- [ ] `pnpm lint && pnpm typecheck && pnpm test` pass.
- [ ] Each acceptance criterion has a test, named after the criterion.
- [ ] The summary lists what changed, how it was verified, and what was NOT verified.
```

The file is 46 lines. Every convention carries its reason, and two cite where the reason is recorded. Boundaries say how they are enforced, so the agent knows that a blocked commit is intended and not something to work around. The "stop and ask" list hands migrations to a skill rather than describing the procedure here. Nothing in it tells the agent to write clean code, follow SOLID principles, or be careful — the model does not need telling, and those lines would cost tokens on every task for no benefit.

The interesting part is what the rewrite removed. A paragraph about a 2025 incident with a deprecated payment SDK described a library the repository no longer used. A long section on React patterns restated rules the linter already enforced. Nobody missed either.

---

## Signals it is working

- The file fits on one or two screens, and every line has a reason you can find.
- New engineers read it in their first week and find it useful, which is a good sign the agent does too.
- The same mistake stops recurring after a line or a check is added for it.
- The change history includes deletions, especially after model upgrades.
- Rules that must never be broken are enforced elsewhere; the file only explains them.
- Tool-specific instruction files point at `AGENTS.md` instead of duplicating it.

---

## Failure modes and mitigations

The usual story is accretion. Every incident review ends with "let's add it to the instructions", because it is the cheapest action available. Nobody owns removal, so nothing is removed. The file grows, the agent's attention spreads thinner across it, and the important lines stop working — which produces more incidents, which produce more paragraphs.

| Failure mode                   | What it looks like                              | Mitigation                                                  |
| ------------------------------ | ----------------------------------------------- | ----------------------------------------------------------- |
| **Generated on day one**       | `/init` output committed and never edited       | Write it yourself, starting from commands and boundaries    |
| **Append-only file**           | Every incident adds a paragraph; nothing leaves | Quarterly pruning; re-test after model upgrades             |
| **Style guide dump**           | Linter rules restated in prose                  | Let the linter enforce them; tell the agent to run it       |
| **Prose security**             | "Never commit secrets" is the only control      | Hooks, permission rules and CI; the file only explains      |
| **Contradicting nested files** | Root and directory files disagree               | Nested files refine, never contradict; review them together |
| **Rules without reasons**      | Nobody dares remove a line                      | A reason, and ideally a link, on every rule                 |

---

## Further reading

- **[Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988)** by Gloaguen et al., ETH Zurich — Controlled study finding that context files do not generally improve task success and add over 20% to inference cost; developer-written files beat generated ones and help most with non-standard practices. (Preprint, revised September 2026.)
- **[How Claude remembers your project](https://code.claude.com/docs/en/memory)** by Anthropic — Documents how Claude Code reads `AGENTS.md` and `CLAUDE.md`, and when each loads. (Vendor documentation.)
- **[AGENTS.md](https://agents.md)** by the Agentic AI Foundation (Linux Foundation) — The open format, the list of agents that read it, and configuration notes for those that need pointing at it.

---

## Related pages

- [Instructions & AGENTS.md](../foundations/instructions-and-agents-md.md) — What belongs in always-on instructions, and the evidence behind keeping them short.
- [Hooks & Deterministic Guards](../foundations/hooks-and-guards.md) — Where rules that must never be broken belong.
- [Portability](../tools/portability.md) — Keeping one source of truth across several agents.
- [Skill Template](skill-template.md) — For procedures that do not belong in the always-on file.
