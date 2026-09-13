# Noita Daily Seed Archive

Archives the daily Noita seed data from Nolla's API (`https://takapuoli.noitagame.com/callback/`)
once per day via GitHub Actions, and publishes it as both a raw JSON file and a small static page
via GitHub Pages.

The endpoint returns a line like:

```
8d7016a611ceb7c6530534c83dc6c74c20ba52c6;1343445086;1626176766;1;
```

which is `version_hash;daily_seed;daily_practice_seed;1;`. This repo parses that into structured
JSON and appends one entry per day to `seed-archive.json`.

## Setup

1. Create a new GitHub repo and push these files (`.github/workflows/seed-archive.yml`,
   `seed-archive.json`, `index.html`).
2. Go to **Settings → Actions → General → Workflow permissions** and set it to
   **"Read and write permissions"** (needed so the workflow can commit back to the repo).
3. Go to **Settings → Pages**, set source to the `main` branch, root folder.
4. Wait for the first Pages deploy (~1 min), then:
   - `https://<username>.github.io/<repo>/` — friendly table view
   - `https://<username>.github.io/<repo>/seed-archive.json` — raw JSON

## Running it manually

The workflow also has `workflow_dispatch` enabled, so you can trigger a run any time from the
**Actions** tab without waiting for the daily cron, e.g. to backfill today's entry right after setup.

## Data format

```json
[
  {
    "date": "2026-09-13",
    "version_hash": "8d7016a611ceb7c6530534c83dc6c74c20ba52c6",
    "daily_seed": "1343445086",
    "daily_practice_seed": "1626176766"
  }
]
```

## Notes

- The GitHub Actions cron scheduler is best-effort and can run a few minutes late; it's not
  suitable if you need the seed the instant it changes.
- If the repo has zero activity for 60+ days, GitHub auto-disables scheduled workflows — a manual
  re-enable or any commit reactivates it.
- Missed days (e.g. an Actions outage) will show as gaps in the archive rather than being
  backfilled automatically.
