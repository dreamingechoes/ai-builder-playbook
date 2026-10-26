# AI Builder Playbook

How to build software with AI as a team: harnesses that carry your working agreements, workflows that move from intent to measured outcome, and software factories that grow without losing judgment.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Content: CC BY 4.0](https://img.shields.io/badge/Content-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Built with MkDocs](https://img.shields.io/badge/Built%20with-MkDocs%20Material-526CFE.svg)](https://squidfunk.github.io/mkdocs-material/)

**Read it at [dreamingecho.es/playbooks/ai-builder](https://dreamingecho.es/playbooks/ai-builder/).**

## What this is

Producing a first version of code has become cheap. Deciding what is worth building, checking that it is right, and owning it when it breaks have not. This playbook is about designing a way of working where agents carry the volume and people keep — and get more room for — the judgment.

> **The model is a component. The process is the craft.**

**What it covers:**

- **Principles** — what I believe about building with AI, and an honest look at what the evidence shows
- **Foundations** — how coding agents work: context, instructions, skills, tools and MCP, subagents, hooks
- **AI Harness** — designing the system around the model so it works for a whole team
- **Workflow** — from intent to measured outcome: research, planning, building, verification, review, rollout, learning
- **Software Factories** — moving from individual agent workshops to a shared delivery system, one workflow at a time
- **Team** — measuring impact honestly, adoption without mandates, roles, growing judgment, sustainable intensity, cost
- **Security** — threat models, sandboxing, data policy, accountability
- **Frontier** — what pioneers are trying, and where this is heading
- **Tools** — choosing tools without chasing every release, portability, and my own stack
- **Templates** — copy-ready briefs, matrices, canvases, and records

**Who it's for:**

- Engineers moving from using agents to designing how they work
- Tech leads building a harness for their team
- Engineering managers and leaders deciding where to invest
- Anyone who wants a grounded introduction to agentic engineering in 2026

**How every page is built:**

1. What problem does this solve?
2. When should I use it — and when not?
3. Who owns it?
4. What does good look like?
5. What usually goes wrong?
6. What can I copy and use today?

Pages that lean on research say plainly how strong the evidence is. Pages with perishable content carry a "last reviewed" date.

## Quick start

### Prerequisites

- Python 3.10+ (for MkDocs)
- Node.js 18+ (for linting and formatting)
- Cairo (for social cards): `brew install cairo` on macOS, `libcairo2-dev` on Debian/Ubuntu

### Setup

```bash
git clone https://github.com/dreamingechoes/ai-builder-playbook.git
cd ai-builder-playbook

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

npm install
```

### Run locally

```bash
source .venv/bin/activate
mkdocs serve
# Open http://127.0.0.1:8000
```

### Build

```bash
mkdocs build --strict
```

## Project structure

```text
.
├── docs/
│   ├── index.md          # Home page
│   ├── principles/       # Core principles, state of the evidence, glossary
│   ├── foundations/      # Agent loop, context, instructions, skills, tools, subagents, hooks
│   ├── harness/          # Designing, gating, testing, and owning a team harness
│   ├── workflow/         # Intent, research, building, verification, review, rollout
│   ├── factories/        # Software factories: spectrum, bottlenecks, tiers, pilots, field reports
│   ├── team/             # Measurement, adoption, roles, judgment, intensity, cost
│   ├── security/         # Threat model, sandboxing, data policy, accountability
│   ├── frontier/         # Beyond code, migrations, personal systems, experiments, what's next
│   ├── tools/            # Landscape, evaluation, portability, my stack
│   └── templates/        # Copy-ready templates
├── src/overrides/        # Theme customisations
├── mkdocs.yml            # MkDocs configuration
├── requirements.txt      # Python dependencies
└── package.json          # Node.js dependencies (linting)
```

## Available commands

```bash
mkdocs serve            # Development server
mkdocs build --strict   # Production build (fails on warnings)

npm run lint            # All linters
npm run lint:md         # Markdown lint
npm run lint:md:fix     # Fix Markdown issues
npm run lint:format     # Check formatting
npm run format          # Format files
```

## Templates

- [Harness Brief](./docs/templates/harness-brief.md)
- [AGENTS.md Skeleton](./docs/templates/agents-md-skeleton.md)
- [Skill Template](./docs/templates/skill-template.md)
- [Human Gate Definition](./docs/templates/human-gate-definition.md)
- [Risk Tier Matrix](./docs/templates/risk-tier-matrix.md)
- [Factory Pilot Canvas](./docs/templates/factory-pilot-canvas.md)
- [Impact Review](./docs/templates/impact-review.md)
- [Agent Incident Postmortem](./docs/templates/agent-incident-postmortem.md)
- [Tool Scorecard](./docs/templates/tool-scorecard.md)
- [Harness Change Record](./docs/templates/harness-change-record.md)

## Contributing

Corrections and suggestions are welcome — this field moves fast and parts of the playbook will age quickly.

1. Fork the repository
2. Create a branch (`git checkout -b fix/outdated-claim`)
3. Run `npm run lint` and `mkdocs build --strict`
4. Open a pull request, with sources for any factual change

## License

- **Code and templates:** [MIT License](LICENSE)
- **Content:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — attribute "Iván González Sáiz" with a link to this repository

## About

Created by **Iván González Sáiz** — engineer and engineering leader building product with AI in remote-first SaaS teams.

- Website: [dreamingecho.es](https://dreamingecho.es)
- Sibling playbook: [Engineering Leadership Playbook](https://dreamingecho.es/playbooks/engineering-leadership/)
- LinkedIn: [ivan-gonzalez-saiz](https://linkedin.com/in/ivan-gonzalez-saiz)
- Bluesky: [@dreamingechoes.bsky.social](https://bsky.app/profile/dreamingechoes.bsky.social)
