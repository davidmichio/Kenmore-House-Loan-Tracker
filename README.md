# Kenmore House Ledger

A shared ownership and loan tracker for 18112 60th Ave NE, Kenmore, WA 98028,
built for David & Brian's joint home purchase financed with a CMG Financial
All In One loan. Fully self-hosted on GitHub: the page is static HTML, the
data is a JSON file in this repo, and every entry is a git commit.

## What it tracks

Every entry is tagged to a person (David or Brian) and is one of three types:

| Type | Counts toward loan position | Counts toward ownership share |
|---|---|---|
| **LOAN - deposit** — money that went directly into the All In One loan | + | + |
| **LOAN - withdrawal** — money that came directly out of the loan | − | − |
| **Misc money spent** — money spent on the house outside the loan (e.g. improvement projects) | — | + |

Derived figures:

- **Loan position** (per person) = deposits − withdrawals
- **Ownership stake** (per person) = loan position + misc money spent
- **Ownership share** = stake ÷ combined stake

## Views

- **Dashboard** — ownership split, per-person loan positions, misc spending
  totals, and a cumulative-stake chart over time.
- **Ledger** — the full audit log: every entry with its date, person, type,
  amount, note, and the timestamp it was recorded (plus an "edited" marker),
  filterable by person, type, and year, with CSV export.

## How it's built

A single self-contained page (`index.html`) with no runtime dependencies
beyond GitHub itself:

- **Data:** `transactions.json` at the repo root is the database. The page
  reads and writes it through the GitHub Contents API, so every add, edit,
  or delete lands as its own commit — `git log transactions.json` is the
  full audit history.
- **Auth:** each partner creates a GitHub token scoped to this repository
  (Contents read/write), pastes it into the page once, and it's kept in
  that browser's local storage. Entries record which GitHub account logged
  them.
- **Concurrency:** writes are sha-guarded; on a mid-air collision the page
  re-reads the file, re-applies the change, and retries.
- **Hosting:** serve `index.html` from GitHub Pages, or simply open the
  file locally — the GitHub API allows browser calls from anywhere.

There is no direct API integration with the loan account — CMG's All In One
servicing portal has no public API — so the intended workflow is a
once-a-month manual logging session.

Note: GitHub Pages sites are public even on private repos (the transaction
data itself stays private — it's only readable with a token — but the page
shell, including names and the address, would be visible to anyone with the
Pages URL). Opening `index.html` from a local copy of the repo avoids that
entirely.
