# create-ghl-snapshot

A [Claude Code](https://claude.com/claude-code) skill that builds a complete, tested, documented **GoHighLevel snapshot** for any business vertical — in six phases, each gated behind your explicit approval.

## What it does

1. **Understand** — researches the business (or the business type) from the web: services, customer types, lead channels, missed-call patterns, review complaints.
2. **Plan** — designs every sub-account object: custom values/fields, tags, pipelines, calendars, forms & surveys, funnels & sites (step by step), email/SMS templates, workflows (with an explicit dependency graph), dashboard, missed-call and edge-case handling.
3. **Build** — creates everything in the sub-account via Chrome browser automation, reading each object back to verify it.
4. **Test** — runs persona-based scenarios, checks workflow ordering from execution logs, fixes and reports before/after.
5. **Onboarding guide** — writes a click-by-click guide for a non-technical owner (phone number, A2P 10DLC, email, calendar, GBP, Facebook, Stripe, team, custom values, turning on automations), with every click path captured live from the UI.
6. **Cleanup** — removes unused objects and test data from the ledger, takes the snapshot.

Everything GHL-specific is observed in the live UI or fetched from official docs at runtime — never from model memory. Everything business-specific comes from the web.

## Install

Project-level (this repo):

```bash
git clone git@github.com:tahsinrefat22/snapshot-creation-skill.git
cd snapshot-creation-skill
claude
```

User-level (available in any folder):

```bash
cp -r .claude/skills/create-ghl-snapshot ~/.claude/skills/
```

Requires Chrome with the Claude-in-Chrome extension connected, and a GHL login.

## Use

```
/create-ghl-snapshot --business-type=Home Service - Plumbing
/create-ghl-snapshot --business-type=Dental Clinic --region=Canada
/create-ghl-snapshot Acme Plumbing, https://acmeplumbing.com, Austin TX, plumbing

/create-ghl-snapshot                      # resume an unfinished build
/create-ghl-snapshot status               # one line per build
/create-ghl-snapshot reverify onboarding  # re-check GHL UI paths after a client hits a wall
```

At each gate, reply `approved` / `go` / `proceed` to continue, or give feedback. When you see a `🖐 NEEDS YOU` block, do the thing and reply `done`.

Build outputs land in `snapshots/<slug>/` (git-ignored).

## Layout

```
.claude/skills/create-ghl-snapshot/
├── SKILL.md            entry point: principles, start/resume, gates, handoffs, reverify
├── phases/01–06        one procedure per phase
├── reference/          workflow doctrine, browser rules, research & testing checklists,
│                       onboarding topics, and the live-verified GHL capability catalog + UI map
└── templates/          skeletons for every output file (state, ledger, plan files, reports, guide)
docs/INITIAL-PLAN.md    the design document the skill was built from
```

## Status

v1. Long-term and seasonal nurturing are deliberately out of scope (see `docs/INITIAL-PLAN.md` §14).
