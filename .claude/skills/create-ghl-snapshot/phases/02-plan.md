# Phase 2 — Plan the snapshot

**Principles in force:** all twelve, especially 2 (no GHL facts from memory), 4 (defaults first), 5 (ledger — pre-existing rows first), 6 (workflow doctrine), 8 (escalation ladder), 11 (no hardcoded business data), 12 (blank or master — ask, ledger, plan on top).

**Goal:** a complete, reviewable design of the sub-account that runs the business with minimum owner touch — or, on a loaded sub-account that already covers the business, a plain statement that nothing (or little) extra is needed.

**Files you need:** `01-business-profile.md` (approved), `reference/browser-rules.md`, `reference/workflow-doctrine.md`, `reference/defaults-first.md`, `reference/ghl-capabilities.md`, `reference/ghl-ui-map.md`, `templates/00-baseline-inventory.md`, `templates/02-plan/*.md`. Outputs: `00-baseline-inventory.md`, `02-plan/00…12.md`, ledger rows.

Write every plan file **incrementally**. Update `STATE.md` after each file.

---

## Step 0 — Open the target sub-account and ask the blank-or-master question

The target sub-account was set at the start protocol (`STATE.md` → `Sub-account`). If it's missing (for example, an old build from before this rule), ask the _Target sub-account_ question from `SKILL.md` now, before anything else.

1. Load Chrome tools. Open **your own dedicated window** (`tabs_context_mcp` with `createIfEmpty: true`), then open and confirm the target per `reference/browser-rules.md` → _Opening the target sub-account_. Never use a tab the user has open.
2. Post this and stop:

```
🖐 NEEDS YOU
What: I've opened sub-account <name> (ID <id>) in my own Chrome window. Is it (a) blank/empty, or (b) loaded from an existing snapshot (e.g., your master snapshot)? If (b), which snapshot?
Where: n/a
Why I can't: only you know how this sub-account was set up.
When done, say: "blank" or "loaded from <snapshot name>"
```

3. Record it in `STATE.md`: `Sub-account: name: <name> · id: <id> · … · confirmed: yes · kind: blank` or `kind: loaded-from: <snapshot>`.

## Step 1 — Capability discovery (one-time; skip what is already stamped)

**Discovery is a one-time investment, not a per-build step.** Before opening any builder, read `reference/ghl-capabilities.md`, `reference/defaults-first.md` and `reference/ghl-ui-map.md`:

- A section with a `Verified: <date>` line is **trusted as-is**. Do not re-scrape it, and do not spot-check it. It is re-verified only when it fails in use during a build (fix it on the spot, re-stamp it, note it in the build log) or when the user runs `/create-ghl-snapshot reverify <area>`.
- Discover **only** sections still marked `Verified: —`, and only if this build's plan needs them. Record them with a date stamp so the next build skips them too.
- Tell the user in one line which sections were reused and which (if any) were discovered now.

The list below is what a *first-ever* discovery covers:

Follow `reference/browser-rules.md` → _Discovery scraping_. Fill or refresh `reference/ghl-capabilities.md`:

- Workflow triggers (every category, every item, note gated/greyed ones)
- Workflow actions (same)
- Workflow conditions / if-else operators / filters available on triggers
- Workflow settings visible (re-entry, timezone, sender, stop-on-response, etc.)
- Form field types; survey question types; conditional logic / scoring presence
- Calendar types on create; availability, team, form, and confirmation options
- Custom field types; custom value creation UI
- Pipeline/stage options
- Email/SMS template builder presence and merge-field picker (record the location-level merge fields it lists — this seeds `defaults-first.md`)
- Dashboard widget types
- Anything else that exists in the left nav that the plan might use (Conversations AI, Reputation, Reporting, Trigger Links, Snippets, Media)

Date-stamp every section you discover. Never re-scrape or sample-check a stamped section (see the rule above).

While you're in each area, record the navigation path in `reference/ghl-ui-map.md` (exact labels). This map is what Phase 3 and Phase 5 will follow.

`STATE.md` → step 1 done.

## Step 2 — Baseline inventory → `00-baseline-inventory.md` and ledger

### If blank

The blank-account defaults are recorded in `reference/defaults-first.md` ("Default objects present in a blank sub-account"). Compare against that list instead of re-learning it. A quick look at each area's list page to confirm it's empty is enough (no builders, no scratch objects). Walk every area (workflows, funnels/sites, forms, surveys, custom fields, custom values, tags, pipelines, calendars, templates, contacts, dashboards, trigger links). Confirm each is empty except GHL defaults. Record the defaults you _do_ find (default pipeline, default calendar, built-in contact fields, location-level merge fields) in `reference/defaults-first.md` with a date stamp, and in `00-baseline-inventory.md` under "GHL defaults present".

If it's **not** blank: stop. Post a handoff listing what you found and ask the user to either clear it or reclassify the account as loaded. Do not delete anything.

### If loaded from a snapshot

Walk every area and record every object with enough detail to plan around it:

- **Workflows:** name, published/draft, trigger(s) and their filters, one-paragraph summary of what it does, which tags/fields/pipelines/calendars it touches, whether it removes contacts from anything, re-entry setting.
- **Custom fields / values / tags:** name, type, apparent purpose, where referenced if visible.
- **Pipelines:** name, stages in order.
- **Calendars:** name, type, form used, team/assignment, availability summary.
- **Forms / surveys:** name, fields/questions, where embedded if visible, what fires on submit.
- **Funnels / sites:** name, steps/pages, what each page is for, embedded forms/calendars.
- **Templates (email/SMS):** name, purpose.
- **Dashboards, trigger links, snippets, Conversation AI / reputation settings:** name, purpose.

**Enter every object into `LEDGER.md` as origin `pre-existing`, status `pre-existing`, phase `baseline`, before writing a single planned object.** The inventory file holds the longer descriptions; the ledger holds the rows.

### Gap analysis (loaded accounts only) → section in `00-baseline-inventory.md`

For each customer type and each journey stage from Phase 1 (arrive → capture → respond → qualify → book → remind → serve → follow-up → review): which pre-existing object(s) already handle it?

- Fully covered → ledger status `reused`; the plan includes the object as-is.
- Partly covered → ledger status `extended`; write exactly what needs adding. Extending a master object needs the user's OK at Gate 2 — flag it.
- Not covered → a new `planned` object.
- Conflicts: a planned trigger overlapping a pre-existing one, overlapping tag/stage names, two workflows that would both message the same contact at the same moment → resolve in the plan, and put every pre-existing workflow into the dependency graph in `10-workflows.md`.

**"Complete enough" is a valid outcome.** If the baseline already runs this business's journeys well, write that plainly in `00-overview.md` and at Gate 2: "The snapshot is complete enough for this business; nothing extra is needed" — or "only these N additions". The plan files then shrink to just those. Phases 4 and 5 still run in full on the whole sub-account.

`STATE.md` → step 2 done.

## Step 3 — `00-overview.md`

The end-to-end story for each customer type in plain language, and an explicit **owner-touch-points** list — every moment a human still must act (answer a call, approve a quote, confirm a booking). The design target is to make this list as short as possible; each remaining item says why it can't be automated.

## Step 4 — `01-customer-journeys.md`

Per customer type, the journey as stages: arrive → capture → respond → qualify → book → remind → serve → follow-up → review. Each stage names the GHL object(s) that handle it (pre-existing or planned). Short-term follow-up (days/weeks after a lead or job) is in scope. Long-term and seasonal nurturing are **out of scope for v1** — add a one-line "attaches here later" note at the end of each journey and design nothing for it.

## Step 5 — Object categories (`02`–`09`, `11`)

For every object: name, purpose, which journey/stage uses it, and — for anything custom — the **defaults-first justification** (what GHL default you checked and why it isn't enough). Check `reference/defaults-first.md` first, every time.

**`02-custom-values.md`** — two lists:

- _Business-identity values_ (everything a page/template/message says about the business): for each, the location-level variable it maps to if one exists (verified in the merge-field picker), else a custom value name, example value, and where it's used. All flagged `owner-must-set`. In **type-only mode** the example values are obviously-placeholder text (`[YOUR BUSINESS NAME]`, `[YOUR PHONE]`) — never a sampled business's real name, phone, or reviews.
- _Operational values_ (response-SLA text, after-hours message, booking policy text, etc.).

**`03-custom-fields.md`** — each field: name, type (from the verified type list), which form/survey/workflow writes it, which workflow condition/reads it. Fields that exist only to feed a workflow condition are called out explicitly.

**`04-tags.md`** — each tag: purpose, who applies it (form, workflow step, manual), who reads it (trigger filter, condition), and whether it's a _state_ tag (removed later) or a _history_ tag (permanent).

**`05-pipelines.md`** — each pipeline: stages in order, what moves a contact in/out of each stage (workflow / manual), which stage means "won"/"lost".

**`06-calendars.md`** — each calendar: type (from the verified list), purpose, availability logic, team assignment, confirmation/reminder behaviour, and **which form it uses** — the default booking form, or a custom form when the journey needs more than the default collects.

**`07-forms-surveys.md`** — form = single-screen capture; survey = multi-step, branching, or scored (qualification that routes by answer, intake, post-service NPS that routes happy → review request and unhappy → owner, quiz-style funnel steps, cancellation reasons). Each one: purpose; where embedded (funnel step / site page / calendar / sent by link); every field/question in order with type and required-ness; for surveys the page grouping and branching (which answer skips to which page); which custom field each answer writes; what fires on submit (workflow trigger, tag, stage, notification). Plus a **variable map** for any business info shown on the form.

**`08-funnels-sites.md`** — first decide **how many** funnels from the journeys: one per distinct entry intent, as many as the business needs (an HVAC business might need emergency-service, free-estimate, maintenance-plan, and review/referral funnels; a salon might need one booking funnel). Each funnel: ordered list of steps. Each step: purpose; the single action the visitor should take; sections top-to-bottom (headline, subhead, trust elements, form/calendar embed, CTA, footer); copy for every section using variables; which form/calendar is embedded; what happens on submit (next step, thank-you page, workflow fired, tag/stage set); tracking notes. Draw the step-to-step sequence. Prefer multi-step (landing → qualify → book → confirm) when the research says visitors need routing. Every page gets a **variable map**: every spot showing business info and the variable that fills it. Reviews/testimonials come from custom values (quote, name, rating) or the reviews widget if discovery confirmed it — never typed in. A page with a literal business name, phone, address, logo, or review is a defect. If the business needs a site beyond funnels, spec it page by page the same way.

**`09-templates-email-sms.md`** — each template: purpose, channel, trigger context (which workflow step sends it), full copy using variables, and the variable map.

**`11-dashboard.md`** — which widgets (from the verified widget list), which pipelines/stages/calendars feed them, what the owner should glance at daily in 2 minutes.

Each file ends with its ledger rows (name, type, origin, status `planned`, used-by).

## Step 6 — `10-workflows.md` (read `reference/workflow-doctrine.md` first)

For every workflow (planned and, on loaded accounts, pre-existing):

- Name, purpose, published-or-draft at ship time
- Trigger(s) with filters — each checked against `ghl-capabilities.md`
- Steps in plain language, numbered; branch points; **goto merges** drawn explicitly; waits; exit conditions
- Which workflows it removes contacts from and at which step; which completion tag/field it sets at the end
- Re-entry setting and why
- Every action checked against `ghl-capabilities.md`

Then the **dependency graph**: for every pair of workflows that can touch the same contact in the same process, one of `A → B` (B must not start until A finished), `A ∥ B` (independent), `A ⊗ B` (mutually exclusive — entering one removes from the other). For every `→` and `⊗` edge, the **mechanism** that enforces it (completion tag required by B's trigger filter; B triggered by an event only A produces; remove-from-workflow step; wait-for-condition; re-entry rules). "It should be fine" is not a mechanism.

**Reality check before Gate 2:** walk every trigger, action, and condition in the file against `ghl-capabilities.md`. Anything not in the catalog → verify in the UI now. If it doesn't exist → escalation ladder (principle 8): same outcome via a different construction (minimalism suspended), then web research verified in the UI, then the closest alternative recorded in `DEVIATIONS.md` and flagged for the user. Every rung you climbed is written down.

## Step 7 — `12-missed-call-and-edge-cases.md`

Per customer type: missed-call handling (what is texted back, how fast, what if they reply, what if they don't, how the owner is alerted, what changes after hours). Then: duplicate leads, wrong number / spam, unsubscribe and "STOP", books then cancels, no-show, goes cold at each stage, replies to a message from a workflow that already moved on. Each case names the workflow step or setting that handles it.

## Step 8 — Ledger

Every planned object is in `LEDGER.md` at status `planned`, with `used-by` filled from the plan (which workflow/page/template references it). Every identity custom value carries the `owner-must-set` flag. Sanity pass: any `planned` row with an empty `used-by` is either a mistake or should not be built — resolve before the gate.

## Step 9 — Gate 2

Post the gate block. Files: `00-baseline-inventory.md`, `02-plan/*`, `LEDGER.md`, `DEVIATIONS.md` if non-empty. Summary: number of workflows / funnels / forms / calendars planned; the "complete enough" verdict on loaded accounts; owner-touch-points count. Needs confirmation: every `extended` pre-existing object, every customer-facing deviation, every open design choice. Say: **"I will not start Phase 3 until you tell me to."** `STATE.md` → awaiting-user (Gate 2). Stop.

On approval: `STATE.md` → `Gates approved: 1, 2`, `Status: Phase 3`, open `phases/03-build.md`.

---

## Quality bar for the plan

- Every trigger/action/condition/field type/calendar type/widget named in the plan exists in `ghl-capabilities.md` with a date stamp.
- Every custom object has a defaults-first justification.
- Every page/form/template has a variable map with zero literal business data.
- Every workflow pair that can touch the same contact has a graph edge and, where ordered, a mechanism.
- Every `planned` ledger row has a `used-by`.
- On loaded accounts: every pre-existing object is in the ledger, and the gap analysis says reuse/extend/untouched for each.
