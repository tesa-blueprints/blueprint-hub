# Program Board Setup

The **tesa Program Board** is a single Org-level GitHub Project hosted in its own org [`tesa-program-board`](https://github.com/orgs/tesa-program-board/). Issues labelled `track: program` in **any org of the `tesase` enterprise** are aggregated here automatically, including issues in repos that don't yet exist (greenfield projects spawned from blueprint templates).

The model and the "what should get `track: program`" guidance live in [`blueprint-project-management/docs/03-issue-management.md#program-level-aggregation`](https://github.com/tesa-blueprints/blueprint-project-management/blob/main/docs/03-issue-management.md#program-level-aggregation). This file describes how the aggregation actually runs.

## How aggregation works

| | |
|---|---|
| Project | `tesa Program` in [`tesa-program-board`](https://github.com/orgs/tesa-program-board/) |
| Sync engine | [`tesa-program-board/program-board-sync`](https://github.com/tesa-program-board/program-board-sync) — scheduled GitHub Actions workflow |
| Frequency | Every 5 minutes (cron) + manual `workflow_dispatch` |
| Auth | Enterprise-owned GitHub App `tesa-program-board-sync`, auto-installed in every enterprise org |
| Direction | Bidirectional: label set → card added; label removed → card archived |
| Runner | `tesa-foundation-runners` runner group (enterprise self-hosted) |

The sync workflow signs an App JWT, lists every installation of the app via `GET /app/installations`, exchanges per-org installation tokens, searches for `is:open is:issue label:"track: program"` in each org, and reconciles the project state. No PATs, no per-repo workflows, no manual repo linking.

## Why this design

- **Templates stay clean** — blueprint repos are templates that get cloned into greenfield projects. Putting per-repo sync workflows into them would pollute every new project. The sync runs from one place.
- **Auto-discovery via app installations** — when a new org joins the enterprise (or a new repo is created in an existing org), it's covered automatically as soon as the app is installed there. Nothing to update in this repo.
- **No PATs** — all auth is through installation tokens issued by the app. Token rotation is per-installation and automatic.
- **GitHub Project v2 limitations** that ruled out simpler setups:
  - `Settings → Manage access` controls user/team access, not repository inclusion.
  - The native "Auto-add to project" workflow is 1-repo-per-workflow and capped per plan — doesn't scale beyond a handful of repos.
  - "Link a project" on the repo side only finds same-org projects — useless cross-org.

## Operating

### Add an issue to the program board
In the issue's source repo (any enterprise org), add the label `track: program`. The card appears on the next sync run (≤5 min).

```bash
gh issue edit <number> --repo <org>/<repo> --add-label "track: program"
```

### Remove an issue from the board
Remove the label. The card is archived on the next run.

```bash
gh issue edit <number> --repo <org>/<repo> --remove-label "track: program"
```

### Trigger a sync immediately
Repo `tesa-program-board/program-board-sync` → Actions → **Sync program board** → **Run workflow**.

### Audit
Look at the project itself, or list all currently-labelled issues across the enterprise:

```bash
for org in $(gh api /enterprises/tesase/orgs --jq '.[].login'); do
  gh issue list --search 'label:"track: program" state:open' --repo "$org" 2>/dev/null
done
```

### Roll out the `track: program` label to a new repo
The label must exist on each repo before contributors can apply it. For a new repo:

```bash
gh label create "track: program" \
  --color "5319e7" \
  --description "Surfaced in the cross-repo program board" \
  --repo <org>/<repo>
```

Add this to your greenfield-project bootstrap or blueprint template if you want it applied consistently. The label color follows [`blueprint-project-management/docs/03-issue-management.md`](https://github.com/tesa-blueprints/blueprint-project-management/blob/main/docs/03-issue-management.md).

## What to put on the program board

Apply `track: program` to issues whose status matters at the **portfolio level**:

- Initiatives or epics that span more than one sprint
- Major bugs or incidents with stakeholder visibility
- Release-tracking issues

**Not** to: routine bugs, refactors, small features, doc fixes, dependency bumps. The board should stay short and meaningful.

## Initial provisioning (one-time)

The pieces below need to exist for the sync to work. Owner: enterprise admin.

1. **Enterprise GitHub App** `tesa-program-board-sync` — created at the enterprise level, permissions `Organization: Projects R/W` and `Repository: Issues: Read`, webhook off, auto-installed in all current and future enterprise orgs. Detailed steps in the [sync repo README](https://github.com/tesa-program-board/program-board-sync#one-time-setup).
2. **Project** `tesa Program` in `tesa-program-board` — manually created (Board layout, Private). Recommended fields: Status, Owner, Target Release. Recommended views: Board by Status, Table by Source Repo, Roadmap by Target Release.
3. **Sync repo configuration** — `APP_ID` (variable) and `APP_PRIVATE_KEY` (secret) in `tesa-program-board/program-board-sync` repo settings.
4. **Label rollout** — `track: program` (color `#5319e7`) created on each enterprise repo. The bootstrap snippet above can be looped over the enterprise org list.

## Troubleshooting

For sync failures, the Actions log of `tesa-program-board/program-board-sync` is the primary diagnostic — it reports per-org issue counts, planned additions and archivals, and any per-org failures.

| Symptom | Where to look |
|---------|---------------|
| Issue labelled but not appearing | Actions log: is the source org listed? If not, the app is not installed there — install via Enterprise → GitHub Apps |
| Card stays after label removed | Sync didn't run since label change. Wait up to 5 min, or trigger workflow manually |
| All runs failing | Check `APP_ID` and `APP_PRIVATE_KEY` in the sync repo settings; verify the app still exists and the key wasn't rotated |
| Specific org failing in logs | App permissions revoked or org renamed. Reinstall the app in that org |

For the full operations runbook, see the [sync repo README](https://github.com/tesa-program-board/program-board-sync).
