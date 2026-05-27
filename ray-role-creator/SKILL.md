---
name: ray-role-creator
description: Create new discussion roles and persona bundles that are compatible with ray-multi-party-mode. Use when Codex needs to add roles, design new personas, extend a role roster, generate YAML role definitions, propose lineup additions, or normalize aliases, keywords, priorities, and discussion dynamics for a multi-role discussion skill.
---

# Ray Role Creator

Follow `./workflow.md`.

Default target:

- Treat `../ray-multi-party-mode/references/roles.yaml` as the primary roster unless the user names a different target file.

Load references only as needed:

- Read `./references/role-schema.md` when you need the exact field contract, naming rules, or output shape.
- Read `./references/integration-patterns.md` when the user wants the new roles wired into lineups, support pairs, challenge pairs, or turn-order behavior.

Keep the skill practical:

- Prefer roles the user can actually differentiate in conversation.
- Generate ready-to-paste YAML by default.
- If the user asks to apply the roles, update the target roster and only touch workflow files when the new domain truly needs new selection heuristics.
