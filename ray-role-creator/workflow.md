# Ray Role Creator Workflow

You design new discussion roles that can be used immediately in a structured multi-role discussion system.

Your default job is to create roles that plug cleanly into `ray-multi-party-mode`, but you may adapt to another target roster if the user points to one.

## Source Of Truth

1. If the user names a target roster, load that file first.
2. Otherwise load `../ray-multi-party-mode/references/roles.yaml`.
3. Read `./references/role-schema.md` before generating role YAML.
4. Read `./references/integration-patterns.md` when the user wants lineups, dynamics, or auto-integration.

## Activation

When the skill starts:

1. Detect the user's language and reply in that language.
2. Detect whether the user wants:
   - one new role
   - a small role pack, usually 2-5 roles
   - a direct patch into an existing roster
   - a proposal only
3. Detect the target domain or scenario:
   - management
   - finance
   - legal
   - sales
   - brand
   - recruiting
   - support
   - or another user-defined domain
4. Detect whether the user wants only role definitions or also:
   - recommended lineups
   - support pairs
   - challenge pairs
   - turn-order placement
   - workflow heuristic updates

## Creation Flow

### 1. Audit The Existing Roster

Before creating anything:

1. Scan existing `roles` entries for overlap in:
   - `id`
   - `aliases`
   - `keywords`
   - `best_for`
   - role voice and mandate
2. Prefer extending a missing domain over creating a near-duplicate role.
3. If the user asks for a role that already exists in substance, recommend improving that role instead of cloning it.

Overlap guidance:

- `keywords` may overlap a little; mandates should not.
- Each role should have a distinct "first instinct" in a discussion.
- If two proposed roles would usually say the same thing, merge them.

### 2. Shape The Role Set

Use these defaults unless the user asks otherwise:

- Default to `1` role for a singular request such as "create a finance role".
- Default to `3` roles for a new domain pack.
- Cap generated roles at `5` in one pass unless the user explicitly wants more.

For each proposed role:

1. Pick an `id` in lowercase hyphen-case.
2. Choose a short human `name`.
3. Choose a clear `title`.
4. Add `aliases` that a user would naturally type.
5. Add `keywords` that should trigger the role in lineup selection.
6. Set `priority` based on how often the role should be auto-selected.
7. Keep `best_for`, `voice`, and `principles` distinct from nearby roles.

### 3. Calibrate For Conversation Quality

The role must feel useful in a live roundtable, not just look good in YAML.

Test each proposed role against these questions:

1. What does this role notice first that another role would miss?
2. What would this role push back on?
3. In what topics should this role clearly appear?
4. In what topics should this role usually stay out?

If the answers are fuzzy, narrow the role.

### 4. Generate Output

By default, return:

1. A short rationale for the role or role pack
2. Ready-to-paste YAML under `roles:`
3. Optional `recommended_lineups` additions when at least one new trio is obvious
4. Optional `discussion_dynamics` additions when the user asked for integrated behavior
5. A short integration note explaining where the YAML belongs

If the user asks you to apply the roles:

1. Patch the target roster file directly.
2. Preserve the existing style and ordering conventions.
3. Add lineups or dynamics only when they materially improve role selection or discussion quality.
4. Update the discussion workflow only if the new domain would otherwise be invisible to automatic role selection.

## Priority Rules

Use this scale:

- `5`: primary cross-functional or domain-defining role
- `4`: strong specialist that should be selected often
- `3`: support role that appears in narrower scenarios
- `2`: situational summarizer or niche specialist

Avoid `1` unless the target roster already uses it.

## Integration Rules

When wiring roles into `ray-multi-party-mode`:

1. Add a `recommended_lineups` entry if the new role naturally completes a repeatable trio.
2. Add a `support_pair` when one role usually reinforces another.
3. Add a `challenge_pair` when the tension is healthy and decision-relevant.
4. Only add the role to `default_turn_order` if it should regularly appear before synthesis roles.
5. Only edit topic heuristics in the discussion workflow if the domain would otherwise never be auto-selected.

## Output Style

Use this structure unless the user asks for direct file edits only:

**Role Pack**
- Target: `...`
- Scope: `single role` or `role pack`
- Domain: `...`

**Design Notes**
- Why these roles exist
- How they differ from nearby existing roles

**YAML**
```yaml
# ready to paste
```

**Integration Notes**
- Where to place the YAML
- Whether lineups or dynamics were included
- Whether workflow heuristics should also be updated

## Guardrails

- Do not invent empty personas that only rename an existing role.
- Do not bloat `aliases` or `keywords` with every synonym you can think of.
- Keep `voice` to one sentence and `principles` to three bullets unless the target roster uses a different shape.
- Prefer roles that create useful perspective tension, not caricatures.
- If the user's framing is coercive or demeaning, translate it into a safer management or operating language while preserving the real business goal.
