# Avionics Mission Control (MCD) — source and data history

This folder is the version history for the team dashboard
([live page](https://claude.ai/artifact/3vNtTkkUjjfwaiXJfMAPwg)). The dashboard itself is hosted on claude.ai;
this repo keeps a copy of its code and a nightly copy of its data so changes can be diffed and rolled back.

| Path | What it is | Who updates it |
|---|---|---|
| `index.html` | The exact page that is live on the dashboard | Alex (with Claude), on every dashboard change |
| `SCHEMA.md` | Data collections and field layouts | Same commit as any schema change |
| `data/*.json` | Snapshot of every data collection | The nightly sync (9:40 PM Mountain), automatically |

**Please don't edit anything in `dashboard/` by hand.** Changes here don't reach the live page; they get overwritten.
Suggest dashboard changes in a GitHub issue or to Alex instead.

## Changing the dashboard (code)

1. Edit `dashboard/index.html` and commit it **first**, message starting with `MCD:` (e.g. `MCD: calendar biweekly repeats`).
2. Publish that exact file to the live page.
3. Record the resulting live version id in a follow-up line of the commit message or a tag, e.g.
   `git tag mcd-1795030539-d0b0`. The live version id is shown by the artifact tools.

The live page and `index.html` on `main` should always be byte-identical. Before editing, check the live page's
SHA-256 against the file here; if they differ, someone published without committing — commit the live copy first.

**Rollback:** `git show <good commit>:dashboard/index.html > index.html`, then publish that file. Then commit it as
`MCD: revert to <short sha>` so `main` matches the live page again.

## Data snapshots

Each `data/<collection>.json` maps document id → `{ version, updatedAt, data }`, keys sorted, so `git diff` and
`git log -p dashboard/data/taskers.json` show exactly what changed each night.

Collections: `crew` (member pages), `updates` (daily-update queue), `taskers`, `calendar`, `events` (timeline
milestones), `agendas` (Avionics sync agendas + delegation board), `photos` (direct gallery uploads; asset ids only),
`meta` (sync bookmark).

**Restore a deleted or damaged item:** find the last good version with
`git log -p dashboard/data/<collection>.json`, then write that document's `data` back to the dashboard
(Claude does this with a database write on `<collection>/<id>`). Snapshots are nightly, so anything created and
deleted on the same day is not captured.

**Not in the snapshots:** image files. Daily-update photos are already in each member's `photos/` folder; photos
dragged straight into the dashboard gallery exist only on the dashboard (the snapshot records their asset ids).
