# Ramlee

Simple AI workflow skills for Claude Code. Branch → review → test → PR → staging → Jira.

## Install

```
/plugin install ramlee
```

## Skills

| Skill | Does |
|---|---|
| `/doctor` | Check gh, Jira, Slack connected + logged in. Read-only. |
| `/code-review` | Review diff vs `main` via `/ponytail-review`. Logic, security, perf, tests. |
| `/generate-test` | Generate tests for branch changes, then review them. |
| `/test-in-browser` | Test in open browser tabs. |
| `/raise-pr` | Draft PR title/body, open PR to `main`. |
| `/merge-to-staging <env>` | Merge branch to `jom` / `terbaik` / `berurusan` / `scm` staging. |
| `/jira-update <status>` | Draft comment, then move ticket status. |

All manual-invoke only (`disable-model-invocation`).

## Flow

```
/doctor → /code-review → /generate-test → /test-in-browser → /raise-pr → /merge-to-staging → /jira-update
```
