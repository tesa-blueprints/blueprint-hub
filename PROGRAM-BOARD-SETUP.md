# Program Board Setup

This document describes how to provision and run the **tesa Program Board** — the cross-repo GitHub Project that aggregates portfolio-relevant issues from the source repositories in `tesa-blueprints` (and, in future, other tesa orgs).

The model is documented in [`blueprint-project-management/docs/03-issue-management.md`](https://github.com/tesa-blueprints/blueprint-project-management/blob/main/docs/03-issue-management.md#program-level-aggregation). This file covers the operational setup.

## What This Is

A single Org-level GitHub Project (`tesa Program`) hosted in its own dedicated org **[`tesa-program-board`](https://github.com/orgs/tesa-program-board/)**. It automatically pulls in issues from the source repos (currently in `tesa-blueprints`) that carry the `track: program` label. The issue stays in its source repo; the Project only holds a card with a back-link. No copies, no sync job.

| Property | Value |
|----------|-------|
| Project name | `tesa Program` |
| Project org | `tesa-program-board` |
| Source repos | `tesa-blueprints/*` (extensible to other tesa orgs later) |
| Entry criterion | Issue has the label `track: program` |
| Aggregation mechanic | Project workflow: Auto-add items |
| Drill-down | Each card links to the source issue |

### Why a separate org for the project?

- **Clear separation of concerns** — repo orgs hold code and issues; the program-board org holds portfolio views. Member lists, permissions, and audit trails stay independent.
- **Cross-org scaling** — the same board can later aggregate issues from `tesa-platform/*`, other domain orgs, etc., without moving any repos.
- **Stakeholder access** — program managers can be members of `tesa-program-board` (read-only on the project) without needing membership in the repo orgs.

## Setup Steps

Most steps need admin permissions on `tesa-program-board` (project) and on the source repos in `tesa-blueprints` (label rollout, repository linking).

### 1. Create the Org Project

1. Go to https://github.com/orgs/tesa-program-board/projects → **New project**
2. Choose **Board** layout
3. Name: `tesa Program`
4. Visibility: **Private**

### 2. Link the Source Repositories (Cross-Org Step)

Because the source repos live in a different org, the project needs explicit access to them:

1. In the project: **Settings → Manage access → Add repository**
2. Search and add each `tesa-blueprints/*` repo from the list below
3. Verify each repo appears with **read** access for the project

Without this step, the Auto-Add workflow has nothing to scan.

### 3. Configure the Auto-Add Workflow

In the project, go to **Workflows → Auto-add to project**:

- Filter: `is:issue label:"track: program"`
- Repositories: list each linked source repo explicitly (cross-org auto-add does **not** support "all org repos"):
  ```
  tesa-blueprints/blueprint-hub
  tesa-blueprints/blueprint-project-management
  tesa-blueprints/blueprint-security-advisory
  tesa-blueprints/blueprint-application-coding-guide
  tesa-blueprints/blueprint-terraform-guide
  ```
- Enable the workflow

Optional companion workflow: **Auto-archive items** when the `track: program` label is removed or the issue is closed.

When a new repo joins `tesa-blueprints` (or another tesa org), repeat steps 2 and 3 for that repo.

### 4. Recommended Fields

Add these custom fields to the project so the board is useful at a glance:

| Field | Type | Source |
|-------|------|--------|
| Status | Single-select | `Backlog`, `Ready`, `In Progress`, `In Review`, `Done` (default) |
| Source Repo | Auto (built-in) | Repository the issue lives in |
| Type | Auto (label-derived) | `type: feature` / `type: bug` / etc. |
| Priority | Auto (label-derived) | `priority: critical` / `high` / `medium` / `low` |
| Owner | Single-select | Team or person responsible |
| Target Release | Iteration / text | Optional milestone alignment |

### 5. Recommended Views

| View | Layout | Group by | Filter |
|------|--------|----------|--------|
| Board | Board | Status | none |
| By Repo | Table | Source Repo | none |
| Roadmap | Roadmap | Target Release | `priority` ≥ medium |
| Stuck Items | Table | Status | `Status: Blocked` or last update >14d |

### 6. Roll Out the `track: program` Label Across All Repos

The label must exist on each repo before contributors can apply it. Run once per repo:

```bash
for repo in blueprint-hub blueprint-project-management blueprint-security-advisory blueprint-application-coding-guide blueprint-terraform-guide; do
  gh label create "track: program" \
    --color "5319e7" \
    --description "Surfaced in the cross-repo program board" \
    --repo "tesa-blueprints/$repo"
done
```

The full list of repos to roll out to lives in [REPO-SECURITY-STATUS.md](REPO-SECURITY-STATUS.md). Add new repos to the loop when they join the org.

## Operating the Board

### What gets `track: program`

Apply the label to issues whose status matters at the **portfolio level**:

- Initiatives or epics that span more than one sprint
- Major bugs or incidents with stakeholder visibility
- Release-tracking issues

Do **not** apply to: routine bugs, refactors, small features, doc fixes, dependency bumps. The board should stay short and meaningful.

### Adding to the board

```bash
gh issue edit <issue-number> --repo tesa-blueprints/<repo> --add-label "track: program"
```

The card appears within ~1 minute via the auto-add workflow.

### Removing from the board

```bash
gh issue edit <issue-number> --repo tesa-blueprints/<repo> --remove-label "track: program"
```

The card archives or disappears (depending on the auto-archive workflow setting).

### Audit query

```bash
gh issue list --search 'org:tesa-blueprints label:"track: program" state:open' \
  --json number,title,repository,state,labels,assignees,updatedAt
```

Run periodically to spot stale or mislabelled items.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Issue has the label but no card | Auto-add workflow not enabled, or repo not linked, or repo not in workflow filter | Check the workflow is on; confirm the repo is linked under Settings → Manage access; confirm it's listed in the workflow filter |
| Card stays after label removed | No auto-archive workflow configured | Enable "Auto-archive items" for unlabelled state, or archive manually |
| New repo's issues not appearing | Cross-org link or workflow filter missing | Add the repo under Settings → Manage access **and** add it to the Auto-Add workflow filter |
| Label missing on a repo | Roll-out script not run | Run the `gh label create` snippet for that repo |
| "Repository not accessible" error in workflow | Project lacks read on the source repo | Re-link via Settings → Manage access; ensure the linker has admin on both the project org and the source repo |
