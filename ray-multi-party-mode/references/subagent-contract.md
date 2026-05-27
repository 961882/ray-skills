# Subagent Contract

Use this contract only when `execution_mode` is `subagent`.

## Purpose

Each active role gets its own subagent so the discussion is based on independently generated viewpoints instead of one assistant simulating every voice locally.

## Role Packet

Pass each subagent only the minimum context it needs:

- role `id`
- role `name`
- role `title`
- role `voice`
- role `principles`
- role `best_for`
- current user question
- session mode
- inferred scenario
- active role lineup

Do not pass the desired final answer, prior synthesis, or hidden conclusions.

## Worker Prompt Shape

Tell each subagent:

1. Stay fully inside the assigned role.
2. Analyze the user's problem from that role's priorities.
3. Keep the response concise and decision-oriented.
4. Do not imitate other roles.
5. Return the exact sections below.

## Required Output Format

Every role subagent should return markdown with these headings:

```md
## Role
[Name] ([Title])

## Bottom Line
[One short paragraph with the role's main stance.]

## What Matters Most
- ...
- ...

## Risks Or Objections
- ...
- ...

## Recommendation
- ...

## Question For User
- Optional. Only include if the role is truly blocked.
```

## Aggregation Rules

The facilitator should:

1. Launch all role subagents before waiting on results.
2. Wait after all launches, not one-by-one.
3. Preserve real disagreements instead of flattening them.
4. Build cross-talk from differences between returned outputs.
5. Mark any failed or timed-out role explicitly.

## Failure Handling

If subagent mode was requested but one or more role subagents fail:

- Continue with the successful role outputs.
- If a missing role is critical, the facilitator may simulate only that missing role as a fallback.
- Clearly disclose any fallback in the final synthesis.
