# Persona testing — deriving scenarios and judging results

## From persona to scenarios

For each customer type in `01-customer-journeys.md`:

1. **Happy path** — the journey exactly as designed, entry to review request.
2. **Edge cases** (pick 2–4 from `12-missed-call-and-edge-cases.md` that matter most for this type):
   - Missed call → replies within minutes
   - Missed call → never replies
   - Submits form twice (duplicate)
   - Books, then cancels; books, then no-shows
   - Replies STOP mid-sequence
   - Goes cold after qualification / after quote
   - Arrives after hours
   - Replies to a message after the workflow has already moved on
3. **Sync scenarios** — one per `→` and `⊗` edge in the dependency graph, designed to _tempt_ the race: trigger both workflows as close together as the UI allows and check the order in the logs.

## Scenario record format

```
### S<persona>-<n>: <title>
Persona: <type>   Entry: <funnel form / calendar / manual add / stage move>
Steps performed:
1. …
Expected (observable):
- WF "<name>" fires; steps 1–4 run in order; tag `state:…` added; stage → "…"; SMS "<template>" queued to send
Actual:
- …
Observed via: execution log / contact timeline / conversation view / opportunity card
Result: PASS | FAIL→FIXED | FAIL→OPEN | INSPECTED
Fix (if any): before → after, and re-test result
Screenshots: …
```

## Result labels

- **PASS** — executed, observed outcome matched expected.
- **FAIL→FIXED** — didn't match; fixed; re-run matched. Before/after recorded.
- **FAIL→OPEN** — didn't match; not fixed (needs user decision or a deviation). Listed in open items.
- **INSPECTED** — could not be executed from a template sub-account (real inbound call, real A2P SMS delivery, real email deliverability). You inspected config and logs and state what you inspected. Never call this "passed".

## What to observe, and where

- Workflow execution log: which steps ran, in what order, with timestamps; which branch taken; waits entered/exited; removals.
- Contact timeline: tags added/removed, field changes, stage moves, appointments, messages (queued/sent/failed).
- Conversation view: message bodies as rendered — check variables resolved (no raw `{{…}}`, no placeholders in the wrong place).
- Opportunity card: stage, value, assigned user.
- Calendar: appointment created with the right calendar, duration, assignee.

## Test-contact hygiene

- Name `ZZ-TEST-<persona>-<n>`, tag `test-contact`, phone/email that cannot reach a real person.
- Ledger every test contact, opportunity, appointment, conversation as `mine` / `test-contact`.
- Never reuse a test contact across scenarios that depend on "first time" state (re-entry rules, duplicate detection); create a fresh one.
