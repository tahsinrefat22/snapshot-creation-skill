# GHL capabilities — LIVE-VERIFIED catalog

**Status: EMPTY. Nothing below is verified.** This file is filled by the Phase 2 discovery routine (`browser-rules.md` → _Discovery scraping_) by reading the live sub-account UI. Until a section carries a `Verified: <date> | sub-account: <name>` line, it must not be used to design or build anything.

Trust rule: entries never expire on a timer. Re-verify a section only when it fails in use (fix on the spot, re-stamp, note in the build log) or when the user runs `/create-ghl-snapshot reverify <section>`.

Section names below are the `<area>` keys the reverify mode accepts.

---

## workflow-triggers

Verified: —

<!-- One line per trigger: `Category › Trigger name` — available filters — notes (gated/labs/greyed) -->

## workflow-actions

Verified: —

<!-- One line per action: `Category › Action name` — key options seen — notes -->

## workflow-conditions

Verified: —

<!-- If/else operators, filter fields, wait types (duration / until / condition / event), goto presence -->

## workflow-settings

Verified: —

<!-- Re-entry options, timezone, sender defaults, stop-on-reply, mark-as-read, etc. -->

## form-fields

Verified: —

<!-- Field types in the form builder palette; custom-field mapping presence -->

## survey-questions

Verified: —

<!-- Question types; conditional logic (yes/no + how); scoring; page/slide structure -->

## calendars

Verified: —

<!-- Calendar types offered on create; availability, team, buffer, form (default vs custom), confirmation, reminder options -->

## custom-fields

Verified: —

<!-- Field types; folder/grouping; object (contact/opportunity) support -->

## custom-values

Verified: —

<!-- Creation UI; merge-field syntax shown; folders -->

## location-merge-fields

Verified: —

<!-- Every `{{location.*}}` / `{{user.*}}` / `{{contact.*}}` merge field the picker lists. This seeds defaults-first.md. -->

## pipelines

Verified: —

<!-- Stage options; opportunity fields; automation hooks visible -->

## templates

Verified: —

<!-- Email builder / SMS template presence; merge-field picker; snippet presence -->

## funnels-sites

Verified: —

<!-- Builder element types; form/calendar embed method; step settings (on-submit target); domain settings location -->

## dashboard

Verified: —

<!-- Widget types; data sources per widget; edit permissions -->

## phone

Verified: —

<!-- LC Phone presence; number purchase flow entry point; missed-call text-back setting if it exists as a setting -->

## a2p

Verified: —

<!-- Where A2P 10DLC registration lives; brand/campaign form fields seen; status display -->

## email

Verified: —

<!-- LC Email presence; dedicated domain setup entry point -->

## integrations

Verified: —

<!-- Google (calendar, GBP), Facebook/Instagram, Stripe entry points -->

## reputation

Verified: —

<!-- Review request feature presence; reviews widget presence and data source -->

## conversation-ai

Verified: —

<!-- Presence, modes, where configured (record only; v1 may or may not use it) -->

## other

Verified: —

<!-- Anything else in the left nav worth knowing about: trigger links, media library, snippets, tasks, memberships… -->
