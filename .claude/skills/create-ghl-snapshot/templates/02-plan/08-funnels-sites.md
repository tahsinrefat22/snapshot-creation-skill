# Funnels and sites — <business>

## How many, and why

| Funnel | Entry intent | Customer types | Why it's separate |
| ------ | ------------ | -------------- | ----------------- |

Builder elements and embed methods from `ghl-capabilities.md` → `funnels-sites` (stamped). A page with a literal business name, phone, address, logo, or review is a defect — every such spot is in the variable map.

---

## Funnel: <name>

Flow:

```
Step 1 <landing> → Step 2 <qualify> → Step 3 <book> → Step 4 <thank you>
                 ↘ (Q = emergency) → Step 3
```

### Step 1 — <page title>

Purpose: … · The one action the visitor should take: …

| Section (top → bottom) | Content / copy (variables inline)                                                                                                                                      | Element type (verified) |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| Header                 | logo `{{location.logo_url}}` · phone `{{location.phone}}` click-to-call                                                                                                |                         |
| Hero                   | headline: … · subhead: …                                                                                                                                               |                         |
| Trust                  | reviews: `{{custom_values.cv_review_1_quote}}` — `{{custom_values.cv_review_1_name}}` ★`{{custom_values.cv_review_1_rating}}` · licence `{{custom_values.cv_license}}` |                         |
| Form / calendar embed  | <form name>                                                                                                                                                            |                         |
| CTA                    | …                                                                                                                                                                      |                         |
| Footer                 | `{{location.name}}` · `{{location.full_address}}` · social `{{custom_values.cv_social_…}}`                                                                             |                         |

On submit / book → next step: … · workflow fires: … · tag: … · stage: …
Tracking notes: …
Variable map: | Spot | Variable |

### Step 2 — …

---

## Site: <name> (only if the business needs one beyond funnels)

### Page — <title>

(same section table + variable map + links to funnels)

## Domain

Ships unconnected; the owner connects their domain in onboarding (topic 11).

## Ledger rows

<!-- type funnel + one row per funnel-step; site + site-page -->
