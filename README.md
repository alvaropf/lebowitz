# Leibowitz Dashboard

An interactive P/B and P/E decomposition tool (after Martin Leibowitz's franchise-value
framework). For each market it splits the valuation multiple into **Book + Moat + Growth**
and shows the years-of-moat the price implies.

Pure HTML/CSS/JS in a single file — no build step, no dependencies.

## Deploy (Vercel + GitHub)

1. Push this folder to a GitHub repo.
2. At vercel.com → **Add New… → Project** → import the repo.
3. Framework preset: **Other**. Leave build settings blank (there's no build step).
4. **Deploy**. Vercel serves `index.html` from the root automatically.

Every later `git push` to `main` redeploys the site. Pushing to any other branch gives
you a preview URL.

## Updating the numbers

`markets.csv` is the **single source of truth**. The dashboard fetches it on load —
there's no data baked into the HTML to keep in sync. To update:

1. Open `markets.csv` (in Excel, Sheets, or any editor).
2. Change the figures, keeping the columns intact.
3. Commit and push. Vercel redeploys in ~30 seconds with the new numbers.

### Columns

| Column   | Meaning                                  | Source                          |
|----------|------------------------------------------|---------------------------------|
| `key`    | Internal id (lowercase, no spaces)       | you choose — must be unique     |
| `name`   | Display name (e.g. `Nasdaq-100`)         | —                               |
| `sub`    | Subtitle (e.g. `US Tech`)                | —                               |
| `ticker` | Ticker shown in the panel                | —                               |
| `pe`     | Trailing P/E                             | FactSet `P_E_RATIO`             |
| `pb`     | Price / Book                             | FactSet `PRICE_TO_BOOK`         |
| `roe`    | Return on equity, % (5Y median)          | FactSet `ROE_5Y_MEDIAN`         |
| `growth` | EPS CAGR, % (5Y)                         | FactSet `EPS_5Y_CAGR`           |
| `coc`    | Cost of capital, % (10Y yield + ERP)     | Manual (Damodaran ERP + yield)  |

Add a market by adding a row; remove one by deleting its row. The order in the file
is the order in the dropdown.

> **Editing in Excel:** when saving, choose **CSV UTF-8 (Comma delimited)**. Don't add
> blank columns or rename the header row.

> **Testing locally:** opening `index.html` by double-click can't read the CSV (browsers
> block local file reads), so it shows built-in sample data with a note. To preview the
> real CSV locally, run a tiny server in the folder — `python3 -m http.server` — and open
> `http://localhost:8000`. On Vercel it just works.

The figures are also editable live in the browser (type into any field in the left panel),
but those edits are session-only and reset on refresh — `markets.csv` is what persists.
