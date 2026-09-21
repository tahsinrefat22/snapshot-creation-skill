# Phase 3 — Build

**Principles in force:** all twelve, especially 2 (verify in UI), 5 (ledger), 7 (state after every object), 8 (escalation ladder), 9 (pre-flight), 10 (handoffs), 11 (hardcode sweep).

**Goal:** everything the approved plan says exists in the sub-account, read back and verified.

**Files you need:** `STATE.md`, `LEDGER.md`, `00-baseline-inventory.md`, the `02-plan/` file for the category you're building right now (not all at once), `reference/browser-rules.md`, `reference/ghl-ui-map.md`, `reference/ghl-capabilities.md`, `reference/workflow-doctrine.md` (for workflows), `templates/03-build-log.md`. Output: objects in GHL, `03-build-log.md`, ledger `created`, `STATE.md`.

**On resume mid-build:** read `STATE.md` → `Next action`; spot-verify in the UI the last 1–2 ledger rows marked `created`; continue. Never rebuild something the ledger says is `created` and the UI confirms.

---

## Step 0 — Pre-flight (do not create anything until every line passes)

Load Chrome tools, `tabs_context_mcp` with `createIfEmpty: true`. Work only in your own MCP tab group; never touch the user's tabs (`reference/browser-rules.md` → _Your own window_). Check, in this order, and post the result as a checklist:

| #   | Check                                                                           | How                                                                                                                                                |
| --- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Chrome MCP connected and responsive                                             | tab context returns tabs                                                                                                                           |
| 2   | Your own window has GHL open and logged in                                      | tab is inside the MCP tab group (never a user tab); no login page, no expired-session banner, no 2FA prompt                                        |
| 3   | Correct sub-account selected                                                    | name/ID read from UI == `STATE.md` `Sub-account` (`confirmed: yes`); if it's wrong, reopen the target per browser-rules; if still wrong, ask       |
| 4   | Sub-account still matches `00-baseline-inventory.md`                            | blank → still blank; loaded → pre-existing objects still there, nothing new appeared. List anything unexpected and ask what it is; never delete it |
| 5   | Gate 2 approved in `STATE.md`; every plan object is in `LEDGER.md` as `planned` | file read                                                                                                                                          |
| 6   | `ghl-ui-map.md` has a path for every area this build touches                    | list missing ones and discover them now, before building                                                                                           |
| 7   | **Labs feature "Brand New Funnel AI & Website AI" is ON for this sub-account** (only if funnels/sites are in scope) | The per-step/page AI builder + AI website builder are gated behind this Agency **Labs** feature, activated **per sub-account** — OFF by default on a new sub-account. Confirm it's enabled for the target. **You cannot enable it yourself** (agency-level — browser-rules forbid it): post a `🖐 NEEDS YOU` with the enable path from `reference/ghl-ui-map.md` → `agency-labs-ai-builder` (Agency view → Settings → Labs → Sub-Accounts tab → search bar → Activate feature → find this sub-account → enable "Brand New Funnel AI & Website AI"). Without it, funnels must be hand-built from blank. |

Any failure → `🖐 NEEDS YOU` with the exact fix, wait, re-run the whole checklist. No partial pass.

`STATE.md` → pre-flight passed <timestamp>.

## Step 1 — Build order

Dependencies dictate the order. Build all of one category before the next:

1. Custom values
2. Custom fields
3. Tags
4. Pipelines and stages
5. Calendars (with their forms — build the custom booking form first if a calendar needs one)
6. Forms and surveys
7. Email / SMS templates
8. Funnels / sites (embed forms and calendars)

   **Sites & funnels — use GHL's AI builder first, then finish by hand:**
   - **Sites:** always start with GHL's **AI site builder** to generate the pages, then edit manually for anything the AI got wrong or couldn't do. Don't hand-build a site from a blank page when the AI can scaffold it.
   - **Funnels:** same approach — try the **AI funnel builder** first — but its capabilities are **more limited** than for sites, so expect to do more manual work (adding/reordering steps, embedding the exact form/calendar, fixing copy, wiring on-submit). Fall back to manual building whenever the AI can't produce what the plan specifies.
   - Either way, the plan (`02-plan/08-funnels-sites.md`) is the source of truth: after the AI generates, reconcile every page section, embed and variable against the plan, replace any literal business data with the merge fields / custom values, and read back before ledgering. Record the AI-builder entry points and any quirks in `reference/ghl-ui-map.md` the first time (map-first thereafter).
9. Workflows (reference everything above)
10. Dashboard

On loaded accounts, `extended` objects are modified in the same category pass as their type, only in the way Gate 2 approved, and the before-state is written to the build log first.

## Step 2 — Per object

For each ledger row with status `planned` in the current category:

1. Open the plan file entry for it. Navigate using `ghl-ui-map.md`. If the path doesn't match the UI → trust-rule failure: re-discover, update the map with today's date, note in log.
2. Create it exactly per spec. One object at a time.
3. **Read it back**: reload/open the object, compare every attribute to the spec. Mismatch → fix → read back again.
4. `LEDGER.md` row → `created`, add the GHL-visible name/ID if the UI shows one.
5. Append to `03-build-log.md`: timestamp, object, result, anything retried.
6. `STATE.md` → `Last completed action`, `Next action`.

**Custom values used inside a template/page/workflow** → when you use one, update its ledger row `used-by` and status `used`. Same for fields, tags, forms, calendars, templates as workflows and pages reference them.

### Funnels / sites — one funnel at a time, one step at a time

Per `08-funnels-sites.md` order: create funnel → add step → build the page section by section per spec → embed form/calendar → set on-submit / next-step behaviour → preview → read back against the spec (sections present, copy uses variables, embed present, submit target correct) → ledger row for the step → `created` → next step. A funnel is `created` only after clicking through the whole path in preview (step 1 → … → thank-you). Then the hardcode sweep (step 4) on that funnel before moving to the next.

### Workflows — last, one at a time (read `reference/workflow-doctrine.md` first)

Per `10-workflows.md`: create → set trigger(s) and filters → add steps in order, drawing branches and goto merges exactly as planned → set waits, removals, completion tag/field → set re-entry and other settings per plan → **open it again and read every step back** against the plan → save → publish or leave draft per plan → ledger `created` → build log. Before publishing anything, confirm every workflow it depends on (`→` edges) exists.

## Step 3 — When GHL can't do what the plan says

Escalation ladder, in order, all rungs logged in `DEVIATIONS.md`:

1. **Same outcome, different construction** — more workflows, more nodes, a different trigger/event/object. Minimalism is suspended here; correctness wins. Try each idea in the UI.
2. **Web research** — GHL help docs, community, tutorials. Verify anything found in the UI before adopting.
3. **Closest workable alternative** — least customer-facing change possible. Record exactly what differs. If it's customer-facing → `🖐 NEEDS YOU` with the options; wait.

Update the plan file and the ledger to reflect the final construction, so Phase 4 tests what was actually built.

## Step 4 — Hardcode sweep

After categories 6, 7, 8 and again after 9: open each built form, template, page, and workflow message body; read the rendered content; search for literal business data — name, phone, address, email, website, reviews/reviewer names, logo file names, hours text. Any hit → replace with the variable → read back → log it. Record "sweep clean" per object in the build log.

## Step 5 — Handoffs to expect (post them, don't discover them)

- Media uploads (logo, images): if automation can't upload, list the files needed, ask the user to upload, then read the URLs back from the media library.
- OAuth connections (Google, Facebook, Stripe): never done in the template — they belong to the onboarding guide. Do not attempt.
- A design choice the plan left open.
- Anything that costs money (phone numbers, premium/labs features).
- Any native browser dialog.

Each is one `🖐 NEEDS YOU` block: what, where (click path), what to say when done. After "done": verify in the UI, log, continue.

## Step 6 — Gate 3

Post the gate block. Files: `03-build-log.md`, `LEDGER.md`, `DEVIATIONS.md`. Summary: objects built per category; deviations count and the customer-facing ones; which workflows are published vs draft; handoffs that happened. Say: **"I will not start Phase 4 until you tell me to."** `STATE.md` → awaiting-user (Gate 3). Stop.

On approval: `STATE.md` → `Gates approved: 1, 2, 3`, `Status: Phase 4`, open `phases/04-test.md`.

---

## Quality bar

- Every `planned` row is `created` (or has a deviation entry explaining why not).
- Every `created` object was read back and matched its spec.
- Hardcode sweep logged clean for every page, form, template, and workflow message.
- Every `→` / `⊗` mechanism from the dependency graph physically exists in the built workflows.
- `03-build-log.md` lets someone reconstruct exactly what was done and in what order.
