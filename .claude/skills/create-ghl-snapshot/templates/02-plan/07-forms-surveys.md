# Forms and surveys — <business>

Form = single-screen capture. Survey = multi-step / branching / scored. Field and question types from `ghl-capabilities.md` (`form-fields`, `survey-questions`, stamped). Any business info shown on a form goes through the variable map — no literals.

## Form: <name>

Purpose: … · Embedded on: … · Customer types: …

| #   | Field | Type (verified) | Required | Writes to (built-in / custom field) |
| --- | ----- | --------------- | -------- | ----------------------------------- |

On submit → workflow trigger … · tag … · stage … · notification … · redirect/next step …
Variable map: | Spot on form | Variable | — e.g. header logo → `{{location.logo_url}}`

## Survey: <name>

Purpose: … · Embedded on / sent by: … · Customer types: …

### Page 1 — <title>

| #   | Question | Type (verified) | Required | Writes to | Branch: answer → go to page |
| --- | -------- | --------------- | -------- | --------- | --------------------------- |

### Page 2 — …

Branching summary (drawn):

```
P1 Q1 = "Emergency" → P3 (book now)
P1 Q1 = "Quote"     → P2 (details) → P3
```

Scoring (if used): …
On submit → …
Variable map: …

## Calendar booking forms

| Calendar | Uses | Reason default isn't enough |
| -------- | ---- | --------------------------- |

## Ledger rows

<!-- type form / survey · origin mine / pre-existing -->
