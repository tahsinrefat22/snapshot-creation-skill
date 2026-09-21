# GHL UI map — LIVE-VERIFIED navigation paths

**Status: PARTIAL — entries with a `Verified:` date are live-verified; the rest are not.** Filled by walking the live sub-account. Labels are copied exactly as displayed. Until an entry carries `Verified: <date>`, do not follow it — discover the path instead and record it here.

Entry format:

```
### <area>
Verified: <date> | sub-account: <name> | view: sub-account | agency
Path: <Left menu label> → <sub-tab> → <button/link>
Notes: <what the page looks like, where the primary button sits, any modal that appears>
```

Trust rule: no timer. Re-verify only on failure in use or `/create-ghl-snapshot reverify <area>`. A path that fails in use is re-discovered immediately and re-stamped.

**Mismatch rule:** if the live UI ever differs from an entry here — renamed, moved, new, or gone (including defaults missing from blank sub-accounts) — update the entry immediately, whether or not it blocked you: changed → re-stamp + `(changed <date>: was "<old>")`; removed → keep the line, mark `REMOVED <date> — <where you looked>`; new → add, stamped. Log it in the phase log under "Reference updates".

---

### login

Verified: —

### open-sub-account-by-id

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: navigate to `https://app.gohighlevel.com/v2/location/<locationId>/launchpad` (any sub-page under `/v2/location/<locationId>/` works the same way)
Notes: shows a blue spinner for ~5–10 s, then the Launchpad "Setup Guide". Browser tab title = agency name, not sub-account name. White-label domains: same path on the agency's domain (unverified).

### sub-account-switcher (used only to open the confirmed target by name — see browser-rules)

Verified: 2026-09-21 (read-only) | sub-account: Home Service - Plumbing Master Template
Path: top of left sidebar, under the agency logo: a box showing the sub-account name (truncated) and its city/region line below, with an up/down chevron on the right
Notes: the full name is truncated in the UI; read it from the element's text/DOM. The city line (e.g., "Dhaka, Dhaka Division") comes from the business address.

### business-profile (name, ID, address, phone, hours — the source of location.\* values)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings (bottom of left nav) → MY BUSINESS → Business Profile  (URL `/v2/location/<id>/settings/company`)
Notes: left card "General Information" — Location ID (copy icon, top right of card), Business Logo Upload/Remove, Friendly Business Name, Legal Business Name, Business Email, Business Phone, Branded Domain, Business Website, Business Niche, Business Currency, blue "Update Information". Right card "Business Physical Address" — street, city, postal, state, country, **Time Zone***, Platform Language, blue "Update". Below: Business Information, Authorized Representative, General checkboxes, Contact Deduplication. No business-hours field.

### custom-values

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → OTHER SETTINGS → Custom Values  (URL `/v2/location/<id>/settings/custom_values`)
Notes: tabs All values / Folders; top right "Add folder" and blue "+ Add custom value"; table Name · Folder · Key · Value.
Click quirk (verified 2026-09-21, settings iframe): after any save, the **first** click on "+ Add custom value"/"Add folder" is swallowed; click again **≥ 6 s later** (two clicks 1–3 s apart open nothing). In the Folder dropdown the first click on an option only focuses — click the option twice. If the modal still won't open, reload the page. Reliable loop: fill → Create → wait 5 s → click button → wait 6 s → click button → wait 2 s → zoom-check modal is open → fill next.

### custom-fields

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → OTHER SETTINGS → Custom Fields  (URL `/v2/location/<id>/settings/fields?tab=field`)
Notes: object tabs All / Contact / Opportunity / Business; Fields / Folders; top right "Create folder" and blue "+ Create field" → right drawer "Create custom field" (Field type, Add to object, Field name, Folder name*, Key, Description, placeholder; Cancel / Create custom field).
Create flow (verified 2026-09-21):
1. **Folder first** — "Create folder" → modal: Select object (Contact) + Folder name → Create. The field drawer can't create folders.
2. "+ Create field" → drawer. Set Field type (double-click the option — first click only focuses; scroll the type list for Radio select / File upload / Date picker / Signature). "Add to object" defaults to the tab's object.
3. Field name (key auto-fills snake_case). Folder name dropdown → double-click the folder option.
4. Dropdown/Radio: type option 1 in the first row (double-click it — post-dropdown first click is swallowed), then "+ Add option" (fixed button) for each further row; the list scrolls after ~6–7 rows. **Radio label input is in a narrow left column (~x=590 at full 1568 width); clicking too far right does nothing and text can fall into the still-focused Folder field.** Verify each row.
5. "Create custom field" (bottom-right; stays put as the drawer scrolls).

### tags

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → OTHER SETTINGS → Tags  (URL `/v2/location/<id>/settings/tags`)
Notes: tabs Tags / Categories; top right "+ Create category" and blue "+ Create tag"; table Tag name · Description · Category · Created · Updated · Actions.
Create flow (verified 2026-09-21):
- **Create category** modal: Category name* + Description → Create category.
- **Create tag** modal: Tag name* · Category (dropdown, default "No category") · Description · Tag color · live Preview → Create tag. Make the category first, then pick it per tag.
- Tag names accept **colons and hyphens** exactly as typed (e.g. `state:awaiting-booking`).
- Category dropdown: first click on an option is swallowed AND closes the dropdown → **reopen and click the option again**.
- **No chaining:** the "Create tag" open-click right after a save is swallowed (global first-click quirk). Do **one tag per batch** (open → name → category reopen-reclick → Create); the gap before the next batch makes the next open reliable.

### pipelines

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → MY BUSINESS → Opportunities & Pipelines → "Pipelines" tab  (URL `/v2/location/<id>/settings/pipeline`)
Notes: blue "+ Create pipeline" top right; table Pipeline name · Total stages · Updated on.
Create-pipeline modal (verified 2026-09-21): Pipeline name* · "Use opportunity-level probability" toggle · display-color style · **Pipeline stages pre-filled with 4 defaults** (New Lead / Contacted / Proposal Sent / Closed), each row = drag handle + stage-name input + show-in-reports + Probability% + delete. Rename via triple-click+type; "+ Add stage" to add; trash icon to remove; **Create** bottom-right. Stage names accept emoji (🚨 rendered). 
Lost reason options: Settings → Custom Fields → **Opportunity** tab → "Lost reason" row → pencil (Edit) → "Define lost reason options" → +Add option rows → **Update field**. (Built-in Status/Lost reason dropdowns are edited here, not in the pipeline modal.)

### calendars

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → BUSINESS SERVICES → Calendars  (URL `/v2/location/<id>/settings/calendars?section=calendars`)
Notes: top tabs Meetings · Services · Rentals · Connections; sub-tabs Calendars · Preferences · My availability; blue "+ New calendar" top right → modal "Choose calendar type" (Personal booking, Round robin, Class booking, Collective booking, Explore more types → Event calendar).
Round-robin quick-create modal (verified 2026-09-21): Calendar name · Add description · **Select team members** (multi-select; REQUIRES >=1 user in the sub-account — else "No Data") · Custom URL · Meeting duration (number + Minutes) · "Advanced settings" link (business hours, notifications, forms) · Cancel / Confirm -> Success modal with booking link + iframe embed. Selecting members shows a "Booking availability" preview (default Weekdays 8:00 AM-5:00 PM). Notifications live only in Advanced settings, not the quick modal. Adding users to a location auto-creates a per-user Personal calendar.

### calendar-forms (custom booking form assignment)

Verified: —

### forms

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Sites → "Forms" tab  (URL `/v2/location/<id>/form-builder/main`)
Notes: empty state shows blue "+ Create form" (when the sub-account has NO forms, "+ Create form" creates "Form N" immediately → builder; when forms already exist it opens a "Create new form" modal (Start from Scratch / From templates) → Create. Either path opens `/v2/location/<id>/form-builder-v2/<formId>`). Builder: left "Form Element" palette (Quick Add / Add Object Fields); top tabs Edit · Settings (On Submit) · Submissions · Notifications · Analytics; top right Preview · Integrate · blue Save. Builder appends a dragged palette element to the END of the field list but ABOVE the Submit button (Submit stays last); the drop Y position does not control placement, so drag anywhere onto the canvas. Palette elements must be DRAGGED (a single click only highlights, does not add). Reorder existing elements by dragging their grip. (verified 2026-09-21)

### surveys

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Sites → "Surveys" tab  (URL `/v2/location/<id>/survey-builder/main`)
Notes: blue "+ Create survey" (creates "Survey N" → `/v2/location/<id>/survey-builder-v2/<id>`). Slides with gear (Slide Settings: name, position, Jump To), "+ Add Slide".

### funnels

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Sites → "Funnels" tab  (URL `/v2/location/<id>/funnels-websites/funnels`)
Notes: blue "+ New funnel" top right; list Name · Last updated · Funnel steps.
Create-funnel modal (2026-09-21): "+ New funnel" opens **Create new funnel** modal with 3 options — **From blank** (has required *Funnel name* field + Create button), **Build with AI** (Beta card), **From templates** (1000+ templates card). Cancel/Create bottom right.

### websites

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Sites → "Websites" tab  (URL `/v2/location/<id>/funnels-websites/websites`)
Notes: blue "+ New website".

### domains

Verified: —

### templates-email

Verified: —

### templates-sms

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Marketing → "Snippets" tab  (URL `/v2/location/<id>/marketing/templates`)
Notes: blue "+ New Snippet" → Add Text Snippet / Add Email Snippet. Text snippet modal: Name*, body with emoji + tag icon ("Custom Values & Trigger Links" merge picker), attachment, test send; Cancel / Save.

### workflows

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Automation → "Workflows" tab  (URL `/v2/location/<id>/automation/workflows`)
Notes: blue "Create workflow ▾" top right → Start from Scratch / Select from Template / Company based workflow. Builder `/v2/location/<id>/automation/workflow/<id>`: "Add new trigger" card, "+" on the line for actions; tabs Builder · Settings · Enrollment history · Execution logs; top right Test workflow, Draft/Publish toggle, Save. First open may show "Introducing auto save" modal. Builder is a cross-origin iframe — screenshots/coordinates only.

### workflow-execution-logs

Verified: —

### dashboard

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Dashboard
Notes: "Dashboard ▾" selector + "+ New" top left; date range, blue "Edit dashboard" top right.

### contacts (create, timeline, add-to-workflow, delete)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account (list only)
Path: left nav Contacts  (URL `/v2/location/<id>/contacts/smart_list/All`)
Notes: tabs Smart Lists · Bulk Actions · Custom Fields · Tasks · Companies; top right Import and blue "+ Add Contact".

### conversations

Verified: —

### opportunities

Verified: —

### media-library

Verified: —

### phone-numbers (LC Phone)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → BUSINESS SERVICES → Phone System → "Phone numbers" tab  (URL `/v2/location/<id>/settings/phone_system?tab=manage`)
Notes: blue "+ Add Number"; sub-tabs Phone Numbers · Number Pools · Verified Caller IDs · Port-In Numbers. Missed-call text-back: Phone System → Voice → "Voicemail & Missed Call TextBack" → left "Missed Call Text Back".

### a2p (LC Phone — 10DLC brand & campaign)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → Phone System → "Trust Center" tab → card "A2P Messaging (SMS)" → "Start Registration" / "Brand & Campaigns"

### email-sending (LC Email — dedicated domain)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: Settings → BUSINESS SERVICES → Email Services  (URL `/v2/location/<id>/settings/smtp_service`)
Notes: shows current domain (shared `mg.msgsndr.net`), link "Add a dedicated domain →", blue "Dedicated Domain And IP" top right.

### integrations (Google, Facebook/Instagram, Stripe)

Verified: —

### google-business-profile

Verified: —

### team (users, notifications)

Verified: —

### mobile-app (install / login instructions shown in UI, if any)

Verified: —

### reputation (review requests, reviews widget)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | view: sub-account
Path: left nav Reputation → Settings tab → left menu Reviews AI / Review Link / SMS Requests / Email Requests / WhatsApp Requests / Reviews QR / Spam Reviews / Integrations  (URL `/v2/location/<id>/reputation/settings?tab=…`)
Notes: blue "Send Review Request" top right on Overview.

### snapshots (agency view — Phase 6 only)

Verified: —

### agency-labs-ai-builder (per-sub-account AI page/funnel builder toggle)
Verified: 2026-09-21 (path + feature name from user) | view: agency
Feature name (exact): **"Brand New Funnel AI & Website AI"**
Path: Agency view → Settings → Labs → **Sub-Accounts** tab → click the **search bar** → **Activate feature** → locate the target sub-account in the list → **enable "Brand New Funnel AI & Website AI"** for it
Notes: This is an AGENCY-level toggle → the assistant NEVER changes it itself (principle 9 / browser-rules "never touch agency settings except the final snapshot step"). It is a `🖐 NEEDS YOU` handoff for the user. It must be ON per sub-account before the in-builder AI appears (default OFF on new sub-accounts). Once ON, the funnel page-builder shows an **"Ask AI"** panel (Assist / Build tabs) — Build mode generates/edits page content from a prompt, scoped via @context chips (a selected element scopes to that element; clear the chip for page-wide). See `reference/ghl-capabilities.md` → "Funnel: adding a step + per-step AI builder" and Phase 3 pre-flight check #7.
