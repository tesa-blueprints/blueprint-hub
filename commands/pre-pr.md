Run the full pre-PR checklist before creating a pull request. This verifies everything from the Definition of Done.

## Step 1: Identify Changes

Run `git diff main...HEAD --stat` and `git log main..HEAD --oneline` to understand what changed.

## Step 2: Run Checks

Execute these checks and report results:

### Code Quality
- [ ] Run `npx eslint . --max-warnings 0` — report result
- [ ] Run `npx prettier --check .` — report result
- [ ] Run `npx tsc --noEmit` — report result (if TypeScript project)

### Tests
- [ ] Run `npm test` — report pass/fail and coverage
- [ ] Verify: Do new features have corresponding tests?
- [ ] Verify: Do bug fixes have regression tests?
- [ ] Verify: Were existing tests updated for behavior changes?

### Terraform (if applicable)
- [ ] Run `terraform fmt -check -recursive` — report result
- [ ] Run `terraform validate` — report result
- [ ] Run `tflint --recursive` — report result

### Security
- [ ] Run `npm audit --audit-level=high` — report result
- [ ] Scan for hardcoded secrets in changed files
- [ ] Verify no `.env` files staged

## Step 3: Documentation Check

- [ ] Is README.md up to date with the changes?
- [ ] Is CHANGELOG.md updated under `[Unreleased]`?
- [ ] Are new public APIs documented with JSDoc/TSDoc?
- [ ] Is architecture documentation updated (if architecture changed)?

## Step 4: PR Readiness

- [ ] Commit messages follow Conventional Commits format
- [ ] All commits reference an issue (`Closes #X` or `Fixes #X`)
- [ ] PR is within 500 lines (suggest splitting if larger)
- [ ] No debug artifacts (console.log, commented-out code, TODO hacks)

## Output Format

### Pre-PR Report

**Status: READY / NOT READY**

#### Passed
- ...

#### Failed (must fix)
- ...

#### Warnings (should fix)
- ...

#### Suggested PR Description
Generate a draft PR description based on the changes:
- Summary
- Changes list
- Type of change
- Testing done
