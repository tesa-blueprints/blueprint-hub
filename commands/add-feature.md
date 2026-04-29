Guide the implementation of a new feature following the full standard workflow. Use $ARGUMENTS as the feature description.

## Workflow

### Step 1: Create GitHub Issue

Create a GitHub Issue for this feature:
- Title: `[Feature]: {feature description}`
- Include: Problem description, desired solution, acceptance criteria
- Labels: `type: feature`
- Ask the user to confirm the issue before proceeding

Two optional questions before creating the issue:

1. **Parent issue?** "Is this feature part of a larger epic or parent issue? If yes, give the issue number — I'll attach the new issue as a sub-issue after creation." If yes, after the issue is created run:
   ```bash
   gh api -X POST repos/{owner}/{repo}/issues/{parent}/sub_issues \
     -f sub_issue_id={new_issue_id}
   ```
2. **Program board?** "Should this be visible on the cross-repo program board (`track: program`)? Default: **no** — only set this when the issue's status matters at the portfolio level (initiatives, releases, major incidents)." If yes, add `track: program` to the labels list.

### Step 2: Create Feature Branch

```bash
git checkout main
git pull origin main
git checkout -b feature/{short-description}
```

Use kebab-case, max 50 characters.

### Step 3: Implement

Implement the feature following project rules from CLAUDE.md:
- TypeScript strict mode, no `any`
- Small functions (< 20 lines), early returns
- Proper naming conventions
- Input validation at boundaries

### Step 4: Write Tests

For every piece of new functionality:
- Unit tests for business logic
- Integration tests for API endpoints (if applicable)
- Update existing tests if behavior changed

Run tests and confirm they pass: `npm test`

### Step 5: Update Documentation

- [ ] Update README.md if the feature is user-facing
- [ ] Add JSDoc/TSDoc to new public APIs
- [ ] Add changelog entry under `[Unreleased]` → `### Added`
- [ ] Update architecture documentation if architecture changed
- [ ] Create an ADR if a significant technology decision was made

### Step 6: Run Pre-Commit Checks

- [ ] `eslint . --max-warnings 0`
- [ ] `prettier --check .`
- [ ] `tsc --noEmit`
- [ ] `npm test`
- [ ] No secrets in code

### Step 7: Commit & PR

Commit with Conventional Commit format:
```
feat({scope}): {description}

{body explaining why}

Closes #{issue-number}
```

Create PR with:
- Summary of changes
- Link to issue
- Testing description
- Screenshots (if UI changes)

## Rules
- Do NOT skip testing — every feature needs tests
- Do NOT skip documentation — if it's user-facing, document it
- Do NOT skip the changelog entry
- Ask the user before making architectural decisions
