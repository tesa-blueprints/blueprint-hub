# Project Rules

> This file was generated from the [tesa-blueprints](https://github.com/tesa-blueprints) organization.
> Sources: blueprint-project-management, blueprint-security-advisory, blueprint-application-coding-guide.
> Use case: TypeScript application with React/Next.js frontend and Node.js backend.

---

## Language

- All documentation, comments, commit messages, PR descriptions, and issue descriptions must be written in English. No exceptions.

## Git & Workflow

- Every code change must have an associated GitHub Issue — no exceptions. Create the issue first, then code.
- Always work on feature branches, never directly on `main`
- Branch naming: `{type}/{description}` — e.g., `feature/user-profile`, `fix/login-redirect`, `chore/update-deps`
- Allowed prefixes: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/`, `test/`
- Keep feature branches short-lived (< 2 days ideal)
- Use Conventional Commits: `{type}({scope}): {description}`
- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`
- First line: max. 72 characters, imperative mood, no uppercase at start, no period at end
- Body: explain the **why**, not the what
- Footer: Issue references with `Closes #123` or `Fixes #456`
- Every PR needs a description with: Summary, Changes, Type of Change, Testing
- PR size: ideally 100-300 lines, maximum 500 lines

## TypeScript

- Use TypeScript strict mode — never disable strict checks
- No `any` — use `unknown` and narrow with type guards
- Prefer `const` over `let`, never use `var`
- Use named exports, avoid default exports (exception: Next.js Pages/Layouts)
- Use `as const` and `readonly` where appropriate
- Prefer interfaces for object shapes, types for unions/intersections
- Use type-only imports: `import type { User } from './types'`

## Functions & Code Style

- Keep functions under 20 lines — extract when they exceed that
- Max. 3 parameters — use an options object beyond that
- Use early returns instead of deep nesting
- Write pure functions where possible
- No empty catch blocks — always handle or re-throw errors
- No magic numbers — use named constants
- No `console.log` in production code — use a structured logger

## Naming

- PascalCase: Types, Interfaces, Classes, React Components
- camelCase: Variables, Functions, Methods
- UPPER_SNAKE_CASE: Constants
- kebab-case: Filenames (except React Components: PascalCase.tsx)
- Boolean variables: prefix `is`, `has`, `can`, `should`
- Functions: verb + noun (`getUserById`, `calculateTotal`)

## Import Ordering

1. Node.js built-ins (`node:fs`, `node:path`)
2. External packages (`express`, `zod`)
3. Internal modules (`@/services`, `@/lib`)
4. Relative imports (`./validation`, `../types`)

Separate groups with a blank line.

## Frontend (React / Next.js)

- Use React Server Components as the default — `'use client'` only for event handlers, state, browser APIs, or hooks
- One component per file, filename = PascalCase.tsx
- Colocate tests, types, and feature-specific files next to the component
- Use semantic HTML: `<nav>`, `<main>`, `<section>`, `<article>`, `<button>`
- All images need `alt` text, use `next/image` instead of `<img>`
- Forms: use React Hook Form + Zod, share validation schemas with backend
- State: Server Components > React Context > Zustand. No Redux for new projects.
- Styling: Tailwind CSS, mobile-first approach
- Use Suspense Boundaries for loading states, Error Boundaries for errors
- No `dangerouslySetInnerHTML` with user input
- No `// eslint-disable` without a justification comment

## Backend (Node.js)

- Layered Architecture: Route Handler → Service → Repository
- Route Handler: Request/Response, input validation (Zod). No business logic.
- Service: Business logic. No HTTP concepts (req, res). Framework-agnostic.
- Repository: Database access. Returns domain objects.
- Validate all inputs with Zod at the route handler level
- Consistent error format: `{ error: { code: "...", message: "..." } }`
- Health check: `GET /health` (always implement)
- Graceful shutdown: implement SIGTERM and SIGINT handlers
- Structured JSON logging with pino, include correlation IDs (`x-request-id`)
- Never expose internal error details to clients
- Rate limiting on public endpoints
- CORS: configure explicitly, no wildcard in production
- `npm ci` instead of `npm install` in CI/CD

## Testing

- Every new feature must include automated tests — no exceptions
- Every bug fix must include a regression test covering the fixed scenario
- When modifying existing features, update the existing tests to reflect new behavior
- Tests must pass in CI before merging — never skip or disable failing tests
- Coverage targets: >= 80% for business logic, >= 90% for utilities

## Security

- Never commit secrets, API keys, passwords, tokens, or certificates
- Use environment variables for configuration, `.env.example` for documentation
- Validate all user inputs at the system boundary using Zod
- Use parameterized queries (Prisma) — never string concatenation for SQL
- Escape output to prevent XSS — React does this by default for JSX
- Use httpOnly, secure, sameSite cookies for session tokens
- Run `npm audit` before adding new dependencies
- Pin production dependencies to exact or tilde versions
- Never disable ESLint security rules
- Never return stack traces or system information to end users
- Log security events but never log sensitive data (passwords, tokens, PII)

## Documentation

- Update README.md when adding new features
- Every project must have architecture documentation (diagram + description)
- Write an ADR for architecture decisions (new technology, pattern changes)
- Maintain the changelog under `[Unreleased]`
- Code comments explain the why, not the what
- Public APIs and exported functions need JSDoc/TSDoc

## Database (Prisma)

- Always use migrations — never change schemas manually
- Avoid N+1: use `include` or `select` for relations
- Use transactions for related operations
- Soft delete for business-critical data
- Index foreign keys and frequently queried fields
