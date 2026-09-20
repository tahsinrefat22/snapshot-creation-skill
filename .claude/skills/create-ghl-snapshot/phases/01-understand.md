# Phase 1 — Understand the business and its customers

**Principles in force:** 1 (hard gate), 3 (no business facts from memory), 7 (durable state). Re-read the principle list in `SKILL.md` if you're resuming.

**Goal:** `01-business-profile.md` — short, precise, readable in 5 minutes, so the user can say "yes, that's the business" (specific mode) or "yes, that's how these businesses run" (type-only mode).

**Two modes** (from `STATE.md` → `Mode`):
- **specific** — a real company: research it *and* its vertical. Steps 1–8 as written.
- **type-only** — a vertical with no company behind it (e.g., `Home Service - Plumbing`): skip step 2's single-business fetch and instead build a **representative business** from a sample of real ones (step 2-T). Everything else is the same. The profile describes the typical business of this type in the given region, and every business-identity value is a placeholder by definition.

**Files you need:** `reference/research-checklist.md`, `templates/01-business-profile.md`. Output goes to `<working dir>/01-business-profile.md`. Write it **section by section as you go**, not at the end.

**Tools:** WebFetch / WebSearch for websites, reviews, industry sources. Chrome MCP (per `reference/browser-rules.md`) when a page needs a real browser (JS-rendered sites, Google Business Profile, review platforms). Load the web tools with one ToolSearch call: `select:WebFetch,WebSearch`.

---

## Steps

### 1. Inputs

*Specific mode:* business name, website URL, location, vertical, anything else they gave. If the business doesn't exist yet (no website), the user's description is the primary source — say so in the Assumptions section.

*Type-only mode:* the type string (keep the user's wording as the display name; derive the vertical and any sub-vertical from it — `Home Service - Plumbing` → vertical *plumbing*, category *home services*), and the region (default United States; write the default into the Assumptions section).

`STATE.md` → Phase 1, step 1.

### 2-T. Representative business (type-only mode — replaces step 2)

You have no company to fetch, so you sample real ones. Web-search for **5–8 actual businesses of this type in the region** (e.g., `plumber <city>` across 3–4 different cities of different sizes; also pull from a marketplace listing like Yelp/Angi/GBP results). For each, fetch the site and note the same things step 2 asks for: services and what's pushed, pricing signals, hours/emergency handling, contact methods and form fields, CTAs, team-size signals, what reviews praise/complain about, visible automation.

Then synthesize the **representative business**: what most of the sample does (services, hours, emergency availability, contact methods, common form fields, common CTAs), where they differ (and which variant the snapshot should default to, with the reason), and the recurring review complaints across the sample — these become the pain points. Record every sampled business with its URL under Sources, and mark the whole profile as "representative of the type; not a specific company".

Identity values (name, phone, address, logo, reviews…) are all placeholders in this mode; note that in section 1 so Phase 2 flags every one of them `owner-must-set` without debate.

`STATE.md` → step 2 done. Continue at step 3.

### 2. The business itself (specific mode — from the web)

Fetch the website (every main page: home, services, about, contact, booking, pricing, FAQ, reviews). Fetch the Google Business Profile if findable (search `"<business name>" <city>`). Fetch review pages (Google, Yelp, Facebook, industry-specific sites like Angi/HomeAdvisor/Healthgrades/Zocdoc where relevant).

Record in `01-business-profile.md` as you find it:

- Services offered, and which are pushed hardest (hero section, top of nav)
- Pricing signals (free estimate? price list? financing?)
- Service area / locations
- Hours; emergency / after-hours availability; how emergencies are handled today
- How a customer contacts them today: phone, form, chat widget, booking tool, text
- Existing CTAs and forms — what fields they ask for
- Team size signals (staff page, "family owned", number of trucks/chairs/providers)
- What reviews praise and what they complain about (speed, communication, price, no-shows, follow-up)
- Any current automation signals (auto-replies, booking confirmations, review requests seen in reviews)

`STATE.md` → step 2 done.

### 3. The vertical (from the web)

Web-search the industry, not the business: `how do <vertical> businesses get customers`, `<vertical> lead response time`, `<vertical> missed call statistics`, `<vertical> seasonality`, `<vertical> average ticket`, `<vertical> customer retention / maintenance cycle`, `<vertical> no-show rate`. Prefer industry associations, trade publications, and survey data over marketing blogs. Record: lead channels (calls vs forms vs walk-ins vs referrals), decision timeline, seasonality, average ticket, repeat/maintenance cycles, review importance, typical no-show and missed-call patterns. Keep the URLs.

Seasonality and maintenance cycles are recorded even though seasonal nurturing is out of scope for v1 — it's cheap and useful later.

`STATE.md` → step 3 done.

### 4. Customer types — derived, not generic

From steps 2–3 (or 2-T–3), define the customer types **this** business must handle. Not a boilerplate list — each type must be traceable to something you found (a service line, a review pattern, a vertical fact, or — in type-only mode — a pattern seen across the sampled businesses). Typical count: 3–6.

For each type, fill the table in the template:

- How they arrive (channel), and in what state (urgent / researching / returning)
- What they need, and within how long (minutes / hours / days)
- What "handled well" looks like to them
- What loses them (slow reply, no callback, no price clarity, no reminder…)
- Rough share of volume, if the research supports an estimate (say "estimate" if it is one)

### 5. How the business runs

Reason from the evidence about the operating rhythm: who answers the phone, what happens when nobody does, how jobs/appointments get scheduled, what happens after the job, how reviews are asked for today, what a lost lead looks like. Mark every inference as inference.

### 6. Pain points the snapshot should solve

Three to seven bullets, each tied to evidence: "Reviews mention slow callbacks (3 of 12 Google reviews) → missed-call text-back + owner alert."

### 7. Assumptions and sources

List every assumption you made, clearly flagged, so the user can correct them. List every source URL used.

### 8. Gate 1

Post the gate block (shape in `SKILL.md`): files = `01-business-profile.md`; summary = 5 lines; confirmation = the assumptions list. Say: **"I will not start Phase 2 until you tell me to."** `STATE.md` → `Gate status: awaiting-user (Gate 1)`. Stop.

On feedback: update the profile, re-post the gate. On approval: `STATE.md` → `Gates approved: 1 (<date>)`, `Status: Phase 2`, open `phases/02-plan.md`.

---

## Quality bar for the profile

- Every factual claim about the business has a source URL or is marked as an assumption.
- Customer types are specific enough that a stranger could role-play each one.
- No section longer than it needs to be; the whole file reads in ~5 minutes.
- Nothing in it is a GHL design decision yet — that's Phase 2.
