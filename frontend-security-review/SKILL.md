---
name: frontend-security-review
description: Reviews frontend code and pull request changes for security risks. Use when checking XSS, unsafe HTML rendering, authentication, authorization, token storage, browser storage, exposed secrets, API security, user input handling, redirects, sensitive data, third-party dependencies, or when the user asks for a frontend security review.
---

# Frontend Security Review

Review the relevant frontend changes as a security-focused senior frontend engineer.

Focus on real security risks that could affect users, data, authentication, authorization, or application integrity.

Do not report theoretical security issues without evidence in the code.

## Determine Changes to Review

When reviewing a branch or pull request:

1. Determine the current Git branch.
2. Determine the appropriate base branch, normally `main`, `master`, or the repository's configured default branch.
3. Compare the base branch with `HEAD`.
4. Review primarily the changed frontend files and changed lines.
5. Read surrounding code when necessary to understand authentication, authorization, data flow, API usage, or user input.
6. Do not review unrelated files unless they are required to understand a security risk.
7. Identify relevant uncommitted changes separately if they exist.

## Review For

### Cross-Site Scripting (XSS)

Check for:

- `dangerouslySetInnerHTML`
- HTML inserted from user-controlled data
- DOM APIs such as `innerHTML`
- Unsanitized content rendered as HTML
- URLs or attributes built from untrusted input

Prefer normal React rendering because React escapes text content by default.

If raw HTML is necessary, recommend proper sanitization.

### User Input

Check for:

- Trusting user input without validation
- User input used to build URLs, HTML, commands, or sensitive requests
- Client-side validation being treated as the only security control

Remember that frontend validation improves UX but must not replace server-side validation.

### Authentication

Check for:

- Authentication state being trusted only from client-side variables
- Tokens exposed unnecessarily
- Sensitive authentication information written to logs
- Incorrect handling of login/logout state
- Missing session-expiry handling where relevant

### Authorization

Check for:

- UI hiding being treated as real authorization
- Sensitive actions protected only by frontend checks
- Role/permission decisions made only in the browser
- API calls assuming that hidden buttons make operations secure

Always make clear:

Frontend authorization checks improve UX.

The backend must enforce actual authorization.

### Token Storage

Check for:

- Access or refresh tokens stored insecurely
- Long-lived sensitive tokens in `localStorage` or `sessionStorage`
- Tokens exposed through logs or error messages
- Tokens included unnecessarily in URLs

When relevant, explain the trade-offs between browser storage and secure HTTP-only cookies.

Do not claim one storage mechanism is universally correct without context.

### Browser Storage

Check for sensitive information stored in:

- `localStorage`
- `sessionStorage`
- IndexedDB
- client-side caches

Examples of data that may be sensitive:

- access tokens
- personal information
- payment data
- confidential business information

### Secrets

Check for:

- API secrets
- private keys
- passwords
- server credentials
- service-account credentials
- private tokens

Frontend environment variables are not secret if they are included in the browser bundle.

Do not treat public configuration values such as API base URLs as secrets unless they actually provide privileged access.

### API Usage

Check for:

- Sensitive data sent in query strings
- Missing HTTPS assumptions in production configuration
- Dangerous operations triggered with GET requests
- Authentication tokens handled incorrectly
- User-controlled API URLs
- Client-side checks being relied on instead of backend enforcement

### Redirects and Navigation

Check for:

- User-controlled redirect URLs
- Open redirect risks
- Unsafe `window.location` usage
- Untrusted URLs passed directly to links or navigation APIs

### Links and External Content

Check for:

- Untrusted URLs
- `target="_blank"` usage where appropriate protections are needed
- External content being embedded without considering trust boundaries

### Sensitive Data Exposure

Check for:

- Personal data logged to the console
- Tokens printed to logs
- API responses exposing more information than the UI needs
- Sensitive data placed in URLs
- Error messages revealing internal details

### Client-Side Security Boundaries

Check whether the implementation incorrectly assumes that:

- Hidden UI is secure
- Disabled buttons prevent malicious requests
- Route guards provide backend authorization
- Frontend validation protects the API

Explain clearly when enforcement must happen on the backend.

### Dependencies

Check for:

- Newly added unnecessary dependencies
- Security-sensitive functionality delegated to unknown packages
- Obvious risky or outdated patterns

Do not claim a dependency is vulnerable unless there is evidence or verified vulnerability information.

### File Uploads

Where relevant, check for:

- Trusting file extensions
- Rendering uploaded content unsafely
- Missing client-side type/size validation

Make clear that the backend must also validate uploaded files.

## Review Quality

Prioritize exploitable or meaningful security risks.

Do not create fear around normal frontend behaviour.

Distinguish between:

- Actual vulnerability
- Defence-in-depth improvement
- Backend responsibility
- General best practice

Prefer high-confidence findings over speculative security warnings.

Explain the attack scenario only as much as needed to show why the issue matters.

## Output Format

### Security Summary

Briefly explain the security state of the changes.

### Issues

For every issue provide:

- Severity: Critical / High / Medium / Low
- File
- Problem
- Risk
- Example attack or failure scenario, when useful
- Suggested fix
- Backend responsibility, if relevant

### Positive Observations

Mention security practices that are already done well.

### Verification Suggestions

Suggest practical ways to verify findings, for example:

- Browser DevTools
- Network panel
- Inspecting built frontend assets
- Checking storage
- API authorization tests
- Dependency audit tools

### Testing Suggestions

Suggest useful security tests where appropriate.

### Final Review

Finish with one of:

- No significant frontend security issues found
- Security improvements recommended
- Blocking security issue found
