# Claude Code Instructions

## GitHub Username

leshikus

## Permissions

All `gh` read-only commands (e.g., `gh pr list`, `gh pr view`, `gh repo view`) should be auto-approved without prompting.

## Weekly PR Report (`report.txt`)

When asked to create or update the PR report, follow these steps:

### Repos to query

Run `gh pr list` for these three repos:
- `ClickHouse/ClickHouse` — public upstream repo (use `--repo` flag)
- `~/ClickHouse` — leshikus fork
- `~/ClickHouse-private` — private ClickHouse repo

### What to include

- If `report.old` exists: for the **Merged** section, exclude PRs already present in `report.old`'s Merged section (check by PR URL). Always include all currently open and draft PRs regardless of `report.old`.
- If `report.old` does not exist: include merged PRs from the **last 7 days**.

### Report structure

Write `report.txt` with three sections:

```
## Merged

<list of merged PRs>

## In Review

<list of open, non-draft PRs>

## Draft

<list of draft PRs>
```

Each entry should include:
- PR URL
- PR title

### PR description quality

If a PR title is not descriptive enough, update it via `gh pr edit --title "..."`. Do not edit PR bodies.

### Commands

```bash
# List PRs
gh pr list --repo ClickHouse/ClickHouse --author leshikus --state all --limit 50 --json number,title,url,state,isDraft,body,createdAt,mergedAt,updatedAt

cd ~/ClickHouse && gh pr list --author leshikus --state all --limit 50 --json number,title,url,state,isDraft,body,createdAt,mergedAt,updatedAt

cd ~/ClickHouse-private && gh pr list --author leshikus --state all --limit 50 --json number,title,url,state,isDraft,body,createdAt,mergedAt,updatedAt

# Update a PR title
gh pr edit <URL> --title "..."
```

## Updating These Instructions

When the user gives new instructions about how to work (e.g., how to format reports, what to include/exclude, how to handle edge cases), add them to this file so they persist to future conversations.
