---
name: accessibility-review
description: Reviews frontend code and pull request changes for accessibility issues. Use when checking accessibility, WCAG concerns, semantic HTML, keyboard navigation, forms, ARIA, focus behaviour, screen reader support, or when the user asks for an accessibility review.
---

# Accessibility Review

Review the relevant frontend changes as an accessibility-focused frontend engineer.

## Determine Changes to Review

When reviewing a branch or pull request:

1. Determine the current Git branch.
2. Determine the appropriate base branch, normally `main`, `master`, or the repository's configured default branch.
3. Compare the base branch with `HEAD`.
4. Review primarily the changed frontend files and changed lines.
5. Read surrounding code when required to understand accessibility behaviour.
6. Do not report unrelated accessibility issues unless they directly affect the changed feature.
7. Identify relevant uncommitted changes separately if they exist.

## Review For

### Semantic HTML

Check for:

- Correct use of headings
- Buttons used for actions
- Links used for navigation
- Proper form elements
- Avoiding unnecessary clickable `div` or `span` elements

### Keyboard Accessibility

Check whether:

- Interactive controls can be reached with the keyboard
- Enter and Space behave correctly where expected
- Focus order is logical
- Keyboard users are not trapped
- Custom controls support keyboard interaction

### Forms

Check for:

- Every input has an accessible label
- Labels are correctly associated with inputs
- Required fields are communicated
- Validation errors are understandable
- Error messages are exposed to assistive technologies

### ARIA

Check for:

- ARIA is used only when necessary
- Roles are correct
- `aria-label` and `aria-labelledby` are meaningful
- Invalid ARIA attributes are avoided

Prefer semantic HTML over unnecessary ARIA.

### Images and Media

Check for:

- Meaningful images have useful `alt` text
- Decorative images use empty `alt`
- Media has suitable accessibility support where relevant

### Focus Management

Check whether:

- Focus is visible
- Modal/dialog focus behaviour is reasonable
- Focus is restored when overlays close
- Dynamic UI changes do not unexpectedly lose focus

### Screen Readers

Check whether:

- Important state changes are communicated
- Error messages are announced
- Interactive controls have accessible names
- Content structure is understandable without visual styling

### WCAG

Check for relevant WCAG concerns, especially:

- 1.1.1 Non-text Content
- 1.3.1 Info and Relationships
- 2.1.1 Keyboard
- 2.4.3 Focus Order
- 2.4.7 Focus Visible
- 3.3.1 Error Identification
- 3.3.2 Labels or Instructions
- 4.1.2 Name, Role, Value

Do not report a WCAG violation unless the code clearly supports the finding.

## Review Quality

Prioritize real accessibility problems that affect users.

Do not report minor style preferences as accessibility issues.

Prefer a small number of high-confidence findings over speculative feedback.

Always explain why the issue matters to an actual user.

## Output Format

### Accessibility Summary

Briefly explain the accessibility state of the changes.

### Issues

For every issue provide:

- Severity: High / Medium / Low
- File
- Problem
- Who is affected
- Why it matters
- Suggested fix
- Relevant WCAG criterion when appropriate

### Positive Observations

Mention accessibility practices that are already done well.

### Testing Suggestions

Suggest practical tests such as:

- Keyboard-only testing
- Screen reader testing
- React Testing Library queries
- axe accessibility checks

### Final Review

Finish with one of:

- No significant accessibility issues found
- Accessibility improvements recommended
- Blocking accessibility issue found
