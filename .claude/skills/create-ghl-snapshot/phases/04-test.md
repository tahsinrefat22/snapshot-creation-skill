# Phase 4 — Test

**Principles in force:** 1, 2, 5 (test contacts go in the ledger), 7, 10. Plus the honesty rule below.

**Goal:** evidence that the sub-account behaves correctly for each customer persona, in `04-test-report.md`, with fixes shown before/after.

**Files you need:** `02-plan/01-customer-journeys.md`, `02-plan/10-workflows.md` (dependency graph), `02-plan/12-missed-call-and-edge-cases.md`, `LEDGER.md`, `reference/persona-testing.md`, `reference/browser-rules.md`, `templates/04-test-report.md`. On loaded accounts also `00-baseline-inventory.md` (pre-existing workflows are under test too).

**Honesty rule:** a real inbound phone call, real A2P-delivered SMS, and real email deliverability cannot be executed from a template sub-account. Those paths are **"verified by inspection, not execution"** — you say exactly what you inspected (trigger config, filters, step order, message body, execution-log dry-run if GHL offers one) and you never label them "passed".

---

## Step 1 — Scenarios (per `reference/persona-testing.md`)

For each customer type: one happy path + 2–4 edge cases drawn from `12-missed-call-and-edge-cases.md` (missed call then reply / no reply; books then cancels; duplicate submission; STOP; goes cold at stage X). Plus one scenario per `→` and `⊗` edge in the dependency graph. Write the scenario list into the report first, each with: steps to perform, expected observable outcomes (which workflow fires, which steps run in what order, which tags/fields/stages change, which messages would send, which notifications fire), and how you'll observe it (workflow execution log, contact timeline, conversation view, opportunity card).

## Step 2 — Test contacts

Create `ZZ-TEST-<persona>-<n>` contacts, tag `test-contact`, using a phone/email pattern that can't reach a real person. Every one goes in `LEDGER.md` as `mine`, type `test-contact`, so Phase 6 removes them. Also ledger any test opportunities, appointments, or conversations you create.

## Step 3 — Execute

Use the most realistic entry point available: submit the live funnel form; book via the live calendar; add the contact to a workflow manually; move a pipeline stage; use GHL's inbound-message simulation if the UI offers one. After each scenario read the execution logs / timeline and record **actual** next to **expected**, step by step. Screenshot anything that fails.

## Step 4 — Sync / order tests

For every `A → B` edge: run the scenario and confirm from execution logs that B did not act before A completed and the enforcing mechanism fired (completion tag set, B's filter respected, remove-from-workflow ran). For every `A ⊗ B`: confirm entering one removed the contact from the other. For every `A ∥ B`: confirm both ran and neither interfered. On loaded accounts: confirm pre-existing workflows did not fire unexpectedly on the test contact, or fired in the order the plan expected.

## Step 5 — Fix

For each failure: diagnose → fix in the UI (following the build rules: read back after the change, update the ledger/plan if the construction changed, log it) → re-run the scenario → record before/after in the report. If a fix needs a deviation, climb the escalation ladder and record it in `DEVIATIONS.md`. If a fix would change customer-facing behaviour, `🖐 NEEDS YOU` first.

## Step 6 — Report

`04-test-report.md` structure: per persona → per scenario → steps, expected, actual, result (`PASS` / `FAIL→FIXED` / `FAIL→OPEN` / `INSPECTED`), fix before/after, re-test result. End with: summary table; the "verified by inspection" list with what was inspected; open items.

## Step 7 — Gate 4

Post the gate block. Files: `04-test-report.md`, `DEVIATIONS.md` if changed. Summary: scenarios run, pass/fixed/open/inspected counts, sync edges verified. Needs confirmation: any open item, any customer-facing fix. Say: **"I will not start Phase 5 until you tell me to."** `STATE.md` → awaiting-user (Gate 4). Stop.

On approval: `STATE.md` → `Gates approved: 1–4`, `Status: Phase 5`, open `phases/05-onboarding-doc.md`.
