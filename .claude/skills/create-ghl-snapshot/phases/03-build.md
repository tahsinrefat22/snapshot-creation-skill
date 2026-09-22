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
   - **Build order & design anchor — start with the Website.** When a Website is in scope, build it FIRST and lock its design style/vibe there (color theme, typography, section style, imagery feel). Then instruct the AI to make **every funnel — and every survey where its builder allows — match that same design pattern**, so the whole sub-account reads as one brand. **Forms are the exception:** the form builder isn't meaningfully design-able, so don't try to style forms to match — leave them on their clean default. (If no Website is in scope, the first funnel you build becomes the design anchor the rest copy.)
   - **Website structure & role (default architecture):**
     - **Multipage** with a nav menu — at minimum Home + the core content pages (e.g. About/Services, Contact) — plus real **Privacy Policy** and **Terms of Service** pages. Those two are also where the site's and funnels' footer privacy/terms links resolve to (point `{{custom_values.privacy_policy_url}}` / `{{terms_url}}` at these pages, or the owner sets them in onboarding).
     - **The website holds NO forms or calendars.** Every conversion action lives in a **funnel**; the website's CTAs ("Book / Request service", "Get an estimate") **redirect to the matching funnel**. This keeps one clean capture path and per-step funnel analytics.
     - **Educational / info pages live on the WEBSITE, not as funnel steps.** Any "learn more about the problem/service" content the funnels want to link out to becomes a website page. A funnel's **"Read More"-style buttons link out to those website info pages**, and each info page carries **CTA buttons back to the relevant funnel form/calendar** to convert the reader — so the info page both educates and funnels the visitor toward signing up. (This is the clean home for content that would otherwise force extra funnel steps.)
   - **Sites:** always start with GHL's **AI site builder** to generate the pages, then edit manually for anything the AI got wrong or couldn't do. Don't hand-build a site from a blank page when the AI can scaffold it.
   - **Funnels:** same approach — try the **AI funnel builder** first — but its capabilities are **more limited** than for sites, so expect to do more manual work (adding/reordering steps, embedding the exact form/calendar, fixing copy, wiring on-submit). Fall back to manual building whenever the AI can't produce what the plan specifies.
   - Either way, the plan (`02-plan/08-funnels-sites.md`) is the source of truth: after the AI generates, reconcile every page section, embed and variable against the plan, replace any literal business data with the merge fields / custom values, and read back before ledgering. Record the AI-builder entry points and any quirks in `reference/ghl-ui-map.md` the first time (map-first thereafter).
   - **Prompting the AI builder — constrain extra CONTENT, not design.** The failure mode is the AI *spilling extra text / functional content that creates unfulfilled obligations* — dead "Read More" buttons that need destination pages, fabricated testimonials / reviews / star ratings, invented stats / awards / years-in-business, orphan sections needing copy the client doesn't have, popups or forms with no target. Every such element forces the owner (or you) to go build something extra just to make the page honest. (Real case: an AI-generated funnel step spilled four "Read More" buttons → a whole set of extra info pages had to be designed to give them a destination.) So write the generation prompt to:
     - **Name the exact sections/blocks to include and say "and nothing beyond these,"** with the copy or copy-intent for each.
     - **Explicitly EXCLUDE:** testimonials / reviews / star ratings, "Read More" or any button linking to a page not in the plan (no dead links), popups / modals, forms not in the plan, invented statistics / awards / guarantees, photos of specific people presented as real staff or customers, and any extra sections.
     - **Do NOT forbid visual design — ask for it.** Explicitly ALLOW and request eye-pleasing styling: a cohesive color theme, section backgrounds / contrast, cards, icons, and decorative/stock imagery (pipes, tools, water, etc.). Decorative imagery fills no obligation, so it is *not* "extra content." Under-constraining design yields a bare wireframe (also a mistake) — ask for polish explicitly, and if a first pass looks flat, re-prompt for imagery + layout rather than settling.
     - Rule of thumb: **block content the client would have to go create to make the page honest; keep design that stands on its own.** Then reconcile against the plan and swap any literals → merge fields / custom values.
   - **Empty page vs. populated page changes what the in-builder "Ask AI → Build" does (verified 2026-09-22).** On an **empty** page, Build fires the **full funnel-page generator** (design tokens → visuals → SEO metadata → page summary) and returns a rich, agency-quality, image-driven page — noticeably more polished. On a page that **already has content**, Build does lighter *incremental* edits/restyling — cleaner and more on-message, but plainer. Use the empty-page path when you want strong design; expect a **heavier literal-scrub** after.
   - **Seed the AI with the real merge fields — don't just forbid literals, hand it the tokens to place.** Forbidding invented data does not stop the generator from inventing a whole fake business; the far bigger time/token saver is to give the AI the exact merge-field tokens for every known value and tell it to drop those in **directly** — in copy, hero/footer text, the nav & footer **logo image fields**, **call buttons**, and **image alt text**. Include at least: logo `{{location.logo_url}}`, business name `{{location.name}}`, phone `{{location.phone}}`, address `{{location.full_address}}`, email `{{location.email}}`, emergency line `{{custom_values.emergency_service_line}}`, plus every other custom value the plan defines (service area, license number, pricing/financing lines, privacy/terms URLs, etc.). Paste the actual token list straight from `02-plan/02-custom-values.md` into the generation prompt. Done well, this turns the post-gen scrub below from a full rewrite into a quick spot-check. (Not doing this is what forced a full manual scrub of an AI page that had invented a business name + logo mark, license #, insurance figures, address, phone, and email.)
   - **The exclusion list reduces but does NOT eliminate fabricated literals — a post-generation literal-scrub is MANDATORY, especially after an empty-page generation.** The full generator will still invent an entire fake business (name + logo mark, license number, insurance figures, address, phone, email, service area) even when the prompt forbids invented data. So after generating, sweep every section and replace *all* literal business data → `{{location.*}}` / custom values (principle 11), and delete any invented stat/award/claim that has no home in the plan. Treat this scrub as a required step, not a spot-check.
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
