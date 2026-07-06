# Standard Workflow — a `/workflow` command for Claude Code

A disciplined, project-agnostic engineering workflow packaged as a Claude Code slash command.

Type `/workflow <what you're changing>` and Claude walks the change through six phases so nothing ships unverified:

```
pre-verify → implement → post-verify → PR → merge-verify → housekeeping
```

It bakes in the habits that actually prevent outages: explain-and-confirm before coding, isolated git worktrees, backwards-compat checks for any data/schema change, CI-enforced verification, baseline-diff testing (no *new* failures rather than the unrealistic "everything green"), migration-collision guards, and post-merge deploy verification.

> The golden rule it enforces: *"it's a tiny change, no need to verify"* is the exact thought that causes outages. The time spent verifying is always less than the time spent recovering from a missed edge case.

---

## Install

### Option A — Plugin (recommended, one-line)

In Claude Code:

```
/plugin marketplace add ishaquehassan/claude-standard-workflow
/plugin install standard-workflow@standard-workflow-marketplace
```

Then use it:

```
/workflow map candidate name field for the legacy payload
```

To update later: `/plugin marketplace update standard-workflow-marketplace`.

> Replace `ishaquehassan/claude-standard-workflow` with your own `owner/repo` if you forked or renamed it.

### Option B — Single file (no plugin system)

Copy the command into your commands folder:

```bash
# Personal (all your projects):
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/workflow.md \
  https://raw.githubusercontent.com/ishaquehassan/claude-standard-workflow/main/plugins/standard-workflow/commands/workflow.md

# — or — per project (commit it so your team gets it):
mkdir -p .claude/commands
cp path/to/workflow.md .claude/commands/workflow.md
```

Then `/workflow` is available in that scope.

---

## What it does, phase by phase

| Phase | Purpose |
|-------|---------|
| **1. Pre-verify** | Understand the change, list callers, edge cases, negative impacts, backwards-compat. Hand a plan + effort estimate to the user and **wait for confirmation before coding.** |
| **2. Implement** | Smallest clean change, atomic conventional commits, no scope creep, migration-collision guard. |
| **3. Post-verify** | Tests / lint / build — with **+0 new failures** as the honest bar, enforced in **CI**, plus a manual smoke and self-review of your own diff. |
| **4. PR** | Summary + Context + Changes + Test plan + Risks + Rollback. Self-review first. |
| **5. Merge-verify** | After merge, confirm the deploy is actually live and the feature works — catch silent deploy failures. |
| **6. Housekeeping** | Record only genuine gotchas, prune notes so context stays loadable, clean up the worktree. |

---

## Customize it

The whole workflow lives in one Markdown file: [`plugins/standard-workflow/commands/workflow.md`](plugins/standard-workflow/commands/workflow.md). Fork the repo and edit that file to match your team's branch names, CI, environments, and conventions.

---

## License

MIT — see [LICENSE](LICENSE).
