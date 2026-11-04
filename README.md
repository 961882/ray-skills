> 中文说明见 [README.zh.md](README.zh.md).

# Ray Skills

[![X](https://img.shields.io/badge/X-@globagi-000000?logo=x&logoColor=white)](https://x.com/globagi)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**Production-ready Agent Skills for Claude Code, Codex and Cursor: a multi-agent roundtable engine, a 6-step content-planning pipeline, and a YAML role generator to extend them.**

---

## What's inside

| Skill | What it does |
|---|---|
| [`ray-multi-party-mode`](ray-multi-party-mode/) | Runs structured discussions between multiple expert roles: roundtables, debates, design reviews and decisions. |
| [`ray-content-topic-planning`](ray-content-topic-planning/) | Turns one long source (whitepaper, article, interview, product docs) into a complete, scored publishing plan. |
| [`ray-role-creator`](ray-role-creator/) | Creates new roles and role packs for `ray-multi-party-mode` as ready-to-paste YAML. |

### `ray-multi-party-mode`: multi-agent roundtable engine

Put a PM, architect, QA, growth lead, security engineer and others in one room, and get their consensus, disagreements, risks and next steps.

- **24 built-in roles** in [`references/roles.yaml`](ray-multi-party-mode/references/roles.yaml). They cover product and engineering (analyst, architect, dev, PM, QA, scrum master, tech writer, UX), business (data, growth, security, operations, customer success) and content (director, strategist, planner, writer, reviewer, visual and graphic design, user psychology, sentiment, aesthetics).
- **5 discussion modes:** `roundtable`, `debate`, `critique`, `planning`, `decision`.
- **2 execution modes:**
  - `simulate` (the default): one agent plays every role, which is fast.
  - `subagent`: each role runs as an independent sub-agent under a defined [subagent contract](ray-multi-party-mode/references/subagent-contract.md), giving you more independent perspectives.
- Picks 3 roles by default and up to 5. You can name the roles yourself or let the skill choose them based on role keywords and priority.

### `ray-content-topic-planning`: content-planning pipeline

Go from one source document to a full publishing plan in a 6-step workflow:

```text
Collect inputs → 20 candidate topics → 5 selected angles → titles & article structure → multi-role scoring → publishing plan
```

- **Inputs:** `source_path` and `identity_path` (who you are, your audience, platform, goals and style) are required. `history_dir`, `output_dir`, `candidate_count` (default 20) and `select_count` (default 5) are optional.
- **Optional voice calibration:** give it 3–5 past articles and it extracts your "content DNA", then re-scores the topics against it.
- **Weighted scoring:** 5 reviewer roles each score 1–10 with a one-line reason, and the skill combines them into a weighted total.
- **Output:** Markdown deliverables such as `YYMMDD-选题｜标题｜结构.md`, plus a history analysis file when you provide past articles.

### `ray-role-creator`: YAML role generator

- Generates new roles or role packs (for example legal, finance, sales or education growth) that follow the schema in [`references/role-schema.md`](ray-role-creator/references/role-schema.md). The schema covers `id`, `name`, `title`, `icon`, `aliases`, `keywords`, `priority`, `best_for`, `voice` and `principles`.
- Checks the existing roster for overlap first, so you don't end up with renamed duplicate personas.
- Can also suggest support pairs, challenge pairs and recommended lineups ([integration patterns](ray-role-creator/references/integration-patterns.md)).
- Targets `ray-multi-party-mode/references/roles.yaml` by default.

---

## Quick start

> Copy each skill's **whole folder**, not just `SKILL.md`. Every skill depends on the `workflow/`, `reference/` or `references/` files next to it.

### Codex

```text
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-multi-party-mode
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-content-topic-planning
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-role-creator
```

Or install manually:

```bash
git clone https://github.com/961882/ray-skills.git
mkdir -p ~/.codex/skills
cp -R ray-skills/ray-* ~/.codex/skills/
```

Restart Codex afterwards.

### Claude Code

```bash
git clone https://github.com/961882/ray-skills.git
mkdir -p ~/.claude/skills          # or .claude/skills for a single project
cp -R ray-skills/ray-* ~/.claude/skills/
```

Restart Claude Code, then ask it to `List all available Skills`.

### Cursor

Cursor uses Rules and `AGENTS.md` rather than scanning `SKILL.md` natively, so reference the skills from a rule:

```bash
git clone https://github.com/961882/ray-skills.git skills/ray-skills
mkdir -p .cursor/rules
```

Create `.cursor/rules/ray-skills.mdc`:

```mdc
---
description: Use Ray Skills for topic planning, multi-party discussion, and role creation.
alwaysApply: false
---

When the user asks for topic planning or content decomposition:
- Read `skills/ray-skills/ray-content-topic-planning/SKILL.md` and follow its workflow files.

When the user asks for a multi-role discussion, roundtable, debate, or party mode:
- Read `skills/ray-skills/ray-multi-party-mode/SKILL.md` and follow its `workflow.md`.

When the user asks to create roles or extend the role roster:
- Read `skills/ray-skills/ray-role-creator/SKILL.md` and follow its `workflow.md`.
```

---

## How it works

Each skill is a self-contained folder:

```text
ray-skills/
├── ray-multi-party-mode/
│   ├── SKILL.md                 # trigger description + entry point
│   ├── workflow.md              # discussion orchestration
│   └── references/
│       ├── roles.yaml           # 24-role roster (single source of truth)
│       └── subagent-contract.md # rules for true multi-agent runs
├── ray-content-topic-planning/
│   ├── SKILL.md
│   ├── workflow/step01-init.md … step06-output.md
│   └── reference/               # scoring roles, topic categories, content-DNA rules, output template
└── ray-role-creator/
    ├── SKILL.md
    ├── workflow.md
    ├── agents/openai.yaml
    └── references/              # role schema, integration patterns
```

1. **Triggering:** the YAML front-matter `description` in each `SKILL.md` tells the agent when to load the skill, so plain requests in English or Chinese work.
2. **Progressive loading:** `SKILL.md` stays short and points to workflow and reference files, which the agent reads only when it needs them.
3. **Data-driven roles:** roles live in YAML, not in prompts. `ray-role-creator` writes to that roster and `ray-multi-party-mode` reads from it, so the system grows without code changes.

### Example prompts

```text
Use ray-multi-party-mode in debate mode: PM, architect, QA and growth —
should we move this feature from MVP to phase two?
```

```text
Use ray-content-topic-planning.
source_path: ./docs/ai-whitepaper.md
identity_path: ./docs/identity.md
history_dir: ./history/
```

```text
Use ray-role-creator to create a 3-role "enterprise sales" pack
(sales lead, solutions consultant, customer success) as ready-to-paste YAML.
```

In Codex you can also call a skill explicitly: `[$ray-multi-party-mode] ...`

---

## Compatible tools

| Tool | Support |
|---|---|
| **Claude Code** | Native Skills, personal (`~/.claude/skills`) or project level (`.claude/skills`) |
| **Codex** | Native Skills (`~/.codex/skills`), installable with `skill-installer` |
| **Cursor** | Through `.cursor/rules/*.mdc` or `AGENTS.md` |

---

## Author

**Ray (Tianyu Cui)** is a former Baidu AI Solution Architect who now builds AI agents and e-commerce AI automation under the **GlobAGI** brand.

- X: [@globagi](https://x.com/globagi)
- GitHub: [@961882](https://github.com/961882)

Issues and PRs are welcome, especially new role packs for `ray-multi-party-mode`.

## License

[MIT](LICENSE) © 2026 Ray
