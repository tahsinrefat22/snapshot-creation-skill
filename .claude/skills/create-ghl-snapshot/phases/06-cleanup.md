# Phase 6 — Cleanup and finalize

**Principles in force:** 2 (snapshot procedure verified live), 5 (only `mine` rows are ever deleted), 7, 10.

**Goal:** no residue, snapshot taken, `06-cleanup-report.md` written, `STATE.md` → `COMPLETE`.

**Files you need:** `LEDGER.md`, `00-baseline-inventory.md`, `reference/browser-rules.md`, `reference/ghl-ui-map.md`, `templates/06-cleanup-report.md`.

---

## Step 1 — Ledger walk

For every row with origin `mine`:

- status `used` → keep. Record "kept — used by <…>".
- status `created` and never marked `used` → open it in the UI and confirm nothing references it (search workflows, pages, templates for its name). If truly unreferenced → delete → record "removed — unused". If something references it → mark `used`, fix the ledger, keep.
- ambiguous → `🖐 NEEDS YOU` with the row and what you found; wait.

Rows with origin `pre-existing` are **never** cleanup candidates, even when `untouched`. If the user wants one gone, that's their explicit instruction, logged as such.

## Step 2 — Test data

Delete every `test-contact` row from the ledger: contacts, and their opportunities, appointments, conversations, and any notes/tasks they spawned. Confirm in the UI that no `ZZ-TEST-` contact remains and that the pipelines show no test opportunities. Record counts.

## Step 3 — Ship state

Confirm each workflow's published/draft state matches the plan (`10-workflows.md` "at ship time"). Confirm custom values flagged `owner-must-set` hold placeholder text that is obviously a placeholder (e.g., `[SET YOUR PHONE NUMBER]`) rather than test values. Confirm no test message drafts are left in conversations.

## Step 4 — Take the snapshot

The procedure is verified live: navigate at agency level (this is the one permitted agency-level action), record the exact path in `ghl-ui-map.md` under `snapshots`, and note what the UI says the snapshot includes/excludes (fetch the official help doc if the UI doesn't say; cite it). Name the snapshot `<Business> — <vertical> — v1 — <YYYY-MM-DD>` unless the user gave a naming convention. Read it back in the snapshot list. If any step needs an agency permission you don't have, `🖐 NEEDS YOU`.

## Step 5 — Report and close

`06-cleanup-report.md`: kept / removed / handoffs / test data removed / ship-state checks / snapshot name and what it includes / anything the user should know before loading it into a client sub-account (e.g., the owner-must-set list count, workflows that ship as draft, A2P dependency).

`STATE.md` → `Status: COMPLETE`, `Snapshot: <name>`.

Post the final summary (no gate — this is the end):

```
## Done — <Business> snapshot
Snapshot: <name>
Working dir: <path>
Built: <n> workflows, <n> funnels, <n> forms/surveys, <n> calendars, <n> templates, <n> custom values (<n> owner-must-set)
Removed in cleanup: <n> unused objects, <n> test contacts
Ship as draft: <workflow names>
Onboarding guide: 05-onboarding-guide.md (<n> sections)
Open items: <none / list>
```
