<p align="center">
  <img src="assets/ramlee.png" width="200" alt="Ramlee">
</p>

# Ramlee

Simple AI workflow skills for Claude Code. Branch → review → test → PR → staging → Jira.

## Install

```
/plugin marketplace add andreepratama27/ramlee-skills
/plugin install ramlee-skills@ramlee
```

Update to the latest version:

```
/plugin marketplace update ramlee
```

## Skills

| Skill | Does |
|---|---|
| `/doctor` | Check gh, Jira, Slack connected + logged in. Read-only. |
| `/code-review` | Review diff vs `main` via `/ponytail-review`. Logic, security, perf, tests. |
| `/generate-test-case` | Generate test case scenarios from branch changes into `docs/test-case-<feature>-scenario.md`. |
| `/test-in-browser [feature]` | Run those scenarios in your open Chrome tab (staging/local only), tick off results. |
| `/raise-pr` | Draft PR title/body, open PR to `main`. |
| `/merge-to-staging <env>` | Merge branch to `jom` / `terbaik` / `berurusan` / `scm` staging. |
| `/jira-update <status>` | Draft comment, then move ticket status. |

All manual-invoke only (`disable-model-invocation`).

## Flow

```
/doctor → /code-review → /generate-test-case → /test-in-browser → /raise-pr → /merge-to-staging → /jira-update
```

## Before `/test-in-browser`: enable remote debugging

`/test-in-browser` drives the Chrome you already have open (so your logins carry over), not a fresh window. The `chrome-devtools` MCP needs remote debugging to attach to it. One-time setup:

**Chrome 144+ (preferred)**

1. In Chrome, open `chrome://inspect/#remote-debugging` and turn remote debugging on.
2. Add the MCP with auto-connect:
   ```bash
   claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest --autoConnect
   ```

**Older Chrome (fallback)**

1. Quit Chrome, then relaunch it with a debug port and a separate profile (Chrome 136+ ignores the port on the default profile, so log in again in this profile):
   ```bash
   open -a "Google Chrome" --args --remote-debugging-port=9222 --user-data-dir="$HOME/.chrome-debug"
   ```
2. Point the MCP at it:
   ```bash
   claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest --browserUrl=http://127.0.0.1:9222
   ```

Restart Claude Code after changing the MCP config. If `/test-in-browser` only sees a single `about:blank` tab, it's not attached to your Chrome: redo the steps above.

## Releasing

Users only get an update when `version` in `.claude-plugin/plugin.json` changes. Bump it **before** committing, in the same commit as your change. Don't add `version` to `marketplace.json`: `plugin.json` wins.

Pick the bump from your commit type:

| Commit | Bump | Example |
|---|---|---|
| `fix:` / `chore:` | patch | 1.0.1 → 1.0.2 |
| `feat:` (new skill) | minor | 1.0.1 → 1.1.0 |
| `feat!:` (skill renamed/removed) | major | 1.0.1 → 2.0.0 |
| `docs:` / `refactor:` | none | — |

```bash
# 1. bump (edit by hand, or with jq):
jq '.version = "1.0.2"' .claude-plugin/plugin.json > tmp && mv tmp .claude-plugin/plugin.json

# 2. commit change + bump together, tag, push
git add -A
git commit -m "fix: tighten allowed hosts in test-in-browser"
git tag v1.0.2
git push && git push origin v1.0.2
```
