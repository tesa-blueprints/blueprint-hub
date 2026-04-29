Guide the fixing of a bug following the full standard workflow. Use $ARGUMENTS as the bug description.

## Workflow

### Step 1: Investigate

Before writing any code:
1. Understand the bug: What is the expected behavior? What actually happens?
2. Find the root cause: Read the relevant code, check logs, reproduce the issue
3. Identify the scope: What files/functions are affected?

### Step 2: Create GitHub Issue (if not exists)

Create a GitHub Issue for this bug:
- Title: `[Bug]: {bug description}`
- Include: Description, steps to reproduce, expected/actual behavior, environment
- Labels: `type: bug`

Two optional questions before creating the issue:

1. **Parent issue?** "Is this bug part of a larger tracking issue or epic? If yes, give the issue number — I'll attach the new issue as a sub-issue after creation." If yes, after the issue is created run:
   ```bash
   gh api -X POST repos/{owner}/{repo}/issues/{parent}/sub_issues \
     -f sub_issue_id={new_issue_id}
   ```
2. **Program board?** "Should this bug appear on the cross-repo program board (`track: program`)? Default: **no** — only set this for major incidents with stakeholder visibility." If yes, add `track: program` to the labels list.

### Step 3: Create Fix Branch

```bash
git checkout main
git pull origin main
git checkout -b fix/{short-description}
```

### Step 4: Write Regression Test FIRST

Before fixing the bug, write a test that reproduces it:
- The test should FAIL before the fix
- The test should PASS after the fix
- This prevents the same bug from recurring

```typescript
it('should handle {scenario} correctly', () => {
  // This test reproduces the bug
  // It should fail before the fix and pass after
});
```

### Step 5: Implement the Fix

- Fix the root cause, not just the symptom
- Keep the fix minimal — don't refactor unrelated code
- Follow project rules from CLAUDE.md

### Step 6: Verify

- [ ] Regression test now passes
- [ ] All existing tests still pass: `npm test`
- [ ] No new linting errors: `eslint . --max-warnings 0`
- [ ] TypeScript compiles: `tsc --noEmit`

### Step 7: Update Documentation

- [ ] Add changelog entry under `[Unreleased]` → `### Fixed`
- [ ] Update README if the fix changes behavior users rely on

### Step 8: Commit & PR

Commit with Conventional Commit format:
```
fix({scope}): {description}

{body explaining what caused the bug and how it was fixed}

Fixes #{issue-number}
```

## Rules
- ALWAYS write a regression test — no exceptions
- Fix the root cause, not the symptom
- Keep the fix minimal and focused
- Do not combine the bug fix with unrelated refactoring
