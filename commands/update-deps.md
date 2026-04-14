Review and update project dependencies safely.

## Step 1: Audit Current State

### Security Audit
```bash
npm audit
```
Report all findings by severity (critical, high, medium, low).

### Outdated Packages
```bash
npm outdated
```
List all packages with available updates, categorized as:
- **Patch updates** (safe to update)
- **Minor updates** (review changelog)
- **Major updates** (breaking changes — careful)

### Unused Dependencies
```bash
npx depcheck
```
List dependencies that appear unused.

## Step 2: Triage

### Critical/High Security Issues
- Fix immediately with `npm audit fix`
- If `npm audit fix` doesn't resolve: manual update or replacement needed

### Unused Dependencies
- Verify they are truly unused (depcheck can have false positives)
- Remove confirmed unused dependencies

### Outdated Dependencies
Categorize by risk:
- **Low risk:** Patch updates → update in batch
- **Medium risk:** Minor updates → update individually, run tests
- **High risk:** Major updates → create separate issue and PR

## Step 3: Update

For each update:
1. Update the package: `npm install {package}@{version}`
2. Run tests: `npm test`
3. Run lint: `eslint . --max-warnings 0`
4. Run type check: `tsc --noEmit`
5. If all pass: include in commit
6. If anything breaks: investigate and fix or revert

## Step 4: Report

### Dependency Health Report

**Security:** X critical, Y high, Z medium findings
**Outdated:** X packages can be updated
**Unused:** X packages can be removed

### Actions Taken
- Updated: {list of updated packages with version changes}
- Removed: {list of removed packages}
- Requires manual intervention: {list with reasons}

### Remaining Issues
- {issues that couldn't be auto-fixed with explanation}

## Rules
- Never update all packages at once — do it incrementally
- Always run tests after each update
- Major version updates get their own issue and PR
- Read the changelog before updating
- Check license changes after updates
