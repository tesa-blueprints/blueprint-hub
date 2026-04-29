# Program Board Setup

This document describes how to provision and run the **tesa Program Board** — the cross-repo GitHub Project that aggregates portfolio-relevant issues from every `tesa-blueprints` repo.

The model is documented in [`blueprint-project-management/docs/03-issue-management.md`](https://github.com/tesa-blueprints/blueprint-project-management/blob/main/docs/03-issue-management.md#program-level-aggregation). This file covers the operational setup.

## What This Is

A single Org-level GitHub Project (`tesa Program`) that automatically pulls in every issue across all org repos labelled `track: program`. The issue stays in its source repo; the Project only holds a card with a back-link. No copies, no sync job.

| Property | Value |
|----------|-------|
| Project name | `tesa Program` |
| Scope | Org-level (`tesa-blueprints` org) |
| Entry criterion | Issue has the label `track: program` |
| Aggregation mechanic | Project workflow: Auto-add items |
| Drill-down | Each card links to the source issue |

## Setup Steps

Perform these once per org. Most steps need org-admin permissions.

### 1. Create the Org Project

1. Go to https://github.com/orgs/tesa-blueprints/projects → **New project**
2. Choose **Board** layout
3. Name: `tesa Program`
4. Visibility: **Private** (matches the majority of org repos)

### 2. Configure the Auto-Add Workflow

In the project, go to **Workflows → Auto-add to project**:

- Filter: `is:issue label:"track: program"`
- Repositories: leave empty to scan **all** org repos, or list them explicitly:
  ```
  tesa-blueprints/blueprint-hub
  tesa-blueprints/blueprint-project-management
  tesa-blueprints/blueprint-security-advisory
  tesa-blueprints/blueprint-application-coding-guide
  tesa-blueprints/blueprint-terraform-guide
  ```
- Enable the workflow

Optional companion workflow: **Auto-archive items** when the `track: program` label is removed or the issue is closed.

### 3. Recommended Fields

Add these custom fields to the project so the board is useful at a glance:

| Field | Type | Source |
|-------|------|--------|
| Status | Single-select | `Backlog`, `Ready`, `In Progress`, `In Review`, `Done` (default) |
| Source Repo | Auto (built-in) | Repository the issue lives in |
| Type | Auto (label-derived) | `type: feature` / `type: bug` / etc. |
| Priority | Auto (label-derived) | `priority: critical` / `high` / `medium` / `low` |
| Owner | Single-select | Team or person responsible |
| Target Release | Iteration / text | Optional milestone alignment |

### 4. Recommended Views

| View | Layout | Group by | Filter |
|------|--------|----------|--------|
| Board | Board | Status | none |
| By Repo | Table | Source Repo | none |
| Roadmap | Roadmap | Target Release | `priority` ≥ medium |
| Stuck Items | Table | Status | `Status: Blocked` or last update >14d |

### 5. Roll Out the `track: program` Label Across All Repos

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
| Issue has the label but no card | Auto-add workflow not enabled or repo not in scope | Check the workflow config; confirm the repo is listed |
| Card stays after label removed | No auto-archive workflow configured | Enable "Auto-archive items" for unlabelled state, or archive manually |
| New repo's issues not appearing | Repo not in workflow scope | Add the repo to the workflow filter |
| Label missing on a repo | Roll-out script not run | Run the `gh label create` snippet for that repo |
