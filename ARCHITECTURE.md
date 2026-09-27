# Architecture

## 1. What is in the system?

This is a small React application used to practise TanStack Query.

The high-level flow is:

```text
React UI
   ↓
TanStack Query
   ↓
API functions
   ↓
JSON Server / data source
```

The application uses:

- React for UI rendering.
- TanStack Query for asynchronous server-state management.
- API functions inside `src/api` for data access.
- JSON Server as the local backend/data source.
- Vite for development and production builds.

---

## 2. Who is responsible for what?

Each responsibility should have one clear owner.

### React components

React components are responsible for:

- Rendering UI.
- Handling user interactions.
- Displaying loading, error, and success states.

Components should not contain direct HTTP or data-access logic.

Current component location:

```text
src/component/
```

Example:

```text
src/component/PostList.jsx
```

### TanStack Query

TanStack Query owns server state.

It is responsible for:

- Fetching asynchronous data.
- Managing loading and error states.
- Caching server responses.
- Refetching data.
- Invalidating cached data when required.

Server data should not be duplicated into React state unless there is a specific reason.

### API layer

The API layer owns communication with the backend/data source.

Location:

```text
src/api/
```

Current API file:

```text
src/api/api.js
```

React components should not directly call `fetch()` or another HTTP client when an API function already exists or can be added to this layer.

### Application setup

`src/main.jsx` owns application bootstrapping.

Global providers such as `QueryClientProvider` should be configured here or in a dedicated provider module if the application grows.

### App component

`src/App.jsx` is responsible for composing the main application UI.

It should not become the location for API implementation or reusable business logic.

---

## 3. Why is it built this way?

### TanStack Query owns server state

Decision:

Use TanStack Query for data coming from the backend.

Reason:

TanStack Query already provides caching, loading states, error states, refetching, and cache invalidation.

Trade-off:

The application depends on TanStack Query conventions rather than managing asynchronous state manually.

### API calls are separated from components

Decision:

Keep backend communication inside `src/api`.

Reason:

Separating data access from UI components makes the code easier to maintain, reuse, test, and change.

A component should focus primarily on presentation and interaction.

---

## 4. What is allowed to touch what?

Preferred dependency direction:

```text
React Components
       ↓
TanStack Query
       ↓
API Functions
       ↓
Backend / JSON Server
```

### Allowed

Components may use TanStack Query hooks.

TanStack Query query functions may call functions from `src/api`.

API functions may communicate with the backend.

### Not allowed

Do not make direct backend requests inside React components when the request belongs in the API layer.

Do not duplicate TanStack Query server data into local React state without a clear reason.

Do not introduce another server-state library while TanStack Query owns server state.

Do not put UI rendering logic inside the API layer.

Do not put API implementation inside `App.jsx`.

---

## 5. How does data move?

Example: loading posts.

```text
PostList
   ↓
useQuery()
   ↓
API function in src/api/api.js
   ↓
JSON Server
   ↓
API response
   ↓
TanStack Query cache
   ↓
PostList renders the result
```

Components should consume the query result rather than manually maintaining duplicated copies of server data.

---

## 6. What must never break?

The following rules are architectural constraints:

1. TanStack Query remains the owner of server state.
2. API communication belongs in the API layer.
3. UI components must not directly access the JSON data source.
4. Do not introduce duplicate ways of fetching the same data without a clear architectural reason.
5. Existing application behaviour must continue to work after architectural changes.
6. New patterns should not be introduced unless there is a reason the existing pattern cannot handle the requirement.

---

## 7. Where does new code belong?

Use the existing structure before creating a new pattern.

### New React component

Add it under:

```text
src/component/
```

Example:

```text
src/component/PostDetails.jsx
```

### New API operation

Add it to:

```text
src/api/api.js
```

If the API layer becomes large, it may later be split by domain.

For example:

```text
src/api/posts.js
src/api/users.js
```

Do not introduce this structure until the size of the application justifies it.

### New TanStack Query logic

For this small project, query logic may remain close to the component using it.

If queries become reusable or complex, introduce a dedicated location such as:

```text
src/queries/
```

Do not create that folder only for architectural appearance. Introduce it when there is real reusable query logic.

### Global providers

Keep application-wide providers near the application entry point.

Current location:

```text
src/main.jsx
```

If providers become numerous, they may later be extracted into a dedicated provider component.

---

## 8. When should the AI agent stop and ask?

If completing a task requires breaking one of the architectural rules above:

1. STOP before making the architectural change.
2. Explain which rule conflicts with the requested task.
3. Identify which files or responsibilities would be affected.
4. Explain why the existing architecture cannot support the requirement.
5. Suggest the smallest change that solves the problem without unnecessarily introducing a new pattern.

Do not silently work around an architectural constraint.

Do not introduce a new architecture, state-management approach, API pattern, or folder structure simply because it makes the immediate task easier.