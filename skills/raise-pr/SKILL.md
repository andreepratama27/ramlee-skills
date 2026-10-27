---
name: "raise-pr"
description: Skill for raising pull requests.
disable-model-invocation: true
---

# Raise Pull Request

Raise a pull request (PR) to merge changes from the current branch into the target branch, typically the main or development branch. This is usually done after code has been reviewed and tested to ensure it is ready for integration.

Use `/raise-pr` to create a pull request. The default target branch is `main`. When this skill is invoked, draft the title and description for the pull request, summarizing the changes made and any relevant context for reviewers, then submit it for review.

Before raising a pull request:
- Ensure that all changes have been committed and pushed to the current branch.
- Verify that the code has been reviewed and tested to meet the project's quality standards.
- Check for any merge conflicts with the target branch and resolve them if necessary.
- Confirm that the pull request title and description accurately reflect the changes and context for reviewers.

## Next Step

End output with:
- If clean -> "✅ Next: `/ramlee:jira-update`"
- If not clean -> "⚠️ Resolve issues before proceeding"