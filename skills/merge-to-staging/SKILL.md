---
name: "merge-to-staging"
description: Skill for merging changes to the staging environment.
disable-model-invocation: true
argument-hint: Specify the staging environment to merge to, e.g., `jom-staging`, `terbaik-staging`, `berurusan-staging`, or `scm-staging`.
---

# Merge to Staging

Merge the changes from the current branch to the staging environment. This is typically done after the code has been reviewed and tested to ensure it is stable and ready for staging.

Use `/merge-to-staging` to perform the merge. Current list of staging environments includes:
- jom-staging
- terbaik-staging
- berurusan-staging
- scm-staging

Example:
```
/merge-to-staging merge to jom-staging
```

## Output Format

The output of `/merge-to-staging` will include the result of the merge operation, indicating whether the merge was successful or if there were any conflicts that need to be resolved.

It may also include a summary of the changes that were merged into the staging environment.