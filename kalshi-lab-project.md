---
name: kalshi-lab-project
description: kalshi-lab repo — read-only Kalshi market analysis tool; structurally cannot place orders
metadata: 
  node_type: memory
  type: project
  originSessionId: 55d75f97-d177-4081-a273-db8b28db5a48
---

kalshi-lab is a personal Kalshi prediction-market analysis tool at
https://github.com/wbhimji/kalshi-lab (private). Local checkout at
`~/claudeworking/kalshi-lab` on the original laptop. Stack: Python +
DuckDB ingest, Streamlit dashboard, composable `analysis/` library
(screeners, trends, liquidity).

**Why:** User explored Kalshi initially as a "rolling longshot chain" path
to large payoffs with small stake, but pivoted to a general analysis tool.
Wants flexible analysis runnable from Claude Code, plus a dashboard, plus
manual-only bet placement.

**How to apply:**
- Order placement is **structurally forbidden** in this codebase. The Kalshi
  SDK is not imported anywhere outside `manual_helpers/`, and
  `tests/test_no_write_endpoints.py` greps for `place_order`/`cancel_order`
  outside that dir and fails the build. Do not add automated trading code
  without an explicit instruction — and even then, do it in a clearly-named
  separate package, never inline.
- `manual_helpers/format_order.py` only prints/clipboards a ticket; user
  places orders themselves on the Kalshi web UI.
- Use a **read-only-scoped** Kalshi API key when configuring `.env`.
- `.env`, `*.pem`, `*.db` are gitignored — each laptop has its own.

See [[risky-ops-structural-separation]] for the general principle.
