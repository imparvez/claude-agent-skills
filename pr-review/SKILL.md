---
name: pr-review
description: Reviews frontend pull requests and code changes for React, TypeScript, code quality, performance, accessibility, testing, and security. Use when the user asks to review a PR, review code changes, inspect a branch, check a pull request, or review changes before merging.
---

# PR Review

Review the code changes as a senior frontend engineer.

## Determine Changes to Review

When reviewing a pull request:

1. Determine the current Git branch.
2. Determine the appropriate base branch, normally `main`, `master`, or the repository's configured default branch.
3. Use the Git diff between the base branch and `HEAD` to identify changes introduced by the current branch.
4. Review primarily the changed files and changed lines.
5. Read surrounding code or related files when necessary to understand the change.
6. Do not review unrelated files unless they are required to understand an issue.
7. Also identify relevant uncommitted changes separately if they exist.

## Review for

### Correctness

Check for:

- Bugs
- Incorrect logic
- Missing edge cases
- Unexpected behaviour

### React

Check for:

- Unnecessary re-renders
- Incorrect hook usage
- Missing dependencies in hooks
- Incorrect state management
- Components doing too much
- Poor component separation

### TypeScript

Check for:

- Incorrect types
- Unnecessary `any`
- Missing null or undefined handling
- Weak interfaces or types

### Performance

Check for:

- Expensive unnecessary calculations
- Repeated rendering
- Large unnecessary imports
- Poor list rendering
- Missing memoisation where it provides real value

Do not recommend memoisation unless there is a clear reason.

### Accessibility

Check for:

- Semantic HTML
- Keyboard accessibility
- Form labels
- Button and link usage
- ARIA usage
- Basic WCAG accessibility concerns

### Testing

Check whether:

- Important behaviour is tested
- Existing tests need updating
- Edge cases should be covered

Prefer behavioural tests using React Testing Library.

### Security

Check for:

- Unsafe user input
- Secrets exposed in frontend code
- Unsafe HTML rendering
- Authentication or authorization mistakes

## Output Format

Use:

### Summary

Briefly explain what the change does.

### Issues

For every important issue provide:

- Severity: High / Medium / Low
- File
- Problem
- Why it matters
- Suggested fix

### Positive Observations

Mention useful things the implementation does well.

### Testing Suggestions

Mention tests that should be added or changed.

### Final Review

Finish with one of:

- No blocking issues found
- Changes recommended before merging
- Blocking issue found

Do not invent problems just to produce feedback.

If the implementation is already good, say so.
