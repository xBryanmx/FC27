# FC 27 SBC fodder calculator

A static site that finds the cheapest mix of fodder cards to finish an
Item Score SBC, including the score you have already submitted, and checks
whether the reward covers the cost. All times on the page are Pacific Time.

## Host it on GitHub Pages

1. Create a public GitHub repository and upload everything in this folder,
   including the hidden `.github` folder.
2. Open **Settings → Pages**, set **Source** to **Deploy from a branch**,
   choose `main` and `/ (root)`, then save.
3. Your site is live at `https://<username>.github.io/<repo>/`.

## How updating works

The page reads `data.json` every time it loads. To update prices or add an SBC,
edit `data.json` on GitHub (pencil icon, then **Commit changes**). The site
refreshes within about a minute.

- `updated`: ISO UTC time of your edit. Change it when you change prices, so
  visitors' saved fodder prices refresh to the new ones.
- `fodder`: one row per rating. `label`, `lo` and `hi` (the rating range),
  `score` (Item Score), `price`, and an optional `tip` shown when there is no
  player list for that rating.
- `sbcs`: one entry per SBC tile. Fields: `id`, `type` (`potm`, `player`,
  `upgrade` or `foundation`, used by the filter buttons), `name`, `reward`,
  `target`, `value` (reward worth, or `null`), `maxRating` (for example `83`
  for the Gold Re-Roll, or `null`), `maxPlayers` (or `null`), `req` (text
  shown when there is no max rating), `starts` and `expires` (UTC, or `null`),
  `repeatable` (`true`, `false` or `null` if unknown), `note`.
- SBCs appear at `starts` and drop off by themselves at `expires`, so you only
  have to add new ones.
- `pack` (optional, inside an SBC): `cards` in the reward pack and `perCard`
  chances in percent for `85+`, `86+`, `87+` and `promo`. These feed the odds
  table. Visitors can also type their own odds on the page.
- `players`: the cheapest cards for each rating (81–88), as
  `["Name", price, "POS"]`. They feed the "Cheapest players by rating" panels
  (6 shown, "Show all" for the rest) and the "Where to buy" lists. `playersNote`
  is the line about where those prices came from.

Rows above an SBC's `maxRating` are skipped by the solver and marked
"Not allowed" in the fodder table.

A scheduled workflow (`.github/workflows/price-reminder.yml`) runs daily at
18:30 UTC (10:30 AM PST, 11:30 AM PDT, just after EA's usual content drop).
If `data.json` is more than 24 hours old, it opens an issue titled
"Update FC 27 prices" so GitHub emails you. GitHub schedules always use UTC.

## Why prices are not fetched automatically

FUTBIN, FUTWIZ and FUT.GG block programmatic access, EA's community API is
partner-only, and scraping can break those sites' terms. So the site stays
manual and compliant. The page shows how old the prices are and turns the
badge red after 36 hours.

## Notes

- `index.html` holds a built-in copy of `data.json` as a fallback for when the
  file can't load (for example, opening the page by double-clicking it). Edit
  `data.json` for real updates.
- No build step. Fonts load from Google Fonts and fall back to system fonts.
- The site never connects to EA or your account.
