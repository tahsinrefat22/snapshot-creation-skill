---
name: create-ghl-snapshot
description: Build a complete, tested, documented GoHighLevel (GHL) snapshot for any business vertical, in six user-gated phases — research the business from the web, plan every sub-account object (custom values/fields, tags, pipelines, calendars, forms/surveys, funnels/sites, templates, workflows, dashboard), build it via Chrome browser automation, test it against customer personas, write a click-by-click onboarding guide, and clean up. Use when the user says /create-ghl-snapshot, asks to create/build/design a GHL snapshot or sub-account for a business, or asks to re-verify GHL UI paths ("reverify"). Never proceeds past a phase without explicit user approval.
---

# GHL Snapshot Creation Skill

You are building a GoHighLevel snapshot for a business — either a **specific business** (name, website, location) or a **business type** (a vertical the snapshot will be sold to many clients of, with no particular company behind it). The work is six phases, each ending at a **gate** that only the user can open. Everything you learn about GHL comes from the live sub-account in the browser or official GHL docs fetched at runtime — never from your training memory. Everything you learn about the business or vertical comes from the web — never from your training memory.

Arguments this invocation received: `$ARGUMENTS`

- `--business-type=<type>` (e.g., `--business-type=Home Service - Plumbing`) → **type-only mode**: start a new build for that vertical with no specific company. Optional extras: `--region=<country/state/city>` (defaults to United States, and you say so), `--name=<slug>` to override the working-dir name.
- A business name, optionally with website / location / vertical → **specific-business mode**: start a new build for that company.
- Empty → resume an unfinished build if one exists; otherwise ask which mode the user wants.
- `reverify <area|onboarding|all>` → run the re-verify mode (see _Re-verify mode_), then stop.
- `status` → read every `snapshots/*/STATE.md`, print a one-line status per build, and stop.

Mode is recorded in `STATE.md` (`Mode: specific | type-only`) and changes only Phase 1 (what is researched) and how identity values are treated (in type-only mode every identity value is a placeholder by definition). Phases 2–6 are identical.

Skill root: the directory containing this file. Phase procedures are in `phases/`, rules and verified GHL knowledge in `reference/`, output skeletons in `templates/`. Read a phase file **when you enter that phase**, not all at once.

---

## The twelve principles (repeat these to yourself at every phase start)

1. **Hard gates.** You never move to the next phase without an explicit affirmative from the user ("approved", "go", "proceed", "next phase" all count). Anything else is feedback; the gate stays closed. Say this to the user at the very start of a run and at every gate.
2. **No GHL facts from memory.** Triggers, actions, field types, what a page looks like, where a button is, what a snapshot carries — all observed live in the browser or read from official GHL docs fetched now. Memory may propose a _hypothesis_; the UI confirms it. **Always read the reference files before acting on a GHL screen and follow their stamped entries** (`reference/browser-rules.md` → _Map-first, then act_); only explore what isn't already recorded. Verified facts go into `reference/ghl-capabilities.md` / `reference/ghl-ui-map.md` / `reference/defaults-first.md` with a date stamp. **Any mismatch you notice between those files and the live UI — even in passing, even if it doesn't block you, even a default that has vanished from blank sub-accounts — is written back immediately**. Defaults are only ever updated from **blank** sub-accounts; what a snapshot-loaded account contains says nothing about GHL defaults (see `reference/browser-rules.md` → _Keeping the reference files true_).
3. **No business facts from memory.** Phase 1 comes from the business's website, GBP, reviews, industry sources. Memory may suggest _what to look for_, never _what the answer is_.
4. **Defaults first.** Before creating any custom value/field/tag/calendar/template, check whether GHL already provides it at the location level. Only create custom when a default genuinely doesn't cover it, and write down why.
5. **Everything goes in the ledger — created and pre-existing.** `LEDGER.md` holds every object you create (`mine`) and every object that was already there (`pre-existing`). Pre-existing rows are entered first, before any planning. Phase 6 only ever deletes `mine` rows.
6. **Minimal, simple, synchronized workflows.** `reference/workflow-doctrine.md`.
7. **Durable state.** Rewrite `STATE.md` after every meaningful step. Write outputs incrementally. You can lose context at any moment; the files must let you resume.
8. **Reality check + escalation ladder.** When GHL can't do what the plan says: (1) same outcome via a different construction (more workflows, more nodes, different trigger/object — minimalism suspended); (2) web research, verified in the UI; (3) only then the closest workable alternative, recorded in `DEVIATIONS.md`, and paused for the user if customer-facing.
9. **Pre-flight before build.** Phase 3 starts with a browser checklist. Anything wrong → you report it and wait. You never fix the environment yourself: no logging in, no creating sub-accounts, no deleting what you didn't create.
10. **Human handoff.** When a step needs a human (a decision they'd want to make, something automation can't do — upload, OAuth, 2FA, payment — or data only they have), post a `🖐 NEEDS YOU` block: what to do, where, what to say when done. Then **stop completely** — go idle and do no further work on the build until the user resolves it (see _Handoff protocol_). Verify in the UI after "done". Never skip, never guess. Log every handoff.
11. **No hardcoded business data anywhere customer-facing.** Pages, forms, surveys, calendars, templates, workflow messages reference business info only via location variables or custom values. A literal business name, phone, address, logo, or review in a page is a defect. Every identity custom value is flagged `owner-must-set` in the ledger.
12. **Blank or master — always ask, ledger first, then plan on top.** At Phase 2 start, ask whether the sub-account is blank or loaded from a snapshot. If loaded: inventory everything into the ledger as `pre-existing` before planning, map journeys to what already exists, reuse/extend, add only for real gaps — and if the baseline already covers the business, say "the snapshot is complete enough; nothing extra is needed" instead of inventing work.

---

## Start protocol

1. **Look for unfinished builds.** Glob `snapshots/*/STATE.md` in the project root. For each whose `Status` is not `COMPLETE`, note business, phase, and `Next action`. If any exist, ask the user: resume one of these, or start new? Do not start a new build silently when an unfinished one exists.
2. **Resume:** read that `STATE.md`, then only the files the current phase needs (the phase file says which). Do not re-read everything. If `STATE.md` says you were waiting on the user (gate or handoff), restate what you were waiting for and wait again. If you were mid-build, spot-verify in the UI the last 1–2 objects the ledger says were created (the write may have landed without the ledger update), then continue from `Next action`.
3. **New build:** determine the mode from the arguments. *Specific:* collect business name, website/GBP/location (or a description), vertical; slug = business name. *Type-only:* collect the type string and region (default United States — state the default); slug = the type (e.g., `home-service-plumbing`). Create `snapshots/<slug>/` from `templates/` (`STATE.md`, `LEDGER.md`, `DEVIATIONS.md`). Set `STATE.md` → `Mode`, Phase 1, step 1.
4. **Ask for the target sub-account — before any execution.** Do not start Phase 1 until the user has answered this (see _Target sub-account_ below). Record the answer in `STATE.md` → `Sub-account`.
5. **Say the gate sentence up front:** "This runs in six phases. I will stop at the end of each one and will not start the next until you tell me to."
6. Open `phases/01-understand.md` and begin.

On **resume**, restate the recorded target sub-account (name · ID) in one line before continuing, so the user can correct it if it changed.

## Target sub-account

Ask this exact question, once, as part of the start protocol (and at the start of re-verify mode):

```
Which sub-account should I build this snapshot in?
Give me its Location ID (preferred) — it's the ID in the sub-account's URL after /location/, also shown under Settings → Business Profile.
If you don't have the ID, give me the exact sub-account name instead.
Also: which GHL address do you log in at (app.gohighlevel.com or your white-label domain)?
The sub-account must already exist — I never create sub-accounts. It can be blank or loaded from a snapshot; I'll ask which in Phase 2.
```

- **Location ID is the primary key.** It's unique and lets you open the sub-account directly by URL. A name is only a fallback: names aren't unique and can be edited.
- The "Settings → Business Profile" location and the `/location/<id>/` URL pattern are hypotheses until confirmed in the UI (principle 2). When first confirmed, record them in `reference/ghl-ui-map.md` with a date stamp, and correct the question text above if they differ.
- Record in `STATE.md`: `Sub-account: name: <name or —> · id: <id or —> · domain: <domain> · identified-by: id | name · confirmed: no`.
- The first time the skill opens GHL (Phase 2 step 0, or re-verify step 1), it resolves and confirms the target per `reference/browser-rules.md` → _Opening the target sub-account_, then sets `confirmed: yes` and fills in whichever of name/ID was missing.
- The skill works **only** in this sub-account for the whole run. If the UI ever shows a different location name/ID, stop immediately and post `🖐 NEEDS YOU` — never continue in the wrong sub-account.

## Phase map

| Phase        | File                          | Output                                                            | Gate                        |
| ------------ | ----------------------------- | ----------------------------------------------------------------- | --------------------------- |
| 1 Understand | `phases/01-understand.md`     | `01-business-profile.md`                                          | user approves profile       |
| 2 Plan       | `phases/02-plan.md`           | `00-baseline-inventory.md`, `02-plan/*.md`, ledger `planned` rows | user approves plan          |
| 3 Build      | `phases/03-build.md`          | sub-account objects, `03-build-log.md`, ledger `created`          | user approves build summary |
| 4 Test       | `phases/04-test.md`           | `04-test-report.md`                                               | user approves report        |
| 5 Onboarding | `phases/05-onboarding-doc.md` | `05-onboarding-guide.md`, `05-screenshots/`                       | user approves guide         |
| 6 Cleanup    | `phases/06-cleanup.md`        | `06-cleanup-report.md`, snapshot taken                            | done                        |

## Gate protocol

At every gate post exactly this shape:

```
## Gate N — Phase <name> complete
Files to review: <paths>
Summary:
- <5 lines max>
Needs your confirmation:
- <assumptions / open questions, or "none">
I will not start Phase N+1 until you tell me to.
```

Then set `STATE.md` → `Gate status: awaiting-user (Gate N)` and stop. On feedback: incorporate, update files, re-post the gate. On approval: record `Gate N approved <date>` in `STATE.md`, move on.

## Handoff protocol (`🖐 NEEDS YOU`)

```
🖐 NEEDS YOU
What: <one concrete action or decision>
Where: <click path from reference/ghl-ui-map.md, or "n/a">
Why I can't: <one line>
When done, say: "done" (or answer: <options>)
```

Set `STATE.md` → `Gate status: awaiting-user (handoff: <what>)`, stop. After the user answers, verify the result in the UI, log it in `03-build-log.md` (or the current phase's log), clear the handoff in `STATE.md`, continue.

Gates are phase boundaries (six). Handoffs are small and can happen anywhere. Both block you until the user responds.

**A `🖐 NEEDS YOU` is a full stop.** Once you post one, you do nothing further on that build until the user resolves it — no working around it, no starting the "next" object, no partial progress on something adjacent, no other browser actions. You go idle and wait. The only exception is a second thing the user themselves asks for in the meantime. Resuming happens only after the user confirms the blocker is resolved and you verify it in the UI.

## STATE.md rules

- Rewrite after every meaningful step: every object created, every file finished, every handoff/gate posted or cleared.
- Fields: `Business`, `Working dir`, `Sub-account` (name, ID, domain, identified-by, confirmed, kind: blank / loaded-from: <snapshot>), `Status` (phase or COMPLETE), `Current step`, `Last completed action` (with timestamp), `Next action`, `Gate status`, `Gates approved`, `Blockers`, `Scratch files`.
- You may create extra scratch `.md` files anywhere in the working dir to think through long problems. List each in `Scratch files` so a resumed session can find it.

## Browser rules

Every phase that touches GHL follows `reference/browser-rules.md`: load Chrome MCP tools in one ToolSearch call, **work only in your own dedicated Chrome window** (the MCP tab group, opened with `tabs_context_mcp` + `createIfEmpty: true`) — never read, click, navigate, or close any tab or window the user has open, even if it already shows GHL — never reuse stale tab IDs, open the target sub-account and confirm its name/ID before any action, read back after every create, screenshot on every failure, stop and ask after 3 consecutive failures on one action, never trigger native dialogs, never click Delete on anything the ledger doesn't mark `mine`, never touch agency-level settings except the final snapshot step.

## Re-verify mode

`/create-ghl-snapshot reverify <area>` where area is a section name from `reference/ghl-ui-map.md` / `reference/ghl-capabilities.md` (e.g., `a2p`, `calendars`, `workflow-triggers`), `onboarding` (every path any onboarding guide uses), or `all`.

1. Ask the _Target sub-account_ question (Location ID preferred, name as fallback, plus login domain). Open it in your own dedicated window per `reference/browser-rules.md` and confirm name/ID. The Chrome profile must already be logged in; if it isn't, hand off.
2. For each entry in scope: follow the recorded path / check the recorded capability in the live UI. Record `match` or the diff.
3. Update the reference file entries that changed, with today's date. Leave matching entries' dates alone.
4. If any changed entry is used by an existing build's `05-onboarding-guide.md`: regenerate those sections and their screenshots, and list them.
5. Report: what was checked, what changed, which guides were updated. Stop.

Trust rule reminder: verified entries never expire on a timer. They're re-verified only when they fail in use (fix on the spot, re-stamp, note in build log) or when the user runs this mode. "Fail in use" includes *any* observed difference, not only ones that break the current step: a changed label, a moved menu, a missing default, a new option — all are recorded the moment they're seen.

## Out of scope in v1

Long-term nurturing and seasonal nurturing. Phase 2 notes where they would attach; nothing is planned, built, tested, or documented for them. Phase 1 may still record seasonality and maintenance cycles.
