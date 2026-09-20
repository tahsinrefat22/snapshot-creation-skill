# GHL Snapshot Creation Skill — Initial Plan

Status: APPROVED 2026-09-19. Skill files written to `.claude/skills/create-ghl-snapshot/` per §3. Next: §15 step 2 — supervised dry run of Phase 2 capability discovery.

---

## 1. What this skill is

A Claude Code skill (`/create-ghl-snapshot`) that takes a business (any vertical) from "here is the business" to a finished, tested, documented GoHighLevel snapshot, in six phases, each gated behind your explicit approval.

Inputs from you at start — one of two modes:

- **Specific-business mode:** business name / website / location (or a description if it doesn't exist yet) + vertical. `/create-ghl-snapshot Acme Plumbing, https://…, Austin TX, plumbing`
- **Type-only mode** (added 2026-09-19): just the vertical, no company — for snapshots sold to many clients of one type. `/create-ghl-snapshot --business-type=Home Service - Plumbing` (optional `--region=…`, default United States). Phase 1 then samples 5–8 real businesses of that type and synthesizes a *representative business*; every identity value is a placeholder by definition. Phases 2–6 are identical.
- Either way the skill must work for any vertical.
- The GHL sub-account the snapshot will be built in, logged in via Chrome — and **which kind it is**: a blank/empty sub-account, or one loaded from an existing snapshot (e.g., your master snapshot). The skill always asks this explicitly; it never assumes blank.

Output at the end:

- A configured sub-account, snapshotted at agency level
- A working directory of markdown files: business profile, full build plan, ledger, test report, onboarding guide

---

## 2. Non-negotiable principles (the rules the skill enforces on itself)

These are baked into SKILL.md and repeated at the top of every phase file, so they survive context loss.

1. **Hard gates.** The skill never moves to the next phase without an explicit "approved" / "go ahead" from you. It says this to you at the very start of a run and at every gate. A gate is passed only by a message from you, never by the skill deciding "this looks fine".
2. **No GHL facts from model memory.** Anything GHL-specific — available workflow triggers/actions, what a settings page looks like, where a button is, what a snapshot does or doesn't carry — must be observed in the live sub-account (via browser) or read from official GHL docs fetched at runtime. Model memory is only allowed for _hypotheses_ which are then verified. Every verified fact is written to a reference file so it doesn't need re-verifying within the run.
3. **No business facts from model memory.** Phase 1 research comes from the web (business website, Google Business Profile, reviews, industry sources, competitor sites). Model memory may suggest _what to look for_, not _what the answer is_.
4. **Defaults first.** Before creating any custom value / field / tag / calendar / template, the skill checks whether GHL already provides it at the location level (e.g., `{{location.phone}}`, `{{location.name}}`, `{{location.address}}`, business hours, default calendar, default pipeline, built-in contact fields like `phone`, `email`, `source`). Only creates custom things when a default genuinely doesn't cover the need, and writes down _why_.
5. **Everything created goes in the ledger.** One file, `LEDGER.md`, lists every object the skill creates (custom value, field, tag, pipeline, stage, calendar, form, funnel, page, template, workflow, trigger link, dashboard widget). Each row: type, name, phase created, why, used-by (filled in as it gets used), status (`planned` → `created` → `used` / `unused`), cleanup decision. Phase 6 works purely from this ledger.
6. **Minimal, simple, synchronized workflows.** Detailed doctrine in §7.
7. **Durable state.** The skill writes state to disk constantly and can resume from any point after a context reset or cooldown. Detailed in §12.
8. **Reality check + escalation ladder.** When a planned thing turns out to be impossible in GHL as designed (trigger doesn't exist, action not available, UI doesn't allow it), the skill does not silently drop it and does not jump straight to a compromise. It climbs this ladder in order, and only moves down a rung when the rung above is exhausted:
   1. **Same outcome, different construction.** Rethink how to achieve the _exact same_ customer-facing result inside GHL: more workflows, more nodes, a different trigger, a different object type (e.g., a tag change instead of a field change as the trigger; a pipeline stage change as an event; a wait-for-condition instead of a direct trigger; a form-submission event instead of a contact-created event; a calendar event instead of a workflow timer). Minimalism (§7.1) is suspended for this search — a correct result with three workflows beats a simplified wrong one. Every attempt is written down in `DEVIATIONS.md` under the blocker, even the failed ones.
   2. **Web research.** Search GHL help docs, community forums, and tutorials for how others achieved the same outcome. Anything found is verified in the sub-account UI before being adopted.
   3. **Closest workable alternative.** Only when 1 and 2 are exhausted: design the nearest alternative that changes as little customer-facing behavior as possible, record exactly what is different and why, and — if the difference is customer-facing — pause and ask you before proceeding.

   The deviation record shows the full ladder: what was planned, every alternative construction tried and why it failed, what was searched and found, and the final choice.

9. **Pre-flight before build.** Phase 3 does not start until the skill has verified, in the browser, that the environment is in order (§5, Phase 3 step 0). Anything out of order is reported to you as a checklist of things to fix manually; the skill waits and re-checks, it never tries to fix the environment itself (no logging in, no creating sub-accounts, no deleting pre-existing objects it didn't create).
10. **Human-handoff protocol.** Whenever a step needs a human — a decision between approaches you'd want to make yourself, an action the browser automation can't do (e.g., a file upload the UI won't accept from automation, a payment, an OAuth consent screen, a 2FA code), or credentials/data only you have — the skill stops, posts a clearly-marked **`🖐 NEEDS YOU`** block saying exactly what to do and where, waits for your "done", then verifies the result in the UI before continuing. It never guesses, never works around a handoff by skipping the step, and logs every handoff in `03-build-log.md`.
11. **No hardcoded business data anywhere customer-facing.** Sites, funnels, forms, surveys, calendars, email/SMS templates, and workflow messages must reference business information only through dynamic variables — location values where they exist (`{{location.name}}`, `{{location.phone}}`, `{{location.address}}`, `{{location.email}}`, `{{location.website}}`, `{{location.logo_url}}`, etc., as verified in the sub-account) and custom values for everything else (tagline, service area, hours text, license number, review quotes and reviewer names, star rating, team photos, brand colors, social links, booking policy text, warranty text — anything that would change if this snapshot were loaded for a different business). Typing the business name, phone, or a review quote directly into a page or template is a defect. Every such custom value is flagged `owner-must-set` in the ledger and becomes a line in the onboarding guide's "fill in your business details" section.
12. **Blank or master — always ask, then inventory.** At the start of Phase 2 the skill asks whether the build sub-account is blank/empty or loaded from an existing snapshot (a master snapshot or similar). If it's loaded, the skill inventories **everything** already in it — workflows (with their triggers and what they do), custom fields, custom values, tags, pipelines and stages, calendars, forms, surveys, funnels/sites, email/SMS templates, dashboards, trigger links, snippets — into `00-baseline-inventory.md`, marked `pre-existing, not mine`, and **plans around it**: reuses what fits (a pre-existing field or pipeline is used instead of creating a duplicate), avoids conflicts (a planned workflow whose trigger overlaps a pre-existing one is reconciled in the dependency graph), and never modifies or deletes a pre-existing object without asking you first. Pre-existing objects are excluded from Phase 6 cleanup. If it's blank, the skill still verifies that in the UI rather than taking it on faith.

---

## 3. Skill anatomy (files that ship with the skill)

```
.claude/skills/create-ghl-snapshot/
├── SKILL.md                      # entry point: principles, phase overview, gate protocol, resume protocol
├── phases/
│   ├── 01-understand.md          # Phase 1 procedure
│   ├── 02-plan.md                # Phase 2 procedure + templates for every plan file
│   ├── 03-build.md               # Phase 3 procedure (browser automation playbooks)
│   ├── 04-test.md                # Phase 4 procedure + report template
│   ├── 05-onboarding-doc.md      # Phase 5 procedure + doc template
│   └── 06-cleanup.md             # Phase 6 procedure + snapshot finalization
├── reference/
│   ├── browser-rules.md          # Chrome MCP rules shared by every phase that touches GHL: setup, read-back, 3-strike, never-list, discovery scraping, screenshots
│   ├── workflow-doctrine.md      # §7 in full: minimal/simple/goto/sync/remove rules
│   ├── defaults-first.md         # the checklist of GHL location-level defaults to check before creating anything (verified list, updated each run)
│   ├── ghl-capabilities.md       # LIVE-VERIFIED catalog of triggers, actions, field types, calendar types, etc. — with "verified on <date>" stamps. Starts empty; filled by discovery (§8)
│   ├── ghl-ui-map.md             # LIVE-VERIFIED navigation map: where things are in the sub-account UI, with date stamps
│   ├── research-checklist.md     # what Phase 1 must find out about any business
│   ├── persona-testing.md        # how to derive personas → scenarios → test cases
│   └── onboarding-topics.md      # checklist of onboarding topics (A2P 10DLC, phone, domain, email, calendar sync, payments, mobile app, GBP, FB/IG, team, notifications, custom values review)
├── templates/                    # markdown skeletons for every output file
└── scripts/                      # optional Playwright helpers if Chrome MCP proves insufficient (see §9)
```

The `reference/ghl-capabilities.md` and `reference/ghl-ui-map.md` files are the skill's "memory of GHL" — but it's _earned_ memory: every entry has a verified-on date, and is re-verified only when it fails in use or when you ask (§8 trust rule) — never on a timer.

---

## 4. Working directory per build (state files)

Each run creates a folder (proposal: `./snapshots/<business-slug>/`) containing:

```
snapshots/<business-slug>/
├── STATE.md                 # current phase, current step, last completed action, next action, gate status. Rewritten after every meaningful step.
├── LEDGER.md                # every created object (§2.5)
├── DEVIATIONS.md            # every "GHL can't do X, did Y instead" with reasoning
├── 01-business-profile.md   # Phase 1 output
├── 00-baseline-inventory.md # Phase 2 step 2: what was in the sub-account before the skill touched it (blank-verified, or full master-snapshot inventory + how the plan uses it)
├── 02-plan/
│   ├── 00-overview.md       # how the snapshot runs the business end-to-end; owner-touch points
│   ├── 01-customer-journeys.md
│   ├── 02-custom-values.md
│   ├── 03-custom-fields.md
│   ├── 04-tags.md
│   ├── 05-pipelines.md
│   ├── 06-calendars.md
│   ├── 07-forms-surveys.md
│   ├── 08-funnels-sites.md
│   ├── 09-templates-email-sms.md
│   ├── 10-workflows.md      # every workflow, step by step, plus the dependency/sync graph
│   ├── 11-dashboard.md
│   └── 12-missed-call-and-edge-cases.md
├── 03-build-log.md          # chronological build log (what was done, what failed, what was retried)
├── 04-test-report.md        # persona scenarios, expected vs actual, before/after fixes
├── 05-onboarding-guide.md   # click-by-click owner guide
├── 05-screenshots/          # screenshots captured from the live UI for the guide
└── 06-cleanup-report.md     # what was removed, what was kept and why
```

---

## 5. Phase-by-phase specification

### Phase 1 — Understand the business and its customers

**Goal:** a short, precise `01-business-profile.md` that you can read in 5 minutes and say "yes, that's the business".

**Process:**

1. Take your inputs. If a website/GBP exists: fetch it (WebFetch / Chrome). Pull services, pricing signals, service area, hours, emergency availability, booking method, reviews (what customers praise/complain about), team size signals, existing CTAs/forms.
2. Web-search the vertical: how businesses of this type typically acquire leads (calls vs forms vs walk-ins), seasonality, average ticket, typical customer decision timeline, common no-show / missed-call patterns, review importance, repeat/maintenance cycles.
3. Reason about **customer types** the business must handle — not a generic list, but derived from the research. Example for HVAC: emergency no-heat caller, quote-shopper for a replacement, maintenance-plan renewer, tenant/landlord, commercial account. For each: how they arrive, what they need within how long, what "handled well" looks like, what causes them to churn.
4. Reason about **how the business runs**: who answers the phone, when nobody answers, how jobs are scheduled, what happens after a job, how reviews are asked for, what a lost lead looks like.
5. Write the profile. Sections: Business snapshot · Services · Customer types (table) · Lead channels · Operating rhythm · Pain points the snapshot should solve · Assumptions I made (flagged clearly) · Sources (URLs).

**Gate 1:** you review the profile. Skill waits. You may correct assumptions; the skill updates the file and re-asks.

### Phase 2 — Plan the snapshot

**Goal:** a complete, reviewable design of the sub-account that runs the business with minimum owner touch.

**Process:**

0. **Sub-account handoff and the blank-or-master question.** Phase 2 opens with a `🖐 NEEDS YOU` block: "Create or pick the sub-account this snapshot will be built in, open it in the Chrome tab, stay logged in, then say 'done' — and tell me: is it **blank/empty**, or **loaded from an existing snapshot** (e.g., your master snapshot)? If loaded, which one?" The skill never creates sub-accounts itself. After "done" it reads the sub-account name/ID from the UI (asks you for the ID / login user if it can't) and records name, ID, and kind (`blank` / `loaded-from: <snapshot name>`) in `STATE.md`.
1. **Capability discovery** (§8). The skill opens the sub-account and verifies/refreshes `ghl-capabilities.md`: available workflow triggers, actions, condition types, calendar types, form field types, what custom value/field types exist. Designing against a verified catalog prevents the "GHL doesn't work that way" discovery from happening late.
2. **Baseline inventory** (§2.12) → `00-baseline-inventory.md` in the working directory.
   - _If blank:_ verify it in the UI (no workflows, funnels, custom fields/values, tags, contacts, templates; only the default pipeline/calendar if GHL created them). Record the GHL defaults found (location values, built-in contact fields, default pipeline/calendar) in `defaults-first.md`. If it turns out not to be blank, stop and ask — you either clear it or reclassify it as loaded.
   - _If loaded from a snapshot:_ walk every area and record every object with enough detail to plan around it — for workflows: name, published/draft, trigger(s), a one-paragraph summary of what it does, which tags/fields/pipelines it touches, and whether it removes contacts from anything; for fields/values/tags: name, type, apparent purpose; for pipelines: stages in order; for calendars: type, form used, team; for forms/surveys/funnels/templates: name and what they're for. Everything is marked `pre-existing, not mine`. The skill then writes a short **"how the plan will use the baseline"** section: what it will reuse as-is, what it wants to extend (needs your OK), what it will leave alone, and any planned object that would conflict with a pre-existing one (same trigger, same purpose, overlapping tag/stage names) and how the conflict is resolved. Pre-existing workflows enter the dependency graph in `10-workflows.md` like any other workflow, because they will run against the same contacts.
   - You review the inventory and the "how the plan will use the baseline" section as part of Gate 2.
3. Write `00-overview.md`: the end-to-end story for each customer type, and an explicit list of **owner-touch points** (things a human still must do) — the design target is to minimize this list.
4. Write `01-customer-journeys.md`: per customer type, the journey as stages: arrive → capture → respond → qualify → book → remind → serve → follow-up → review. Each stage says which GHL object handles it. Short-term follow-up (the days/weeks right after a lead or job — chasing an unbooked lead, post-job check-in, review request) is in scope. **Long-term nurturing and seasonal nurturing are out of scope for v1** (§14) — the journey file notes where they would attach, and nothing is built for them.
5. Design each object category (files 02–12). For each object: name, purpose, which journey/stage uses it, and — for anything custom — the **defaults-first justification** ("why `{{location.phone}}` isn't enough here").
   - `02-custom-values.md` is split into two lists: **business-identity values** (everything a page/template/message needs to say about the business — per §2.11, all flagged `owner-must-set`) and **operational values** (internal settings like response-time SLA text, after-hours message). For each identity value: the location-level variable it maps to if one exists, else the custom value name and an example value.
   - `07-forms-surveys.md` and `08-funnels-sites.md` include, per page/form, a **variable map**: every spot on the page that shows business info and which variable fills it. A page design with a literal business name, phone, address, logo, or review in it is rejected at Gate 2.
   - Reviews/testimonials on pages are driven by custom values (quote, reviewer name, rating) or GHL's reviews widget if discovery confirms it exists and pulls from the connected GBP — never typed in.
   - **Forms vs. surveys — the skill builds both and decides per use.** A form is for a single-screen capture (contact request, quote request, calendar booking questions). A survey is for anything multi-step, branching, or scored: lead qualification that routes by answers (emergency vs. routine, residential vs. commercial, budget/timeline), new-customer intake, post-service feedback / NPS (which then routes happy customers to the review request and unhappy ones to the owner), quiz-style funnel steps, cancellation reasons. `07-forms-surveys.md` specifies each one as: purpose, where it's embedded (funnel step / site page / calendar / sent by link in a workflow), every field/question in order with type and whether required, for surveys the page-by-page grouping and the branching logic (which answer skips to which page), which custom field each answer writes to, and what fires on submit (workflow trigger, tag, pipeline stage, notification). Fields that only exist to feed a workflow condition are called out so the ledger can trace them. Calendar booking forms are replaced by a custom form when the default one can't collect what the journey needs — the spec says which calendar uses which form. All of this is verified against what discovery confirms the survey builder can actually do (question types, conditional logic, scoring).
   - **Funnels are designed step by step, and there can be as many as the business needs.** The skill first decides _how many_ funnels from the customer journeys — one per distinct entry intent, not one-size-fits-all (e.g., an HVAC business may need an emergency-service funnel, a free-estimate funnel, a maintenance-plan funnel, and a review/referral funnel; a salon may need only a booking funnel). Each funnel in `08-funnels-sites.md` is specified as an ordered list of steps, and each step as a full page spec: purpose of the step, the single action the visitor should take, sections top-to-bottom (headline, subhead, trust elements, form/calendar embed, CTA, footer), copy for every section (via variables per §2.11), which form or calendar is embedded, what happens on submit/book (next step, thank-you page, which workflow fires, which tag/pipeline stage is set), and tracking notes. Step-to-step flow is drawn as a sequence so you can see the whole path a visitor takes. Multi-step funnels (e.g., landing → qualify → book → confirm) are preferred over one long page when the journey research shows visitors need to be qualified or routed. The site (if the business needs one beyond funnels) gets the same page-by-page treatment.
6. Workflows (`10-workflows.md`) follow §7 doctrine. Every workflow gets: trigger(s), step list in plain language, branch points, goto merges, exit conditions, which other workflows it removes contacts from or waits for, and its place in the **dependency graph** (must-run-before / independent-of).
7. `12-missed-call-and-edge-cases.md`: missed call handling per customer type, after-hours, duplicate leads, wrong-number/spam, unsubscribe, contact replies "STOP", contact books then cancels, contact goes cold at each stage.
8. Dashboard (`11-dashboard.md`): which widgets, which pipelines/stages feed them, what the owner should glance at daily.
9. Populate `LEDGER.md` with every planned object at status `planned`.

**Gate 2:** you review the plan files. Skill waits. Iterates on feedback.

### Phase 3 — Build

**Goal:** everything in the plan exists in the sub-account.

**Process:**

0. **Pre-flight check** (§2.9) — run in the browser before anything is created, results posted to you as a pass/fail checklist:

- Chrome MCP is connected and responsive.
- A GHL tab is open and logged in (no login page, no expired-session banner, no 2FA prompt).
- The **correct sub-account** is selected — the skill reads the sub-account name/ID from the UI and compares it to what Phase 2 recorded in `STATE.md`; on mismatch it asks you to switch, it never switches itself.
- The sub-account **still matches `00-baseline-inventory.md`**: for a blank account, it's still blank; for a loaded account, the pre-existing objects are still there and nothing new has appeared since the inventory. Anything unexpected is listed and the skill asks you what it is before building — it never deletes anything it didn't create.
- `STATE.md` says Gate 2 was approved; `LEDGER.md` has every planned object.
- `ghl-ui-map.md` has an entry for every area Phase 3 will touch (missing ones are discovered now, before building starts).
  If any line fails: the skill posts a `🖐 NEEDS YOU` block with exactly what to fix (e.g., "Log in to GHL in the Chrome tab and open sub-account X, then say 'done'"), waits, and re-runs the check. It does not proceed on a partial pass.

1. Build order (dependencies dictate this): custom values → custom fields → tags → pipelines/stages → calendars → forms/surveys → email/SMS templates → funnels/sites (which embed forms/calendars) → workflows (which reference everything) → dashboard.
2. For each object: navigate (using `ghl-ui-map.md`, verifying as it goes), create, **verify creation by reading it back from the UI**, update `LEDGER.md` (`planned` → `created`), append to `03-build-log.md`, update `STATE.md`.
   - Funnels are built **one funnel at a time, one step at a time**, in the order specified in `08-funnels-sites.md`: create the funnel → add step → build the page section by section per its spec → embed the form/calendar → set the on-submit/next-step behavior → preview the page → read back and compare to the spec → mark the step `created` in the ledger → next step. A funnel is not marked complete until every step's flow (step 1 → step 2 → … → thank-you) has been clicked through in preview.
3. When something can't be built as planned → escalation ladder (§2.8): alternative constructions first, web research second, compromise last. Non-customer-facing deviations: record and continue. Customer-facing deviations: record and pause for your call.
4. Workflows are built last and one at a time; after each, the skill opens it and reads back every step to confirm it matches the plan, then saves/publishes per the plan (some workflows are meant to be published, some left draft until testing).
5. Custom values used inside templates/workflows are ticked in the ledger with "used in: <workflow/template name>".
6. **Handoffs during build** (§2.10). Expected ones the skill should anticipate rather than discover: uploading images/logos/brand assets to the media library (if automation can't upload, the skill prepares the file list and asks you to upload, then reads the URLs back); any OAuth connection (Google, Facebook, Stripe) — these are deferred to the onboarding guide anyway, never done in the template; choosing between two equally valid designs when the plan left it open; anything that costs money (phone numbers, premium features). Each handoff is one `🖐 NEEDS YOU` block: what, where (click path from `ghl-ui-map.md`), what to say when done. After "done", the skill verifies in the UI and updates the ledger/build log.
7. **Hardcode sweep.** After funnels/sites, forms, and templates are built, the skill re-opens each one and scans the rendered content for literal business data (name, phone, address, email, reviews, logo file names). Any hit is fixed to the variable and recorded in the build log. Same sweep runs again after workflows (message bodies).

**Gate 3:** you get a build summary (what was built, what deviated, what's still draft). Skill waits.

### Phase 4 — Test

**Goal:** evidence that the sub-account behaves correctly for each customer persona, with a report.

**Process:**

1. Derive scenarios from `01-customer-journeys.md` and `12-missed-call-and-edge-cases.md`: for each persona, the happy path plus 2–4 edge cases.
2. Create clearly-named test contacts (`ZZ-TEST-<persona>`), tagged `test-contact`, logged in the ledger so Phase 6 removes them.
3. Execute scenarios by the most realistic means available: submit the live form on the funnel, book via the live calendar, manually add a contact to a workflow, change pipeline stage, simulate inbound SMS reply where possible. Read workflow execution logs, conversation logs, contact timeline to confirm each step fired in the expected order.
4. **Sync/order tests specifically:** for every "A must finish before B" edge in the dependency graph, run the scenario and confirm from the execution logs that B did not act before A completed, and that "remove from workflow" actions fired where designed. On a loaded (master-snapshot) sub-account this includes the pre-existing workflows: the test confirms they didn't fire unexpectedly on the test contact, or fired in the order the plan expected.
5. What cannot be truly simulated (a real missed phone call, real A2P-delivered SMS, real email deliverability) is listed explicitly as **"verified by inspection, not execution"** with what was inspected.
6. Fix issues found; report each with before/after.
7. Write `04-test-report.md`: per persona → per scenario → steps, expected, actual, pass/fail, fix applied (before/after), re-test result.

**Gate 4:** you review the test report. Skill waits.

### Phase 5 — Onboarding guide

**Goal:** `05-onboarding-guide.md` — the simplest possible click-by-click document for a non-technical owner.

**Process:**

1. Topics come from `reference/onboarding-topics.md` — proposed baseline: log in · install mobile app · buy/port phone number (LC Phone) · **A2P 10DLC registration via LC Phone (brand + campaign, what to type, how long it takes, why SMS won't send until approved)** · set up email sending (LC Email — dedicated domain if the client has one) · connect calendar (Google/Outlook) · connect Google Business Profile · connect Facebook/Instagram · connect Stripe/payments · add team members and set who gets notified · review & fill custom values (the ones the ledger says the owner must set) · connect domain to funnel/site · turn on the workflows that were left draft · daily 2-minute dashboard routine · what to do when a lead comes in.
2. **Every click path is captured live**: the skill navigates the actual sub-account, records the exact menu labels, and takes screenshots into `05-screenshots/`. Nothing about UI location comes from memory.
3. Writing style rules: one action per line; "Click the blue **Save** button at the top right"; screenshot after every 2–3 steps; no jargon without a one-line plain explanation; no "you may want to" — only "do this".
4. Where a step requires the owner's own information (EIN for A2P, business address, etc.), the guide has a fill-in box listing exactly what to have ready before starting.
5. **"Fill in your business details" section** is generated from the ledger: every custom value flagged `owner-must-set` (§2.11) becomes one line — the value's plain-English label, where it appears on the site/messages (so the owner understands why it matters), an example, and the click path to set it. This section comes first in the guide after login, because until it's done the site and messages show placeholders.

**Gate 5:** you review the guide. Skill waits.

### Phase 6 — Cleanup and finalize

**Goal:** no residue, snapshot taken.

**Process:**

1. Walk `LEDGER.md`. Anything with status `created` but never marked `used` → verify in the UI it truly has no references → delete → record in `06-cleanup-report.md`. Anything ambiguous → ask you. Objects in `00-baseline-inventory.md` (`pre-existing, not mine`) are never candidates for cleanup, even if the plan ended up not using them — if you want them gone, that's your call, made explicitly.
2. Remove all test contacts, test opportunities, test appointments, test conversations.
3. Confirm the workflows that should be published are published and the ones that should ship as draft are draft.
4. Take the snapshot at agency level (procedure verified live, not from memory). Record snapshot name and what it includes.
5. Final `STATE.md`: `COMPLETE`.

**Gate 6 / done:** final summary to you.

---

## 6. Gate protocol (how the skill talks to you at gates)

At each gate the skill posts:

- Which phase just finished, and the file(s) to review
- A 5-line summary
- Open questions / assumptions it needs you to confirm
- The exact sentence: **"I will not start Phase N+1 until you tell me to."**

Accepted approvals: any clear affirmative from you ("approved", "go", "proceed", "next phase"). Anything else is treated as feedback to incorporate, and the gate stays closed.

**Gates vs. handoffs.** A gate is a phase boundary (six in total). A handoff (`🖐 NEEDS YOU`, §2.10) can happen anywhere inside a phase and is smaller: one concrete thing for you to do or decide, then the skill continues within the same phase. Both block the skill until you respond; both are recorded in `STATE.md` so a resumed session knows it's waiting on you and for what.

---

## 7. Workflow design doctrine

1. **Fewest workflows that stay understandable.** Prefer one workflow per _process_ (e.g., "New Lead Handling") with internal branching over many micro-workflows — but split when a process has genuinely independent tasks (e.g., "Review Request" is independent of "Appointment Reminders").
2. **Simple steps.** Prefer built-in actions over clever chains. No step exists without a reason written in the plan.
3. **If/else → goto merge.** When a branch splits only to differ in one or two steps, the branches rejoin via goto to a shared continuation instead of duplicating the tail. The plan file draws this explicitly.
4. **Explicit dependency graph.** For each pair of workflows that can touch the same contact in the same process, the plan states one of: `A → B` (B must not start until A finished), `A ∥ B` (independent, may run in parallel), `A ⊗ B` (mutually exclusive — entering one removes from the other).
5. **Ordering is enforced mechanically, not hoped for.** Enforcement tools, chosen per case from what capability discovery confirms exists: a completion tag or custom field set at the end of A that B's trigger filter requires; B's trigger being an event that only A produces (e.g., stage change); "Remove from workflow" at the right step; "wait for condition"/"wait until" steps; workflow re-entry settings (allow re-entry or not) set deliberately.
6. **Remove contacts from workflows** whenever continuing would harm the customer: booked → remove from chase; replied → remove from follow-up sequence; opted out → remove from all messaging; won/lost → remove from pipeline nurture. Each removal is written into the plan as an explicit step with the triggering condition.
7. **Reality check loop.** Every trigger/action/condition in the plan is checked against `ghl-capabilities.md` _before_ Gate 2. If it's not in the catalog, the skill verifies in the UI; if it still doesn't exist, it climbs the escalation ladder (§2.8) — same outcome via a different construction first, web research second, compromise last — and records the whole ladder in `DEVIATIONS.md`. Rule 1 (fewest workflows) yields to correctness during this search: the goal is the planned customer outcome, achieved by whatever GHL construction actually works.
8. **Missed calls are first-class.** Every persona's journey has an explicit missed-call branch: what is texted back, how fast, what happens if they reply, what happens if they don't, how the owner is notified, and what happens after hours.

---

## 8. GHL ground truth: capability discovery and UI map

Because GHL's UI and feature set change frequently and the skill must not rely on memory:

- **Discovery routine** (runs at the start of Phase 2, and on demand): open the workflow builder in the sub-account, enumerate every trigger category and trigger, every action category and action, every condition/filter type. Open forms builder and record field types. Open calendars and record calendar types and options. Open custom fields/values and record types. Write to `reference/ghl-capabilities.md` with a "verified <date>" stamp per section.
- **UI map** (`reference/ghl-ui-map.md`): for each area the skill needs (settings, custom values, fields, tags, pipelines, calendars, forms, funnels, templates, workflows, dashboard, phone numbers, A2P, email, integrations, team, snapshots at agency level), the verified path: left-menu label → sub-tab → button. Date-stamped.
- **Trust rule (no time-based expiry).** A verified entry stays trusted indefinitely. It is re-verified only when (a) it **fails in use** — the skill follows the recorded path/uses the recorded capability and the UI doesn't match — in which case the skill re-verifies that entry on the spot, updates it with a new date stamp, and notes the change in the build log; or (b) **you explicitly ask** — typically because a client hit a wall in the onboarding guide and reported it. For (b) the skill has a re-verify mode: `/create-ghl-snapshot reverify <area>` (e.g., `reverify a2p`, `reverify calendars`, `reverify onboarding` for every path the guide uses, `reverify all`). Re-verify walks the live UI, diffs against the recorded entry, updates the reference file, and — if the onboarding guide of any existing build is affected — regenerates those sections and their screenshots, reporting exactly what changed. The date stamps are kept for information only; they never trigger anything on their own.
- **Docs fallback:** when the UI alone doesn't explain behavior (e.g., what a snapshot does or doesn't carry over), the skill fetches official GHL help docs at runtime and cites the URL in the relevant plan/onboarding file.

---

## 9. Execution tooling (how the skill drives GHL)

Proposal, in order of preference:

1. **Claude-in-Chrome MCP** (primary). Uses your logged-in session, no credential handling, can screenshot, read page, click, fill forms, run page JS. Best for: navigation, one-off creation, reading back what was built, capturing onboarding screenshots.
2. **Playwright scripts** (fallback, in `scripts/`), only if Chrome MCP proves too slow or unreliable for repetitive creation (e.g., 40 custom fields, 15 tags). Would reuse a persistent browser profile so you log in once. Each script is small, single-purpose, and logs what it created for the ledger.
3. **GHL public API** — _not_ in scope for v1. Reason: workflows, funnels, forms, and dashboards are not creatable through the public API in any complete way, so the browser is required anyway; mixing two channels adds failure modes. Revisit later for bulk-simple objects (custom fields, tags) if it saves meaningful time.

Browser-automation safety rules: never click Delete without the ledger saying the object is the skill's own creation; never touch agency-level settings except the final snapshot step; stop and ask after 3 consecutive failures on the same action; screenshot on every failure and record in the build log.

---

## 10. Testing method and honest limits

Covered in Phase 4. Key honesty points the skill must always state in the report:

- Real inbound calls, real SMS delivery (A2P-gated), and real email deliverability cannot be executed from a template sub-account; those paths are verified by inspection of the workflow config and execution-log simulation where GHL provides it.
- Anything verified by inspection is labeled as such, never presented as "passed".

---

## 11. Context durability and resume protocol

- `STATE.md` is the single source of truth for "where am I". Rewritten after every meaningful step (not just at phase ends). Format: current phase, current sub-step, last completed action (with timestamp), next action, gate status (open/closed/awaiting-user), blockers.
- Every phase file writes its output incrementally (append as you go), never "hold it all in context and write at the end".
- **On skill start**, the first thing the skill does is look for `snapshots/*/STATE.md`. If one exists and is not `COMPLETE`, it asks whether to resume that build, and if yes, reads `STATE.md` + the current phase's files (not everything) and continues from "next action".
- The skill never re-does completed work on resume; it trusts the ledger and build log, but spot-verifies in the UI the last 1–2 objects created before the reset (in case the write happened but the ledger update didn't).
- The skill is allowed (and told) to create additional scratch markdown files in the working directory whenever it needs to think through something long — e.g., `02-plan/scratch-workflow-graph.md` — and to list them in `STATE.md` so they're findable.

---

## 12. Risks and mitigations

| Risk                                                                     | Mitigation                                                                                                |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| GHL UI changes break the UI map                                          | Re-verify on failure in use; `reverify` mode when a client reports a wall; never memorize                 |
| Browser automation flakiness                                             | Read-back verification after every create; 3-strike stop-and-ask; screenshots on failure                  |
| Model "invents" a GHL feature                                            | Capability catalog check before Gate 2; deviation log                                                     |
| Context reset mid-build                                                  | STATE.md + ledger + incremental writes + resume protocol                                                  |
| Over-creation of custom objects                                          | Defaults-first rule with written justification; ledger-driven cleanup                                     |
| Workflows racing each other                                              | Mandatory dependency graph + mechanical enforcement + sync tests in Phase 4                               |
| Onboarding guide drifts from real UI                                     | Screenshots and labels captured live in Phase 5, same run                                                 |
| Skill runs ahead of the user                                             | Gate protocol with explicit "I will not start Phase N+1" sentence                                         |
| Test data leaks into the snapshot                                        | Test contacts tagged and ledgered; Phase 6 removes before snapshot                                        |
| Build starts in the wrong / non-blank / logged-out sub-account           | Phase 3 pre-flight checklist; skill waits for you to fix, never fixes the environment itself              |
| Automation hits something it can't do (upload, OAuth, 2FA, payment)      | `🖐 NEEDS YOU` handoff; verified after "done"; never skipped                                              |
| Business name/phone/reviews hardcoded into pages → snapshot not reusable | §2.11 rule, variable map required at Gate 2, hardcode sweep in Phase 3, owner-must-set list in onboarding |

---

## 13. Decisions (answered 2026-09-19)

| #   | Question                     | Decision                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Skill location               | **Project-level first**: `.claude/skills/create-ghl-snapshot/` in this folder. Copy to `~/.claude/skills/` once stable.                                                                                                                                                                                                                                                                                                                                              |
| 2   | Working directory per build  | **`./snapshots/<business-slug>/`** inside this project.                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3   | Template sub-account         | **Skill asks you to create/pick one** at the start of Phase 2 via a `🖐 NEEDS YOU` block, and **always asks whether it's blank or loaded from a snapshot** (e.g., master). You create/pick it, open it in Chrome, stay logged in, say "done". If the skill can't read the sub-account from the UI it asks you for the sub-account ID / user. Kind + ID recorded in `STATE.md`; contents inventoried in `00-baseline-inventory.md` and planned around (§2.12). |
| 4   | Phone/email stack            | **LC Phone + LC Email.** Onboarding guide's phone-number, A2P 10DLC, and email-domain sections are written for the LC flows (verified live, as with everything else).                                                                                                                                                                                                                                                                                         |
| 5   | Execution tooling            | **Chrome MCP only** to start. Playwright helpers only if we hit a wall.                                                                                                                                                                                                                                                                                                                                                                                       |
| 6   | Gate approvals               | **Any clear affirmative** passes a gate.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 7   | Re-verification of GHL facts | **No time-based expiry.** Re-verify only when an entry fails in use, or when you explicitly ask (e.g., after a client reports a wall in the onboarding guide) via `/create-ghl-snapshot reverify <area\|onboarding\|all>`. See §8 trust rule.                                                                                                                                                                                                                        |
| 8   | Onboarding topic list        | **Baseline list is complete** for now; you'll add items as they come up. `reference/onboarding-topics.md` is the single place to add them.                                                                                                                                                                                                                                                                                                                    |

---

## 14. Out of scope for v1 — TODO for later versions

These are deliberately **not built** by the skill in v1. The skill knows they exist (so Phase 2 leaves a clear attachment point in the journeys and doesn't half-build them) but does not plan, build, test, or document them.

| Item                    | What it would cover                                                                                                                                                                                            | Where it attaches later                                                                                                    |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Long-term nurturing** | Multi-month drip sequences for cold leads and past customers: educational content, periodic check-ins, reactivation offers, "we haven't heard from you" campaigns                                              | After the short-term follow-up ends without a booking, and after the review stage for past customers                       |
| **Seasonal nurturing**  | Vertical-specific seasonal campaigns (pre-summer AC tune-up, pre-winter furnace check, holiday promotions, annual maintenance reminders, tax-season pushes, etc.) driven by dates rather than contact behavior | Date/recurring triggers on the whole past-customer segment; ties into the maintenance-cycle findings from Phase 1 research |

When these are added, they get their own plan file (`13-nurture.md`), their own workflow doctrine notes (long sequences need re-entry rules, frequency caps, and opt-out handling that short workflows don't), their own persona test scenarios, and their own onboarding-guide section. Phase 1 research may still _record_ seasonality and maintenance cycles now, since that's cheap and useful later.

---

## 15. What happens after you approve this plan

1. Write `SKILL.md` and the six phase files + reference skeletons (no GHL facts filled in — those get earned on the first run).
2. Do a **dry run of Phase 2's capability discovery** against a real sub-account with you watching, to validate the Chrome MCP approach and seed `ghl-capabilities.md` / `ghl-ui-map.md`.
3. Then run the skill end-to-end on a first business, fixing the skill as we go.
