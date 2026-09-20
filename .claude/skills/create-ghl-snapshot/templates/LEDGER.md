# LEDGER — <business>

Every object in the sub-account that this build touches or could touch. **Pre-existing rows are entered first** (Phase 2 step 2), before any `planned` row. Phase 6 only ever deletes rows with origin `mine`.

**Origin:** `mine` | `pre-existing`
**Status (mine):** `planned` → `created` → `used` | `unused`
**Status (pre-existing):** `pre-existing` → `reused` | `extended` | `untouched`
**Flags:** `owner-must-set` (identity custom values) · `draft-at-ship` (workflows) · `test-contact` (Phase 4 data)

| #   | Type | Name | Origin | Phase | Status | Why / what it does | Used by | Flags | Cleanup decision |
| --- | ---- | ---- | ------ | ----- | ------ | ------------------ | ------- | ----- | ---------------- |
|     |      |      |        |       |        |                    |         |       |                  |

Types: custom-value · custom-field · tag · pipeline · stage · calendar · form · survey · funnel · funnel-step · site · site-page · template-email · template-sms · workflow · trigger-link · dashboard-widget · snippet · test-contact · test-opportunity · test-appointment · other

## Sanity checks (run before Gate 2 and before Gate 3)

- [ ] Every `planned` row has a non-empty `Used by`.
- [ ] Every identity custom value carries `owner-must-set`.
- [ ] On loaded accounts: every pre-existing object from `00-baseline-inventory.md` has a row.
- [ ] No two rows share a name within the same type.
