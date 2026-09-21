# GHL capabilities — LIVE-VERIFIED catalog

**Status: PARTIAL — sections with a `Verified:` date are live-verified; the rest are not.** This file is filled by the Phase 2 discovery routine (`browser-rules.md` → _Discovery scraping_) by reading the live sub-account UI. Until a section carries a `Verified: <date> | sub-account: <name>` line, it must not be used to design or build anything.

Trust rule: entries never expire on a timer. Re-verify a section only when it fails in use (fix on the spot, re-stamp, note in the build log) or when the user runs `/create-ghl-snapshot reverify <section>`.

**Mismatch rule:** if the live UI ever differs from an entry here — renamed, moved, new, or gone (including defaults missing from blank sub-accounts) — update the entry immediately, whether or not it blocked you: changed → re-stamp + `(changed <date>: was "<old>")`; removed → keep the line, mark `REMOVED <date> — <where you looked>`; new → add, stamped. Log it in the phase log under "Reference updates".

Section names below are the `<area>` keys the reverify mode accepts.

---

## workflow-triggers

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (full list scrolled in Add trigger → Triggers tab; "Apps" tab not yet scraped)

Picker: Add new trigger → "Add trigger" panel (search box, Triggers / Apps tabs, expand-to-modal icon). Each trigger opens a config: "CHOOSE A WORKFLOW TRIGGER" dropdown, "WORKFLOW TRIGGER NAME", "+ Add filters", Cancel / Save trigger. A workflow can hold several triggers.

- **Contact** › Birthday reminder · Contact changed · Contact created · Contact DND · Contact tag · Custom date reminder · Note added · Note changed · Task added · Task reminder · Task completed · Contact engagement score
- **Events** › Inbound webhook (premium/crown icon) · Scheduler · **Call details** · Email events · **Customer replied** · Inbound email · Conversation AI trigger · Custom trigger (greyed out) · **Form submitted** · **Survey submitted** · Trigger link clicked · Facebook lead form submitted · TikTok form submitted · Video tracking · Number validation · Messaging error - SMS · LinkedIn lead form submitted · Funnel/website pageview · Quiz submitted · Prospect generated · Click to WhatsApp ads · External tracking event · User replied · AI studio form submitted · **New review received** · Video testimonial received
- **Appointments** › **Appointment status** · **Customer booked appointment** · Service booking · Rental booking
- **Opportunities** › Opportunity status changed · Opportunity created · Opportunity changed · **Pipeline stage changed** · Stale opportunities
- **Affiliate** › Affiliate created · New affiliate sales · Affiliate enrolled in campaign · Lead created
- **Courses** › Category started · Category completed · Lesson started · Lesson completed · New signup · Offer access granted · Offer access removed · Product access granted · Product access removed · Product started · Product completed
- **Payments** › Invoice · Payment received · Order form submission · Order submitted · Documents & contracts · **Estimates** · Subscription · Refund · Coupon code applied · Coupon redemption limit reached · Coupon code expired · Coupon code redeemed
- **Ecommerce stores** › Shopify abandoned cart (deprecating soon) · Shopify order placed · Shopify order fulfilled (deprecating soon) · Order fulfilled · Product review submitted · Abandoned checkout · Add products to cart · Product viewed
- **IVR** › Start IVR trigger
- **Facebook/Instagram events** › Facebook - Comment(s) on a post (greyed) · Instagram - Comment(s) on a post (greyed)
- **Communities** › Private channel access revoked · Group access revoked · Private channel access granted · Group access granted · Community group member leaderboard level c… · Group joining request rejected · Member registered for group event · Group comment created · Group post created · Requested to join group
- **Certificates** › Badges issued · Certificates issued
- **Communication** › TikTok - comment(s) on a video · Transcript generated · Conversations SLA
- **Google ads** › Google lead form submitted
- **Client portal** › Client portal file uploaded · User login (cp/memberships)
- **Events management** › Event checked in (greyed) · Event registration (greyed)

Filter details verified:
- **Appointment status** — "Fires on status change (booked, cancelled, no-show)." Who to enroll: Contact only / Contact and guests / Guests only. Default filter Event type = Normal. Filters: **Appointment status is** (new · confirmed · cancelled · Showed · No-show · invalid) · Created by/modified by · Has tag · **In calendar** · custom fields.
- **Opportunity status changed** — "Triggers when an opportunity's status is updated." Filters: Assigned to · Expected close date · Forecast probability · **In pipeline** · Lead value · Lost reason · Moved from status · **Moved to status** · Tag · opportunity/contact custom fields.
- **Call details** — "Fires after a call ends with the selected status." Filters (Standard fields): Call direction · Call status · Custom disposition · In workflow; plus Custom field (none yet). **Call status values: busy · canceled · completed · no-answer · voicemail** (multi-select). → There is NO separate "missed call" trigger; missed call = Call details + Call status ∈ {no-answer, busy, voicemail, canceled} (+ Call direction = inbound — values not yet opened).

## workflow-actions

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (Actions tab scrolled end to end; tail of "Communities" skipped — not relevant; "Apps" tab not scraped)

Picker: "+" on the canvas line → "Actions" panel (search, Actions / Apps tabs). 👑 = premium (crown icon); (greyed) = shown but disabled in this account.

- **Contact** › Create contact · Find contact · Update contact field · **Add contact tag** · **Remove contact tag** · **Assign to user** · Remove assigned user · **Enable/disable DND** · Add to notes · Copy contact 👑 · Edit conversation · **Add task** · Delete contact · Modify contact engagement score · Add contact followers · Remove contact followers · Merge contact · Email verification
- **Custom objects** › Create Associated Record for Contact · Update Associated Record for Contact · Clear fields of Associated Record for Contact
- **Communication** › **Send email** · **Send SMS** · Slack 👑 · Call · Voicemail · Messenger · Instagram DM · Manual action to SMS · Manual action to call · GMB messaging (greyed) · **Send internal notification** · **Send review request** · Conversation AI · Facebook interactive messenger (greyed) · Instagram interactive messenger (greyed) · Reply in comments (greyed) · Whatsapp: customer service window check · Whatsapp: send flows · Send live chat message · Appointment booking conversation AI bot · Update conversation AI bot and status · Book appointment · Log external call · WhatsApp · WhatsApp media · WhatsApp interactive messages · TikTok interactive messenger · RCS interactive message (greyed, BETA) · Send RCS (greyed, BETA) · Add internal comments (BETA)
- **Send data** › Webhook · Custom webhook 👑 · Google Sheets 👑
- **Internal** › **If / else** · **Wait** · Goal event · Split · **Update custom value** · **Go to** · Date/time formatter · Number formatter · Math operation · Set event start date · **Add to workflow** · **Remove from workflow** · Array formatter · Drip · Text formatter · Custom code 👑
- **Workflow AI** › AI Agent (BETA 👑)
- **External AI models** › GPT powered by OpenAI 👑 · AI image generation 👑
- **Eliza** › Eliza AI appointment booking · Send to Eliza agent platform
- **Appointments** › **Update appointment status** · Create appointment / booking note · **Generate one time booking link**
- **Opportunity** › **Create/update opportunity** · Remove opportunity · Add owner to opportunity · Remove owner from opportunity · Add follower(s) to opportunity · Remove follower(s) from opportunity · Find opportunity · Create opportunity · Update opportunity
- **Payments** › Stripe one time charge · Send invoice · **Send estimate** · Send documents & contracts · Send recurring invoice · Update inventory
- **Marketing** › Add to Google Analytics · Add to Google Ads · Facebook - Add to custom audience · Facebook - Remove from custom audience · Meta conversion API · Generate marketing audit report
- **Affiliate** › Add to affiliate manager · Update affiliate · Add to affiliate campaign · Remove from affiliate campaign · Add leads under an affiliate · Add manual sales for an affiliate
- **Membership** › Course grant offer · Course revoke offer · Grant course access (greyed) · Revoke course access (greyed)
- **IVR** (all greyed) › Gather input on call · SAY OR PLAY MESSAGE · Connect to call · End call · Record voicemail
- **Certificates** › Issue certificate · Issue badge
- **Agent Studio** › Invoke flow agents · Invoke managed agents
- **AI Actions** (all 👑) › AI translate · AI summarize · AI intent detection · AI decision maker · AI extract data (BETA) · AI analyze image
- **Associations** › Remove associated records from workflow · Add associated records to workflow · Associate records (BETA)
- **Communities** › Smart push notification · Grant group access · Revoke group access · Grant private channel access · … (rest not scraped)
- **Conversation AI** (all greyed) › AI capture information · Book appointment · End conversation · AI splitter · AI message · Custom message · Transfer bot · Continue conversation · Services booking
- **Voice AI** › Voice AI outbound call (BETA)

Design notes: the core toolkit a plumbing snapshot needs is all present and not premium — Send SMS / Send email / Send internal notification / Send review request / Add task / If-else / Wait / Go to / Add-Remove from workflow / tags / DND / Create-update opportunity / Update appointment status / Generate one time booking link. Premium (👑) and greyed items must not be used in the design.

## workflow-conditions

Verified: 2026-09-21 (Wait types + goto presence; If/else operator list not yet opened) | sub-account: Home Service - Plumbing Master Template

- **Wait** ("Holds a contact for a specific time, until a condition exists, or until the contact replies") — "How long do you want to wait?" types:
  1. For a set period of time (e.g., 2 days, 6 hours, 30 minutes)
  2. Until a specific date/time
  3. Until a recurring window opens (New) (e.g., every Tuesday, 15th of each month)
  4. Until a scheduled date/time (e.g., 1 hour before scheduled appointment)
  5. **Until the contact replies** — options: "Reply to" (select a step) + **Timeout** toggle
  6. Until a user replies (a user sends a message to the contact)
  7. Until the contact takes a specific action (e.g., clicks a link, opens an email)
  8. Until specific conditions are met (custom segment on any fields)
  Also "Set up with AI" link.
- **Go to** exists (Internal › Go to). **If / else**, **Split**, **Goal event**, **Drip** exist (Internal).
- **If / else** (verified 2026-09-21): Action name · Scenario recipe (pre-built templates or "Build your own") · **multiple branches** ("+ Add branch", "Reorder branches") each with condition segments joined AND/OR ("+ Add segment") · a **None branch** ("When no condition is met").
  - Condition groups: Contact details › (Standard fields · Custom Fields · Contact First Attribution · Contact Last Attribution) · Company › · **Date/Time** › (Current Day of week · Current Day of month · Current month · Current year · **Current hour** · Time of the day) · Workflow trigger · Workflow contact · Events › (Email event · Trigger link clicked — enabled only with matching steps) · Custom values · **Contact reply** › (Contact replied · **Replied message** · Intent type — enabled only after a "Wait until the contact replies" step).
  - Contact standard fields usable: Last appointment at · Full name · First/Last name · Email · Phone · Company name · Full address · Address 1 · Assigned user · City · State · Country · Time zone · Postal code · Date of birth · Source · Website · **Tags** · Contact type · **DND enabled channels** · Valid WhatsApp · Valid email.
  - Operators for Replied message: Is · Is not · **Contains** · Does not contain · **Is any of (comma separated)** · Is none of (comma separated) · Is not empty · Is empty.
- **Wait "Until the contact replies"** creates two canvas branches: **Contact reply** and **Time out** ("What will happen after N minutes").
- **Send SMS** action: Action name · Templates ("Select template") · Message (merge-field tag, AI) · inline body allowed.
- **Send internal notification**: Type of notification = Email · Notification · SMS · WhatsApp; To user type = **All Users · Assigned owners · Custom Number · Particular User**; Notify followers (Contact followers / Opportunity followers); Templates; Message.
- Discarding an unsaved trigger/action shows GHL's in-app "Unsaved changes — discard?" modal (Cancel / Confirm) — not a browser dialog.

## workflow-settings

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template | Path: workflow → "Settings" tab (next to Builder / Enrollment history / Execution logs)

- **Contact**: Allow re-entry (toggle; default ON on a new workflow — "If the Contact attempts to re-enter while it is still enrolled… it will get skipped"; appointment/invoice triggers re-enter even if disabled) · Allow multiple opportunities (default ON) · **Stop on response** (default OFF — "Ends workflow for a contact if the contact responds to a message that is sent from this workflow")
- **Communication**: Timezone (default "Account timezone"; wait steps and time windows follow it) · **Time window → Specific time** (default OFF — "Restrict actions from being sent outside the window you define") · Sender details: From name / From email (defaults, overridable per email action) · **From number** (dropdown "Select from number" — empty in this sub-account: no phone number yet) · Conversations: Mark as read (default OFF)
- Builder top bar: "Standard builder" selector, Test workflow, Draft/Publish toggle, Saved indicator. First open showed an "Introducing auto save" modal (chose Continue with Manual Save).

## form-fields

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Sites → Forms (URL `/form-builder/main`) → "Create form" (creates "Form 0" immediately and opens builder `/form-builder-v2/<formId>`). Blank account: no forms (marketing empty state with "Create form" / "Try form preview").
- Builder tabs: Edit · Settings · Submissions · Notifications · Analytics; top: Back · form name (pencil) · Preview · Integrate · Save; desktop/mobile toggles.
- **New form default content:** First Name · Last Name · Phone* · Email* · **two A2P consent checkboxes pre-filled** — (1) non-marketing texts from [BUSINESS NAME] about [USE_CASE_FROM_CAMPAIGN_DESCRIPTION], "Message frequency varies… Text HELP… reply STOP to opt out"; (2) marketing & promotional messages from [BUSINESS NAME]… — then Submit button, then "Privacy Policy | Terms of Service" links. Placeholders in [BRACKETS] must be replaced.
- **Form Element palette** — tabs "Quick Add" and "Add Object Fields" (existing custom fields):
  - Personal Info: Full Name · First Name · Last Name · Date of birth · Phone · Email
  - Submit: Submit
  - Payments: Sell Products · Collect Payment
  - Address: Address (Updated) · City · State · Country · Postal Code · Organization · Website
  - Text: Single Line · Multi Line · Text Box List
  - Choice Elements: Single Dropdown · Multi Dropdown · Checkbox · Radio
  - Rating: Rating (New)
  - Customized: Text · Html · Bot protection · Source · T & C · Score
  - **Address (Updated) element bundles sub-fields:** one element renders Street Address + City + State + Country + Postal Code, each toggleable via checkboxes in the right panel (Address Fields), plus an **Auto-Complete Address** toggle (on by default; "Mandatory to select address in search bar" off). So a separate Postal Code element is NOT needed when using Address (Updated) — uncheck Country for US-only forms. (verified 2026-09-21)
  - Other Elements: Image · File Upload · Monetary · Number · Date Picker · Signature
- **Settings → On Submit:** Redirect to URL · Message (rich text, default "We appreciate your feedback!") · Order Confirmation; plus Message Styling.

## survey-questions

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Sites → Surveys (`/survey-builder/main`) → "Create survey" (creates "Survey 0" immediately; builder `/survey-builder-v2/<id>`). Blank account: no surveys (empty state + industry templates list with "Use template").
- Structure: **slides** ("Slide 1" … "+ Add Slide"); each slide has Settings (gear) · duplicate · delete; slide footer "Submit".
- **Slide Settings → General Settings:** Slide Name · Slide Position · **Jump To** (dropdown — slide-level branching/skip) · Image Layout Settings (Survey Default / Independent).
- Element palette: same "Quick Add" / "Add Object Fields" families as forms (Personal Info, Payments, Address, …). Per-question conditional logic not yet inspected.
- Marketing copy on the Surveys page: "Multi & one-question-at-a-time surveys"; workflows can react to completed **and abandoned** surveys.

## calendars

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (types only; per-calendar options not yet opened)
Path: Settings → Calendars (tabs: Meetings · Services (New) · Rentals (New) · Connections; sub-tabs Calendars · Preferences · My availability; left: Groups + "New group")
- "New calendar" → "Choose calendar type": **Personal booking** (one-on-one with a specific team member) · **Round robin** (distributes among team members in rotation) · Class booking (one host, many participants) · Collective booking (multiple hosts, one participant) · "Explore more types" → Event calendar ("Important: this calendar type has key differences"; physical events, no host).
- Blank account: 0 calendars, no default calendar.

## custom-fields

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Custom Fields (object tabs All / Contact / Opportunity / Business; Fields / Folders; Create folder; + Create field)
- Create custom field drawer: **Field type** · **Add to object** (Contact / Opportunity…) · Field name · Folder name (required) · Key · Description · default value / Placeholder text · live preview.
- **Field types:** Single line · Multi line · Text box list · Number · Phone · Monetary · Dropdown (single) · Dropdown (multiple) · Radio select · Checkbox · File upload · Date picker · Signature.
- Merge keys: `{{contact.<key>}}`, `{{opportunity.<key>}}`, `{{business.<key>}}`. **Key auto-generates from the field name (snake_case)** — verified 2026-09-21 (e.g. "How Did You Hear About Us" → `how_did_you_hear_about_us`).
- **Folder is required and must already exist** — the create-field drawer's Folder dropdown only lists existing folders; make the folder first via "Create folder" (modal: Select object + Folder name). (verified 2026-09-21)
- **Dropdown (single/multiple)** shows a "Dropdown options" table (+ Add option); **Radio select** shows a "Radio options" table with an extra "Upload icon" column and an "Allow customized option" toggle; the label input is in a narrow left column. Options list scrolls inside the drawer after ~6–7 rows. (verified 2026-09-21)

## custom-values

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Custom Values (tabs All values / Folders; "Add folder"; "+ Add custom value"; columns Name · Folder · Key · Value). Blank account: none.
- Add custom value modal: **Name*** · Value (optional) · Folder (optional dropdown) · Cancel / Create. Add folder modal: Name* (max 100) · Cancel / Create. (verified 2026-09-21)
- **Key is auto-generated from the name** as `{{custom_values.<snake_case_name>}}` — e.g. "Privacy Policy URL" → `{{custom_values.privacy_policy_url}}`, "Emergency Service Line" → `{{custom_values.emergency_service_line}}`. Choose names with the key in mind. List sorts A→Z by name. (changed 2026-09-21: was "Key syntax not yet seen")

## location-merge-fields

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Picker: tag icon "Custom Values & Trigger Links" in any message editor (verified in Marketing → Snippets → New Snippet → Add Text Snippet). Root groups: Account · Appointment · Attribution · Calendar · Contact · Company · Message · Right now · User · Invoice · Documents & Contracts · Campaign · **Custom Values** · Custom Fields · **Trigger Links**.
- **Account** (= this location/business): Name `{{location.name}}` · Full Address `{{location.full_address}}` · Address Line 1 · City · State · Country · Postal Code · Email `{{location.email}}` · Phone `{{location.phone}}` · Website `{{location.website}}` · Logo URL `{{location.logo_url}}` · Owner › · ID  (keys in backticks were inserted and read back; the rest follow the same `location.` prefix but are unconfirmed)
- **Appointment**: Start Date Time `{{appointment.start_time}}` · Start Date · Start Time · End Date Time · End Date · End Time · Timezone · Cancellation Link · Reschedule Link `{{appointment.reschedule_link}}` · Meeting Location · Notes · Add to Google Calendar · Add to iCal/Outlook · Assigned User ›
- No "review link" merge field in the picker → review asks use the "Send review request" workflow action (or a custom value holding the review URL).
- Text snippet editor shows approximate SMS cost, character/segment count, attachments, "Test Snippet" send-to-phone.

## pipelines

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Opportunities & Pipelines → "Pipelines" tab → "+ Create pipeline". Blank account: **no pipelines** (no default).
- "Opportunities & Pipelines" tab: Allow different owners of Contacts and their Opportunities (ON) + auto-follower checkboxes (both ON).
- Built-in opportunity fields: see defaults-first.md.

## templates

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
- Marketing tabs: Social Planner · Emails · **Snippets** · Countdown Timers · **Trigger Links** · Affiliate Manager · Brand Boards · Ad Manager · Prospecting.
- **Snippets** (Marketing → Snippets, URL `/marketing/templates`): "New Folder", "+ New Snippet" → **Add Text Snippet** / **Add Email Snippet**. Blank account: none. Text snippet = Name + body (emoji, merge-field picker) + attachment / file URL + test send.
- Email templates: Marketing → Emails (builder not yet opened).

## funnels-sites

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (list pages only; builder not yet opened)
Path: Sites (tabs: Funnels · Websites · Stores · Webinars · Analytics · Blogs · WordPress · Client Portal ▾ · Forms · Surveys · Quizzes · Chat Widget · QR Codes · ⚙). Funnels: "+ New funnel" (0 funnels). Websites: "+ New website" (0 websites).

## dashboard

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (default dashboard only; widget picker not opened)
Path: left nav Dashboard. Top: "Dashboard ▾" selector · "+ New" · date range (Last 30 days) · AI icon · "Edit dashboard" · ⋮. Default widgets seen: Opportunity status · Opportunity value · Conversion rate (all with pipeline filter) · Funnel · Stage distribution (more below fold).

## phone

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Phone System (tabs: Phone numbers · Regulatory Bundles · Messaging · Voice · Trust Center · Additional Settings)
- Phone numbers: **none** ("No Data"); "+ Add Number"; sub-tabs Phone Numbers · Number Pools · Verified Caller IDs · Port-In Numbers.
- Voice sub-tabs: Call Recording & Transcription · **Voicemail & Missed Call TextBack** · Call Scripts · Custom Dispositions · VoIP deskphone (SIP) · Other Settings.
  - Voicemail: Incoming Call Timeout slider (No Timeout / 20 / 40 / 60 sec; GHL recommends ≤20 s) · Voicemail Audio upload · Save Call Settings. Banner suggests Voice AI Agents.
  - **Missed Call Text Back: "Enable Missed call textback" checkbox — CHECKED by default in this blank sub-account**, message box empty with "Customize". Also "Enable Missed call WhatsApp back" (needs WhatsApp subscription).
  - ⚠ Design implication: native text-back + a Call-details workflow would double-text. Plan must pick one (and whether snapshots carry this setting is unverified).

## a2p

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Phone System → **Trust Center** tab → card "A2P Messaging (SMS) — Required registration to send business text messages through U.S. carriers" → buttons **Start Registration** · **Brand & Campaigns**. Other cards: SHAKEN/STIR (Voice) · CNAM (Voice) · Voice Integrity. Form fields not opened (owner does this — handoff in onboarding).

## email

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: Settings → Email Services (tabs: Email services · Reply & Forward Settings · Email Analytics · Risk Assessment · Bounce Classification · Postmaster Tools · Advanced Settings)
- Currently on **shared domain `mg.msgsndr.net`** ("You're using a shared domain… Add a dedicated domain") → email sending works out of the box; "Dedicated Domain And IP" button for owner setup.

## integrations

Verified: —

<!-- Google (calendar, GBP), Facebook/Instagram, Stripe entry points -->

## reputation

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
Path: left nav Reputation (tabs: Overview · Requests · Reviews · Video Testimonials (New) · Widgets · Listings · GBP Optimization · Settings). Overview "Get Started" 0/6: Connect Google Business Profile · Setup Review Link · Configure Reviews AI · Create a Review Widget · Send your 1st Review Request · Connect more platforms. "Send Review Request" button top right.
- Settings menu: Reviews AI · **Review Link** · **SMS Requests** · Email Requests · WhatsApp Requests · Reviews QR · Spam Reviews · Integrations.
- **Reviews AI: "Auto Responses" is SELECTED by default** (auto-replies to reviews; options Suggestive / Off; wait time before responding) — owner decision, flag in onboarding.
- **SMS Review Requests: toggle OFF**; "When to send SMS after check-in?" (Immediately…), "Until clicked, repeat this every" (Don't Repeat…), "Maximum retries"; Default SMS sender number (none); "Manage Your SMS Templates" (none; Set SMS Templates / Create New).

## conversation-ai

Verified: —

<!-- Presence, modes, where configured (record only; v1 may or may not use it) -->

## other

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
- Left nav: Ask AI · Launchpad · Dashboard · Conversations · Calendars · Contacts · Opportunities · Payments · AI Agents · Marketing · Automation · Sites · Memberships · Media Storage · Reputation · Reporting · App Marketplace · Settings.
- Contacts: 0 contacts; tabs Smart Lists · Bulk Actions · Custom Fields · Tasks · Companies; Import / + Add Contact.
- Trigger Links (Marketing → Trigger Links): none; "+ Add Link"; tabs Link / Analyze.
- Settings nav: Business Profile · Billing · Users · Opportunities & Pipelines · Calendars · Email Services · Phone System · WhatsApp · Objects · Custom Fields · Custom Values · Import Data · Manage Scoring · Domains & URL Redirects · External Tracking · Integrations · Private Integrations · Tags · Labs · Audit Logs · Brand Boards.
- Most sub-account pages render inside cross-origin iframes (`client-app-*.leadconnectorhq.com`): DOM/accessibility reads return nothing — work by screenshot + coordinates. Chrome stops painting the window if it is hidden/minimized → screenshots time out; the user must keep the automation window visible.


## funnels-ai-builder (verified 2026-09-21 · sub-account: Home Service - Plumbing Master Template)
"+ New funnel" → "Build with AI" (Beta) → Continue → wizard step 1 "What industry does your company serve?": **Name of the business*** (free text) + **Select industry*** (dropdown; default "Home Builder"). Banner: **"5/5 free funnel generations remaining. You'll be charged $1.04 after 5 free generations."** → so first 5 AI funnel generations are FREE per sub-account, $1.04 each after. Next/Previous buttons.
NOTE (literals risk): AI-generated pages bake the entered business name into copy as literal text → must be cleaned to merge fields ({{location.*}}/custom values) per principle 11 after generation.

### Navigation Menu element — logo (verified 2026-09-21, page-builder)
The AI-generated header is one **Navigation Menu** element (breadcrumb: Header › 2 Column Row › 1st Column › Navigation Menu). Panel = General (Menu Items + reorder/edit each; Image Actions = logo click behavior: Go to website Url / Website / Open in New Tab; Typography) + Styles. **No logo image-source/upload field** exposed (checked General, element gear toolbar, double-click drill-in). Placeholder shows "LOGO HERE" on a blank account. Practical rule: don't try to set {{location.logo_url}} on the nav logo — treat as onboarding (owner uploads logo in Settings → Business Profile). Re-check whether it auto-resolves from account logo on a later account that has a logo uploaded.

### FAQ widget — editing collapsed items (verified 2026-09-21, user-shown)
The AI "FAQ" element is an accordion widget. To edit a collapsed item: **select the FAQ element → right panel General tab shows "FAQ List" with "List Item 1/2/3" rows (each has +, duplicate, delete icons) → click a List Item row and that item's inner content (question + answer) reveals/expands in the canvas**, where you double-click to edit Q or A inline. Panel also has: FAQ Type (Separated/Contained/Simple), Show Image + IMAGE src, Icon (Close) = chevron-down. Delete an item via the trash icon on its List Item row. NOTE: the AI generated the 6-item FAQ as TWO separate FAQ widgets (one per column, 3 items each) — that's how a duplicate question can appear across columns.

### Page-builder is ONE cross-origin iframe + dropdown quirk (verified 2026-09-21)
- The ENTIRE funnel/page builder (canvas AND right-side properties panel) loads inside a single cross-origin iframe. So `read_page`/`find`/`form_input`/`get_page_text`/`javascript_tool` CANNOT reach any builder element — everything is coordinate-clicks + screenshots only.
- **Dropdown selection quirk:** coordinate-clicking an option in a builder dropdown frequently does NOT register (value reverts). RELIABLE method: click the dropdown to open it (a first click may be swallowed — click again and screenshot to confirm open), then use KEYBOARD (Down/Up arrows to the option + Return). Typing does NOT filter these dropdowns.
- **Button element ▸ Button Actions ▸ "link to"** options: Open popup, Website URL, Download File, Hide & Show Elements, Scroll to Element, Step, Next step, One click up/down sell product, Call, SMS, Email address, Membership, Add to cart, Buy now, Collections.
- **"Scroll to Element" target picker** lists EVERY element on the page by its generic name (Section / 1 Column Row / 1st Column / Headline / Paragraph / Image / Button / Form …) in ~document order. With many repeated generic names it's very hard to reliably identify a specific target (e.g. the Form) via automation. Practical tip for next time: first RENAME the target element to something distinctive (Element name field) so it stands out in the picker — then keyboard-nav to it.

### Funnel page-builder — Button element (verified 2026-09-21)
- **Edit button text via the properties panel Text field**, NOT inline. Double-clicking a button + Ctrl+A does not enter its text editor — Ctrl+A selects the whole browser page instead (harmless but does nothing). Reliable: select button → General ▸ Text Options ▸ **Text** field → triple-click → type.
- **"Website URL" button action accepts merge fields / custom values.** Typing `{{custom_values.<key>}}` directly into the Website field works (used for a Google-reviews CTA → `{{custom_values.google_reviews_url}}`). Pair with the **Open in New Tab** toggle for external links.
- Adding a new button to an empty column: hover column → **+ Add** → Quick Add panel → **Button** (under "Form"). New button defaults to link-to = "Open popup" and text "Get Started".

### Funnel page-builder — Popup Settings (verified 2026-09-21)
- Toolbar icon **"Popup Settings"** (in the top element toolbar) opens the popup manager (right panel): lists each popup with a drag handle, a status circle, name, and an edit pencil; a **+ Create New Popup** button; order = z-index priority.
- Popup editor (General tab): **Element name**, **Disable Popup** toggle, **Close popup on clicking outside** toggle, **Width** (e.g. Full Page), **Show popup on** (trigger: e.g. **Exit**), Background, Position, Overlay Color.
- **No delete control** for a popup in this manager/editor — to remove a popup from a published site, set **Disable Popup = ON** (dormant but still present in the snapshot). Opening Popup Settings always renders the popup in the canvas for editing regardless of the disabled state.
- A popup Form element with no form assigned shows **"You do not have any forms yet"** in the builder and **"Unable to find form"** (red) on the live/preview site.

## Funnel: adding a step + per-step AI builder (verified 2026-09-21; corrected 2026-09-21)
**The per-step / per-page AI builder IS available — but it is GATED behind an Agency Labs feature that must be activated PER SUB-ACCOUNT.** Until that feature is on for the target sub-account, the add-step flow is manual only (that is what was seen the first time and wrongly recorded as "no per-step AI").

- **With the Labs feature OFF (default on a new sub-account):** adding a step to an existing funnel offers only:
  1. Funnel steps ▸ "Add new step or import" → modal "New step in funnel" = **Name for page** (req) + **Path** + **Import from ClickFunnels URL** (Beta). Create → makes an empty step. No AI option in this modal.
  2. The new empty step's CONTROL box shows **"Use existing"** and **"Create from blank"** only.
  3. "Create from blank" → page-builder on a blank canvas, manual tools only (Quick Add, Sections, Rows, Elements, Prebuilt Sections, etc.). Toolbar icons: Add, Background, Popup, Cookie Consent — none is AI.
- **With the Labs feature ON:** the per-step/page AI builder becomes available so a single new step/page can be AI-generated (no need to hand-build from blank). ← **enable this before building funnels** (see enable path below and pre-flight check in `phases/03-build.md`).
- **Enable path (AGENCY view — the assistant must NOT do this itself; it is a `🖐 NEEDS YOU` handoff, per principle 9 / browser-rules "never touch agency-level settings"):**
  Agency view → **Settings → Labs → Sub-Accounts tab** → click the **search bar** → **Activate feature** → find the target sub-account in the list → **enable the setting** for it.
  - **Exact feature name (confirmed 2026-09-21): "Brand New Funnel AI & Website AI".** This is the toggle that turns on both the per-step/page AI builder (in-builder "Ask AI" → Build panel) and the AI website builder for that sub-account.
- Separately, **whole-funnel AI generation** still exists at NEW-FUNNEL creation ("+ New funnel → Build with AI"); that path produced FN1 with fabricated reviews/popup/dead-links, so whatever AI path is used, reconcile against the plan and strip literals/extras (principle 11) after generation.
