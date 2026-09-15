---
name: eventedge-cross-market-check
description: >-
  Compare a topic across Polymarket and Kalshi via EventEdge Oracle to spot
  cross-market divergence, paying per request over x402.
api: EventEdge Oracle Agent Context API
generated: '2026-09-15'
method: generated
source: openapi/eventedge-oracle-openapi.json
operations:
  - crossMarketTopicCheck
  - predictionMarketChangeWatch
---

# EventEdge: cross-market check

Read-only cross-market comparison for research and agent decision context. No trade
execution.

## Steps

1. **Preflight (free).** `GET /v1/agent/preflight` to confirm routes and the $0.005
   price for cross-market checks.
2. **Cross-check a topic.** Call `crossMarketTopicCheck`:
   `GET /v1/market/cross-check?term=<topic>` (the `term` query param is required).
   Returns an indicative divergence view of the topic across Polymarket and Kalshi.
3. **Settle x402.** Unpaid calls return `402 Payment Required` with the
   `payment-required` challenge (USDC on Base, `eip155:8453`). Pay and retry.
4. **Watch for movement (optional).** Follow up with `predictionMarketChangeWatch`:
   `GET /v1/market/change-watch?term=<topic>` ($0.005) to read observed quote changes
   between recurring scans.

## Notes

- `term` is required for cross-check; optional for change-watch.
- Output is indicative research context, not a trade signal or execution instruction.
