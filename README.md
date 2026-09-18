# Kenmore House Ledger

A shared ownership and loan tracker for 18112 60th Ave NE, Kenmore, WA 98028,
built for David & Brian's joint home purchase financed with a CMG Financial
All In One loan.

**Live dashboard:** https://claude.ai/artifact/LF9PvQpZsRsAGnNaiSo3f1

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

A single self-contained page (`index.html`) published as a Claude artifact.
Entries live in the artifact's shared database, so both partners see the same
ledger; the page renders example data (clearly badged) until the first real
entry is saved. There is no direct API integration with the loan account —
CMG's All In One servicing portal has no public API — so the intended workflow
is a once-a-month manual logging session.
