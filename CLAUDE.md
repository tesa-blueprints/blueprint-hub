# Blueprint Hub

This repository is the central entry point of the [tesa-blueprints](https://github.com/tesa-blueprints) organization. It provides combined `CLAUDE.md` starters, custom Claude Code slash commands, and the integration guide that ties the four content blueprints together.

## What This Repo Contains

- `starters/` — Pre-built combined `CLAUDE.md` files (TypeScript app, Terraform, fullstack) that merge rules from multiple blueprints. Each starter ships with a Definition of Done checklist Claude self-checks before commits.
- `commands/` — Custom slash commands (`/project:review`, `/project:pre-pr`, `/project:add-feature`, etc.) to be copied into a project's `.claude/commands/`.
- `CLAUDE-INTEGRATION-GUIDE.md` — Full setup guide for using blueprints with Claude Code in a target project.
- `REPO-SECURITY-STATUS.md` — Tracking of security posture across the org's repositories.
- `PROGRAM-BOARD-SETUP.md` — Setup and operations of the cross-repo program-level GitHub Project. Issues labelled `track: program` in any org repo are auto-aggregated here for portfolio visibility.

This repo itself does not contain code — it curates and points to the four content blueprints.

## Sibling Repos in the `tesa-blueprints` Multi-Repo Workspace

When opened via the `tesa-blueprints.code-workspace` multi-root workspace, the following sibling repos are available alongside this one. Each has its own `CLAUDE.md` and `claude-config/CLAUDE.md` — respect the repo-local rules when editing files inside that repo.

| Sibling | Absolute Path | Role |
|---------|---------------|------|
| `blueprint-project-management` | `/home/azureuser/projects/blueprint-project-management` | Git workflows, issues, PRs, releases, documentation standards |
| `blueprint-security-advisory` | `/home/azureuser/projects/blueprint-security-advisory` | OWASP, clean code, secrets, scanning, repo setup |
| `blueprint-application-coding-guide` | `/home/azureuser/projects/blueprint-application-coding-guide` | TypeScript, React/Next.js, Node.js, testing, auth |
| `blueprint-terraform-guide` | `/home/azureuser/projects/blueprint-terraform-guide` | Terraform + Azure, modules, state, CI/CD pipelines |

### Cross-Repo Working Rules

- **Edit each repo independently.** Each repo has its own `.git`, its own remote, its own PR flow. No cross-repo commits — when a change spans repos, open one PR per repo.
- **Use absolute paths** when editing sibling files (e.g., `Read`/`Edit` on `/home/azureuser/projects/blueprint-security-advisory/...`). `Grep`/`Explore` only see the current working directory's tree, so launch from the right folder or pass an explicit search path.
- **Hub ↔ Blueprint consistency.** When the structure or rules of any blueprint change (`docs/`, `templates/`, or `claude-config/CLAUDE.md`), check whether the corresponding entries here in `blueprint-hub` need updating:
  - `README.md` — the Blueprints table and the structure description
  - `starters/CLAUDE-*.md` — the combined rules files (these inline content from `claude-config/CLAUDE.md` of multiple blueprints)
  - `CLAUDE-INTEGRATION-GUIDE.md` — references to blueprint paths or commands
  - `commands/` — slash commands that reference blueprint conventions
- **Starters are downstream of `claude-config/`.** The source of truth for any rule lives in a single blueprint's `claude-config/CLAUDE.md`. Starters here are derived combinations — never edit a starter rule without first updating the upstream blueprint.

## Editing This Repo

- Keep all content in English (org-wide rule).
- When adding a new starter, also add it to the README's Starters table.
- When adding a new slash command, also add it to the README's Custom Commands table and to `CLAUDE-INTEGRATION-GUIDE.md`.
- Treat `REPO-SECURITY-STATUS.md` as a status snapshot — update it whenever a repo's security posture changes (CodeQL, branch protection, secret scanning).
