# HEALTH LOG — Project Documentation

## Live site
Deployed at: https://healthtracking.pages.dev (or your custom domain if configured)

## Stack
- Single HTML file (`index.html`) — all JS, CSS, charts inline
- Hosted on Cloudflare Pages, auto-deploys on every GitHub push
- Data persists in browser via `localStorage` + Cloudflare artifact storage key `fuel-log:days`
- Charts: Chart.js 4.4.1 via CDN

## How Claude updates the site
Claude (in the HEALTH LOG project conversation) has:
- GitHub token stored in conversation memory → pushes `index.html` to this repo
- Cloudflare auto-deploys on every push to `main` — no manual step needed
- Full deploy takes ~30 seconds after Claude pushes

**GitHub repo:** `arosales-stack/healthtracking`
**Cloudflare project:** `healthtracking`
**Branch:** `main`
**File:** `index.html` (root)

## Daily food logging workflow
Logging happens in the HEALTH LOG Claude project conversation:
1. Drop food items in natural language any number of times during the day
2. Claude keeps a running total but does not write anything yet
3. Signal end of day: "log it" / "done" / "c'est tout" / "fin"
4. Claude generates a one-tap updater artifact
5. Tap it once → writes to `fuel-log:days` → dashboard updates

**Garmin burn:** give weekly total (Claude divides by 7) or daily figure directly.

## Daily website update workflow
When you want the live site updated after any code change:
1. Ask Claude to make the change in the project conversation
2. Claude edits `index.html` locally and pushes to GitHub via API
3. Cloudflare deploys automatically — done in ~30 seconds
4. No terminal, no manual upload needed

## Storage schema
Key: `fuel-log:days`
```
{
  "YYYY-MM-DD": {
    cal,      // total kcal eaten
    p,        // protein g
    c,        // carbs g
    f,        // fat g
    s,        // sugar g
    burn      // Garmin kcal burned (0 if not provided)
  }
}
```

## Targets
- Intake: 1800–1900 kcal
- Protein: ≥125g
- Carbs: 150–200g
- Fat: 50–70g
- TDEE: ~2,341 kcal/day (Garmin 1-year avg: 1,631 resting + 710 active)

## Energy data (Garmin Connect — Jun 2025–Jun 2026)
- Avg active: 4,971 kcal/week → ~710/day
- Avg resting: 11,417 kcal/week → ~1,631/day
- Avg total: 16,388 kcal/week → ~2,341/day

## Activity data
Sourced from Garmin CSV export (`Activities_3.csv`, 280 rows).
Baked into `index.html` as the `ACTIVITIES` array.
Types tracked: Cardio, Running, Cycling, Indoor Cycling, Yoga.

## Panels
- **History** — 7/30/90D: intake vs TDEE vs active kcal, macros, training volume
- **Training** — sessions by type, daily volume, HR per session, recent session list
- **Analysis** — full 8-month timeline, energy balance, training vs active kcal, carb% vs intensity, integrated pattern readout

## GitHub token
Stored in this conversation's context. If the conversation is reset or token expires,
generate a new classic token at github.com → Settings → Developer settings →
Personal access tokens → Tokens (classic) → repo scope only.
Token expiry: 30 days from creation (check GitHub for current expiry).

## Cloudflare
Project name: `healthtracking`
Account ID: stored in conversation
Git integration: connected to `arosales-stack/healthtracking` main branch
No build command — static file deploy only.
