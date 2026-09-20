# Phase 5 — Onboarding guide

**Principles in force:** 1, 2 (every click path captured live — nothing about UI location from memory), 7, 10, 11 (owner-must-set values become the first section).

**Goal:** `05-onboarding-guide.md` — the simplest possible click-by-click guide for a business owner who is not technical. One action per line. Screenshots. No jargon without a one-line plain explanation.

**Files you need:** `reference/onboarding-topics.md`, `reference/ghl-ui-map.md`, `reference/browser-rules.md`, `LEDGER.md` (for `owner-must-set` values and draft workflows), `templates/05-onboarding-guide.md`. Output: the guide + `05-screenshots/`.

**Audience rule:** write for someone who has never used software like this. Every step names the exact label on screen and where it is ("left menu", "top right", "blue button"). Never "configure", "navigate", "integrate" — say "click", "type", "choose".

---

## Step 1 — Topic list

Take the baseline from `reference/onboarding-topics.md`. Drop topics the plan doesn't use (no calendars → no calendar-sync section). Add anything the build introduced that the owner must touch (from `00-overview.md` owner-touch-points, and ledger `draft` workflows). Order: the "have this ready" checklist → log in → fill in business details → phone → A2P → email → the rest → daily routine → what to do when a lead comes in.

## Step 2 — Capture every path live

For each topic, in the sub-account (and agency view only where the topic genuinely lives there, e.g., some LC Phone/A2P screens): navigate the real path, record the exact labels, take a screenshot every 2–3 steps into `05-screenshots/<topic>-<n>.png`. Follow `browser-rules.md`. If a path in `ghl-ui-map.md` doesn't match → re-discover, update the map with today's date.

Where a step needs the owner's own data or credentials (Google login, EIN, business address, Stripe login), you stop at that screen, screenshot it, and write what the owner will see and type — you don't complete it. If a screen only appears after an action you can't perform, say so in the guide ("after you sign in with Google, you'll see…") and mark that step `[not captured — appears after your login]`.

When the UI alone doesn't explain a behaviour (how long A2P approval takes, why SMS won't send until then, what a dedicated email domain does), fetch the official GHL help article at runtime, explain it in one plain sentence, and cite the URL at the end of the section.

## Step 3 — Write the guide (template structure)

1. **Before you start — have these ready** (fill-in box: EIN, legal business name, address, website, the phone number to port or the area code to buy in, Google account email, Stripe login, logo file, review quotes)
2. **Log in** (+ mobile app install)
3. **Fill in your business details** — generated from the ledger: every `owner-must-set` custom value → plain-English label, where it shows up (so they know why it matters), an example, click path to set it. This comes first because until it's done the site and messages show placeholders.
4. **Get your phone number** (LC Phone)
5. **Register for texting (A2P 10DLC)** — brand + campaign; what to type in each box; how long approval takes; the sentence "until this is approved, texts will not send"; what to do if rejected
6. **Set up email sending** (LC Email; dedicated domain if they have one)
7. **Connect your calendar** (Google/Outlook)
8. **Connect Google Business Profile**
9. **Connect Facebook / Instagram**
10. **Connect payments** (Stripe)
11. **Add your team and choose who gets notified**
12. **Connect your website domain** to the funnel/site
13. **Turn on the automations** — the exact list of workflows that ship as draft, with the click path to publish each, in the order the dependency graph requires
14. **Your 2-minute daily routine** (dashboard)
15. **When a lead comes in — what to do** (per customer type, in 3–5 lines)

Style: numbered steps; one action per step; bold the on-screen label; screenshot reference after every 2–3 steps; a "✅ You're done with this part when…" line at the end of each section.

## Step 4 — Self-check

Read the whole guide as the owner. Every step must be doable with only what's on screen plus the "have these ready" box. Every screenshot referenced exists. No step relies on memory of the UI — each has a captured path.

## Step 5 — Gate 5

Post the gate block. Files: `05-onboarding-guide.md`, `05-screenshots/`. Summary: sections count, screenshots count, steps marked `[not captured]`. Needs confirmation: any topic you dropped or added. Say: **"I will not start Phase 6 until you tell me to."** `STATE.md` → awaiting-user (Gate 5). Stop.

On approval: `STATE.md` → `Gates approved: 1–5`, `Status: Phase 6`, open `phases/06-cleanup.md`.
