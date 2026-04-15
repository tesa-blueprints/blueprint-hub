Review the current codebase against the project standards defined in CLAUDE.md.

If the user provided specific files or a scope, focus on that. Otherwise, review the most recently changed files (check `git diff` and `git log`).

## What to Check

1. **Code Quality:** Functions under 20 lines, early returns, no magic numbers, proper naming conventions, no `any` types
2. **Security:** No hardcoded secrets, input validation with Zod at boundaries, parameterized queries, no `dangerouslySetInnerHTML` with user input, proper error messages (no stack traces to users)
3. **Testing:** Every feature has tests, every bug fix has regression tests, tests are meaningful (not just coverage fillers)
4. **Documentation:** README accurate, JSDoc on public APIs, code comments explain "why" not "what"
5. **Architecture:** Layered architecture respected (Controller → Service → Repository), proper separation of concerns
6. **Git Hygiene:** Conventional commit messages, issue references, no debug artifacts (console.log, TODO hacks)
7. **Dependencies:** No unused dependencies, lockfile committed, production deps pinned
8. **Environment:** If code targets production: no debug output, no console.log, strict CORS, no PII in logs, HTTPS only
9. **Data Protection:** If handling user data: data minimization applied, no PII logged, deletion/export capability exists

## Output Format

Provide a structured report:

### Summary
One paragraph: overall assessment.

### Issues Found
For each issue:
- **File:Line** — Description of the problem
- **Severity:** Critical / Warning / Suggestion
- **Fix:** What should be changed

### What Looks Good
Briefly note things that are well done — positive reinforcement matters.

### Recommended Next Steps
Ordered list of what to fix first.

## Rules
- Reference specific files and line numbers
- Be concrete: show the problematic code and suggest the fix
- Distinguish between must-fix (Critical) and nice-to-have (Suggestion)
- Check CLAUDE.md rules and verify the code follows them
