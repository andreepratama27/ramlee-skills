---
name: "test-in-browser"
description: Run the test case scenarios from `/generate-test-case` in an already-open Chrome tab via the chrome-devtools MCP, then tick off results in the scenario file. Use when the user says "test in browser" or "run the scenarios".
disable-model-invocation: true
---

# Test In Browser

Run the scenarios from `docs/test-case-<feature>-scenario.md` in one existing Chrome tab and write the results back into that file.

Usage: `/test-in-browser [feature]`

## Allowed hosts

Only run against staging or local hosts. A host is allowed if it contains `jom`, `berurusan`, `terbaik`, or `scm`, or is `localhost`, `127.0.0.1`, or starts with `local.*`. Everything else counts as production and must not be tested.

## Steps

1. **Pick the scenario file.** If `feature` is given, use `docs/test-case-<feature>-scenario.md`. Otherwise list the `docs/test-case-*-scenario.md` files and ask the user to pick one with `AskUserQuestion`. If there are none, stop and suggest running `/generate-test-case` first.
2. **Attach to the user's existing Chrome.** Out of the box, the `chrome-devtools` MCP launches its own fresh Chrome window. This skill must instead drive the Chrome the user already has open, so remote debugging has to be set up first.
   - **Check:** call `list_pages`. If it errors, or returns only a single `about:blank` tab (the sign of a fresh MCP-launched browser), stop. Don't run in that window. Show the user the setup below.
   - **Setup (preferred, Chrome 144+):** open `chrome://inspect/#remote-debugging` in the user's Chrome, turn on remote debugging, and run the MCP with `--autoConnect`:
     `claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest --autoConnect`
   - **Setup (fallback, older Chrome):** quit Chrome, then relaunch it with a debug port and a separate profile (Chrome 136+ ignores the port on the default profile, so the user must log in again in that profile):
     `open -a "Google Chrome" --args --remote-debugging-port=9222 --user-data-dir="$HOME/.chrome-debug"`
     then point the MCP at it: `claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest --browserUrl=http://127.0.0.1:9222`
   - After changing the MCP config, the user must restart Claude Code, then rerun `/test-in-browser`.
3. **Pick the tab.** Call `list_pages`. Using `AskUserQuestion`, offer up to 4 tabs whose host matches the scenario URLs; the user can choose "Other" to name a different tab. Then `select_page` it.
   - If the tab's host is not allowed, stop the whole run.
   - Warn the user that this tab will be navigated away from its current page, and that it won't be restored afterwards.
4. **Run each case in that tab only.** Go to each URL with `navigate_page` in the selected tab. Never open a new tab or window. If a case's URL host is not allowed, mark it `SKIPPED (non-staging host)` and continue.
5. **Sensitive input.** Hand control to the user for passwords, OTP, 2FA, captcha, and login. Stop and ask with `AskUserQuestion`: "Fill <field> in the tab, then pick Continue". The options are **Continue** and **Skip case**. Never type these values, and never read them back from the page. If a case needs a logged-in state, ask the user to log in first.
6. **On failure,** log the expected result, the actual result, any console errors (`list_console_messages`), and any failed network requests (`list_network_requests`). Then continue with the next case. Don't retry.
7. **Write the results back.** In the scenario file, tick each checklist item that has run (`- [x]`) and add one line under it: `PASS`, `FAIL: <reason + evidence>`, or `SKIPPED: <reason>`. Finish with a summary in chat that gives the pass, fail, and skip counts and lists the failures.
