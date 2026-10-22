# Fork notes

Personal fork of [`MadsLorentzen/ai-job-search`](https://github.com/MadsLorentzen/ai-job-search)
(now also reachable as `ai-job-search-dev/ai-job-search`; GitHub redirects between them).

## Branch layout

| Branch | Tracks | Purpose |
|---|---|---|
| `master` | `upstream/master` | **Mirror of upstream.** Never used for our own work. Exists so upstream updates can be pulled in and reviewed without touching `main`. |
| `main` | `origin/main` | **Our working branch.** Everything we personalize lives here. |

Remotes:

```
origin    git@github.com:xaicd/ai-job-search.git          # our repo
upstream  https://github.com/MadsLorentzen/ai-job-search.git  # the framework
```

## Pulling upstream updates

```bash
git fetch upstream
python3 tools/check_upstream_updates.py --remote upstream   # which of my files changed upstream?
python3 tools/upstream_triage.py --remote upstream           # which upstream commits are worth reviewing?
git merge upstream/master                                     # into main
```

Both tools are report-only: they never merge, push, or edit anything.

## The candidate profile is not in git

This repository is **public** (GitHub does not allow private forks of public repos), and
`/setup` writes real personal data — name, address, phone, email, LinkedIn, employment
history, salary expectations, reference contacts — into three files. Those are therefore
**untracked and gitignored**, not "ignored after the fact":

- `CLAUDE.md`
- `.claude/skills/job-application-assistant/01-candidate-profile.md`
- `.claude/skills/job-application-assistant/02-behavioral-profile.md`

The pristine placeholder versions live on `master` (the upstream mirror). To restore them
into a fresh clone:

```bash
git checkout master -- CLAUDE.md \
  .claude/skills/job-application-assistant/01-candidate-profile.md \
  .claude/skills/job-application-assistant/02-behavioral-profile.md
```

They are then ignored again, and `/setup` fills them in locally.

Everything else personal is already gitignored by upstream: `documents/**`,
`job_search_tracker.csv`, `salary_data.json`, `gmail_sync/`, `company_research/*.json`,
`job_scraper/seen_jobs.json`, and the generated `cv/main_*` / `cover_letters/cover_*`
documents.

## Keeping a backup of the profile

The profile files are only on this machine. If you want a private backup, push them to a
**private** repository as a second remote rather than re-adding them here:

```bash
git remote add private git@github.com:<you>/<private-repo>.git
```

## Upstream's hardcoded slug

`tools/check_upstream_updates.py` still defaults to `MadsLorentzen/ai-job-search`. That URL
redirects to the renamed `ai-job-search-dev/ai-job-search` and works as-is, but the constant
can be updated if the redirect ever goes away.
