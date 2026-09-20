# Missed calls and edge cases — <business>

## Missed calls — per customer type

| Customer type | Text-back (template, within) | If they reply | If they don't (nudges, spacing, stop) | Owner notified how | After hours difference | Handled by (workflow, step) |
| ------------- | ---------------------------- | ------------- | ------------------------------------- | ------------------ | ---------------------- | --------------------------- |

## Edge cases

| Case                                             | What should happen | Handled by (workflow step / setting) | Test scenario id |
| ------------------------------------------------ | ------------------ | ------------------------------------ | ---------------- |
| Duplicate lead (same phone/email submits twice)  |                    |                                      |                  |
| Wrong number / spam                              |                    |                                      |                  |
| Replies STOP / unsubscribes                      |                    |                                      |                  |
| Books, then cancels                              |                    |                                      |                  |
| No-show                                          |                    |                                      |                  |
| Goes cold after respond                          |                    |                                      |                  |
| Goes cold after qualify                          |                    |                                      |                  |
| Goes cold after quote                            |                    |                                      |                  |
| Replies to a message after the workflow moved on |                    |                                      |                  |
| Arrives after hours                              |                    |                                      |                  |
| Existing customer arrives as a "new lead"        |                    |                                      |                  |
| Owner replies manually mid-sequence              |                    |                                      |                  |
