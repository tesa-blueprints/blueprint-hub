# tesa-blueprints

Central knowledge base and standards organization for clean, secure software development.

## Quick Start: Add Standards to Your Project

Copy a ready-made CLAUDE.md starter into your project root — Claude Code will automatically follow the rules:

```bash
# TypeScript App (React/Next.js + Node.js)
cp starters/CLAUDE-typescript-app.md /path/to/your/project/CLAUDE.md

# Terraform (Azure)
cp starters/CLAUDE-terraform.md /path/to/your/project/CLAUDE.md

# Fullstack (App + Infrastructure)
cp starters/CLAUDE-fullstack.md /path/to/your/project/CLAUDE.md
```

For the full setup guide, see [CLAUDE-INTEGRATION-GUIDE.md](CLAUDE-INTEGRATION-GUIDE.md).

## Blueprints

| Repository | Description | For Claude |
|------------|-------------|------------|
| [blueprint-project-management](https://github.com/tesa-blueprints/blueprint-project-management) | Git workflows, issues, PRs, releases, documentation | `claude-config/CLAUDE.md` |
| [blueprint-security-advisory](https://github.com/tesa-blueprints/blueprint-security-advisory) | OWASP, clean code, secrets, scanning, repo setup | `claude-config/CLAUDE.md` |
| [blueprint-terraform-guide](https://github.com/tesa-blueprints/blueprint-terraform-guide) | Terraform + Azure, modules, state, CI/CD | `claude-config/CLAUDE.md` |
| [blueprint-application-coding-guide](https://github.com/tesa-blueprints/blueprint-application-coding-guide) | TypeScript, React, Node.js, testing, auth | `claude-config/CLAUDE.md` + frontend + backend |

## Structure

Every blueprint repo follows the same structure:

```
CLAUDE.md               Overview for Claude sessions exploring this blueprint
README.md               Overview and table of contents for humans
docs/                   Human-readable guides (numbered)
templates/              Copy-ready configs, workflows, and templates
claude-config/          Reusable CLAUDE.md files to copy into your projects
```

## Starters (Combined Rules)

Pre-built, all-in-one CLAUDE.md files that combine rules from multiple blueprints:

| Starter | Use Case | Sources |
|---------|----------|---------|
| [CLAUDE-typescript-app.md](starters/CLAUDE-typescript-app.md) | React/Next.js + Node.js | PM + Security + App Coding |
| [CLAUDE-terraform.md](starters/CLAUDE-terraform.md) | Terraform + Azure | PM + Security + Terraform |
| [CLAUDE-fullstack.md](starters/CLAUDE-fullstack.md) | App + Infrastructure | All 4 blueprints |

## This Repo

`placeholder-codespace` serves as the central hub and working environment (GitHub Codespace) for managing the blueprints. It contains:
- `starters/` — Ready-to-use combined CLAUDE.md files
- `CLAUDE-INTEGRATION-GUIDE.md` — Full guide on how to use blueprints with Claude Code
