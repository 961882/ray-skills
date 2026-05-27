# Ray Skills

[![X](https://img.shields.io/badge/X-@someagi-000000?logo=x&logoColor=white)](https://x.com/someagi)

一组面向内容策划、多人角色讨论和角色库扩展的 Agent Skills。

本仓库包含 3 个 skill：

- `ray-content-topic-planning`：从长文、白皮书、访谈稿、产品资料中拆出可发布的内容选题计划。
- `ray-multi-party-mode`：让 AI 用多个专业角色进行圆桌讨论、辩论、方案评审和决策建议。
- `ray-role-creator`：为 `ray-multi-party-mode` 创建新的讨论角色，并生成可直接粘贴的 YAML 角色定义。

## 仓库结构

建议保持以下目录结构：

```text
ray-skills/
├── README.md
├── ray-content-topic-planning/
│   ├── SKILL.md
│   ├── workflow/
│   └── reference/
├── ray-multi-party-mode/
│   ├── SKILL.md
│   ├── workflow.md
│   └── references/
└── ray-role-creator/
    ├── SKILL.md
    ├── agents/
    ├── workflow.md
    └── references/
```

每个 skill 必须保留自己的完整目录。不要只复制 `SKILL.md`，因为这些 skill 依赖同目录下的 `workflow/`、`reference/` 或 `references/` 文件。

`ray-content-topic-planning/state/` 是运行时状态目录，会在执行时生成，不需要预先提交旧的 `config.json`。

## 安装

下面用 `961882/ray-skills` 作为 GitHub 仓库占位符。发布后请替换成你的真实仓库地址。

### 安装到 Codex

Codex 的 skill 默认安装到 `$CODEX_HOME/skills`，通常是 `~/.codex/skills`。

推荐方式是在 Codex 里使用 `skill-installer`：

```text
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-content-topic-planning
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-multi-party-mode
$skill-installer install https://github.com/961882/ray-skills/tree/main/ray-role-creator
```

也可以手动安装：

```bash
git clone https://github.com/961882/ray-skills.git
mkdir -p ~/.codex/skills
cp -R ray-skills/ray-content-topic-planning ~/.codex/skills/
cp -R ray-skills/ray-multi-party-mode ~/.codex/skills/
cp -R ray-skills/ray-role-creator ~/.codex/skills/
```

安装后重启 Codex，让新 skill 被重新扫描。

### 安装到 Claude Code

Claude Code 支持个人级和项目级 Skills。

个人级安装：

```bash
git clone https://github.com/961882/ray-skills.git
mkdir -p ~/.claude/skills
cp -R ray-skills/ray-content-topic-planning ~/.claude/skills/
cp -R ray-skills/ray-multi-party-mode ~/.claude/skills/
cp -R ray-skills/ray-role-creator ~/.claude/skills/
```

项目级安装：

```bash
git clone https://github.com/961882/ray-skills.git
mkdir -p .claude/skills
cp -R ray-skills/ray-content-topic-planning .claude/skills/
cp -R ray-skills/ray-multi-party-mode .claude/skills/
cp -R ray-skills/ray-role-creator .claude/skills/
```

安装后重启 Claude Code。你可以让 Claude Code 列出可用 skill：

```text
List all available Skills
```

### 在 Cursor 中使用

Cursor 官方主要使用 Rules 或 `AGENTS.md` 来提供可复用上下文。推荐把本仓库作为规则引用，而不是假设 Cursor 会原生扫描 `SKILL.md`。

项目级使用方式：

```bash
git clone https://github.com/961882/ray-skills.git skills/ray-skills
mkdir -p .cursor/rules
```

然后创建 `.cursor/rules/ray-skills.mdc`：

```mdc
---
description: Use Ray Skills for topic planning, multi-party discussion, and role creation.
alwaysApply: false
---

When the user asks for topic planning, content decomposition, 拆选题, 选题规划, 内容拆解, or topic planning:
- Read `skills/ray-skills/ray-content-topic-planning/SKILL.md`.
- Follow the workflow files referenced by that skill.

When the user asks for 多人讨论, 多角色讨论, 圆桌讨论, party mode, role debate, cross-functional workshop, or multi-agent discussion:
- Read `skills/ray-skills/ray-multi-party-mode/SKILL.md`.
- Follow `skills/ray-skills/ray-multi-party-mode/workflow.md`.

When the user asks to create roles, add personas, extend a role roster, generate YAML role definitions, or update multi-party-mode roles:
- Read `skills/ray-skills/ray-role-creator/SKILL.md`.
- Follow `skills/ray-skills/ray-role-creator/workflow.md`.
```

如果你只想用最简单的方式，也可以在项目根目录创建 `AGENTS.md`，把上面的规则内容放进去。

## 三个 Skill 分别做什么

### ray-content-topic-planning

用途：从一份源材料中拆出完整内容发布计划。

适合场景：

- 从白皮书、长文、课程稿、访谈稿里拆公众号选题。
- 从产品资料里拆内容营销计划。
- 已经有 3-5 篇历史文章，希望用个人风格校准选题评分。
- 想得到“候选选题、精选方向、标题、结构、评分、发布节奏”的完整交付物。

核心流程：

```text
参数收集
→ 生成 20 个候选选题
→ 筛选 5 个精选方向
→ 深度拆解标题和文案结构
→ 多角色评分
→ 输出发布计划
```

典型输出：

- `YYMMDD-选题｜标题｜结构.md`
- 如果提供历史文案：`YYMMDD-历史文案分析与重新评分.md`

### ray-multi-party-mode

用途：让 AI 模拟或编排多个专业角色，对一个问题进行结构化讨论。

适合场景：

- 想让 PM、架构师、设计师、QA、增长、运营等角色一起评审一个方案。
- 想做圆桌讨论、辩论、批判性评审、决策收敛。
- 想看到不同角色的共识、分歧、风险和下一步建议。
- 想用真实 subagent 模式让每个角色独立分析。

支持的讨论模式：

- `roundtable`：平衡圆桌讨论。
- `debate`：强调冲突、取舍和反方观点。
- `critique`：评审方案缺陷和风险。
- `planning`：拆执行步骤、顺序和责任。
- `decision`：收敛选项并推荐一个方向。

执行模式：

- `simulate`：默认模式，由一个 Agent 模拟多个角色，速度快。
- `subagent`：每个角色使用独立子 Agent，适合更高置信度的多视角比较。

### ray-role-creator

用途：为 `ray-multi-party-mode` 创建新的角色或角色包。

适合场景：

- 想给多人讨论系统增加“法务”“财务”“销售”“品牌”“招聘”等新角色。
- 想生成 ready-to-paste 的 YAML 角色定义。
- 想扩展 `roles.yaml` 中的 aliases、keywords、priority、best_for、voice、principles。
- 想设计角色之间的 support pairs、challenge pairs、recommended lineups。

默认目标：

```text
ray-multi-party-mode/references/roles.yaml
```

这个 skill 会先检查现有角色是否重叠，再创建新角色，避免生成只是换名字的空泛 persona。

## 如何调用

### 自然语言触发

安装后，最推荐的调用方式是直接说需求。Agent 会根据 `SKILL.md` 里的 description 自动判断是否使用。

示例：

```text
帮我从这份白皮书拆一轮公众号选题规划。
```

```text
让产品、架构、QA 三个角色一起评审这个方案。
```

```text
帮我给 multi-party-mode 增加 3 个适合教育产品讨论的角色。
```

### 显式点名 Skill

如果你的客户端支持显式点名 skill，可以直接写 skill 名称。

```text
[$ray-content-topic-planning]
源文案：/path/to/source.md
身份目标：/path/to/identity.md
历史文案：/path/to/history/
请输出到：/path/to/output/
```

```text
[$ray-multi-party-mode]
用 debate 模式，让 PM、架构师、QA、增长四个角色讨论：
我们要不要把这个功能从 MVP 移到第二期？
```

```text
[$ray-role-creator]
请为 ray-multi-party-mode 创建一个 3 人“企业销售”角色包，
包含销售负责人、解决方案顾问、客户成功。
输出可直接粘贴到 roles.yaml 的 YAML。
```

在不支持 `[$skill-name]` 语法的客户端中，可以改成：

```text
Use the ray-multi-party-mode skill.
...
```

或：

```text
请按 ray-content-topic-planning 这个 skill 的流程执行。
...
```

## 使用示例

### 示例 1：从白皮书拆内容选题

```text
请按 ray-content-topic-planning 执行完整选题规划。

源文案：/Users/me/docs/ai-whitepaper.md
身份目标：/Users/me/docs/my-positioning.md
历史文案：/Users/me/articles/history/
输出目录：/Users/me/output/topic-plan/

要求：
- 候选选题 20 个
- 精选方向 5 个
- 给出公众号标题备选
- 给出每篇文章结构
- 用历史文案校准评分
```

预期结果：

```text
/Users/me/output/topic-plan/260527-选题｜标题｜结构.md
/Users/me/output/topic-plan/260527-历史文案分析与重新评分.md
```

### 示例 2：多人角色讨论一个产品决策

```text
请按 ray-multi-party-mode，用 decision 模式讨论：

我们正在做一个 AI 写作工具，当前有两个方向：
A. 继续加强长文生成
B. 转向“选题规划 + 大纲 + 人类审稿”的工作流

请自动选择 3-5 个最有用的角色，
输出每个角色观点、核心分歧、风险和最终推荐。
```

也可以指定角色：

```text
请按 ray-multi-party-mode，
用 PM、增长、客户成功、技术架构四个角色，
评审这个上线方案是否应该本周发布。
```

### 示例 3：创建新角色包

```text
请按 ray-role-creator 创建一个“教育产品增长”角色包。

目标：
- 能用于 ray-multi-party-mode
- 角色数量：3 个
- 需要包含：获客、转化、续费视角
- 输出 ready-to-paste YAML
- 如果有明显的 recommended_lineups，也一起给出
```

预期输出包含：

```yaml
roles:
  - id: ...
    name: ...
    title: ...
    icon: ...
    aliases:
      - ...
    keywords:
      - ...
    priority: ...
    best_for:
      - ...
    voice: ...
    principles:
      - ...
```

### 示例 4：把新角色应用到已有 roster

```text
请按 ray-role-creator，把刚才生成的 3 个角色应用到：
ray-multi-party-mode/references/roles.yaml

同时检查是否需要补充 recommended_lineups 和 discussion_dynamics。
只在确实有帮助时修改。
```

## 常见问题

### 为什么不能只复制 SKILL.md？

因为这些 skill 不是单文件提示词。它们依赖同目录的 workflow、reference、template 或 roles 文件。只复制 `SKILL.md` 会导致 Agent 找不到执行流程或角色库。

### Codex 和 Claude Code 的安装目录有什么区别？

Codex 默认使用：

```text
~/.codex/skills/
```

Claude Code 默认使用：

```text
~/.claude/skills/
```

Claude Code 也支持项目级目录：

```text
.claude/skills/
```

### Cursor 能不能像 Claude Code 一样直接安装 Skills？

Cursor 官方主要通过 `.cursor/rules`、User Rules 或 `AGENTS.md` 管理可复用指令。为了兼容性，本 README 推荐用 Cursor Rule 引用这些 skill 文件，让 Cursor 在相关场景下读取并遵循它们。

### 三个 skill 可以一起用吗？

可以。常见组合是：

```text
ray-role-creator
→ 创建或扩展角色库
→ ray-multi-party-mode
→ 用新角色进行讨论
```

或：

```text
ray-content-topic-planning
→ 生成选题计划
→ ray-multi-party-mode
→ 多角色评审 Top 5 选题
```

## 致谢与来源说明

Ray Skills 的工作流组织、多角色讨论和角色编排设计受到 BMad Method 等开源 Agent 工作流项目的启发。

感谢 [翔宇工作流](https://xiangyugongzuoliu.com) 在 AI 工作流实践中翔宇 Skill 规范的启发。

本项目不是 BMad、BMad Method 或 BMad Code, LLC 的官方项目，也未获得其赞助、背书或授权。BMad 相关名称和商标归其各自权利人所有。

如果你在自己的项目中继续修改、分发或引用这些 skill，请同时保留本项目的 License，并尊重上游项目的版权和商标声明。

## 参考

- Codex Skills: https://github.com/openai/skills
- Claude Code Skills: https://docs.claude.com/en/docs/claude-code/skills
- Cursor Rules: https://docs.cursor.com/en/context
