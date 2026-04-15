Set up this repository with the tesa-blueprints standards. This command configures everything needed for a standards-compliant project.

Ask the user: What type of project is this? (Use $ARGUMENTS if provided)
- TypeScript App (React/Next.js + Node.js)
- Terraform (Azure)
- Fullstack (App + Terraform)

## Step 1: CLAUDE.md

Copy the appropriate starter CLAUDE.md into the project root:
- TypeScript App → Use the rules from CLAUDE-typescript-app.md
- Terraform → Use the rules from CLAUDE-terraform.md
- Fullstack → Use the rules from CLAUDE-fullstack.md

Write the content directly — do not rely on external files being available.

## Step 2: GitHub Templates

Create the following files:

### `.github/ISSUE_TEMPLATE/bug_report.yml`
Standard bug report form with: description, steps to reproduce, expected/actual behavior, priority, environment.

### `.github/ISSUE_TEMPLATE/feature_request.yml`
Standard feature request form with: problem description, desired solution, alternatives, acceptance criteria, priority.

### `.github/PULL_REQUEST_TEMPLATE.md`
PR template with: summary, changes, type of change (checkboxes), related issues, testing, screenshots, checklist.

### `.github/CODEOWNERS`
Create with sensible defaults based on project structure.

## Step 3: Security

### `.github/dependabot.yml`
Configure for npm (daily security, weekly versions) and github-actions ecosystems.

### `.github/workflows/security-scan.yml`
CodeQL scanning on push to main, PRs, and nightly.

### `.github/workflows/dependency-review.yml`
Dependency review on PRs — block high severity and GPL licenses.

### `SECURITY.md`
Security policy with reporting process and response times.

### `.pre-commit-config.yaml`
Pre-commit hooks: detect-secrets, trailing whitespace, end-of-file-fixer, check-yaml, check-json, no-commit-to-branch.

## Step 4: CI/CD (TypeScript projects)

### `.github/workflows/ci.yml`
Lint → Type Check → Test (with coverage) → Security Audit → SBOM → Build.

## Step 5: Code Config (TypeScript projects)

### `tsconfig.json`
Strict TypeScript configuration.

### `.eslintrc.json`
ESLint with TypeScript, import order, security rules.

### `.prettierrc`
Standard Prettier config.

### `.env.example`
Template with common variables (NODE_ENV, PORT, DATABASE_URL, etc.)

## Step 6: Documentation

### `README.md`
Create a README skeleton with all required sections: Project Name, Prerequisites, Getting Started, Architecture, Development, Testing, Deployment.

### `CHANGELOG.md`
Initialize with Keep a Changelog format.

## Step 7: Git Config

### `.gitignore`
Comprehensive gitignore for the project type (node_modules, .env, dist, .terraform, etc.)

## Step 8: Compliance Baseline

Ask the user: Does this project handle personal data of EU users?

If yes, remind them of GDPR requirements:
- Data minimization in all queries
- No PII in logs
- Data deletion and export endpoints needed
- EU data residency (West Europe / North Europe)
- Breach notification process (72 hours)

For all projects, note the applicable frameworks:
- ISO 27001 (secure development lifecycle)
- Cyber Resilience Act (SBOM, vulnerability disclosure, secure by default)
- NIS2 (if applicable: incident reporting, supply chain security)

## Output

After setup, display:
1. List of all files created
2. Remaining manual steps (enable branch protection, Dependabot, CodeQL in GitHub settings)
3. Applicable compliance frameworks and any additional steps needed
4. Link to the full setup checklist
5. Reminder to configure environment-specific security (see environment security matrix)
