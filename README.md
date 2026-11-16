# Buying a theme with ETFs

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-GITHUB-USER/YOUR-REPO/blob/main/theme_etfs.ipynb)

Companion notebook to the Medium post **"Buying a Theme Instead of Picking Stocks: The AI Build-Out in Seven ETFs"**.

It takes a handful of thematic ETFs and shows:

1. **The theme in layers**: an arc diagram of which ETF covers which part of the theme.
2. **What you actually own**: a Sankey from funds to categories to the top holdings, plus a fund-overlap heatmap.
3. **Could you have picked the winners?** Each fund vs its top holdings, over a long and a short period.
4. **What the portfolio would be worth today**, compared with the S&P 500.

> **Not investment advice.** An educational exploration only. Data comes from Yahoo Finance through the
> unofficial `yfinance` library and may be incomplete, delayed or wrong.

## Run it

- **Colab:** click the badge above, then *Runtime → Run all*.
- **Locally:** `pip install -r requirements.txt`, then open `theme_etfs.ipynb` in Jupyter and run all cells.

The first run downloads data from Yahoo (a minute or two) and caches it in `data/`. Charts are saved to `charts/`
as HTML and PNG; summary tables as CSV.

## Use your own theme

Everything you need to change is in the **Configuration** cell:

| Setting | What it does |
|---|---|
| `ETFS` | Your funds, one per layer, in display order |
| `ARC_LINKS` | How the layers depend on each other (your thesis) |
| `WEIGHTS` | `None` for equal weight, or your own split |
| `TOP_N_SANKEY`, `TOP_N_PERF` | How many top holdings to show (Yahoo provides up to 10) |
| `LONG_PERIOD`, `SHORT_PERIOD`, `GROWTH_PERIOD` | Time windows for the performance charts |
| `CATEGORY_SOURCE` | How companies are grouped in the Sankey (below) |
| `HOLDINGS_FILE` | Optional CSV with full holdings (`fund, symbol, name, weight`) to go beyond Yahoo's top 10 |

Colors, fonts and sizes live in one place, the **Style** cell.

### Grouping companies into categories

- `"yahoo"`: Yahoo's industry or sector for each company. Automatic, works for any theme.
- `"manual"`: your own tags, in `MANUAL_TAGS` or in `data/categories.csv` (`symbol, category`; the file wins).
- `"claude"`: Claude proposes a category for each untagged company, choosing from your `CATEGORIES`, with a
  confidence and a reason, and writes them to `data/categories.csv`. Review and edit the file; later runs use it
  and only ask about new companies. Needs an Anthropic API key in the `ANTHROPIC_API_KEY` environment variable
  (in Colab: the key icon on the left, as a secret named `ANTHROPIC_API_KEY`).

Every run writes `data/categories_review.csv` listing each company, its category and where the category came from.

## Notes and limitations

- Holdings are **today's** top holdings. Looking back over long periods flatters them, because companies become
  top holdings partly by rising.
- Returns are total returns (dividends reinvested), buy and hold, no taxes or trading costs. Non-US stocks are
  converted to US dollars.
- Yahoo occasionally changes or rate-limits its data. If a download fails, wait a minute and rerun; set
  `REFRESH = True` to download everything again.
