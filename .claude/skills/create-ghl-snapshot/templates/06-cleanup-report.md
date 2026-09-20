# Cleanup report — <business>

## Ledger walk (origin `mine` only)

| Object | Type | Status before        | Decision | Reason                                               |
| ------ | ---- | -------------------- | -------- | ---------------------------------------------------- |
|        |      | used                 | kept     | used by …                                            |
|        |      | created (never used) | removed  | no references found in workflows / pages / templates |
|        |      | created (never used) | kept     | found referenced by … — ledger corrected             |

Pre-existing objects: never touched (<n> rows, <n> reused, <n> extended, <n> untouched).

## Test data removed

| Kind                   | Count | Verified in UI                  |
| ---------------------- | ----- | ------------------------------- |
| Contacts (`ZZ-TEST-*`) |       | no `ZZ-TEST` in contacts search |
| Opportunities          |       | pipelines show none             |
| Appointments           |       |                                 |
| Conversations          |       |                                 |

## Ship-state checks

| Check                                             | Result |
| ------------------------------------------------- | ------ |
| Workflows published/draft as planned              |        |
| `owner-must-set` values hold obvious placeholders |        |
| No test drafts left in conversations              |        |

## Snapshot

Name: … · Taken: <timestamp> · Path used: (recorded in ghl-ui-map.md → snapshots)
What the UI / docs say it includes: … · Excludes: … · Source: <URL>

## Before loading into a client sub-account

- Owner must set <n> custom values (guide section 2)
- <n> workflows ship as draft — turn on in guide section 12 order
- Texting depends on A2P approval (guide section 4)
- …

## Handoffs during cleanup

- …
