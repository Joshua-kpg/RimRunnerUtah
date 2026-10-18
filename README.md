# RimRunnerUtah

Unofficial Utah HS boys basketball ratings. Not affiliated with the UHSAA or MaxPreps.
Status: **LIVE** at https://rimrunnerutah.web.app/ (Firebase Hosting, live since 2026-10-18). Backup copy: GitHub Pages.
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy.
Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 09:30 and 16:00 UTC
(2:30 am / 9:00 am Mountain standard time) and on manual dispatch. Before the Utah boys season opens
(`SEASON_START` in index.html, on/after Tue Nov 17, 2026, the UHSAA First Contest Allowed date) the script makes no requests.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Utah scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. Finals dated on UHSAA moratorium days (Nov 26-29, Dec 21-25, Jan 1) are ignored.
The page recomputes Elo from those finals. It commits only when data changed, then deploys (index.html + logos/) to
Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
