# Role Schema

Use this contract when generating roles for `ray-multi-party-mode` compatibility.

## Required Role Fields

Each role entry should look like this:

```yaml
- id: finance-lead
  name: Fiona
  title: Finance Lead
  icon: "💼"
  aliases:
    - finance
    - finance lead
    - fp-and-a
    - budgeting
  keywords:
    - budget
    - margin
    - forecast
    - pricing
    - cash flow
    - spend
    - roi
    - finance
  priority: 4
  best_for:
    - budget planning
    - ROI trade-offs
    - pricing review
    - headcount and spend constraints
  voice: commercially sharp and constraint-aware; pushes the room to face cost, return, and downside exposure
  principles:
    - every strategy has a cost structure
    - preserve optionality when uncertainty is high
    - tie recommendations to economic consequences
```

## Field Rules

### `id`

- Use lowercase hyphen-case.
- Keep it short and durable.
- Prefer a domain plus responsibility, such as `finance-lead` or `legal-counsel`.

### `name`

- Use a short human name.
- Keep it stable across future edits.

### `title`

- Make it clear and user-facing.
- Avoid internal jargon unless the user explicitly uses it.

### `icon`

- Use a single emoji.
- Prefer icons that visually distinguish nearby roles.

### `aliases`

- Include 3-5 natural phrasings a user might actually type.
- Include the common short form only if people really use it.
- Do not mirror every keyword here.

### `keywords`

- Include 8-12 trigger terms.
- Mix topic words and job-to-be-done words.
- Bias toward words that help lineup selection.

### `priority`

- Match the target roster's existing scale.
- Higher priority means more likely to be auto-selected.

### `best_for`

- Keep 3-5 concrete discussion use cases.
- These should sound like work someone would genuinely hand to that role.

### `voice`

- One sentence only.
- Describe how the role sounds in a meeting, not its biography.

### `principles`

- Keep three principles.
- Make them operational and discussion-relevant.

## Distinctness Test

A new role should pass all of these:

1. It has a unique first instinct.
2. It has at least one clear tension with an existing role.
3. It would be selected for at least one topic where no current role is ideal.
4. A human can explain in one sentence why this role exists.

## Optional Companion Additions

Only add these when useful:

- `recommended_lineups`
- `discussion_dynamics.support_pairs`
- `discussion_dynamics.challenge_pairs`

For companion additions, reuse existing scenario labels when possible:

- `product`
- `architecture`
- `platform`
- `security`
- `compliance`
- `delivery`
- `release`
- `planning`
- `decision`
- `strategy`
- `growth`
- `metrics`
- `customer`
- `operations`
- `onboarding`
- `implementation`
- `fast-path`

## Bad Smells

Avoid roles like these:

- A renamed copy of `pm`
- A role that only says "we should be strategic"
- A role with generic `keywords` like `business`, `work`, or `team`
- A role whose principles would fit almost anyone in the roster
