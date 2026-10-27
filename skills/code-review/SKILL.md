---
name: "code-review"
description: Skill for performing code reviews with ponytail-audit.
disable-model-invocation: true
---

# Code Review

Review diff vs base branch (default to `main`). Goal: to catch issue before PR, not nitpicks the styles (linter will do that).

Focus on:
- Logic errors
- Security vulnerabilities
- Performance issues
- Code readability and maintainability
- Test coverage and quality (if the code includes tests)

Use `/ponytail-review` to perform the code review.

Example:
```
/ponytail-review --base main --head current-feature-branch
```

## Output Format

The output of `ponytail-audit` will include a summary of the issues found during the code review, categorized by type (e.g., logic errors, security vulnerabilities, performance issues). Each issue will include a description, the affected code snippet, and recommendations for fixing it.

The output may also include a final assessment indicating whether the code passes the review or if there are critical issues that need to be addressed before merging.