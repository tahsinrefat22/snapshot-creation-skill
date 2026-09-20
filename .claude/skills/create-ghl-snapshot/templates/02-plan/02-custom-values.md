# Custom values — <business>

Defaults-first: every row below first checked `reference/defaults-first.md` → `location-merge-fields` (stamped). If a location merge field covers it, the row says so and **no custom value is created**.

## A. Business-identity values (all `owner-must-set`)

| Need                               | Location merge field? (verified) | Custom value name (if needed) | Example value | Used by (pages / templates / workflows) | Owner-guide label |
| ---------------------------------- | -------------------------------- | ----------------------------- | ------------- | --------------------------------------- | ----------------- |
| Business name                      | `{{location.name}}` ✔            | —                             |               |                                         |                   |
| Phone                              |                                  |                               |               |                                         |                   |
| Address                            |                                  |                               |               |                                         |                   |
| Email                              |                                  |                               |               |                                         |                   |
| Website                            |                                  |                               |               |                                         |                   |
| Logo                               |                                  |                               |               |                                         |                   |
| Tagline                            | —                                | `cv_tagline`                  |               |                                         | "Your tagline"    |
| Service area text                  |                                  |                               |               |                                         |                   |
| Hours text                         |                                  |                               |               |                                         |                   |
| Licence / cert #                   |                                  |                               |               |                                         |                   |
| Review 1 quote / name / rating     |                                  |                               |               |                                         |                   |
| Social links                       |                                  |                               |               |                                         |                   |
| Booking / cancellation policy text |                                  |                               |               |                                         |                   |
| Guarantee / warranty text          |                                  |                               |               |                                         |                   |
| Brand colours                      |                                  |                               |               |                                         |                   |

## B. Operational values

| Name                     | Purpose | Default value shipped | Used by | Defaults-first justification |
| ------------------------ | ------- | --------------------- | ------- | ---------------------------- |
| `cv_after_hours_message` |         |                       |         |                              |
| `cv_response_sla_text`   |         |                       |         |                              |

## Ledger rows

<!-- copy to LEDGER.md: type custom-value · origin mine · status planned · flags owner-must-set where applicable -->
