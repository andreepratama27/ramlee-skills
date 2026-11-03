---
name: "generate-test-case"
description: Skill for generating test cases scenario based on code changes on the active branch.
disable-model-invocation: true
---

# Generate Test

Generate test case scenario based on the code changes on this branch. This is used for end to end testing on the live browser.

Use `/generate-test-case` to generate the tests.

Example:
```
/generate-test-case
````

## Output Format

The output of `/generate-test-case` will include the generated test cases based on the code changes in the current branch. Each test case will include the test description, the affected code snippet, and the generated test code, or you can give a summary which component or pages (url) will be impacted by this changes, show it using checklist so later the `/test-in-browser` skill can use it to determine which tabs to run the tests on and checked after execution is done.

The output may also include a summary indicating the number of test cases generated and any potential gaps in test coverage. Put it on `/docs/test-case-<features-name>-scenario.md` and let the `/test-in-browser` skill use it to determine which tabs to run the tests on and check after execution is done.

## Next Step

After generating the tests using `/generate-test`, you can proceed to run the tests in the browser using the `/test-in-browser` skill. Make sure to review the generated test cases and confirm the impacted components or routes before executing the tests.