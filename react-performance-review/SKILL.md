---
name: react-performance-review
description: Reviews React and frontend code changes for performance issues. Use when checking unnecessary re-renders, React Query usage, expensive calculations, state placement, effects, memoization, list rendering, network requests, bundle size, or when the user asks for a React performance review.
---

# React Performance Review

Review the relevant frontend changes as a senior React performance engineer.

Focus on real performance problems that could affect users or application scalability.

Do not recommend optimizations without evidence or a clear reason.

## Determine Changes to Review

When reviewing a branch or pull request:

1. Determine the current Git branch.
2. Determine the appropriate base branch, normally `main`, `master`, or the repository's configured default branch.
3. Compare the base branch with `HEAD`.
4. Review primarily the changed frontend files and changed lines.
5. Read surrounding code when necessary to understand rendering, state, data fetching, or component relationships.
6. Do not review unrelated files unless they are required to understand a performance issue.
7. Identify relevant uncommitted changes separately if they exist.

## Review For

### React Re-renders

Check for:

- Components re-rendering unnecessarily
- State stored higher than necessary
- Frequently changing state causing large subtrees to render
- Unstable object, array, or function props
- Context values causing unnecessary consumers to update
- Components doing too much work during render

Do not recommend `React.memo`, `useMemo`, or `useCallback` automatically.

Recommend them only when there is a clear performance reason.

### State Management

Check for:

- State that could remain local but is stored globally
- Large shared state causing broad updates
- Derived state unnecessarily stored instead of calculated
- Duplicate state
- State updates causing avoidable renders

### Effects

Check for:

- Effects running more often than necessary
- Incorrect dependencies
- Effects being used for calculations that could happen during render
- Duplicate API calls caused by effects
- State updates inside effects creating unnecessary render cycles

### React Query / Server State

Check for:

- Poor query key design
- Missing variables in query keys
- Unnecessary refetching
- Incorrect staleTime or cache behaviour
- Over-invalidating queries
- Under-invalidating queries
- Duplicate server state stored in another state manager
- Fetching data that is already available in the cache
- Pagination or infinite-query inefficiencies
- Mutation behaviour that causes unnecessary requests or re-renders

### Lists

Check for:

- Unstable or incorrect keys
- Using array indexes as keys when list order can change
- Rendering very large lists without virtualization
- Expensive work repeated for every list item
- Large child components re-rendering unnecessarily

### Expensive Calculations

Check for:

- Sorting or filtering large arrays on every render
- Repeated transformations
- Heavy synchronous work in the render path
- Duplicate calculations

Only suggest memoization when the calculation is expensive enough to justify it.

### Component Structure

Check for:

- Very large components that cause broad re-renders
- State that could be moved closer to the component that uses it
- Component boundaries that make optimization difficult

Do not suggest splitting components purely for style.

There should be a practical rendering, maintainability, or reuse benefit.

### Network Performance

Check for:

- Duplicate requests
- Sequential requests that could safely run in parallel
- Refetching unchanged data
- Large responses when only small parts are needed
- Missing request cancellation where relevant
- Mutations causing unnecessary network requests

### Bundle Performance

Check for:

- Importing large libraries for small functionality
- Missing lazy loading for genuinely large or rarely used features
- Heavy dependencies added unnecessarily
- Obvious opportunities for code splitting

Do not recommend code splitting for tiny components.

### Browser Performance

Where relevant, check for:

- Expensive DOM operations
- Excessive event listeners
- Large synchronous loops
- Main-thread blocking work
- Large storage operations
- Repeated layout-triggering behaviour

## Review Quality

Prioritize issues with measurable or likely user impact.

Prefer high-confidence findings over theoretical micro-optimizations.

Do not recommend optimization simply because a React API exists.

Do not treat every re-render as a bug.

A re-render is only a concern when the work it causes is meaningful.

Explain the trade-off of every optimization.

## Output Format

### Performance Summary

Briefly explain the performance state of the changes.

### Issues

For every issue provide:

- Severity: High / Medium / Low
- File
- Problem
- Why it matters
- When it becomes noticeable
- Suggested fix
- Trade-off, if relevant

### Positive Observations

Mention performance decisions that are already good.

### Measurement Suggestions

Suggest how the issue could be verified, for example:

- React DevTools Profiler
- Browser Performance panel
- Network panel
- React Query Devtools
- Lighthouse
- Bundle analysis

### Testing Suggestions

Suggest practical performance-related tests or checks where useful.

### Final Review

Finish with one of:

- No significant performance issues found
- Performance improvements recommended
- Blocking performance issue found
