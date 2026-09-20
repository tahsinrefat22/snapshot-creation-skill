# Workflow design doctrine

Read before writing `10-workflows.md` (Phase 2) and before building any workflow (Phase 3).

## 1. Fewest workflows that stay understandable

One workflow per _process_ ("New Lead Handling", "Appointment Lifecycle", "After the Job") with internal branching — not one micro-workflow per message. Split only when a process contains genuinely independent tasks that share nothing but the contact (review requests are independent of appointment reminders; both are independent of lead chasing).

## 2. Simple steps

Prefer the built-in action over a clever chain. Every step has a one-line reason in the plan. A step with no reason is deleted.

## 3. If/else → goto merge

When a branch exists only to vary one or two steps (different message for emergency vs routine, different wait for hours vs after-hours), the branches **rejoin** via goto to a shared continuation. Never duplicate the tail of a workflow across branches. Draw it in the plan:

```
[trigger] → [if emergency?]
   yes → [SMS: emergency text] → [notify owner: call now] → (goto MERGE)
   no  → [SMS: standard text]                              → (goto MERGE)
MERGE: [wait 5 min] → [if replied?] → …
```

Only use goto to merge or to loop deliberately (with an exit condition). Never goto backwards without a counter or a condition that guarantees exit.

## 4. Explicit dependency graph

For every pair of workflows that can touch the same contact in the same process, the plan states exactly one:

- `A → B` — B must not start (or must not act) until A has finished.
- `A ∥ B` — independent; may run in parallel; neither reads or writes anything the other depends on at the same moment.
- `A ⊗ B` — mutually exclusive; entering one removes the contact from the other.

Example (HVAC "Alex" walking in as an emergency caller): `Missed-Call Text-Back → Lead Qualification` (qualification must not message before the text-back is sent), `Lead Qualification ∥ Owner Alert` (unrelated tasks, same contact, fine in parallel), `Lead Chase ⊗ Appointment Lifecycle` (booking removes from chase).

## 5. Ordering is enforced mechanically, not hoped for

Pick per edge from what `ghl-capabilities.md` confirms exists:

- **Completion marker**: A's last step adds a tag (`state:textback-sent`) or sets a field; B's trigger filter requires it.
- **Event-only trigger**: B's trigger is an event only A produces (pipeline stage change, tag added, appointment status change).
- **Remove from workflow**: the step in A or B that makes the other obsolete removes the contact from it.
- **Wait for condition / wait until**: B waits on the marker instead of polling.
- **Re-entry settings**: deliberately allow or forbid re-entry; write the reason.

"They'll probably run in the right order" is not a mechanism. If none of the above can enforce an edge, that is a blocker → escalation ladder.

## 6. Remove contacts from workflows when continuing would harm the customer

Written as explicit steps with the triggering condition:

- Booked → remove from lead chase.
- Replied → remove from the no-reply follow-up sequence (or branch on "replied" and exit).
- Opted out / STOP → remove from every messaging workflow.
- Won / lost → remove from pipeline nurture; set the final stage.
- Cancelled → remove from reminder sequence; enter the re-book sequence.

## 7. Reality check loop

Every trigger, action, condition, and filter in the plan is checked against `ghl-capabilities.md` before Gate 2. Not in the catalog → verify in the UI now. Doesn't exist → escalation ladder: (1) same outcome via a different construction — more workflows, more nodes, different trigger/event/object; minimalism (rule 1) is suspended here, correctness wins; (2) web research, verified in the UI; (3) closest workable alternative, recorded in `DEVIATIONS.md`, user asked if customer-facing.

## 8. Missed calls are first-class

Every customer type's journey has an explicit missed-call branch: what is texted back and how fast; what happens if they reply (who is notified, what's offered — a booking link, a callback promise); what happens if they don't (how many nudges, spacing, then stop); how the owner is notified; what changes after hours (different message, next-morning callback promise, emergency escalation if the vertical needs it).

## 9. State tags vs history tags

State tags (`state:…`) describe where the contact is now and are removed when no longer true. History tags (`hist:…`) record that something happened and are permanent. Never use a history tag as a trigger filter for "currently in X".

## 10. Naming

`NN – Process – Purpose` (e.g., `01 – Leads – Missed Call Text-Back`). Numbering follows the order a contact typically encounters them. Draft-at-ship workflows carry ` [DRAFT until A2P]` or the reason in the name so the onboarding guide can list them.

## 11. Messages

Every message body uses variables for business identity (principle 11). Every SMS ends with the opt-out phrase GHL requires if the UI or docs say one is needed. No message is sent within a workflow without a preceding check that the contact hasn't opted out, unless GHL enforces that automatically (verify and record in `ghl-capabilities.md`).
