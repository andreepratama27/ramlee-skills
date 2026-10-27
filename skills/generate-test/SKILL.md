---
name: "generate-test"
description: Skill for generating tests based on code changes.
disable-model-invocation: true
---

# Generate Test

Generate tests based on the code changes on this branch.

Use `/generate-test` to generate the tests.

Example:
```
/generate-test 
````

Use `/ponytail-review` to perform a code review for the generated tests.

## Output Format

The output of `/generate-test` will include the generated test cases based on the code changes in the current branch. Each test case will include the test description, the affected code snippet, and the generated test code.

The output may also include a summary indicating the number of test cases generated and any potential gaps in test coverage.