# Browser rules (apply in every phase that touches GHL)

## Setup

- Load the Chrome MCP tools in **one** ToolSearch call: `select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__find,mcp__claude-in-chrome__get_page_text,mcp__claude-in-chrome__form_input,mcp__claude-in-chrome__javascript_tool,mcp__claude-in-chrome__read_console_messages,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__tabs_close_mcp`
- Never reuse a tab ID from a previous session or from `STATE.md` — re-discover it. Record the tab ID you're using in `STATE.md` only as a hint, never as truth.

## Map-first, then act (read before you click)

Before interacting with any GHL area, **consult the reference files first** and act from them:

1. **Look it up first.** Check `ghl-ui-map.md` (paths, create-flows, click quirks), `ghl-capabilities.md` (triggers/actions/options/field types), and `defaults-first.md` (built-ins/defaults). If there's a stamped entry, **follow it** instead of re-exploring — that is the whole point of the one-time discovery.
2. **Compare to what you see.** When you reach the live screen, check reality against the entry:
   - **Matches** → proceed, change nothing.
   - **Differs** (renamed/moved/new option/removed) → update the entry on the spot, re-stamp it, and log it (see _Keeping the reference files true_). Defaults change only from a blank sub-account.
   - **Not in the files** → discover it, then add a stamped entry so the next run reads it instead of rediscovering.
3. **Never rediscover a stamped entry from scratch** just because you're unsure — trust it until it fails in use.

This ordering (read map → act → reconcile) is mandatory in every phase that touches GHL.

## Known GHL-iframe quirks (apply in EVERY area, not just where first seen)

Sub-account pages render in cross-origin iframes; these behaviours recur across custom values, custom fields, calendars, forms, workflows, funnels, etc. Assume them everywhere:

- **First-click-after-a-change is swallowed.** Right after a save, or after opening/selecting from a dropdown, the *next* click often only focuses (does nothing). **Click dropdown options twice**, and re-click a button that didn't respond. When adding a modal/drawer, if it doesn't appear, click again ≥ a few seconds later; if still stuck, reload the page.
- **Type into the intended input, then verify.** A click that lands slightly off (e.g. a narrow label column) leaves focus on the previous field, so typed text can go into the wrong box. Zoom-check a field after typing before moving on.
- **Screenshots freeze when the automation window is hidden/minimised** (Chrome stops painting). Keep the window visible; if `computer:screenshot` times out, that's the cause — ask the user to bring it forward.
- **Long `type` strings time out.** A single `computer:type` of roughly >250 chars (or anything taking >30s to dispatch) fails with `CDP sendCommand "Input.dispatchKeyEvent" timed out … renderer may be frozen`, even when the window is visible and screenshots work. Split long text (e.g. consent/rich-text) into ≤~60-char chunks across multiple `type` actions (batch them). A short test-type confirms the renderer is alive.
- **Option/list tables scroll inside the drawer** after ~6–7 rows; scroll the list (not just the page) to reach later rows. Fixed buttons like "+ Add option" stay put.
- Verify each create by reading it back (per _Creating things_) — the quirks above mean a step can silently no-op.

## Your own window — never the user's

The skill always works in a Chrome window it opened for itself. It never reuses a window or tab the user is using.

1. First browser call of every session: `tabs_context_mcp` with `createIfEmpty: true`. If no MCP tab group exists, this opens a **new Chrome window** with its own tab group and an empty tab. That window is yours.
2. Work only in tabs inside the MCP tab group. If you need another tab, use `tabs_create_mcp` (it opens inside the same group). Record the tab ID in `STATE.md` → `Chrome tab hint`.
3. **Tabs outside the MCP tab group belong to the user. They are off-limits:** don't read, screenshot, click, type in, navigate, or close them. This applies even when one is already logged into GHL on the right sub-account. "The user already has it open" is never a reason to use their tab.
4. If `tabs_context_mcp` reports an existing MCP group that isn't in its own window (for example, the user dragged a tab into it), don't use it. Tell the user and ask them to close that group so you can start a fresh window.
5. Your window shares the Chrome profile's login cookies, so it's normally already signed in to GHL. If it lands on a login, 2FA, or expired-session page, hand off (`🖐 NEEDS YOU`: "log in to GHL in the window I opened, then say done"). Never log in yourself.
6. Close the tabs you created (`tabs_close_mcp`) when the run is complete (after Phase 6) or when the user asks. Leave them open between phases, because a resumed session will re-discover them.

## Opening the target sub-account

The target comes from `STATE.md` → `Sub-account` (asked at the start protocol; see `SKILL.md` → _Target sub-account_).

- **By Location ID (preferred):** in your own tab, navigate to the recorded GHL domain. Read the URL the app settles on. If it follows a `/location/<id>/…` pattern, navigate to that same URL with the target ID swapped in. Once confirmed, record the working URL pattern in `ghl-ui-map.md`. If it doesn't follow that pattern, find the path by exploring your own tab only (not agency settings), or hand off.
- **By name (fallback):** open the sub-account switcher in your own tab **read-only**, and search for the exact name. Exactly one match → select it. Zero or several matches → don't guess: post `🖐 NEEDS YOU` with the matches you saw and ask for the Location ID. Selecting the confirmed target in the switcher is the only thing you ever do in it.
- **Confirm before any action:** read the location name and ID from the UI (URL and business profile). Post one line: "Working in sub-account **<name>** (ID `<id>`) in my own Chrome window." If it doesn't match what the user gave, stop and ask. Then fill in `STATE.md` → `Sub-account` (missing name or ID, `confirmed: yes`).
- Re-check the location name/ID at the start of every session and whenever a page looks unfamiliar. If the tab has ended up in another sub-account, stop and hand off.

## Reading the UI

- Use `read_page` / `find` / `get_page_text` to read labels, menus, and lists. Use screenshots to confirm layout when text alone is ambiguous.
- When recording a UI path for `ghl-ui-map.md`, copy labels **exactly as displayed** (case, punctuation, icons described in words).
- Use `javascript_tool` only for reading (querying DOM, scraping option lists) or for scrolling. Do not use it to bypass a UI that refuses an action — that's a handoff.

## Creating things

- One object at a time. After every create: navigate to the object (or reload the list), **read it back**, compare to the plan, then update the ledger. A create is not done until the read-back matches.
- After every failure: screenshot → note in the phase log (`03-build-log.md` during build) → retry once with a different approach (different selector, scroll, wait). After **3 consecutive failures on the same action**, stop and post `🖐 NEEDS YOU` with the screenshot and what you tried.
- If the recorded UI path in `ghl-ui-map.md` doesn't match what you see, this is a trust-rule failure: re-discover the path now, update the entry with today's date, note it in the log, continue. The same goes for anything else that doesn't match (see _Keeping the reference files true_).

## Keeping the reference files true

**Record every mismatch you observe — always.** Whenever the live UI differs from what a reference file says, update the file on the spot, whether or not it affects the current task. This covers a trigger, action, option, field, label, menu path, default or setting that has been renamed, moved, added or removed. A default that no longer exists in a blank sub-account counts too (for example, a built-in field that is gone).
- Changed: edit the entry, re-stamp it with today's date, and append `(changed <date>: was "<old>")`.
- Removed: keep the line and mark it `REMOVED <date> — <where you looked>`, so nobody plans against it again.
- New (seen while passing by): add it, stamped.
- Note each one in the current phase's log (`03-build-log.md` during build) under "Reference updates".

**Which account kind can change which file:**
- **Defaults** (`defaults-first.md`: built-in fields, default objects, default-on settings) change **only on evidence from a blank sub-account**. In a sub-account loaded from a snapshot, extra or missing custom values, fields, tags, pipelines, calendars and similar objects come from that snapshot, not from GHL. Record them in that build's `00-baseline-inventory.md` only, never in `defaults-first.md`.
- **Platform behaviour** (`ghl-capabilities.md` trigger/action/condition lists and options, `ghl-ui-map.md` labels and paths, the merge-field picker) is the same in every sub-account, so record differences from any account kind. Premium (👑), greyed or labs items can depend on the agency's plan: note the sub-account they were seen in rather than calling them removed.

This is how the one-time discovery stays accurate without ever being re-run in full.

## Never

- Never click Delete/Remove on anything unless the ledger row is origin `mine` and the phase is 6 (or a build-phase fix of your own object, logged).
- Never trigger native `alert`/`confirm`/`prompt` dialogs. If a GHL action shows a browser-native confirm, stop and hand off. (GHL's own in-app modals are fine.)
- Never touch agency-level settings or other sub-accounts. The account switcher is used only to open the confirmed target sub-account by name (see _Opening the target sub-account_). The only agency-level action is taking the snapshot in Phase 6.
- Never read, click, navigate, or close a tab or window outside your own MCP tab group. Those are the user's (see _Your own window — never the user's_).
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
