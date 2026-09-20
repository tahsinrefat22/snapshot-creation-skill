# Workflows — <business>

Doctrine: `reference/workflow-doctrine.md`. Every trigger / action / condition below exists in `ghl-capabilities.md` (stamped) — the reality check is done before Gate 2. Pre-existing workflows (loaded accounts) are listed here too, because they run against the same contacts.

## Index

| #   | Workflow                           | Origin | Purpose | Ship state                 | Trigger(s) |
| --- | ---------------------------------- | ------ | ------- | -------------------------- | ---------- |
| 01  | 01 – Leads – Missed Call Text-Back | mine   |         | published / draft [reason] |            |

---

## 01 – <Process> – <Purpose>

Origin: mine / pre-existing (reused / extended: <what changes>) · Ship state: … · Re-entry: <allow / block> because …

**Trigger(s)** (verified): <trigger> — filters: …

**Steps**

```
1. [trigger]
2. [if/else: <condition>]                       (verified condition type)
   yes → 3a. [action …]  → 4a. [action …]  → goto MERGE
   no  → 3b. [action …]                    → goto MERGE
MERGE:
5. [wait <duration|until|condition>]
6. [if replied?]
   yes → 7. [remove from workflow: <other>] → 8. [add tag hist:…] → END
   no  → 9. [action …] → 10. [wait …] → 11. [action …] → END
```

Each step's reason: 2 — …, 3a — …, …

**Removes contact from:** <workflow> at step <n> when <condition>
**Completion marker:** tag `state:…` / field … set at step <n> (read by: …)
**Messages used:** <template names>
**Objects used:** tags … · fields … · pipeline/stage … · calendar …

---

## Dependency graph

| A                        | Relation | B                        | Mechanism (for → and ⊗)                                           |
| ------------------------ | -------- | ------------------------ | ----------------------------------------------------------------- |
| 01 Missed Call Text-Back | →        | 02 Lead Qualification    | 01 sets `state:textback-sent`; 02's trigger filter requires it    |
| 02 Lead Qualification    | ∥        | 03 Owner Alert           | unrelated tasks; neither writes what the other reads              |
| 04 Lead Chase            | ⊗        | 05 Appointment Lifecycle | 05 step 2 removes from 04; 04 exit condition "appointment booked" |

Drawn:

```
01 → 02 ∥ 03
04 ⊗ 05
```

## Reality-check log (pre-Gate 2)

| Item | In catalog? | Verified in UI? | Outcome / deviation # |
| ---- | ----------- | --------------- | --------------------- |

## Ledger rows

<!-- type workflow · flags draft-at-ship where applicable -->
