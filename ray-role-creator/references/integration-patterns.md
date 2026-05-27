# Integration Patterns

Use this file when the user wants the new roles to behave like first-class citizens inside `ray-multi-party-mode`.

## Recommended Lineups

Add a lineup when:

- the new role naturally belongs in a repeatable trio
- the trio maps to a common user request
- the trio is more useful than a one-off combination

Keep lineup names short and descriptive:

- `finance-review-trio`
- `legal-risk-trio`
- `sales-forecast-trio`

## Support Pairs

Add a support pair when one role often sharpens or strengthens another role's contribution.

Preferred shape:

```yaml
- source: finance-lead
  target: pm
  intensity: medium
  scenarios:
    - decision
    - strategy
    - prioritization
  reason: product choices become more grounded when cost and return are made explicit
```

Guidance:

- Prefer `low` or `medium`.
- Use `high` only when the reinforcement should consistently shape the round.
- Keep `reason` concrete.

## Challenge Pairs

Add a challenge pair when the disagreement creates a healthy trade-off.

Preferred shape:

```yaml
- source: legal-counsel
  target: growth-strategist
  intensity: high
  scenarios:
    - launch
    - compliance
    - growth
  tension: acquisition speed and aggressive claims vs regulatory exposure and policy risk
```

Guidance:

- Prefer `medium` or `high`.
- The tension should be decision-relevant, not theatrical.
- Avoid challenge pairs that repeat an existing tension with new labels only.

## Turn Order Placement

Add the new role to `default_turn_order` only when it should frequently open or shape the room.

Placement heuristics:

- strategic business roles often sit near `pm` and `analyst`
- technical governance roles sit near `architect`, `security-engineer`, or `qa`
- synthesis roles stay near the end

## Workflow Heuristic Updates

Only update `ray-multi-party-mode/workflow.md` when:

- the new domain is common enough to deserve automatic lineup selection
- the domain cannot be inferred from existing heuristics

If you update heuristics:

1. Add one concise row to the topic mapping table.
2. Add one short role rule if the role deserves special handling.
3. Do not overfit the workflow to a single custom team.
