# Defaults first — what GHL already provides at the location level

Before creating **any** custom value, field, tag, calendar, pipeline, or template, check this list. If a default covers the need, use it and write "default: <name>" in the plan. If it doesn't, write the one-line reason.

**Status of the lists below: UNVERIFIED hypotheses.** They are prompts for what to look for during Phase 2 discovery (`ghl-capabilities.md` → `location-merge-fields`, `business-profile`, and the blank-account walk). Replace each `?` with what the merge-field picker / UI actually shows, and stamp the section. Never build against an unstamped line.

**Mismatch rule:** if the live UI ever differs from an entry here — renamed, moved, new, or gone (including defaults missing from blank sub-accounts) — update the entry immediately, whether or not it blocked you: changed → re-stamp + `(changed <date>: was "<old>")`; removed → keep the line, mark `REMOVED <date> — <where you looked>`; new → add, stamped. Log it in the phase log under "Reference updates". **This file changes only on evidence from a blank sub-account.** Objects present or missing in a sub-account loaded from a snapshot come from that snapshot; they belong in the build's baseline inventory, not here.

## Location-level merge fields

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (see ghl-capabilities.md → location-merge-fields)
| Need | Actual (from picker) |
|---|---|
| Business name | `{{location.name}}` ✔ |
| Business phone | `{{location.phone}}` ✔ |
| Business email | `{{location.email}}` ✔ |
| Business address | `{{location.full_address}}` ✔ (+ Address Line 1 / City / State / Country / Postal Code) |
| Website | `{{location.website}}` ✔ |
| Logo | `{{location.logo_url}}` ✔ |
| Business hours | **no merge field and no Business Profile field** → custom value needed |
| Assigned user | Appointment › Assigned User ›, User › (keys unconfirmed) |
| Appointment details | `{{appointment.start_time}}` ✔, `{{appointment.reschedule_link}}` ✔, Cancellation Link, Meeting Location, Timezone |
| Review link | **none in picker** → "Send review request" action, or custom value |

## Built-in contact fields (do not recreate as custom fields)

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template (Settings → Custom Fields → Contact, all locked)
- General Info: Business name `{{contact.company_name}}` · Street address `{{contact.address1}}` · City `{{contact.city}}` · Country `{{contact.country}}` · State `{{contact.state}}` · Postal code `{{contact.postal_code}}` · Website `{{contact.website}}` · Timezone `{{contact.timezone}}`
- Contact: First name `{{contact.first_name}}` · Last name `{{contact.last_name}}` · Email `{{contact.email}}` · Phone `{{contact.phone}}` · Date of birth `{{contact.date_of_birth}}` · Contact source `{{contact.source}}` · Contact type `{{contact.type}}` (dropdown; editable options)
- Built-in opportunity fields: Opportunity name · Pipeline · Stage · Status · Lead value (Monetary `{{opportunity.monetary_value}}`) · Owner · Opportunity source · Lost reason (dropdown; editable) · Forecast expected close date · Forecast probability
- (Custom fields created by a build go in their own folder, e.g. "Plumbing Job Info"; folders must be made before the fields.)
- Adding a **user** to a sub-account auto-creates that user's **Personal calendar** (pre-existing side effect, never delete). A blank sub-account has **0 users**, so calendars can't be built until the owner adds >=1 user (handoff).
- Built-in **business** fields (object "Business", folder Company Info): Company Name `{{business.name}}` · Phone · Email · Website · Address · City · State · Postal Code · Country · Description — these describe a *contact's company* (B2B object), not our location.

## Default objects present in a blank sub-account

Verified: 2026-09-21 | sub-account: Home Service - Plumbing Master Template
- Workflows: none · Custom values: none · Tags: none · Pipelines: none · Calendars: none · Phone numbers: none
- Business Profile (Settings → Business Profile): Location ID shown with copy icon; Friendly Business Name; Legal Business Name; Business Email; Business Phone; Branded Domain; Business Website; Business Niche (currently "Home Builder"); Business Currency (unset); physical address; **Time Zone (currently GMT+06:00 Asia/Dhaka)**; Platform Language; Outbound communication language for custom values; Business Information (type, industry, registration); Authorized Representative; General (incl. **"Make Email compliant by adding an Unsubscribe link" — CHECKED by default**); Contact Deduplication (Allow Duplicate Contact OFF; match by Email, then Phone). **No business-hours field on this page.**

## Sub-account settings that can replace a custom value

Verified: —

<!-- Business hours (settings page), missed-call text-back setting (if LC Phone provides one natively), auto-reply settings, opt-out language settings, appointment reminder defaults on calendars -->

## Rules of thumb (not GHL facts — design guidance)

- If the business info lives in the business profile, use the merge field; the owner fills the profile once during onboarding.
- If GHL has a native setting (e.g., calendar reminders) that does what a planned workflow does, prefer the setting unless the plan needs behaviour the setting can't do — and say which.
- If a custom value would only ever be read by one template, ask whether it's really business-variable or just copy. Copy goes in the template; business-variable data goes in a value.
