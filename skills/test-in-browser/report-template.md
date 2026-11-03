# QA report template

Use plain Markdown only: headings, tables, lists, and emoji. Don't use HTML or put lists inside table cells, because Jira doesn't render them well.

```markdown
## QA Report: <Feature name>

| | |
|---|---|
| **Verdict** | ✅ PASSED / ❌ FAILED / ⚠️ PASSED WITH ISSUES |
| **Environment** | <host, e.g. jom.mudah.my> |
| **Browser** | Chrome <version> |
| **Tested on** | <YYYY-MM-DD HH:mm> |
| **Tester** | <name> (via `/test-in-browser`) |
| **Scenario file** | `docs/test-case-<feature>-scenario.md` |

### Summary
| Total | ✅ Pass | ❌ Fail | ⏭️ Skipped |
|:---:|:---:|:---:|:---:|
| 12 | 9 | 2 | 1 |

<1–2 plain sentences: what works, what's broken, does it block release.>

### ❌ Failures

#### TC-03: <case title>
- **Severity:** 🔴 High / 🟠 Medium / 🟡 Low
- **URL:** <url>
- **Steps to reproduce:**
  1. …
  2. …
- **Expected:** …
- **Actual:** …
- **Console:** `TypeError: Cannot read properties of undefined (reading 'id')`
- **Network:** `POST /api/listing → 500`

### ⏭️ Skipped
| Case | Reason |
|---|---|
| TC-07 | Needs OTP, user skipped |

### All results
| # | Case | Result | Note |
|---|---|---|---|
| TC-01 | Login with valid email | ✅ Pass | |
| TC-03 | Submit listing | ❌ Fail | 500 on submit |
| TC-07 | OTP verification | ⏭️ Skipped | Needs OTP |

### Notes / follow-ups
- <flaky behaviour, out-of-scope bugs spotted, things to retest>
```

Leave out the Failures or Skipped section when it has no entries.
