---
name: "jira-update"
description: Skill for updating Jira issues status.
disable-model-invocation: true
argument-hint: Specify the Jira issue status to update, e.g., `In Progress`, `Done`, `To Do`.
---

# Jira Update

Update the status of Jira issues directly from the current branch. This is typically done to reflect the progress of tasks, such as moving an issue from "In Progress" to "Done". Check the workflow of your Jira project to ensure the correct status transitions are applied.

Before change the Jira Status:
- Make sure you aware with the ticket you are going to update. If you are unsure, check the branch name or commit messages for references to the Jira issue, or ask the users to specify the Jira ticket.
- Draft a comment about the progress or context of the issue. After user review, then put it as a comment on the ticket.
- Ensure that you have the necessary permissions to update the Jira issue status. If you do not have the required permissions, request them from your Jira administrator or ask someone with the appropriate access to make the update.
- Verify that the status you want to update to is valid according to your Jira project's workflow. Attempting to set an invalid status may result in an error or be rejected by Jira.
- After review of the draft comment and the Jira issue details, proceed to update the status of the Jira issue using the appropriate command or interface.

Use `/jira-update` to update the status of Jira issues.