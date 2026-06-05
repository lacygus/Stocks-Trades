# Congressional Stock-Trading Analysis

An exploratory analysis of **U.S. House of Representatives stock trades**, joined to each
member's **party affiliation**, to study how trading behavior differs across parties.

## Data

| Source | What it provides |
|---|---|
| [House Stock Watcher](https://housestockwatcher.com/) (`all_transactions.json`) | Every reported House stock transaction: representative, ticker, type, dates, amounts |
| [`unitedstates/congress-legislators`](https://github.com/unitedstates/congress-legislators) | Current + historical member metadata, including party |

Both are downloaded at runtime, so there is nothing to commit.

## What the notebook does

1. Loads the transaction dataset and cleans column types.
2. Attaches each representative's **party** by matching names against the
   `congress-legislators` dataset.
3. Explores trading patterns by party and ticker (e.g. top-10 traded stocks, Tesla trades).
4. Runs permutation tests on the missingness of `ticker` / `owner` fields.

## Why party affiliation no longer breaks every year

The original version scraped party from `house.gov/representatives` with BeautifulSoup.
That page is rebuilt every new Congress, which broke the hardcoded 438-member count, the
`<td>` table parsing, and the single-letter party detection, so the step needed manual
patching each year.

It now joins against the community-maintained, versioned
[`congress-legislators`](https://github.com/unitedstates/congress-legislators) JSON
(current **and** historical members, matched on normalized names). There is no HTML to
break, so the pipeline is stable across Congresses.

## Run

```bash
pip install -r requirements.txt
jupyter notebook stocks.ipynb
```

Requires an internet connection (both datasets are fetched live).
