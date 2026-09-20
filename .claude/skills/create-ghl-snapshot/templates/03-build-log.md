# Build log — <business>

Chronological. Append only. One line per action; a block for anything that failed, deviated, or was handed off.

## Pre-flight

| #   | Check                               | Result | Note |
| --- | ----------------------------------- | ------ | ---- |
| 1   | Chrome MCP responsive               |        |      |
| 2   | GHL tab logged in                   |        |      |
| 3   | Correct sub-account (<name> / <id>) |        |      |
| 4   | Matches baseline inventory          |        |      |
| 5   | Gate 2 approved; ledger complete    |        |      |
| 6   | UI map covers all areas             |        |      |

Passed at: <timestamp>

## Log

```
<timestamp> | <category> | <object> | created | read-back: match
<timestamp> | <category> | <object> | FAILED attempt 1 | <what happened> | screenshot: …
<timestamp> | <category> | <object> | retry 2 | <different approach> | created | read-back: match
<timestamp> | UI-MAP | <area> | path mismatch → re-discovered | new path: … | stamped <date>
<timestamp> | HANDOFF | <what> | posted → user: "done" | verified: …
<timestamp> | DEVIATION | D<n> | <title> | see DEVIATIONS.md
<timestamp> | SWEEP | <object> | hardcode sweep: clean | or: <literal found> → <variable> | fixed
<timestamp> | EXTENDED | <pre-existing object> | before: … | after: … | per Gate 2 approval
```

## Hardcode sweep summary

| Object | Swept after | Result |
| ------ | ----------- | ------ |
