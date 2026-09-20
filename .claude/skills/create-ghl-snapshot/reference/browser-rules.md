# Browser rules (apply in every phase that touches GHL)

## Setup

- Load the Chrome MCP tools in **one** ToolSearch call: `select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__find,mcp__claude-in-chrome__get_page_text,mcp__claude-in-chrome__form_input,mcp__claude-in-chrome__javascript_tool,mcp__claude-in-chrome__read_console_messages,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__tabs_close_mcp`
- Call `tabs_context_mcp` first, every session. Never reuse a tab ID from a previous session or from `STATE.md` — re-discover it. Record the tab ID you're using in `STATE.md` only as a hint, never as truth.
- Prefer the GHL tab the user already has open (they were asked to open the sub-account). Confirm the sub-account name/ID shown in the UI before doing anything.

## Reading the UI

- Use `read_page` / `find` / `get_page_text` to read labels, menus, and lists. Use screenshots to confirm layout when text alone is ambiguous.
- When recording a UI path for `ghl-ui-map.md`, copy labels **exactly as displayed** (case, punctuation, icons described in words).
- Use `javascript_tool` only for reading (querying DOM, scraping option lists) or for scrolling. Do not use it to bypass a UI that refuses an action — that's a handoff.

## Creating things

- One object at a time. After every create: navigate to the object (or reload the list), **read it back**, compare to the plan, then update the ledger. A create is not done until the read-back matches.
- After every failure: screenshot → note in the phase log (`03-build-log.md` during build) → retry once with a different approach (different selector, scroll, wait). After **3 consecutive failures on the same action**, stop and post `🖐 NEEDS YOU` with the screenshot and what you tried.
- If the recorded UI path in `ghl-ui-map.md` doesn't match what you see, this is a trust-rule failure: re-discover the path now, update the entry with today's date, note it in the log, continue.

## Never

- Never click Delete/Remove on anything unless the ledger row is origin `mine` and the phase is 6 (or a build-phase fix of your own object, logged).
- Never trigger native `alert`/`confirm`/`prompt` dialogs. If a GHL action shows a browser-native confirm, stop and hand off. (GHL's own in-app modals are fine.)
- Never touch agency-level settings, other sub-accounts, or the account switcher. The only agency-level action is taking the snapshot in Phase 6.
- Never log in, enter 2FA codes, complete OAuth, enter payment details, or upload files the UI won't accept from automation — hand off.
- Never leave a form half-filled and navigate away without noting it in the log.

## Discovery scraping (Phase 2)

- Workflow builder: open a new workflow, open the "add trigger" picker, scroll through every category, record every trigger name. Same for "add action" and for the condition/filter builder. Record what's greyed out or gated (premium/labs) too.
- Forms/surveys builder: record every field/question type in the palette, and whether conditional logic/scoring exists.
- Calendars: record calendar types offered on create, and the availability/form/team options visible.
- Custom fields/values: record the field types on create.
- Write everything into `reference/ghl-capabilities.md` with a date stamp per section. Then the plan is designed against this list, not against memory.

## Screenshots for the onboarding guide (Phase 5)

- Take the screenshot **after** the page has fully loaded and after any hover/tooltips are gone.
- File name: `05-screenshots/<topic>-<step-number>.png`. Note the file name inline in the guide.
- Crop is not required; a clear full-window shot with the target control visible is enough. Describe the target's location in words in the guide as well ("blue button, top right").
