# Job Search Dashboard

A live, hosted view of the nightly job search tracker — mark applications, leave notes, and see new listings at a glance from any computer.

## What this is

- `index.html` — the whole dashboard (single self-contained page, no build step).
- `data/jobs.json` — the data file the page reads and writes. Seeded from tonight's Job Tracker.

The nightly job search still runs against the Excel file and Google Sheet as before. This dashboard is an additional live view on the same data — Claude will periodically pull your comments/applied-marks from `data/jobs.json` here and fold anything relevant into the next Sheet update.

## Hosting on GitHub Pages

1. In this repo, go to **Settings → Pages**.
2. Under "Build and deployment", set **Source: Deploy from a branch**, branch **main**, folder **/(root)**.
3. Save. GitHub will give you a URL like `https://<your-username>.github.io/<repo-name>/` within a minute or two.

## Connecting the dashboard so it can save your changes

The page is static (no server), so "Mark Applied" and comments save by writing directly back to `data/jobs.json` in this repo using GitHub's API, from your browser.

1. Create a **fine-grained personal access token**: GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token.
   - Repository access: **Only select repositories** → this repo.
   - Permissions: **Contents → Read and write**. Nothing else is needed.
2. Open the dashboard, click the gear icon (top right), and fill in:
   - Owner: your GitHub username
   - Repository: this repo's name
   - Branch: `main`
   - Data file path: `data/jobs.json`
   - Personal access token: the one you just created
3. Click Save. The token is stored only in that browser's local storage — it's never sent anywhere except GitHub's API. You'll need to repeat this step once per browser/computer you use.

Without a token configured, the dashboard still loads and displays everything read-only (via a plain fetch of `data/jobs.json`) — you just can't save changes from that browser.

## Notes

- "New" badge = whichever date is the most recent batch in the data (i.e., last night's additions).
- Fit Score and Fit Notes are Claude's read-only assessment; your own comments are a separate field for your reference (and for Claude to read back during future runs).
- If two devices save around the same time, the second save could occasionally overwrite the first — this is a lightweight personal tool, not built for concurrent multi-user editing.
