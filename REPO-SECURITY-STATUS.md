# Blueprint Repos — Security Status

This document lists the protection and audit measures active on the tesa-blueprints organization's own repositories.

Last updated: 2026-04-19

## Protection Measures

### Branch Protection (all 5 repos, branch: `main`)

| Setting | Value |
|---------|-------|
| Require pull request before merging | **Yes** |
| Dismiss stale PR approvals on new commits | **Yes** |
| Required approving reviews | 0 (bump to 1+ when team grows) |
| Require conversation resolution before merging | **Yes** |
| Require linear history | **Yes** (squash merge only) |
| Enforce for administrators | **Yes** |
| Allow force pushes | **No** |
| Allow deletions | **No** |

### Security Features

| Feature | blueprint-hub (public) | Private Repos (4) |
|---------|----------------------|-------------------|
| Secret Scanning | Enabled (auto) | **Enabled** |
| Push Protection | Enabled (auto) | **Enabled** |
| Dependabot Alerts | Enabled | **Enabled** |
| Dependabot Security Updates | Enabled | **Enabled** |
| Vulnerability Alerts | Enabled | **Enabled** |

## What This Means

### Nobody can bypass the PR process

- All changes to `main` must go through a Pull Request
- Direct pushes are blocked for everyone, including administrators
- Force pushes and branch deletions are blocked

### Full audit trail

Every change is tracked via multiple GitHub mechanisms:

| What | Where |
|------|-------|
| Who made the change | Commit author + PR author (GitHub requires signed authentication) |
| When it was made | Commit timestamp + PR merge timestamp |
| What was changed | Commit diff + PR diff |
| Why it was made | PR description + commit message + linked issue |
| Who reviewed/approved | PR review history |
| Who merged it | PR merge event |

All of this is permanently stored in:
- **Commit history** — immutable after protection (no force push)
- **PR history** — all reviews, comments, and conversations
- **GitHub Audit Log** — organization-level settings changes, access changes, repo events

### Automatic security checks

- **Secret Scanning:** Detects and alerts on committed secrets (AWS keys, tokens, etc.)
- **Push Protection:** Blocks pushes that contain recognized secrets before they enter the repo
- **Dependabot:** Alerts on vulnerable dependencies; auto-creates PRs for security updates

## How to Make Changes Now

```bash
# 1. Clone the repo
gh repo clone tesa-blueprints/{repo-name}

# 2. Create a feature branch
git checkout -b docs/update-xyz

# 3. Make your changes, commit, push
git commit -am "docs: update xyz"
git push origin docs/update-xyz

# 4. Open a PR
gh pr create --fill

# 5. Merge via squash (after review if required, or immediately if review count is 0)
gh pr merge --squash --delete-branch
```

Direct `git push origin main` is now rejected.

## Recommendations (Future)

When team members are added:

1. **Bump required approvals to 1 or 2** per repo:
   ```bash
   gh api --method PUT repos/tesa-blueprints/{repo}/branches/main/protection/required_pull_request_reviews \
     -f required_approving_review_count=1
   ```

2. **Add CODEOWNERS** file to each repo so the right people are auto-requested for review

3. **Enable required signed commits** (if GPG/SSH signing is set up):
   ```bash
   gh api --method POST repos/tesa-blueprints/{repo}/branches/main/protection/required_signatures
   ```

4. **Set up CODEOWNERS-based enforcement:**
   ```bash
   gh api --method PATCH repos/tesa-blueprints/{repo}/branches/main/protection/required_pull_request_reviews \
     -f require_code_owner_reviews=true
   ```

## Limitations

- **GitHub Advanced Security** (for private repos): not currently needed — we enabled the free Secret Scanning + Push Protection via the API. For CodeQL on private repos, GHAS license would be needed.
- **Required status checks**: Not configured (no CI workflows required to pass yet). When CI is added, status checks should be made required.
