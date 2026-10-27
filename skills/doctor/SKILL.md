---
name: "doctor"
description: Checks that GitHub, Jira and Slack are connected and logged in for the Ramlee skills. Use when setting up a new machine or when a Ramlee skill fails on a missing tool, e.g. "doctor", "check setup", "is jira connected".
disable-model-invocation: true
model: "haiku"
---

# Doctor

Check that every tool the Ramlee skills depend on is available and logged in, so `/raise-pr`, `/jira-update` and `/merge-to-staging` run without surprises. Only read. Never post, comment, push or change anything.

Run all three checks, in parallel where possible:

## GitHub

Used to push, pull, raise PRs and watch GitHub Actions.

1. Run `gh --version`. If it fails, gh is not installed.
2. Run `gh api user -q .login` to get the logged-in account. If it fails, run `gh auth status` to see why. A common cause is an invalid `GITHUB_TOKEN` or `GH_TOKEN` env var overriding a valid keyring login.
3. Run `gh repo view --json viewerPermission -q .viewerPermission`. `WRITE`, `MAINTAIN` or `ADMIN` means the account is the right one for this repo. Anything else means the wrong account is active, so suggest `gh auth switch`. If the repo has no GitHub remote, say that access could not be checked.

## Jira

Used to comment on tickets and move ticket status.

Call `atlassianUserInfo` from the Atlassian MCP connector. If the tool is not in your tool list, the connector is not installed. If it returns an auth error, the connector needs authentication. On success, also call `getAccessibleAtlassianResources` and report the site name.

## Slack

Used to post task status.

Call `slack_read_user_profile` for the current user from the Slack MCP connector. Treat a missing tool or an auth error the same way as for Jira. On success, report the user's display name.

## Output Format

One line per tool, then a final verdict. Keep it short, with no extra explanation.

```
✅ GitHub — logged in as <login>, WRITE access to <owner/repo>
✅ Jira — connected to <site> as <name>
❌ Slack — connector needs authentication. Fix: claude.ai Settings > Connectors, or run /mcp
```

For each ❌ line, give the reason and a one-line fix:
- gh not installed → `brew install gh`
- gh not logged in → `gh auth login`
- invalid token env var → unset `GITHUB_TOKEN` or `GH_TOKEN` in the shell profile
- wrong account → `gh auth switch -u <account>`
- MCP connector missing or not authenticated → connect it in claude.ai Settings > Connectors, or run `/mcp`

## Next Step

End output with:
- If all ✅ -> "✅ All set. Ramlee skills are ready"
- If any ❌ -> "⚠️ Fix the items above, then rerun `/ramlee:doctor`"
