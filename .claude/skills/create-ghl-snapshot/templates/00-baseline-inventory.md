# Baseline inventory — <business>

Sub-account: <name> · <id> · kind: **blank** | **loaded-from: <snapshot>** · inventoried <date>

Every object listed here is also a row in `LEDGER.md` with origin `pre-existing`. This file holds the longer descriptions; the ledger holds the rows. Nothing here is ever deleted by the skill.

---

## A. GHL defaults present (both kinds)

<!-- default pipeline, default calendar, built-in contact fields, location merge fields seen in the picker, default dashboard widgets — names exactly as displayed -->

## B. Blank verification (blank only)

| Area                     | Empty? | What was found |
| ------------------------ | ------ | -------------- |
| Workflows                |        |                |
| Funnels / sites          |        |                |
| Forms / surveys          |        |                |
| Custom fields            |        |                |
| Custom values            |        |                |
| Tags                     |        |                |
| Pipelines                |        |                |
| Calendars                |        |                |
| Templates                |        |                |
| Contacts                 |        |                |
| Trigger links / snippets |        |                |

## C. Pre-existing objects (loaded only)

### Workflows

```
Name: … · published/draft · re-entry: …
Trigger(s): … (filters: …)
Does: <one paragraph>
Touches: tags … · fields … · pipeline/stage … · calendar …
Removes from: …
```

### Custom fields

| Name | Type | Apparent purpose | Referenced by |
| ---- | ---- | ---------------- | ------------- |

### Custom values

| Name | Apparent purpose | Referenced by |
| ---- | ---------------- | ------------- |

### Tags

| Name | Apparent purpose | Applied by / read by |
| ---- | ---------------- | -------------------- |

### Pipelines

| Pipeline | Stages in order |
| -------- | --------------- |

### Calendars

| Name | Type | Form | Team | Availability summary |
| ---- | ---- | ---- | ---- | -------------------- |

### Forms / surveys

| Name | Fields / questions | Embedded where | On submit |
| ---- | ------------------ | -------------- | --------- |

### Funnels / sites

| Name | Steps / pages | Purpose per page | Embeds |
| ---- | ------------- | ---------------- | ------ |

### Templates

| Name | Channel | Purpose |
| ---- | ------- | ------- |

### Dashboards, trigger links, snippets, Conversation AI / reputation settings

- …

## D. Gap analysis (loaded only)

| Customer type | Journey stage                                                                     | Handled by (pre-existing) | Coverage              | Ledger status         | What's needed |
| ------------- | --------------------------------------------------------------------------------- | ------------------------- | --------------------- | --------------------- | ------------- |
|               | arrive / capture / respond / qualify / book / remind / serve / follow-up / review |                           | full / partial / none | reused / extended / — |               |

### Conflicts and resolutions

- <planned X vs pre-existing Y> → <resolution>

### Verdict

**Complete enough / additions needed:** <plain statement — "the snapshot is complete enough for this business; nothing extra is needed" | "only these N additions: …" | "substantial gaps: …">
