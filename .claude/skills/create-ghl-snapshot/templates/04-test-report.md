# Test report — <business>

Labels: **PASS** (executed, matched) · **FAIL→FIXED** (before/after shown, re-test passed) · **FAIL→OPEN** (needs decision) · **INSPECTED** (cannot be executed from a template sub-account; what was inspected is stated — never called "passed").

## Summary

| Persona   | Scenarios | PASS | FAIL→FIXED | FAIL→OPEN | INSPECTED |
| --------- | --------- | ---- | ---------- | --------- | --------- |
| **Total** |           |      |            |           |           |

Sync edges verified: <n> of <n> (`→` …, `⊗` …, `∥` …) · Pre-existing workflows checked (loaded accounts): …

## Test contacts (all in LEDGER.md as test-contact)

| Contact | Persona | Scenarios |
| ------- | ------- | --------- |

---

## Persona: <type>

### S1-1: Happy path — <title>

**Entry:** … · **Test contact:** …

**Steps performed:**

1. …

**Expected (observable):**

- …

**Actual:**

- …

**Observed via:** …
**Result:** …
**Fix:** before → after · re-test: …
**Screenshots:** …

### S1-2: <edge case>

…

## Sync / order scenarios

### SYNC-1: <A> → <B>

How the race was tempted: …
Expected: B did not act before A completed; mechanism <…> fired
Actual: …
Result: …

## Verified by inspection, not execution

| Path                      | Why it can't be executed here | What was inspected                                                    |
| ------------------------- | ----------------------------- | --------------------------------------------------------------------- |
| Real inbound missed call  | needs a live number + A2P     | trigger config, filters, text-back step, timing, owner alert step     |
| Real SMS delivery         | A2P not approved in template  | message bodies rendered with variables resolved; queued state in logs |
| Real email deliverability | no sending domain in template | template rendering; sender settings                                   |

## Open items

- …
