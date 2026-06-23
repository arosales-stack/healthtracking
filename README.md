# HEALTH LOG — Project Documentation

## Live site
Deployed at: https://healthtracking.pages.dev

## Stack
- Single HTML file (`index.html`) — all JS, CSS, charts inline
- Hosted on Cloudflare Pages, auto-deploys on every GitHub push
- Data persists in browser via `localStorage` + storage key `fuel-log:days:v2`
- Charts: Chart.js 4.4.1 via CDN

## CRITICAL: What Claude must do at the start of every new conversation
1. Fetch the live `index.html` from GitHub via API — NEVER use the stale project file
2. Work from that fetched file for all edits

## CRITICAL: What Claude must do on every single deploy
1. Edit `index.html` with the required changes
2. Push `index.html` to GitHub via API
3. Rebuild and present the updated `health-log.xlsx` — NO EXCEPTIONS
4. Confirm both are done before ending the response

## Daily food logging workflow
1. User drops food items in natural language throughout the day
2. Claude keeps a running total, one line per item, running total updated after each
3. Oats = 38 kcal per tablespoon — non-negotiable
4. Signal end of day: "log it" / "done" / "c'est tout" / "fin"
5. Claude writes the day to the seed data in index.html, pushes to GitHub, rebuilds Excel

## Training categories (as of Jun 21 2026)
- **LowerBody** — glutes, legs, hip hinge (orange #E8A23D)
- **UpperBody** — push, pull, shoulders (purple-pink #C084FC)
- **Cardio** — BodyCombat, conditioning, intervals (red #E0314F)
- **Running** — (teal #2BB6A3)
- **Cycling** — (purple #6B5FD4)
- **Yoga** — (grey #9A9499)
Garmin lumps strength into "Cardio" — user tells Claude actual session breakdown daily.

## Daily Garmin burn
User provides daily or weekly screenshot. Claude logs exact daily figure per day.
GARMIN_DAILY lookup in index.html overrides weekly averages for exact dates.

## Storage schema
Key: `fuel-log:days:v2`
```
{
  "YYYY-MM-DD": {
    cal, p, c, f, s, burn, est (optional)
  }
}
```

## Nutrition targets
- Intake: 1800–1900 kcal
- Protein: ≥125g
- Carbs: 150–200g
- Fat: 50–70g
- TDEE: ~2,341 kcal/day (Garmin 1-year: 1,631 resting + 710 active)

## GitHub repo
arosales-stack/healthtracking — token in memory, repo scope, 30-day expiry
Deploy: push index.html to root of main branch via GitHub Contents API

## Cloudflare
Project: healthtracking — connected to GitHub main, auto-deploys on push
Account ID: cc4b3a9350fc1bc09d651d21ad508681

## Excel file: health-log.xlsx
Three sheets: Food Log, Training Log, Energy Summary
Must be rebuilt and presented on EVERY deploy. No exceptions.
Dark theme matching the dashboard.
