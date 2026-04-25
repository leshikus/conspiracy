# Claude Code Instructions

## GitHub Username

leshikus

## Permissions

The following `gh` commands should be auto-approved without prompting:
- `gh pr list`
- `gh pr view`
- `gh pr status`
- `gh pr checks`
- `gh pr diff`
- `gh repo view`

## Weekly PR Report (`report.txt`)

When asked to create or update the PR report, follow these steps:

### Repos to query

Run `gh pr list` for these three repos:
- `ClickHouse/ClickHouse` — public upstream repo (use `--repo` flag)
- `~/ClickHouse` — leshikus fork
- `~/ClickHouse-private` — private ClickHouse repo

### What to include

- For the **Merged** section, include PRs merged between 13:00 Munich time (CET/CEST) on Monday of last week and 13:00 Munich time on Monday of this week.
- Always include all currently open and draft PRs.

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

## Daily Report (`report-daily.txt`)

When asked to create a daily report, write `report.txt` with the following format:

```
## Daily Report YYYY-MM-DD

## Waiting for Review

<URL>
<title>
<status note>

...

## In Progress

<URL>
<title>
<status note>

...

## Merged

<URL>
<title>
```

Each entry includes:
- PR URL
- PR title
- a status note (except Merged entries); for unmerged PRs, start with one of: `Draft`, `Testing`, or nothing (use `Draft: <summary of what was done, derived from the PR body>` if the PR is a draft, `Testing` if it has a human approval, omit the status label if it is not a draft and has no human approval), followed by any additional context

### Determining review status

For every open non-draft PR, fetch its review state before writing the status note — `gh pr list` does not include this:

```bash
gh pr view <number> --repo ClickHouse/ClickHouse --json reviews \
  --jq '[.reviews[] | select(.state=="APPROVED") | .author.login]'
```

If the result contains any human login (i.e. not a bot), the status is `Testing`; otherwise omit the status label and do not write who approved the PR.

Section order: Waiting for Review (open non-draft PRs with no human approval) first, then In Progress, then Merged last.

### Time range

Include only PRs with activity (created, updated, or merged) after 13:00 Munich time (CET/CEST) of the previous day.

## Updating These Instructions

When the user gives new instructions about how to work (e.g., how to format reports, what to include/exclude, how to handle edge cases), add them to this file so they persist to future conversations.
