# Defaults first — what GHL already provides at the location level

Before creating **any** custom value, field, tag, calendar, pipeline, or template, check this list. If a default covers the need, use it and write "default: <name>" in the plan. If it doesn't, write the one-line reason.

**Status of the lists below: UNVERIFIED hypotheses.** They are prompts for what to look for during Phase 2 discovery (`ghl-capabilities.md` → `location-merge-fields`, `business-profile`, and the blank-account walk). Replace each `?` with what the merge-field picker / UI actually shows, and stamp the section. Never build against an unstamped line.

## Location-level merge fields

Verified: —
| Need | Hypothesis to check | Actual (from picker) |
|---|---|---|
| Business name | `{{location.name}}` | ? |
| Business phone | `{{location.phone}}` | ? |
| Business email | `{{location.email}}` | ? |
| Business address (full / parts) | `{{location.full_address}}`, `{{location.address}}`, `{{location.city}}`, `{{location.state}}`, `{{location.postal_code}}` | ? |
| Website | `{{location.website}}` | ? |
| Logo | `{{location.logo_url}}` | ? |
| Business hours | (is there a merge field, or only a settings page?) | ? |
| Timezone | `{{location.timezone}}` | ? |
| Assigned user name / phone / email | `{{user.name}}`, `{{user.phone}}`, `{{user.email}}` | ? |
| Contact basics | `{{contact.first_name}}`, `{{contact.phone}}`, `{{contact.email}}`, `{{contact.source}}` | ? |
| Appointment details | `{{appointment.start_time}}`, `{{appointment.title}}`, calendar name, reschedule/cancel links | ? |
| Trigger link / review link | (reputation feature's review link merge field?) | ? |

## Built-in contact fields (do not recreate as custom fields)

Verified: —

<!-- List what the contact record shows by default: name, phone, email, address, source, DND, tags, assigned user, type, timezone, date of birth, … -->

## Default objects present in a blank sub-account

Verified: —

<!-- Default pipeline? Default calendar? Default dashboard widgets? Default email templates? Any default workflows or trigger links? Record names exactly. -->

## Sub-account settings that can replace a custom value

Verified: —

<!-- Business hours (settings page), missed-call text-back setting (if LC Phone provides one natively), auto-reply settings, opt-out language settings, appointment reminder defaults on calendars -->

## Rules of thumb (not GHL facts — design guidance)

- If the business info lives in the business profile, use the merge field; the owner fills the profile once during onboarding.
- If GHL has a native setting (e.g., calendar reminders) that does what a planned workflow does, prefer the setting unless the plan needs behaviour the setting can't do — and say which.
- If a custom value would only ever be read by one template, ask whether it's really business-variable or just copy. Copy goes in the template; business-variable data goes in a value.
