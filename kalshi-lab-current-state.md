---
name: kalshi-lab-current-state
description: "kalshi-lab in-flight work — what's scaffolded, what's stubbed, what's next"
metadata: 
  node_type: memory
  type: project
  originSessionId: 55d75f97-d177-4081-a273-db8b28db5a48
---

State as of 2026-06-28 (last updated by Opus 4.7).

**Scaffolded and committed** (initial commit `b8edcdf` on
[[kalshi-lab-project]] repo `main`):
- `ingest/client.py` — RSA-signed read-only REST client.
- `ingest/rest_poller.py` — paginated markets refresh + per-tick orderbook
  snapshots into DuckDB. `--once` or continuous (`--interval` seconds).
- `ingest/ws_subscriber.py` — WS stub; channel-message → DuckDB writer is
  TODO.
- `ingest/schema.sql` — tables: events, markets, market_snapshots, trades,
  watchlist.
- `analysis/{screeners,trends,liquidity}.py` — `longshots()`,
  `mispriced_within_event()`, `newly_listed()`, `price_momentum()`,
  `volume_surge()`, `price_series()`, `walk_book()`.
- `web/app.py` — Streamlit dashboard: Opportunities / Movers / Market
  detail / Watchlist / Events pages.
- `manual_helpers/format_order.py` — prints/clipboards a ticket; does NOT
  import the Kalshi SDK (see [[risky-ops-structural-separation]]).
- `tests/test_no_write_endpoints.py` — grep guard.
- `scripts/smoke_test.py` — hits live endpoints, flags schema mismatches,
  dry-runs a DB write.

**Not yet done / known stubs:**
- User has NOT yet run smoke test against real Kalshi API. Field names in
  `rest_poller.py` (e.g. `volume_24h` vs `volume`) may need tweaks once
  smoke test reports actual response shapes.
- `trades` table is created but unfilled — `volume_surge()` returns empty
  until WS subscriber writes to it.
- No alerting (cron + ntfy/Slack) yet.
- WS subscriber doesn't persist messages.
- `kalshi_market_url()` slug logic is a best guess; verify against real
  Kalshi URLs.

**Logical next steps (in order):**
1. Generate read-only API key at https://kalshi.com/account/api-keys,
   drop PEM into project, fill `.env`, run `python -m scripts.smoke_test`
   and fix any field mismatches it surfaces.
2. First real poll: `python -m ingest.rest_poller --once`.
3. `streamlit run web/app.py` — sanity-check Opportunities page.
4. After data has accumulated a few hours, build out a `trends` notebook
   or two as a feel for what additional screeners would be useful.
5. (Optional) Wire WS subscriber to write `trades` and live snapshots.
6. (Optional) Cron the poller via launchd on whichever laptop runs it.

**Repos:**
- code: https://github.com/wbhimji/kalshi-lab (private)
- memory (this dir): https://github.com/wbhimji/claude-memory-claudeworking
  (private)
