# Changelog

## Unreleased

## 0.1.0

### Added

- `deuce sweep`: finds local branches merged into the base branch, by a merged pull request (`gh`, which also sees squash merges), ancestry, or `git cherry`, and says which evidence it used. A dry run by default; `--apply` removes each branch's clean worktree, deletes the local branch (`git branch -d`; the force delete only for a squash-merged PR whose head is the tip) and the remote branch (with a lease on the sha deuce saw), then fetches with `--prune`, fast-forwards the base branch and prunes worktree metadata.
- Refusals, each leaving the branch untouched: dirty, locked or missing worktrees, the main worktree, the worktree deuce runs from, commits not in the merged PR, a remote branch with new commits, several PRs or a closed PR, a PR merged into another branch, and a delete `git branch -d` would refuse. Protected names (`main`, `master`, `develop`, `release/*`, plus `.deuce.toml` and `DEUCE_PROTECT`) and fresh branches are kept.
- `deuce status`: every branch and whether it is stale, read-only.
- An audit log of every applied action (`~/.local/state/deuce/log.tsv`, or `$DEUCE_STATE_DIR`), and `deuce undo --last` to recreate the last deleted branch at its logged tip.
- A per-repository lock, so two applies cannot race.
- `--json` report with `schema_version` 1.
- Claude Code plugin: the `deuce` skill, `/deuce:sweep` (dry run first, `--apply` only after the user approves) and `/deuce:status`, and evals.
