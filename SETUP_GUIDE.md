# Setup Guide

This mirrors nattadasu's original repo structure exactly: a name header, an auto-generated
metrics dashboard, a guestbook, and a live activity feed. Files alone don't activate any of
it — GitHub needs a few one-time settings. Do these in order.

## 1. Create the special repo

GitHub only renders a profile README if the repo is named **exactly** like your username and public.

- Create a new repo: `az4if/az4if` → **Public**, no README/gitignore/license (you're bringing your own)
- Push everything in this folder (`README.md`, `.github/`) to the `main` branch

## 2. Let Actions push commits

The guestbook and activity workflows commit back into the repo, which needs write access.

- **Settings → Actions → General → Workflow permissions**
- Select **"Read and write permissions"** → Save

## 3. Create the guestbook issue

The guestbook workflow reads from **Issue #1**, so it must be the *first* issue created in the repo.

- **Issues** tab → **New issue** → title it anything, e.g. "Sign my guestbook 📝" → leave it open
- If it doesn't land on `#1` (something else already took that number), update `issue: 1` in
  `.github/workflows/guestbook.yml` to match the real number

## 4. Add your three secrets

**Settings → Secrets and variables → Actions → New repository secret** — add each of these:

| Secret name | Value |
|---|---|
| `METRICS_TOKEN` | A GitHub Personal Access Token (classic) — create one at github.com/settings/tokens with scopes `public_repo` and `read:user` |
| `LASTFM_API_KEY` | The API key from your Last.fm developer account |
| `STEAM_TOKEN` | Your Steam Web API key |

Never paste these values directly into `README.md` or any `.yml` file — the workflows already
reference them safely as `secrets.NAME`, which keeps them encrypted and out of the public repo.

## 5. Run everything once manually

Scheduled workflows won't wait for their first cron tick — trigger them by hand the first time:

- **Actions** tab → run **Update README - Recent Activity**, **Update Last Commit Badge**, **Update Guestbook**, and **Metrics**
  (use "Run workflow" on each)

## Notes

- The metrics image and activity feed can take a few minutes (or up to an hour on the schedule)
  to appear the first time.
- If a workflow fails, check its log in the **Actions** tab — it'll say exactly which secret or
  permission is missing.
- If you ever need to rotate `LASTFM_API_KEY` or `STEAM_TOKEN`, just generate a new one from the
  same dashboards and update the secret value — no code changes needed.
